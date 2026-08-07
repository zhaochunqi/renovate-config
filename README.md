# renovate-config

所有 repo 共享的 Renovate preset。配置的唯一事实来源是 `default.json`,改配置只改这一个文件,所有引用它的 repo 下次 Renovate 运行时自动生效。

## 使用方式(新 repo 接入)

1. 在该 repo 安装 [Renovate GitHub App](https://github.com/apps/renovate)
2. 在 repo 根目录添加 `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>zhaochunqi/renovate-config"]
}
```

3. 某个 repo 需要特殊行为时,在它自己的 `renovate.json` 里追加覆盖项(后写的覆盖 preset),不要在本 repo 里为单个 repo 加例外

## 当前 preset 行为

- 继承 `config:recommended` + 语义化 commit
- 时区 Asia/Shanghai,每天早 6 点前跑
- minor / patch 更新自动合并(`platformAutomerge`),major 仍需人工 review
- PR 统一打 `dependencies` 标签

## 注意

- `default.json` 由 Renovate 用 JSON5 解析器读取,允许注释和尾逗号;但请保守使用,保持内容尽量是严格 JSON
- 配置项参考:<https://docs.renovatebot.com/configuration-options/>
