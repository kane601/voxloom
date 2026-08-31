# VoxLoom 外部链接/URL 审计与待办清单

> 本工程 fork 自上游 `jamiepine/voxloom`。已执行两轮统一替换：
> 1. 所有仓库链接 `jamiepine/voxloom` → `kane601/voxloom`
> 2. 所有官网域名 `voxloom.sh` → `aispanvok.com`
>
> 下方 P0~P4 为**中后期需要逐项修改/调整**的链接与占位项。等项目与外部网站全部打通后，按本清单统一更新一次。

---

## 已完成（当前状态快照，无需再动）

| 位置 | 内容 | 现状 |
|---|---|---|
| `tauri/src-tauri/tauri.conf.json` | updater `endpoints` | `https://github.com/kane601/voxloom/releases/latest/download/latest.json` |
| 全工程 20+ 文件 | `jamiepine/voxloom` | 已全部替换为 `kane601/voxloom`（含 CHANGELOG 历史记录） |
| 全工程 21 文件 | `voxloom.sh` 域名 | 已全部替换为 `aispanvok.com`（含 `docs.`、`api.` 子域） |
| `landing/src/lib/constants.ts` | `DONATE_URL` / `SPONSOR_CHECKOUT_URL` | 已指向 `https://aispanvok.com/` |
| `app/.../AboutPage.tsx` | 作者署名 / 捐赠按钮 | 署名 `kane601`，按钮指向 `aispanvok.com` |
| `landing/src/components/Footer.tsx` | "Also By"（Spacebot/Spacedrive） | 已替换为单个 `aispanvok.com` 链接 |
| `landing/src/components/Banner.tsx` | spacebot.sh 推广横幅 | 已改为 `aispanvok.com`（该组件当前未被引用） |
| `backend/config.py` | 云服务默认地址 | `https://aispanvok.com` / `https://api.aispanvok.com` |

---

## P0 — 签名与发布

### 1. ✅ 已完成 —— 全新签名密钥对已生成（2026-08-28）
- 公钥**已写入** `tauri/src-tauri/tauri.conf.json` → `plugins.updater.pubkey`：
  ```
  dW50cnVzdGVkIGNvbW1lbnQ6IG1pbmlzaWduIHB1YmxpYyBrZXk6IDEzRDFDMDhFMUMyRjU4MUYKUldRZldDOGNqc0RSRTZENWkzanFaenNSOWRPV2tuQTJGaHZVWHEzZlh6a3VpZVM5QXpBa21mWDUK
  ```
- 私钥位置：`%USERPROFILE%\.tauri\voxloom.key`（**密码：`voxloom-signing-key`**）
- 上游原始私钥已备份：`%USERPROFILE%\.tauri\voxloom.key.upstream-bak`（+ `.pub.upstream-bak`）—— 已用新 key 替换后，**旧 key 不再用于发布**，可留作历史或删除
- 重新生成命令（如需换新）：`cd tauri && bun tauri signer generate -w "$HOME\.tauri\voxloom.key" -f --ci`（`-f` 覆盖，先自行备份）
- ⚠️ 私钥/备份文件**切勿提交到仓库**（已确认 `.gitignore` 之外要手工注意）

### 2. 待办 —— 在 GitHub 仓库配置 Secrets（Settings → Secrets and variables → Actions → New repository secret）
| Secret 名 | 必填 | 值 |
|---|---|---|
| `TAURI_SIGNING_PRIVATE_KEY` | **必须** | 运行 `Get-Content "$HOME\.tauri\voxloom.key" -Raw`，复制**整行 base64 文本**（不要 base64 解码成两行再粘贴） |
| `TAURI_SIGNING_PRIVATE_KEY_PASSWORD` | **必须** | `voxloom-signing-key`（当前私钥有密码；留空会导致 CI 在签名步骤挂起/失败） |
| `APPLE_CERTIFICATE` / `APPLE_CERTIFICATE_PASSWORD` / `APPLE_SIGNING_IDENTITY` | 可选 | Apple Developer 公证证书（暂无则留空） |
| `WINDOWS_CERTIFICATE` / `WINDOWS_CERTIFICATE_PASSWORD` | 可选 | Windows 代码签名证书（暂无则留空） |

> `release.yml` 已包含"无证书自动降级为不签名"的逻辑，暂无证书也可正常出包，但安装包会有 SmartScreen 提示。

---

## P1 — 域名与站点部署（打通官网）

