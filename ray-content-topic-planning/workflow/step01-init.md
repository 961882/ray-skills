# Step 01: 参数收集

> 执行者：主 Agent
> 输入：用户触发
> 输出：`state/config.json`

## 执行说明

收集以下参数，缺少必填项时询问用户。

## 参数表

| 参数 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `source_path` | ✅ | 源文案/白皮书路径 | `/path/to/whitepaper.md` |
| `identity_path` | ✅ | 身份和目标文件路径 | `/path/to/身份和目标.md` |
| `history_dir` | ❌ | 历史文案目录路径 | `/path/to/history/` |
| `output_dir` | ❌ | 输出目录（默认源文案同目录） | `/path/to/output/` |
| `candidate_count` | ❌ | 候选选题数量（默认 20） | `20` |
| `select_count` | ❌ | 精选数量（默认 5） | `5` |

## 收集逻辑

1. 检查用户消息中是否已通过 `@[path]` 提供了源文案和身份目标
2. 如有 `@[history_dir]`，标记为"有历史文案"模式
3. 未提供 `output_dir` 时，使用源文案所在目录
4. 未提供 `candidate_count` / `select_count` 时使用默认值

## 输出文件

将参数写入 `state/config.json`：

```json
{
  "source_path": "/absolute/path/to/source.md",
  "identity_path": "/absolute/path/to/identity.md",
  "history_dir": "/absolute/path/to/history/",
  "output_dir": "/absolute/path/to/output/",
  "candidate_count": 20,
  "select_count": 5,
  "has_history": true,
  "date_prefix": "260404"
}
```

## 验证检查点

- [ ] `source_path` 文件存在且可读
- [ ] `identity_path` 文件存在且可读
- [ ] `history_dir`（如提供）目录存在且包含 .md 文件
- [ ] `output_dir` 存在或可创建

## 下一步

→ Step 02: 批量出题
