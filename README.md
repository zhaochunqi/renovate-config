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

本 repo 自己的 `renovate.json` 也 extends 了这个 preset,属于 dogfooding,是有意的。

## 什么时候生效

改 `default.json` **不是立刻生效**的:Renovate 下次运行(通常几小时内,或被 webhooks 触发)时才会拉取新版本。在此之前下游 repo 仍按旧 preset 跑。

## 改配置的规矩

| 风险 | 例子 |
| --- | --- |
| 低 | 注释、`labels`、加 `automerge` 的 update type |
| 高 | 增删 `automerge` 规则、动 `platformAutomerge`、`enabled: false`、删 `extends` |

高风险改动:先在一个 repo 上验证,再合到 main;commit message 里写清会影响哪些下游行为。

## 下游的逃生口

某个 repo 临时不想要自动合并,不用改这里,在自己的 `renovate.json` 里覆盖:

```json
{
  "extends": ["github>zhaochunqi/renovate-config"],
  "automerge": false
}
```

其他常用覆盖:`"minimumReleaseAge": null`(不等冷静期)、`"dependencyDashboard": true`、`"platformAutomerge": false`。

## 当前 preset 的几个已知副作用

- `minimumReleaseAge: 3 days`:新版本 PR 会立刻创建,只是等满 3 天才自动合并。`vulnerabilityAlerts` 里关掉了这个限制,安全修复不等。
- `config:recommended` 带了 `mergeConfidence: age-confidence-badges`:置信度低的非 major 更新**不会**被自动合并,会留成开放 PR 等人看。这是预期行为。
- `dependencyDashboard: false` 覆盖了 `config:recommended` 的 `:dependencyDashboard`。副作用:下游若用 `prCreation: "approval"` 或 `dependencyDashboardApproval`,PR 永远不会创建,得在自己配置里改回去。
- `lockFileMaintenance` 也在 automerge 白名单里,锁文件刷新 PR 会自动合。

## 注意

- `default.json` 由 Renovate 用 JSON5 解析器读取,注释和尾逗号合法,编辑器按严格 JSON 报的错可忽略
- 配置项参考:<https://docs.renovatebot.com/configuration-options/>
- CI 用 `renovate-config-validator --strict` 校验,并每周一跑一次,用来发现上游 schema / validator 的 breaking 变更;validator 版本在 workflow 里钉死
