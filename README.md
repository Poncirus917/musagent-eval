# MusAgent 评测与实验

课程项目 **MusAgent**（面向诗词创作的可解释检索增强生成系统）中**本人负责的评测与实验部分**的独立存档。

> 系统主体实现由团队成员完成，代码不在本仓库。本仓库只包含我产出的评测数据、实验结果图表与报告，
> 用于说明我在该项目中的实际工作。

## 项目背景

MusAgent 是一条 9 模块的 NLP 流水线 + RAG：分词 → 实体识别 → 主题扩展 → 关键词 → 摘要 →混合检索（BM25 + BGE + Cross-Encoder）→ 情感融合 → RAG 抽取 → 双通道生成，最后接一个 **Critic Agent** 对生成结果自评，低于阈值则触发重写。

## 个人负责部分

1. **评测集构建**：为分词、检索、情感分析、LLM 降级等模块设计用例，覆盖正常输入与边界输入。
2. **检索对比实验**：混合检索（BM25 + BGE + Cross-Encoder）与单路 BM25 的 top-5 结果重叠分布对比，带标准差误差棒。
3. **消融实验**：分别去掉查询扩展、去掉完整流水线，量化各模块的增量贡献。
4. **Critic Agent 有效性验证**：基于真实生成样本的打分分布，验证自评闭环是否真的提升质量，而非只是多调一次 LLM。
5. **报告撰写**：实验设计、结果分析与结论。

## 结果图表

| 文件 | 内容 |
|---|---|
| [`figures/fig7_ablation_chart.png`](figures/fig7_ablation_chart.png) | 检索通道消融实验（带误差棒） |
| [`figures/fig7_data.json`](figures/fig7_data.json) | 上图的原始实验数据 |
| [`figures/fig10_llm_vs_template.png`](figures/fig10_llm_vs_template.png) | LLM 生成 vs 模板生成的多维质量对比 |
| [`figures/fig11_critic_agent.png`](figures/fig11_critic_agent.png) | Critic Agent 评分分布与重写触发情况 |
| [`figures/fig3_hybrid_retrieval.png`](figures/fig3_hybrid_retrieval.png) | BM25 与混合检索的原理对比 |

## 目录结构

```
musagent-eval/
├── figures/          # 实验结果图与原始数据
├── scripts/
│   └── regenerate_fig7_10_11.py   # 生成图 7 / 10 / 11 的分析脚本
├── report/
│   └── 《自然语言语义处理概论》大作业报告.docx    # 最终版：实验设计、结果与结论
└── README.md
```

## 关于复现

`scripts/regenerate_fig7_10_11.py` 是实验中实际使用的分析脚本，依赖：

```
pip install matplotlib numpy jieba
```

脚本需要完整的诗歌语料（`data/poems_extracted.json`，约 2.6 MB）才能重跑，
该语料属于团队项目资源，未包含在本仓库中。**图中数据以 `figures/fig7_data.json` 为准**，
该文件是实验记录的直接导出，可独立用于核对结论。

## 说明

图表只呈现数据；实验设计、结论与局限性讨论见 `report/`。
