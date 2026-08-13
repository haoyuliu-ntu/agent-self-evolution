# Mall Policy Skill Optimization

这是基于当前“客服自进化 Agent”项目完成的独立实验目录。

## 快速查看

1. 最终技能：`mall-policy-advisor/SKILL.md`
2. 初始技能：`mall-policy-advisor-workspace/skill-snapshot-v1/SKILL.md`
3. 对比结果：`mall-policy-advisor-workspace/benchmark.md`
4. 可视化评审：`mall-policy-advisor-workspace/review.html`

## 复现实验

在本目录运行：

```powershell
python .\scripts\benchmark.py
```

脚本会重新评分两个版本的6份答案，并更新：

- `mall-policy-advisor-workspace/skill_metrics.json`
- `mall-policy-advisor-workspace/benchmark.json`
- `mall-policy-advisor-workspace/benchmark.md`
- 每个测试目录中的 `grading.json` 和 `metrics.json`

该复现流程不需要API key。token采用 `ceil(UTF-8 bytes / 3)` 的中文相对代理值，不能当作API账单token。
