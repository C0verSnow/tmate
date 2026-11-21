# Arch Linux SSH 调试

GitHub 托管的 Ubuntu runner 会启动一个 Arch Linux 容器。SSH 会话进入该 Arch 容器，里面可以用 `pacman` 装软件。每次运行都会创建新容器。

## 连接步骤

1. 在 [GitHub 账号设置](https://github.com/settings/keys) 注册 SSH 登录公钥，并保留配对的私钥。工作流只授权触发账号登记的登录公钥。新增或更换钥匙后，重新启动工作流。
2. 打开 [Actions → Arch Linux SSH](https://github.com/C0verSnow/tmate/actions/workflows/arch-linux-ssh.yml)，选择 **Run workflow**，选分支后启动。要手动登录，将 **只运行自动验证** 留空。
3. 打开本次运行的 **Arch Linux** 作业。等 **Verify SSH and show connection instructions** 通过后，查看该步骤打印的授权公钥指纹和 `SSH connection` 命令。请在作业仍运行时使用本次地址。
4. 在 Bash 或 Windows PowerShell 中运行下面的命令。按实际路径替换钥匙文件名，将 `SESSION@gha.warp.build` 换成本次地址。第一条命令显示公钥指纹，应与步骤日志中的一条授权公钥指纹相同。

   ```sh
   ssh-keygen -lf "$HOME/.ssh/id_ed25519.pub"
   ssh -o IdentitiesOnly=yes -o HostKeyAlgorithms=ssh-ed25519 -i "$HOME/.ssh/id_ed25519" SESSION@gha.warp.build
   ```

5. 首次连接时，核对 WarpBuild 的 Ed25519 主机指纹 `SHA256:+aWjAPAeKuvuZSPG92Q8yaEkd5eb2T+uF9Jp+8Sb/+A`。吻合后输入 `yes` 并按回车。登录后运行 `grep '^ID=arch$' /etc/os-release`，应输出 `ID=arch`。
6. 调试结束后在会话中运行 `touch /continue`，或在 GitHub 取消本次运行。作业最多运行 30 分钟，从安装软件时开始计时。

## 连接失败时

- `Permission denied (publickey)` 时，在连接命令中加入 `-v`。查看 `Offering public key` 行，核对客户端提交的钥匙指纹。`-i` 应指向私钥文件，核对指纹时使用 `.pub` 文件。
- `Connection closed` 时，检查 Actions 作业是否仍在运行，并核对地址来自当前运行。如果作业已结束或取消，重新运行工作流并复制新地址。向维护者报告时，提供运行链接、出错的 UTC 时间和 SSH 错误行。

## 远端测试

每次运行都会在 GitHub Actions 上安装 Arch 软件包、启动 tmate、校验中继地址和主机指纹，并实际发起一次无钥匙的 SSH 连接，确认中继拒绝登录。仓库内发往 `main` 的 PR 和勾选 **只运行自动验证** 的手动运行会在验证后关闭会话。验证通过的普通手动运行会保留会话，供使用授权私钥的客户端登录。

工作流使用上游维护的 [`mxschmitt/action-tmate@v3`](https://github.com/mxschmitt/action-tmate/releases/tag/v3)，随 v3 系列更新。上游没有 `latest` 标签或分支。原来的 [tmate 公共中继已停运](https://github.com/tmate-io/tmate/issues/336)，这里使用 [WarpBuild 的公开中继及其主机指纹](https://github.com/WarpBuilds/action-debugger/blob/v2/action.yml)。Arch 软件包来自 [Arch Linux 官方仓库](https://archlinux.org/packages/extra/x86_64/tmate/)。
