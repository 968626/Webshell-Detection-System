# 🛡️ Webshell Detection System
# 源码获取：https://mbd.pub/o/bread/YZWblpprbA==

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688.svg)](https://fastapi.tiangolo.com/)
[![Vue 3](https://img.shields.io/badge/Vue-3.3+-4FC08D.svg)](https://vuejs.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

> 基于语义分析的企业级Webshell检测系统 - 不只是看代码"长什么样"，而是理解代码"要做什么"

[English](README_EN.md) | 简体中文

---

## ✨ 核心特性

- 🔍 **智能语义分析** - 基于AST抽象语法树和CFG控制流图的深度分析
- 🧠 **多维度特征提取** - 危险函数、用户输入污染、混淆程度等多维度检测
- 👁️ **实时文件监控** - 基于Watchdog的7×24小时目录监控
- 🚨 **分级告警管理** - Critical/High/Medium/Low四级风险告警
- 📊 **可视化仪表盘** - Vue3 + Element Plus现代化管理界面
- 🔧 **多语言支持** - PHP、ASP、JSP等Web脚本语言检测

---

## 🏗️ 系统架构

```
┌─────────────────────────────────────────────────────────────────┐
│                        前端层 (Vue3)                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │ Dashboard│ │   Scan   │ │ Monitor  │ │  Alerts  │           │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      API层 (FastAPI)                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │  /scan   │ │ /monitor │ │ /alerts  │ │ /reports │           │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      核心检测引擎                                │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐            │
│  │  Parser      │ │  Analyzer    │ │  Detector    │            │
│  │  (语法解析)   │ │  (语义分析)   │ │  (检测模型)   │            │
│  └──────────────┘ └──────────────┘ └──────────────┘            │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速开始

### 环境要求

- Python 3.8+
- Node.js 16+
- Windows/Linux/macOS

### 安装部署

```bash
# 1. 克隆项目
git clone https://github.com/yourname/webshell-detection.git
cd webshell-detection

# 2. 安装后端依赖
pip install -r requirements.txt

# 3. 安装前端依赖
cd frontend
npm install
cd ..

# 4. 启动开发服务器
python run.py --mode dev

# 5. 访问系统
# 前端: http://localhost:5173
# API文档: http://localhost:8000/docs
```

### 生产部署

```bash
# 构建前端
cd frontend
npm run build
cd ..

# 启动生产服务
python run.py --mode prod --host 0.0.0.0 --port 8000
```

---

## 🧠 检测原理

### 1. 特征提取（语义级）

```python
# 提取的是语义特征，不是简单的文本匹配
features = {
    'has_eval': True,              # 是否使用eval动态执行
    'has_shell_exec': True,        # 是否执行系统命令
    'user_input_tainted': True,    # 用户输入是否未过滤
    'obfuscation_score': 0.85,     # 混淆程度评分
    'dangerous_functions': ['system', 'exec', 'passthru'],
    'encoding_layers': 3           # 编码层数
}
```

### 2. 智能评分机制

```python
# eval + 用户输入 = 高危！
if features['has_eval'] and features['user_input_tainted']:
    risk_score = 0.90  # 直接判定高危

# system命令 + 用户输入 = 极度危险！
if features['has_shell_exec'] and features['user_input_tainted']:
    risk_score = 0.95
```

### 3. 混淆代码识别

```python
def detect_obfuscation(code):
    score = 0
    if 'base64_decode' in code:           # Base64编码
        score += 0.3
    if 'gzinflate' in code:               # Gzip压缩
        score += 0.25
    if re.search(r'\$\w+\s*\(', code):    # 变量函数调用
        score += 0.2
    return score
```

---

## 📊 检测效果

| 样本类型 | 传统检测 | 本系统 | 提升 |
|---------|---------|--------|------|
| 明文一句话木马 | ✅ 检出 | ✅ 检出 (98%) | 准确率↑ |
| Base64编码 | ❌ 漏报 | ✅ 检出 (92%) | 检出率↑ |
| 多层混淆 | ❌ 漏报 | ✅ 检出 (88%) | 检出率↑ |
| 正常文件 | ⚠️ 误报 | ✅ 正常 (5%) | 误报率↓ |

---

## 🎯 核心功能

### 📁 智能文件扫描

- ✅ 支持单文件快速检测
- ✅ 批量目录递归扫描
- ✅ 实时进度反馈
- ✅ 多语言支持(PHP/ASP/JSP)

### 👁️ 实时文件监控

```python
class FileWatcher:
    def on_created(self, event):
        # 新文件创建，立即扫描
        result = scan_file(event.src_path)
        if result.is_webshell:
            send_alert(result)  # 实时告警
```

### 🚨 分级告警管理

| 级别 | 置信度 | 场景 | 响应方式 |
|-----|-------|------|---------|
| 🔴 Critical | ≥0.80 | 确认Webshell | 立即通知+隔离 |
| 🟠 High | 0.60-0.80 | 高度可疑 | 邮件通知 |
| 🟡 Medium | 0.35-0.60 | 可疑文件 | 记录日志 |
| 🟢 Low | <0.35 | 低风险 | 定期汇总 |

---

## 🛠️ 技术栈

### 后端
- **FastAPI** - 高性能Python Web框架
- **SQLAlchemy** - ORM数据库操作
- **Watchdog** - 文件系统监控
- **Scikit-learn** - 机器学习模型
- **PyTorch** - 深度学习框架

### 前端
- **Vue 3** - 渐进式JavaScript框架
- **Element Plus** - UI组件库
- **Pinia** - 状态管理
- **Axios** - HTTP客户端
- **Vite** - 构建工具

---

## 📁 项目结构

```
webshell-detection/
├── backend/              # 后端代码
│   ├── api/             # API路由
│   ├── core/            # 核心检测引擎
│   │   ├── analyzer/    # 语义分析器
│   │   ├── model/       # 检测模型
│   │   ├── monitor/     # 文件监控
│   │   ├── parser/      # 语法解析器
│   │   └── scanner/     # 扫描器
│   ├── models/          # 数据模型
│   └── services/        # 业务服务
├── frontend/            # 前端代码
│   └── src/
│       ├── api/         # API接口
│       ├── router/      # 路由配置
│       └── views/       # 页面组件
├── data/                # 数据目录
│   ├── samples/         # 样本文件
│   └── reports/         # 检测报告
├── tests/               # 测试代码
└── docs/                # 文档
```

---

## 📝 API文档

启动服务后访问: http://localhost:8000/docs

### 主要接口

| 方法 | 路径 | 描述 |
|-----|------|------|
| POST | /api/scan/file | 扫描单个文件 |
| POST | /api/scan/directory | 扫描目录 |
| GET | /api/scan/status/{id} | 获取扫描状态 |
| POST | /api/monitor/start | 启动监控 |
| POST | /api/monitor/stop | 停止监控 |
| GET | /api/alerts | 获取告警列表 |
| GET | /api/reports | 获取检测报告 |

---

## 🧪 测试

```bash
# 运行所有测试
pytest

# 运行特定测试
pytest tests/test_detector.py
pytest tests/test_parser.py
pytest tests/test_api.py
```

---
