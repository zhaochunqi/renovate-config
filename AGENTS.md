# AGENTS.md

本 repo 是所有 repo 共享的 Renovate preset,唯一文件是 `default.json`。各 repo 用 `"extends": ["github>zhaochunqi/renovate-config"]` 引用,改这里会影响所有引用它的 repo。

规则:

- 只改 `default.json`;单 repo 的例外放到那个 repo 自己的 `renovate.json`
- 文件按 JSON5 解析,注释/尾逗号合法;key 和字符串保持双引号
- 配置项:<https://docs.renovatebot.com/configuration-options/>
