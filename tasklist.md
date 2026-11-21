# 本文档为4象限的任务清单，全文书写大白话

## 问题1及解决模板：tmate 默认中继已停运

- [1795242056] 遇到的问题：默认中继已停运，Action 会卡在等会话就绪。通用解决步骤：读上游的停运公告，找有官方说明的可用中继，将现场查到的主机指纹和官方配置核对，再用远端 Actions 验证。

## 问题2及解决模板：Action 没有 latest 引用

- [1795242214] 遇到的问题：GitHub 上标记为最新的 Release 不代表仓库有 `latest` 标签。通用解决步骤：查询上游标签和最新 Release。要跟随更新时，使用上游维护的浮动标签并确认它指向的提交。

## 问题3及解决模板：接受 SSH 主机指纹后断开或登录失败

- [1795244082] 遇到的问题：旧工作流打印了会话地址，作业随后取消，客户端还出现了公钥认证失败。通用解决步骤：看当前运行状态和错误发生的 UTC 时间，重新复制仍在运行的会话地址，用 `ssh-keygen` 核对授权的公钥指纹，再用 `ssh -v -o IdentitiesOnly=yes -i` 确认客户端提交了对应钥匙。

## 问题4及解决模板：本机默认网络连接 GitHub 或 SSH 中继失败

- [1795244236] 遇到的问题：Mac 默认网络上的 GitHub HTTPS 连接报 `EOF`，SSH 也没拿到中继主机钥匙。通用解决步骤：查地址和本机路由，确认是哪个网络接口在走流量。在单次命令中绑定可用接口，通过 TLS 校验访问 GitHub，通过官方指纹校验 SSH 中继。

## 问题5及解决模板：SSH 公钥已通过，会话还没进入 shell 就断开

- [1795245210] 遇到的问题：首次真机测试通过公钥认证后断开，Actions 报 tmate 套接字上没有服务。通用解决步骤：核对 SSH 调试日志和同一时刻的 Actions 日志。启动新会话，用有行列尺寸的终端连接，按页面指示退出 tmate 提示，再执行一条命令验证 shell 可用。

## 想做：

## 做完：

- [1795241983] 当前目录里没有目标仓库。从 GitHub 同步 `C0verSnow/tmate`，切到 `feature/arch-linux-tmate`。
- [1795242056] 在 GitHub 比较了 `mxschmitt/action-tmate`、上游的 `tmate-ssh-server` 和 WarpBuild 的实现。保留指定 Action，用可达的 WarpBuild 中继，并核对了主机指纹。
- [1795242214] 按最新版本要求改用 `mxschmitt/action-tmate@v3`。GitHub 的最新正式版是 `v3.24`，两个标签指向同一提交。
- [1795242275] 新增 Arch Linux SSH 工作流和 README 使用说明，YAML 和 Bash 语法检查已通过。[PR #2](https://github.com/C0verSnow/tmate/pull/2) 已创建。
- [1795244082] PR #2 已合并。从 `main` 建立独立的 `fix/issue-3-ssh` 工作区，保留旧分支的本地记录。
- [1795244082] 在 GitHub 核对了 [action-tmate 的实现](https://github.com/mxschmitt/action-tmate/blob/v3/src/index.js) 和 [WarpBuild 的配置](https://github.com/WarpBuilds/action-debugger/blob/v2/action.yml)。沿用触发账号公钥限制和中继指纹，使用分离模式来统一 PR 与手动运行的验证步骤。
- [1795244236] 已更新工作流和 README，通过 YAML、Bash 语法和差异检查。Mac 已用 SSH 核对中继主机指纹。
- [1795245210] 已创建 [PR #4](https://github.com/C0verSnow/tmate/pull/4)，[自动检查](https://github.com/C0verSnow/tmate/actions/runs/44878094155) 确认无钥匙连接会被拒绝。
- [1795245210] 用本机 Firefox 添加临时公钥。首轮从 Mac 通过公钥认证后断开。[复测](https://github.com/C0verSnow/tmate/actions/runs/44879135121) 用 Bash 发起 SSH，按 `q` 退出 tmate 提示后读取到 `ID=arch`。执行 `touch /continue` 后 SSH 结束，Actions 作业成功。

## 没做：

- [1795242275] 按仓库约定不做本地编译，运行结果以 GitHub Actions 为准。

## 在做：

- [1795245210] 提交实测记录，等最终 PR 检查通过后合并，核对 Issue #3 是否关闭。
