# 本文档为4象限的任务清单，全文书写大白话

## 问题1及解决模板：tmate 默认中继已停运

- [1795242056] 遇到的问题：默认中继已停运，Action 会卡在等会话就绪。通用解决步骤：读上游的停运公告，找有官方说明的可用中继，将现场查到的主机指纹和官方配置核对，再用远端 Actions 验证。

## 想做：

- [1795241983] 用 GitHub Actions 准备一个能通过 SSH 登录的 Arch Linux 环境，跑测试，更新说明，合并 PR 并关闭 Issue #1。

## 做完：

- [1795241983] 当前目录里没有目标仓库。从 GitHub 同步 `C0verSnow/tmate`，切到 `feature/arch-linux-tmate`。
- [1795242056] 在 GitHub 比较了 `mxschmitt/action-tmate`、上游的 `tmate-ssh-server` 和 WarpBuild 的实现。保留指定 Action，用可达的 WarpBuild 中继，并核对了主机指纹。

## 没做：

- [1795242136] 按仓库约定不做本地编译，运行结果以 GitHub Actions 为准。

## 在做：

- [1795242136] 添加工作流和使用说明，准备远端验证。
