#  LightLog - Enhanced TCN 日志异常检测工具

🚀 **基于深度学习的轻量级日志异常检测** | **MCP 服务即插即用**

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.8%2B-orange)](https://tensorflow.org/)
[![FastMCP](https://img.shields.io/badge/FastMCP-2.0%2B-green)](https://github.com/jlowin/fastmcp)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

* 原项目地址：https://github.com/Aquariuaa/LightLog

* MCP封装项目地址：https://github.com/Plc912/Lightlog-Master.git
* MCP封装项目作者邮箱：3522236586@qq.com

基于增强时序卷积网络（Enhanced TCN）的深度学习日志异常检测工具，支持 BGL 和 HDFS 数据集。

---

## ✨ 特性亮点

- ✅ **深度学习模型**：Enhanced TCN 时序卷积网络
- ✅ **语义分析**：Word2Vec 语义向量 + PCA-PPA 降维
- ✅ **MCP 服务**：支持 Cursor/Claude 无缝集成
- ✅ **异步任务**：后台运行，支持长时间训练
- ✅ **多数据集**：支持 BGL 和 HDFS 日志
- ✅ **实时推理**：快速异常检测能力

---

## 📦 快速开始

1. 安装依赖

**⚠️ 重要提示**：

- **本 MCP 封装需要 Python 3.11+**
- **原始 LightLog 项目使用 Python 3.6 + TensorFlow 1.8.0**

bash
 进入项目目录
cd lightlog-master

 安装依赖（需要 Python 3.11+）
pip install -r requirements.txt

**主要依赖**：

- fastmcp>=2.0.0 - MCP 服务框架（需要 Python 3.11+）
- tensorflow>=2.8.0 - 深度学习框架（现代版本）
- keras>=2.8.0 - 高级神经网络 API
- scikit-learn>=1.0.0 - 机器学习工具
- pandas>=1.3.0 - 数据处理
- numpy>=1.21.0 - 数值计算

2. **启动 MCP 服务**

bash
 方式一：直接启动
python lightlog_mcp_server.py

 方式二：使用脚本
./start_lightlog_mcp.sh     Linux/Mac
start_lightlog_mcp.bat      Windows

服务默认在 **http://127.0.0.1:2225** 启动

3. **配置客户端**

**Cherry Studio配置**：
{
  "mcpServers": {
    "LHt_X8mqeXrQmdQ6hKp43": {
      "isActive": true,
      "name": "lightlog-master",
      "type": "sse",
      "description": "基于增强时序卷积网络（Enhanced TCN）的深度学习日志异常检测工具，支持 BGL 和 HDFS 数据集。",
      "baseUrl": "http://127.0.0.1:2225/sse",
      "installSource": "unknown"
    }
  }
}

4. **开始使用**

配置好 MCP 服务后，直接在 Cursor 中用自然语言调用：

"帮我用 Enhanced TCN 训练 BGL 数据集"
"训练 HDFS 日志异常检测模型"
"测试刚才训练的模型性能"
"显示所有任务状态"

 **使用 Python API**

python
 训练 BGL 模型
result = lightlog_train_model(dataset="bgl", epochs=100)
task_id = result["task_id"]

 查询训练状态
status = lightlog_get_task(task_id)
print(f"进度: {status['progress']:.1%}")

 获取结果
if status["status"] == "succeeded":
    metrics = status["result"]["metrics"]
    print(f"F1-Score: {metrics['f1_score']}")
    print(f"Precision: {metrics['precision']}")
    print(f"Recall: {metrics['recall']}")

---

## 🛠️ **可用工具**

| 工具                    | 功能                   | 数据集   |
| ----------------------- | ---------------------- | -------- |
| lightlog_train_model    | 训练 Enhanced TCN 模型 | BGL/HDFS |
| lightlog_test_model     | 测试模型性能           | BGL/HDFS |
| lightlog_get_model_info | 获取模型架构信息       | -        |
| lightlog_list_tasks     | 列出所有任务           | -        |
| lightlog_get_task       | 查询任务详情           | -        |

---

## 📊 模型架构

 Enhanced TCN 网络结构

输入: (300, 20) - 300个时间步，20维特征
├── ResBlock (filters=3, kernel=3, dilation=1)
├── ResBlock (filters=3, kernel=3, dilation=2)
├── ResBlock (filters=3, kernel=3, dilation=4)
├── ResBlock (filters=3, kernel=3, dilation=8)
├── GlobalAveragePooling1D
└── Dense (2, softmax) - 二分类输出

 数据处理流程

1. **语义向量生成**：Word2Vec 从日志模板生成 300 维语义向量
2. **降维处理**：PCA-PPA 降维到 20 维特征
3. **序列构建**：构建 300 长度的时间序列
4. **模型训练**：Enhanced TCN 进行异常检测

---

## 📚 使用指南

 数据集支持

 BGL 数据集

- **来源**：Blue Gene/L 超级计算机日志
- **特点**：时间序列日志，滑动窗口分析
- **预处理**：已完成语义向量和序列构建

 HDFS 数据集

- **来源**：Hadoop 分布式文件系统日志
- **特点**：块级日志序列分析
- **预处理**：已完成语义向量和序列构建

## 使用场景

 场景 1：快速训练

"帮我用 Enhanced TCN 训练 BGL 数据集"
"训练 HDFS 日志异常检测模型"
"用默认参数训练 BGL 模型"

**Python API**：
python
 使用默认参数训练
result = lightlog_train_model(dataset="bgl")

 场景 2：自定义训练

"训练 BGL 模型，训练轮数设为 50，批次大小 32"
"用 HDFS 数据训练，训练集比例 0.8"
"训练模型，epochs=200，batch_size=128"

**Python API**：
python
 自定义参数训练
result = lightlog_train_model(
    dataset="hdfs",
    epochs=50,
    batch_size=32,
    train_ratio=0.8
)

 场景 3：模型测试

"测试刚才训练的模型性能"
"评估模型在测试集上的表现"
"获取模型的 F1 分数和准确率"

**Python API**：
python
 测试模型
result = lightlog_test_model(
    model_path="./lightlog_model_bgl_abc123.h5",
    dataset="bgl"
)

 场景 4：任务管理

"查看所有训练任务"
"显示任务列表"
"检查训练任务完成了没有"
"获取任务 abc-123-def 的详细结果"

**Python API**：
python
 列出所有任务
tasks = lightlog_list_tasks()

 查询特定任务
status = lightlog_get_task(task_id)

---

## 💬 自然语言参考

 **基础训练**

"帮我用 Enhanced TCN 训练 BGL 数据集"
"训练 HDFS 日志异常检测模型"
"用深度学习模型进行日志异常检测"
"训练时序卷积网络模型"

 **自定义训练**

"训练 BGL 模型，训练轮数设为 50"
"用 HDFS 数据训练，批次大小 32"
"训练模型，epochs=200，train_ratio=0.8"
"自定义参数训练 Enhanced TCN"

 **模型测试**

"测试刚才训练的模型性能"
"评估模型在测试集上的表现"
"获取模型的 F1 分数和准确率"
"测试模型推理速度"

 **任务管理**

"查看所有训练任务"
"显示任务列表"
"检查训练任务完成了没有"
"获取任务详细结果"

 **模型信息**

"显示 Enhanced TCN 模型架构"
"获取模型详细信息"
"查看支持的数据集"
"了解模型功能特性"

---

## 🗂️ 项目结构

lightlog-master/
├── lightlog_mcp_server.py           MCP 服务主程序 
├── start_lightlog_mcp.sh/bat         启动脚本
├── lightlog_config_example.json      配置示例
├── requirements.txt                  依赖清单
├── README.md                         说明文档
│
├── Enhanced TCN for Log Anomaly Detection on the BGL Dataset/
│   ├── data/                         BGL 数据
│   │   ├── bgl_semantic_vec.json     语义向量
│   │   ├── bgl_data.csv              训练数据
│   │   └── bgl_label.csv             标签数据
│   ├── model/                        模型文件
│   ├── result/                       结果文件
│   ├── train.py                      原始训练脚本
│   └── test_BGL.py                   原始测试脚本
│
├── Enhanced TCN for Log Anomaly Detection on the HDFS Dataset/
│   ├── data/                         HDFS 数据
│   │   ├── hdfs_semantic_vec.json    语义向量
│   │   ├── log_train.csv             训练数据
│   │   └── log_test_2000.csv         测试数据
│   ├── model/                        模型文件
│   ├── result/                       结果文件
│   ├── train.py                      原始训练脚本
│   └── test.py                       原始测试脚本
│
└── BGL&HDFS dataset and Methods of data processing/
    ├── BGL/                          BGL 数据处理
    └── HDFS/                         HDFS 数据处理

---

## 🔧 高级配置

 环境变量

bash
 设置最大并发任务数
export LIGHTLOG_MAX_CONCURRENT=4

 Windows PowerShell
$env:LIGHTLOG_MAX_CONCURRENT=4

 远程部署

bash

1. 服务器启动（开放 2225 端口）
   python lightlog_mcp_server.py
2. 客户端配置
   {
   "url": "http://your-server-ip:2225/sse"
   }

---


## 📖 技术原理

 Enhanced TCN 架构

Enhanced TCN（增强时序卷积网络）是一种专门用于时间序列分析的深度学习架构：

1. **残差块**：每个 ResBlock 包含两个卷积层，使用不同的膨胀率
2. **膨胀卷积**：通过膨胀率 [1,2,4,8] 捕获不同时间尺度的模式
3. **全局池化**：GlobalAveragePooling1D 将序列特征聚合
4. **分类输出**：Dense 层输出二分类结果（正常/异常）

 PCA-PPA 降维

1. **PCA 降维**：将 300 维语义向量降维到 20 维
2. **PPA 去均值**：去除前 7 个主成分，减少噪声影响
3. **特征保持**：保留最重要的时序模式特征

 语义向量生成

1. **Word2Vec 训练**：从日志模板学习词汇语义
2. **向量聚合**：将模板中词汇向量求和得到模板向量
3. **语义表示**：捕获日志事件的语义相似性

---

## 📄 引用

基于 LightLog 项目实现：

bibtex
@article{lightlog2023,
  title={Enhanced TCN for Log Anomaly Detection},
  author={LightLog Team},
  journal={Journal of Log Analysis},
  year={2023}
}

**原项目**: https://github.com/Aquariuaa/LightLog

---

## 🤝 贡献与反馈

- **原项目**: https://github.com/Aquariuaa/LightLog
- **Issues**: 在原项目提交 Issue
- **MCP 封装**: 基于 FastMCP 框架

如有问题：

- 🐛 提交 [Issue](https://github.com/Aquariuaa/LightLog/issues)
- 📧 查看原项目文档
