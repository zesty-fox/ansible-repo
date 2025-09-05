# Ansible Pull Cron 任务设置仓库

本仓库用于通过 `ansible-pull` 模式，在目标机器上设置一个 Cron 定时任务来定期执行一个**已存在的** `/usr/local/bin/ansible-pull-start.sh` 脚本。

## 核心功能

-   在开机时触发一次 `/usr/local/bin/ansible-pull-start.sh`。
-   每分钟触发一次 `/usr/local/bin/ansible-pull-start.sh`。

## 如何使用

### 前提条件

-   目标机器上已经存在一个可执行的 `/usr/local/bin/ansible-pull-start.sh` 脚本。
-   目标机器上已经安装了 `ansible`。

### 步骤 1: 准备您的仓库

1.  **解压并上传**：解压 `ansible-pull-repo.zip` 并将里面的所有文件和目录上传到您自己的 Git 仓库 (例如 GitHub, GitLab)。
2.  **提交更改**：将您的修改 `commit` 并 `push` 到您的远程仓库。

### 步骤 2: 在目标机器上初始化 (Bootstrap)

在您想要管理的任何一台新机器上，只需以 root 用户或使用 sudo 执行以下**一次性**命令即可。

```bash
# 请将 URL 替换为您自己的仓库地址
ansible-pull -U [https://github.com/your-username/your-ansible-pull-repo.git](https://github.com/your-username/your-ansible-pull-repo.git)
```
此命令会下载本仓库并执行 `local.yml`，从而将 Cron 定时任务配置好。

### 如何验证

初始化后，您可以登录到目标机器，检查 cron 任务是否已成功添加：

```bash
crontab -l
```

您应该能看到类似下面的输出：
```
# Ansible: ansible-pull on reboot
@reboot /usr/local/bin/ansible-pull-start.sh > /dev/null 2>&1
# Ansible: ansible-pull per minute
*/1 * * * * /usr/local/bin/ansible-pull-start.sh > /dev/null 2>&1
```
