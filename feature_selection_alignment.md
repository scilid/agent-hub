# 特征选择后的索引对齐解决方案

## 问题描述

当`new_stock`只保留聚类特征，删除了`symbol`等标识列时：

```python
# 常见情况：new_stock只包含数值特征
original_data = stock[['symbol', 'price', 'volume', 'pe_ratio', 'market_cap']]
new_stock = stock[['price', 'volume', 'pe_ratio', 'market_cap']]  # 删除了symbol！

# 聚类后无法直接匹配
kmeans_final = KMeans(n_clusters=4, random_state=10).fit(new_stock)
new_stock['cluster'] = kmeans_final.labels_

# ❌ 这样会出错，因为new_stock没有symbol列
target_index = stock[stock['symbol'] == 'sz002829'].index
```

## 解决方案

### 方案1：保留标识列（推荐）

```python
# ✅ 在new_stock中保留symbol列
feature_columns = ['price', 'volume', 'pe_ratio', 'market_cap']  # 只用于聚类的特征
new_stock = stock[feature_columns + ['symbol']].copy()  # 保留symbol

# 只对特征列进行聚类
kmeans_final = KMeans(n_clusters=4, random_state=10).fit(new_stock[feature_columns])
new_stock['cluster'] = kmeans_final.labels_

# 直接查找，不依赖索引
target_result = new_stock[new_stock['symbol'] == 'sz002829']
if len(target_result) > 0:
    cluster_num = target_result['cluster'].iloc[0]
    print(f"股票 sz002829 属于第 {cluster_num} 个集群")
else:
    print("未找到目标股票")
```

### 方案2：分离特征和标识，通过索引连接

```python
# ✅ 分离处理，保持索引对齐
feature_columns = ['price', 'volume', 'pe_ratio', 'market_cap']

# 1. 创建只包含特征的数据（用于聚类）
features_only = stock[feature_columns].copy()

# 2. 执行聚类
kmeans_final = KMeans(n_clusters=4, random_state=10).fit(features_only)

# 3. 将聚类结果添加到原始数据（索引对齐）
stock_with_clusters = stock.copy()
stock_with_clusters['cluster'] = kmeans_final.labels_

# 4. 查找目标股票
target_result = stock_with_clusters[stock_with_clusters['symbol'] == 'sz002829']
if len(target_result) > 0:
    cluster_num = target_result['cluster'].iloc[0]
    symbol_confirm = target_result['symbol'].iloc[0]
    print(f"股票代码: {symbol_confirm}")
    print(f"所属集群: {cluster_num}")
```

### 方案3：使用索引作为桥梁

```python
# ✅ 通过索引连接特征数据和标识数据
feature_columns = ['price', 'volume', 'pe_ratio', 'market_cap']

# 1. 特征数据（保持原始索引）
new_stock = stock[feature_columns].copy()

# 2. 聚类
kmeans_final = KMeans(n_clusters=4, random_state=10).fit(new_stock)

# 3. 创建聚类结果Series（与原始数据索引对齐）
cluster_series = pd.Series(kmeans_final.labels_, index=new_stock.index, name='cluster')

# 4. 查找目标股票索引
target_index = stock[stock['symbol'] == 'sz002829'].index

# 5. 通过索引获取聚类结果
if len(target_index) > 0:
    idx = target_index[0]
    cluster_num = cluster_series.loc[idx]
    symbol_confirm = stock.loc[idx, 'symbol']
    print(f"股票代码: {symbol_confirm}")
    print(f"所属集群: {cluster_num}")
```

### 方案4：创建映射字典

```python
# ✅ 创建symbol到聚类的映射
feature_columns = ['price', 'volume', 'pe_ratio', 'market_cap']

# 1. 特征数据
new_stock = stock[feature_columns].copy()

# 2. 聚类
kmeans_final = KMeans(n_clusters=4, random_state=10).fit(new_stock)

# 3. 创建symbol到cluster的映射
symbol_to_cluster = dict(zip(stock['symbol'], kmeans_final.labels_))

# 4. 直接查找
target_symbol = 'sz002829'
if target_symbol in symbol_to_cluster:
    cluster_num = symbol_to_cluster[target_symbol]
    print(f"股票 {target_symbol} 属于第 {cluster_num} 个集群")
else:
    print(f"未找到股票 {target_symbol}")

# 5. 可选：查看所有结果
cluster_df = pd.DataFrame({
    'symbol': stock['symbol'],
    'cluster': kmeans_final.labels_
})
print(cluster_df[cluster_df['symbol'] == target_symbol])
```

## 完整的推荐实现

```python
import pandas as pd
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

def robust_kmeans_clustering(stock_data, feature_columns, target_symbol='sz002829', n_clusters=4):
    """
    健壮的K-means聚类实现，处理特征选择后的索引对齐问题
    
    Parameters:
    stock_data: 原始股票数据，包含symbol列
    feature_columns: 用于聚类的特征列名列表
    target_symbol: 目标股票代码
    n_clusters: 聚类数量
    """
    
    # 1. 验证输入
    if 'symbol' not in stock_data.columns:
        raise ValueError("数据中必须包含'symbol'列")
    
    missing_features = [col for col in feature_columns if col not in stock_data.columns]
    if missing_features:
        raise ValueError(f"缺少特征列: {missing_features}")
    
    # 2. 准备特征数据
    features_data = stock_data[feature_columns].copy()
    
    # 处理缺失值
    features_data = features_data.fillna(features_data.median())
    
    # 标准化特征
    scaler = StandardScaler()
    scaled_features = scaler.fit_transform(features_data)
    
    # 3. K-means聚类
    kmeans = KMeans(n_clusters=n_clusters, random_state=10)
    cluster_labels = kmeans.fit_predict(scaled_features)
    
    # 4. 创建完整结果数据框
    result_df = stock_data[['symbol']].copy()
    result_df['cluster'] = cluster_labels
    
    # 5. 查找目标股票
    target_result = result_df[result_df['symbol'] == target_symbol]
    
    if len(target_result) == 0:
        return {
            'status': 'error',
            'message': f'未找到股票 {target_symbol}'
        }
    
    cluster_num = target_result['cluster'].iloc[0]
    
    # 6. 统计信息
    cluster_counts = result_df['cluster'].value_counts().sort_index()
    
    return {
        'status': 'success',
        'target_symbol': target_symbol,
        'cluster': cluster_num,
        'total_clusters': n_clusters,
        'cluster_distribution': cluster_counts.to_dict(),
        'cluster_dataframe': result_df
    }

# 使用示例
feature_cols = ['price', 'volume', 'pe_ratio', 'market_cap']  # 替换为您的实际特征列
result = robust_kmeans_clustering(stock, feature_cols, 'sz002829', 4)

if result['status'] == 'success':
    print(f"股票 {result['target_symbol']} 属于第 {result['cluster']} 个集群")
    print(f"各集群分布: {result['cluster_distribution']}")
else:
    print(result['message'])
```

## 关键要点

1. **保留标识信息**：要么在处理数据中保留`symbol`列，要么单独维护映射关系
2. **索引一致性**：确保聚类结果的索引与原始数据对齐
3. **分离关注点**：将特征选择、聚类计算、结果映射分开处理
4. **验证结果**：始终验证找到的股票和聚类结果是否正确

选择最适合您数据结构和处理流程的方案！