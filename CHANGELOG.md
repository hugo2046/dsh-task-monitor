# 更新日志

本仓库是 [3450892121/dsh-task-monitor](https://github.com/3450892121/dsh-task-monitor) 的个人 fork。
这里只记 **本仓库相对上游的改动**；上游自己的更新不在此重复，需要时看 `git log upstream/main`。
格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号跟着包 `dsh-task-monitor/package.json` 的 `version` 走。

## [0.8.1] - 2026-10-20

### 修复

- **分支被其它工作区占用时不再「点了没反应」**：同一个分支不能同时检出在两个 worktree，git 会拒绝 `switch`。此前界面把 git 原文丢进面板底部一条 4 秒后消失的灰字里，下拉框随即弹回原分支；现在改为在分支行下方就地显示红色说明，指出是哪个工作区占着、接下来该怎么处理。
- 分支下拉（「环境信息」与「提交或推送」对话框两处）会标注「（已在其它工作区检出）」，悬停显示占用它的工作区路径。
- 宿主 `/taskmonitor/git/info` 新增 `occupied` 字段（分支 → 占用它的工作区）；`/taskmonitor/git/switch` 失败时返回结构化错误码 `branch-in-use` 与 `worktree` 路径，`fatal: … is already used by worktree at …` 已译为中文说明。
- 「技能与 MCP」不再漏掉 `run_code` 内经 PTC 派发的工具调用（除 `tool/call` 外同时解析 `tool/ptc-dispatch`）。

## 基线 · 上游 0.8.0（2026-10-10）

fork 的起点，不是本仓库发布的版本；功能集合按当时的 README 与 `spec/` 归纳。

- 窗口右侧独立「任务监控」浮层，挂在官方根级槽位 `shell.overlay`（不是侧边栏标签页），按会话显示五块只读信息：环境信息（变更统计 / 工作区 / 分支 / 未推送提交）、后台进程（官方 `ctx.jobs` 实时名单）、技能与 MCP、产出（可点击预览）、网页查阅
- 「提交或推送」对话框：分支选择、提交说明（留空自动生成）、包含未暂存变更、提交 / 提交并推送 / 推送
- GitHub 一键发布与重新发布（可改仓库名、可选私有 / 公开）、令牌两步登录、从系统凭据导入、自动识别本机代理端口
- 显隐与让位：会话已建立且右侧边栏收起时出现，侧边栏展开自动隐藏；面板出现时会话区整体左移
- 会话头部快捷开关，以及「设置 → 通用 → 任务监控面板」总开关
- 纯 JS 双面插件（宿主半边 + 界面半边），无构建步骤

## 维护约定

- 每次改动在「未发布」下加条目，按 **新增 / 变更 / 修复 / 移除** 分类；只写使用者能感知的变化，内部重构不必逐条记录。
- 修复类条目写清「原来会怎样、现在怎样」，让一年后的人看得懂。
- 发版时把「未发布」改成 `[x.y.z] - YYYY-MM-DD`，同步升 `dsh-task-monitor/package.json` 的 `version`，再打 git 标签。
- **吸收上游**：`git fetch upstream` → `git log --oneline main..upstream/main` 看上游新增 → `git merge upstream/main`（想保持线性就 `git rebase upstream/main`）。本地改动集中在 `client.js` / `index.js`，冲突大概率也在这两个文件；所以本地改动尽量拆成独立、意图单一的提交，冲突好解。吸收完上游只在「未发布」里记本地新增或调整，不逐条抄上游 changelog。