| # | 事项 | 位置/现状 | 目标 |
|---|---|---|---|
| 3 | 官网 `landing/` 部署 | 原 voxloom.sh 就是本目录，链接已改 | 部署到 `https://aispanvok.com`，所有路由直接可用 |
| 4 | 文档站 `docs/` 部署 | 原 docs.voxloom.sh | 部署到 `docs.aispanvok.com`（需配 DNS 子域） |
| 5 | 云后端部署 | `backend/config.py` 已默认 `api.aispanvok.com` | 在 `api.aispanvok.com` 部署后端，否则 App「云账号」功能连接失败；也可用环境变量 `VOXLOOM_CLOUD_URL` / `VOXLOOM_CLOUD_API_URL` 覆盖 |
| 6 | DNS 子域 | — | `api.` / `docs.` 子域解析到对应服务 |

---

## P2 — 社交/社区账号（换成你自己的）

| # | 文件 | 现状 | 需改为 |
|---|---|---|---|
| 7 | `landing/src/lib/pricing.ts`（`CLOUD_NOTIFY_URL`） | `https://x.com/VoxLoomAI` | 你自己的 X/Twitter 账号 |
| 8 | `landing/src/posts/why-voxloom-has-a-token.md` | 文中 `[@VoxLoomAI](https://x.com/VoxLoomAI)` | 同上 |
| 9 | `app/src/components/ServerTab/GeneralPage.tsx` | `https://discord.gg/StkzQasqPS`（上游 Discord） | 你自己的 Discord 邀请链接 |
| 10 | `docs/content/docs/overview/troubleshooting.mdx` | `https://discord.gg/voxloom`（标注 coming soon） | 同上 |
| 11 | `landing/src/components/TutorialsSection.tsx` | 教程视频 `youtube.com/watch?v=${tutorial.id}`，id 是上游的 | 换成你自己的教程视频 id（数据源见 `landing/src/lib/tutorials.ts`） |

---

## P3 — 代币功能（业务决策项，非纯改链接）

上游为 $VOXLOOM 发行了 Solana 代币并部署了验证页面。**去留需要你决策**：保留上游数据 / 换成你自己的 / 删除整个代币功能。

涉及位置：

| 文件 | 内容 |
|---|---|
| `landing/src/lib/constants.ts` | `TOKEN_CONTRACT_ADDRESS`、`TOKEN_PUMP_URL`、`TOKEN_SOLSCAN_URL`、3 个 Streamflow 锁仓合同链接、Solscan 交易链接、`TODO(kane601)` 待补的两条 `txUrl`（launch lock / second burn）、"Jamie's dev/treasury wallet" 注释 |
| `landing/src/app/token/page.tsx` | 代币透明度页整体叙事（第一人称、提到上游其他项目） |
| `landing/src/posts/why-voxloom-has-a-token.md` | 整篇博客为上游作者第一人称叙事 |

---

## P4 — 占位/杂项

| # | 文件 | 现状 | 建议 |
|---|---|---|---|
| 12 | `landing/src/lib/constants.ts` `SPONSOR_CONTACT_EMAIL` | `contact@aispanvok.com`（占位） | 换成真实可收件邮箱 |
| 13 | `landing/src/lib/constants.ts` `DONATE_URL` / `SPONSOR_CHECKOUT_URL` | 均指向 aispanvok.com 首页 | 如后续接打赏/支付渠道，替换为真实链接 |
| 14 | `README.md` Trendshift 星标徽章 | 图片绑定上游 repo ID 21213 | fork 无对应数据，可移除或保留展示上游趋势 |
| 15 | `tauri/src-tauri/Cargo.toml` | 注释 `jamiepine/keytap` | **保留**——外部依赖 `keytap` 的真实仓库名，勿改 |

---

## 保留项（第三方外部依赖，无需修改）

- **HuggingFace 模型源**：`backend/backends/__init__.py` 中的 `Qwen/...`、`Kokoro-82M`、`openai/whisper-*` 等均为第三方公开仓库；`backend/backends/hume_backend.py` 的 `unsloth/Llama-3.2-1B` 同理。若日后换成自有模型，统一改这里即可。
- **PyTorch 下载源**：`.github/workflows/release.yml` 中 `download.pytorch.org`。
- **fumadocs 文档**：`docs/source.config.ts` 中的官方文档链接。

---

## 统一更新前的自检命令（PowerShell，工程根目录执行）

```powershell
# 1. 不应再出现上游项目引用（预期仅剩 Cargo.toml 的 keytap 注释与 CHANGELOG 保留项）
rg -n "jamiepine" --glob '!CHANGELOG.md' .
rg -n "voxloom\.sh|spacebot|spacedrive\.com|buymeacoffee" .

# 2. 确认已全部指向你的仓库
rg -n "kane601" .
rg -n "aispanvok\.com" .
```
