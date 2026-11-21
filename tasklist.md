# 本文档为4象限的任务清单，全文书写大白话

## 问题1及解决模板：tmate 默认中继已停运

- [1795242056] 遇到的问题：默认中继已停运，Action 会卡在等会话就绪。通用解决步骤：读上游的停运公告，找有官方说明的可用中继，将现场查到的主机指纹和官方配置核对，再用远端 Actions 验证。

## 问题2及解决模板：Action 没有 latest 引用

- [1795242214] 遇到的问题：GitHub 上标记为最新的 Release 不代表仓库有 `latest` 标签。通用解决步骤：查询上游标签和最新 Release。要跟随更新时，使用上游维护的浮动标签并确认它指向的提交。

## 问题3及解决模板：接受 SSH 主机指纹后断开或登录失败

- [1795244082] 遇到的问题：旧工作流打印了会话地址，作业随后取消，客户端还出现了公钥认证失败。通用解决步骤：看当前运行状态和错误发生的 UTC 时间，重新复制仍在运行的会话地址，用 `ssh-keygen` 核对授权的公钥指纹，再用 `ssh -v -o IdentitiesOnly=yes -i` 确认客户端提交了对应钥匙。

## 问题4及解决模板：本机默认网络连接 GitHub 或 SSH 中继失败

- [1795244236] 遇到的问题：Mac 默认网络上的 GitHub HTTPS 连接报 `EOF`，SSH 也没拿到中继主机钥匙。通用解决步骤：查地址和本机路由，确认是哪个网络接口在走流量。在单次命令中绑定可用接口，通过 TLS 校验访问 GitHub，通过官方指纹校验 SSH 中继。

## 想做：

- [1795244082] 为 Issue #3 提交 PR，跑通远端检查和本机 SSH 登录后合并，核对 Issue 是否关闭。

## 做完：

- [1795241983] 当前目录里没有目标仓库。从 GitHub 同步 `C0verSnow/tmate`，切到 `feature/arch-linux-tmate`。
- [1795242056] 在 GitHub 比较了 `mxschmitt/action-tmate`、上游的 `tmate-ssh-server` 和 WarpBuild 的实现。保留指定 Action，用可达的 WarpBuild 中继，并核对了主机指纹。
- [1795242214] 按最新版本要求改用 `mxschmitt/action-tmate@v3`。GitHub 的最新正式版是 `v3.24`，两个标签指向同一提交。
- [1795242275] 新增 Arch Linux SSH 工作流和 README 使用说明，YAML 和 Bash 语法检查已通过。[PR #2](https://github.com/C0verSnow/tmate/pull/2) 已创建。
- [1795244082] PR #2 已合并。从 `main` 建立独立的 `fix/issue-3-ssh` 工作区，保留旧分支的本地记录。
- [1795244082] 在 GitHub 核对了 [action-tmate 的实现](https://github.com/mxschmitt/action-tmate/blob/v3/src/index.js) 和 [WarpBuild 的配置](https://github.com/WarpBuilds/action-debugger/blob/v2/action.yml)。沿用触发账号公钥限制和中继指纹，使用分离模式来统一 PR 与手动运行的验证步骤。
- [1795244236] 已更新工作流和 README，通过 YAML、Bash 语法和差异检查。Mac 已用 SSH 核对中继主机指纹。

## 没做：

- [1795242275] 按仓库约定不做本地编译，运行结果以 GitHub Actions 为准。

## 在做：

- [1795244236] 准备提交 PR 和跑远端检查。Mac 现有的 SSH 钥匙与触发账号的公钥指纹都不匹配，正等待匹配的钥匙。
