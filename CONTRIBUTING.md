# Contributing

Before opening a pull request, read [`SECURITY.md`](./SECURITY.md) and the current evidence in [`docs/COMPATIBILITY.md`](./docs/COMPATIBILITY.md). Never attach cookies, tokens, account data, conversation text, provider HTML, or local profile files.

## Adapter PR SOP

Adapters live in `adapters/*.json` and must validate against `adapters/schema.json`.

For selector updates:
1. Change only the affected provider adapter unless a schema change is intentional.
2. Preserve ordered selector semantics; first match wins.
3. Keep `schemaVersion` unchanged unless parser compatibility changes.
4. Increment `adapterVersion` for content updates.
5. Keep `urls.app`, `urls.login`, `urls.match`, and `urls.ssoMatch` inside the URL scopes bundled with the installed app. New or broader scopes require an app release and security review; remote hot updates are rejected if they expand them.
6. Smoke-test login, prompt insertion, automatic send, completion detection, response capture, and new-session behavior. Check image-only completion when supported.
7. Run `pnpm verify` and `cargo test --manifest-path src-tauri/Cargo.toml adapters::tests`.

New providers are not adapter-only in v1. The catalog contains `chatgpt`, `claude`, `gemini`, `grok`, and `meta`; exactly four are active, with Meta AI as the default standby. Adding another provider or changing the four-active invariant requires coordinated adapter, host, UI, test, and security-review changes.

Use the repository's adapter issue form and pull-request checklist so reports remain reproducible and privacy-safe. Vulnerabilities must be reported privately according to [`SECURITY.md`](./SECURITY.md), not through a public adapter issue.

## Agent Source Contract SOP

The Agent-ready source lane is a maintained product surface. Read [`agent-release.json`](./agent-release.json) and [`docs/AGENT-READY-SOURCE-RELEASE.md`](./docs/AGENT-READY-SOURCE-RELEASE.md) before changing `.agents/`, `.claude/`, or `scripts/agent/`.

1. Keep `agent-release.json`, its schema, package scripts, lifecycle scripts, and both Skill bodies aligned. Bump the contract/Skill version together for an incompatible interface change.
2. Preserve explicit-only invocation. Opening the repo must never launch code or install anything.
3. Keep project-local actions separate from host changes. Lifecycle scripts must not install/uninstall host tools or global packages, elevate privileges, modify `PATH`/profiles/security settings, or perform automatic rollback.
4. Preserve one-object JSON stdout, documented exit codes, current-run READY evidence, and identity-verified stop behavior. Process creation alone is not readiness.
5. Update side-effect and privacy declarations whenever a script writes a new path, uses a new shared cache, or accesses new application data.
6. Keep Codex and Claude Skill instruction bodies synchronized; only tool-specific frontmatter/metadata may differ.
7. Run `pnpm agent:verify`, `node scripts/agent/launch.mjs --dry-run --json`, and `pnpm verify`. CI reruns the contract tests on Windows, macOS, and Linux.
8. Do not add Docker, an embedded agent runtime, daemon, installer generation, credential access, log upload, or host package-management behavior to this lane.

<!-- freedom-repository-guide:start -->
## 自由工坊：從一個成果到一個 PR

AI 開發公會可用的多模型桌面協作工具來源。 保留 Tauri 桌面端、provider adapters、注入腳本與 Agent source lane。

先看[本倉 Issues](https://github.com/FreeTWAI/multi-ai-chat-desktop/issues)與[現有 PR](https://github.com/FreeTWAI/multi-ai-chat-desktop/pulls)。提出問題、這一輪範圍、完成條件與可投入時間，在 Issue 認領並協調重疊工作；維護者已直接派工時不必重複等待，將約定連回交接即可。使用自己的 fork／分支，PR 送到 **FreeTWAI/multi-ai-chat-desktop:main**。

交給 Agent 前先讓它讀 [AGENTS.md](AGENTS.md)。PR 寫明變更用途、使用者可見結果、驗證命令、限制與原 Issue；附上可公開的合成案例或重現方式。Issue／PR 是程式協作的記錄，平台名片與公會身分不取代 repo 維護者的審查。

現有 provider web sessions 留在桌面工具，不可轉成 Freedom 會員身分或公開憑證。adapter scope 與 explicit-only Agent lifecycle 按上游規則，平台只連公開專案與成果。

### 這個模組怎麼驗證

選擇與修改範圍相符的既有入口：

```sh
pnpm typecheck
pnpm test
pnpm agent:verify
```

命令列在這裡不表示本輪已執行。先核對依賴與環境，再記錄實際結果；缺工具、桌面、媒體或授權時寫 `not_run` 與原因，不能補造成功。純文件修改以連結／路徑核對與 `git diff --check` 為主。

### 署名與上游

工坊 Fork：上游產品／授權來源為 [teddashh/multi-ai-chat-desktop](https://github.com/teddashh/multi-ai-chat-desktop)；本次協作的 Issue／PR 送到 **FreeTWAI/multi-ai-chat-desktop**，不是自動送往上游。 保留原作者與授權檔，另列真正完成文件、測試、設計、程式或協作的人。使用 AI 時如實交代協作範圍；只有實際 GitHub PR／review／合併紀錄可以作為對應貢獻證據，不能靠自填帳號推定。

自願貢獻不保證案源、XP、收益或雇用。若產生付費合作，由當事人另定條款與 Seller 外部收款；平台不代收。秘密、客戶資料、真實交易單據與未授權素材不進公開 Issue／PR。
<!-- freedom-repository-guide:end -->
