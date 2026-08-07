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

## 注意

- `default.json` 由 Renovate 用 JSON5 解析器读取,注释和尾逗号合法,编辑器按严格 JSON 报的错可忽略
- 配置项参考:<https://docs.renovatebot.com/configuration-options/>
