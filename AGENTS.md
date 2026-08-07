# AGENTS.md

## 这个 repo 是什么

`zhaochunqi/renovate-config` 是所有 repo 共享的 Renovate preset。其他 repo 通过各自根目录的 `renovate.json` 引用它:

```json
{ "extends": ["github>zhaochunqi/renovate-config"] }
```

影响面:改 `default.json` 会影响**所有**引用它的 repo 的依赖更新行为(几十个 PR 的创建/合并策略)。改动前先想清楚波及范围。

## 文件

- `default.json` — 唯一的 preset 文件,Renovate 解析 `github>zhaochunqi/renovate-config` 时默认找它
- `README.md` — 面向人的使用说明

## 修改规则

1. 只改 `default.json`,不要新建别的 preset 文件,除非用户明确要求多套 preset
2. 文件用 JSON5 解析,允许注释/尾逗号,但保持严格 JSON 风格,key 和字符串都用双引号
3. 修改后验证语法:JSON5 语义上合法即可;如果移除了所有注释和尾逗号,可以用 `jq . default.json` 严格校验
4. 可查配置项:<https://docs.renovatebot.com/configuration-options/>;preset 语法:<https://docs.renovatebot.com/config-presets/>
5. 单个 repo 的特殊需求不要加进这里 — 让那个 repo 在自己的 `renovate.json` 里覆盖
6. 高风险改动(如调整 `automerge`、schedule、prConcurrentLimit)在 commit message 里写清楚原因和影响面

## 验证

没有 CI。改完后确认:

- 语法能被 JSON5 解析
- `extends` 引用的 preset 名称拼写正确(参考官方文档)
