# Arch Linux SSH 调试

GitHub 托管的 Ubuntu runner 会启动一个 Arch Linux 容器。SSH 会话进入该 Arch 容器，里面可以用 `pacman` 装软件。每次运行都会创建新容器。

## 连接步骤

1. 在 [GitHub 账号设置](https://github.com/settings/keys) 注册一个 SSH 公钥，并保留对应的私钥。只有触发工作流的账号所注册的 SSH 公钥可以登录。
2. 打开 [Actions → Arch Linux SSH](https://github.com/C0verSnow/tmate/actions/workflows/arch-linux-ssh.yml)，选择 **Run workflow**，选分支后启动。
3. 打开本次运行的 **Arch Linux** 作业，展开 **Debugging with tmate**，复制日志里的 `SSH:` 命令。如果私钥不在默认位置，给命令加上 `-i /path/to/private-key`。
4. 在终端运行 `cat /etc/os-release`，确认 `ID=arch`。调试结束后运行 `touch /continue`，或在 GitHub 取消本次运行。单次运行最长 30 分钟。

首次连接时，WarpBuild 中继的 Ed25519 主机指纹应为 `SHA256:+aWjAPAeKuvuZSPG92Q8yaEkd5eb2T+uF9Jp+8Sb/+A`。

## 远端测试

仓库内 `main` 的 PR 会在 GitHub Actions 上安装 Arch 软件包、启动 tmate、检查中继返回的 SSH 地址，并确认未提供私钥的 SSH 连接被拒绝。测试结束后会关闭会话。手动启动时也可以勾选 **只运行自动验证**。

工作流使用上游维护的 [`mxschmitt/action-tmate@v3`](https://github.com/mxschmitt/action-tmate/releases/tag/v3)，随 v3 系列更新。上游没有 `latest` 标签或分支。原来的 [tmate 公共中继已停运](https://github.com/tmate-io/tmate/issues/336)，这里使用 [WarpBuild 的公开中继及其主机指纹](https://github.com/WarpBuilds/action-debugger/blob/v2/action.yml)。Arch 软件包来自 [Arch Linux 官方仓库](https://archlinux.org/packages/extra/x86_64/tmate/)。
