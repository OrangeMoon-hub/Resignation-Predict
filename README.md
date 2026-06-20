# 🏭 物流蓝领离职预测

> 基于随机森林的蓝领员工离职风险预测模型 — 近 1 年数据模拟验证

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-orange)](https://scikit-learn.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](./LICENSE)

---

## 📊 模型性能

| 指标 | 数值 |
|------|------|
| **召回率 (Recall)** | **0.74** |
| **精确率 (Precision)** | 0.41 |
| **F1-Score** | 0.51 |

> 阈值策略偏向召回：宁可多预警，不漏掉高风险员工。

---

## 🧠 技术方案

```
┌──────────┐    ┌──────────────┐    ┌─────────────────┐    ┌──────────┐
│ 数据库    │───▶│ 特征工程      │───▶│ Balanced RF      │───▶│ 离职概率  │
│ 多表关联  │    │ 人事/经济/    │    │ + SMOTEENN      │    │ Top 7    │
│           │    │ 强度/稳定    │    │ + 阈值调优       │    │ 影响因子  │
└──────────┘    └──────────────┘    └─────────────────┘    └──────────┘
```

### 特征体系（四大维度）

- **人事特征** — 工龄、入职月份、岗位、仓库类型
- **经济特征** — 薪资水平、时薪、收入排名、满勤率
- **强度特征** — 考勤工时、标准工时、效率评分
- **稳定特征** — 历史绩效趋势、潜力新员工标识、福利改善标志

### 模型策略

- **算法**：Balanced Random Forest + 手动阈值调优
- **样本平衡**：SMOTEENN（过采样 + 欠采样混合）
- **特征筛选**：累积重要性 > 99% 的特征保留
- **去冗余**：高相关性特征对（r > 0.85）逐对裁剪
- **阈值策略**：降低分类阈值提升召回，优先捕获高风险人群
- **可解释性**：SHAP 分析 + Top 7 影响因子逐样本输出

---

## 📁 项目结构

```
.
├── Resign_predict.ipynb   # 完整建模流程（Jupyter Notebook）
├── requirements.txt       # Python 依赖
├── README.md
└── LICENSE
```

## 🚀 快速开始

```bash
# 1. 安装依赖
pip install -r requirements.txt

# 2. 设置数据库连接（环境变量）
export DB_URL='mysql+pymysql://user:password@host:port/db'

# 3. 启动 Jupyter
jupyter notebook Resign_predict.ipynb
```

---

## ⚠️ 注意事项

- 本 Notebook 为 **Jupyter 交互式实验环境**，包含多次迭代的模型训练 cell，存在冗余设计，非生产级脚本
- 数据库连接通过环境变量 `DB_URL` 注入，代码中不含任何真实凭证
- 原始数据库包含多表关联查询（人效明细、薪资明细、仓库指标、离职记录），表结构因涉及内部数据未开源

---

## 📄 License

MIT © OrangeMoon
