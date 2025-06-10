# Agent Hub 学习规划 (Learning Roadmap)

## 项目概述

Agent Hub 包含三个独立的 AI Agent 项目，每个项目都有其独特的技术栈和应用场景：

- **LanguageMentor**: 英语私教系统
- **GitHub Sentinel**: 仓库监控工具  
- **ChatPPT**: PPT 生成助手

## 学习方式建议

### 推荐方案：分项目独立学习

#### 为什么选择这种方式？

1. **项目独立性强**
   - 每个项目都有独立的依赖环境（requirements.txt）
   - 不同的配置文件和环境变量
   - 避免依赖冲突

2. **更好的开发体验**
   - Cursor 的智能提示更精准（只针对当前项目）
   - 文件搜索和导航更高效
   - 调试和运行更方便

3. **学习效果更佳**
   - 专注于单个项目，避免混淆
   - 更容易理解项目架构
   - 便于深入学习每个项目的细节

## 学习路径规划

### 阶段一：LanguageMentor（入门级 - 1-2周）

**开始方式：**
```bash
cd language_mentor
code .  # 或者用 Cursor 打开
```

**学习重点：**
- ✅ AI Agent 的基本架构
- ✅ 提示工程（Prompt Engineering）
- ✅ Gradio 界面开发
- ✅ 对话式交互设计

**技术栈：**
- Python 基础
- OpenAI API / LLaMA 模型
- Gradio Web 框架
- JSON 数据处理

**学习目标：**
- 理解 AI Agent 的工作原理
- 掌握基本的对话系统开发
- 学会使用 Gradio 构建 Web 界面
- 了解多语言学习场景设计

### 阶段二：GitHub Sentinel（中级 - 2-3周）

**开始方式：**
```bash
cd github_sentinel
code .
```

**学习重点：**
- ✅ API 集成和数据处理
- ✅ 定时任务和守护进程
- ✅ 报告生成和通知系统
- ✅ 数据挖掘和分析

**技术栈：**
- GitHub API
- 定时任务调度
- 邮件通知系统
- Docker 容器化
- 单元测试

**学习目标：**
- 掌握第三方 API 集成
- 学会设计后台服务
- 理解数据采集和处理流程
- 掌握容器化部署

### 阶段三：ChatPPT（高级 - 3-4周）

**开始方式：**
```bash
cd chatppt
code .
```

**学习重点：**
- ✅ 多模态 AI 应用开发
- ✅ 文档解析和格式转换
- ✅ 复杂的业务流程设计
- ✅ 模板系统和布局管理

**技术栈：**
- 多模态 AI 模型
- 文档处理（DOCX, Markdown）
- PowerPoint 生成
- 语音识别（Whisper）
- 图像处理

**学习目标：**
- 掌握多模态 AI 应用开发
- 学会复杂文档处理
- 理解企业级应用架构
- 掌握模板系统设计

## 环境配置指南

### 虚拟环境管理

为每个项目创建独立的虚拟环境：

```bash
# LanguageMentor 环境
conda create -n language_mentor python=3.10
conda activate language_mentor
cd language_mentor && pip install -r requirements.txt

# GitHub Sentinel 环境
conda create -n github_sentinel python=3.10
conda activate github_sentinel
cd github_sentinel && pip install -r requirements.txt

# ChatPPT 环境
conda create -n chatppt python=3.10
conda activate chatppt
cd chatppt && pip install -r requirements.txt
```

### 必要的配置

1. **API 密钥配置**
   - OpenAI API Key
   - GitHub Token（用于 GitHub Sentinel）

2. **网络代理配置**（如需要）
   ```bash
   git config --global http.proxy http://127.0.0.1:7897
   git config --global https.proxy http://127.0.0.1:7897
   ```

## 学习检查点

### LanguageMentor 完成标准
- [ ] 成功运行基本对话功能
- [ ] 理解场景化学习设计
- [ ] 能够修改提示词优化对话效果
- [ ] 掌握 Gradio 界面定制

### GitHub Sentinel 完成标准
- [ ] 成功配置仓库监控
- [ ] 理解数据采集和处理流程
- [ ] 能够生成项目进展报告
- [ ] 掌握定时任务配置

### ChatPPT 完成标准
- [ ] 成功生成 PPT 文件
- [ ] 理解多模态输入处理
- [ ] 能够自定义模板和布局
- [ ] 掌握复杂业务流程设计

## 进阶学习建议

### 技术深入方向
1. **AI Agent 架构设计**
   - 学习更复杂的 Agent 框架（如 LangChain）
   - 研究多 Agent 协作模式

2. **性能优化**
   - 模型推理优化
   - 并发处理和缓存策略

3. **生产部署**
   - 云服务部署
   - 监控和日志系统
   - 安全性考虑

### 项目扩展方向
1. **功能增强**
   - 添加新的学习场景（LanguageMentor）
   - 支持更多数据源（GitHub Sentinel）
   - 增加更多输出格式（ChatPPT）

2. **技术栈升级**
   - 集成最新的 AI 模型
   - 采用更现代的前端框架
   - 实现微服务架构

## 学习资源推荐

### 官方文档
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Gradio Documentation](https://gradio.app/docs/)
- [GitHub API Documentation](https://docs.github.com/en/rest)

### 相关技术
- Python 异步编程
- Docker 容器化
- 单元测试最佳实践
- AI 提示工程

## 学习时间规划

| 阶段 | 项目 | 预计时间 | 主要目标 |
|------|------|----------|----------|
| 1 | LanguageMentor | 1-2周 | 入门 AI Agent 开发 |
| 2 | GitHub Sentinel | 2-3周 | 掌握数据处理和后台服务 |
| 3 | ChatPPT | 3-4周 | 精通多模态应用开发 |
| 4 | 综合实践 | 1-2周 | 项目优化和扩展 |

**总计：7-11周**

## 注意事项

1. **循序渐进**：严格按照难度顺序学习，不要跳跃
2. **动手实践**：每个项目都要实际运行和调试
3. **记录笔记**：记录遇到的问题和解决方案
4. **代码理解**：不仅要会用，还要理解实现原理
5. **持续更新**：关注项目更新和技术发展

---

*创建时间：2025年1月*  
*最后更新：2025年1月*  
*状态：规划中* 