# 本文档为4象限的任务清单，全文书写大白话

## 问题1及解决模板：tmate 默认中继已停运

- [1795242056] 遇到的问题：默认中继已停运，Action 会卡在等会话就绪。通用解决步骤：读上游的停运公告，找有官方说明的可用中继，将现场查到的主机指纹和官方配置核对，再用远端 Actions 验证。

## 问题2及解决模板：Action 没有 latest 引用

- [1795242214] 遇到的问题：GitHub 上标记为最新的 Release 不代表仓库有 `latest` 标签。通用解决步骤：查询上游标签和最新 Release。要跟随更新时，使用上游维护的浮动标签并确认它指向的提交。

## 想做：

- [1795242275] 远端测试通过后合并 PR，确认 Issue #1 已关闭。

## 做完：

- [1795241983] 当前目录里没有目标仓库。从 GitHub 同步 `C0verSnow/tmate`，切到 `feature/arch-linux-tmate`。
- [1795242056] 在 GitHub 比较了 `mxschmitt/action-tmate`、上游的 `tmate-ssh-server` 和 WarpBuild 的实现。保留指定 Action，用可达的 WarpBuild 中继，并核对了主机指纹。
- [1795242214] 按最新版本要求改用 `mxschmitt/action-tmate@v3`。GitHub 的最新正式版是 `v3.24`，两个标签指向同一提交。
- [1795242275] 新增 Arch Linux SSH 工作流和 README 使用说明，YAML 和 Bash 语法检查已通过。[PR #2](https://github.com/C0verSnow/tmate/pull/2) 已创建。

## 没做：

- [1795242275] 按仓库约定不做本地编译，运行结果以 GitHub Actions 为准。

## 在做：

- [1795242275] GitHub Actions 已装好 Arch 软件包，正在验证 tmate 中继、SSH 地址和公钥限制。
