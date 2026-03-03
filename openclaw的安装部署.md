OpenClaw 的官网在：https://openclaw.ai
官方文档在：https://docs.openclaw.ai/zh-CN
github 上的项目地址为：https://github.com/openclaw/openclaw

安装 OpenClaw 推荐的操作系统是 Linux 和 macOS，因此在Windows下，可以在 WSL2 的 Linux 环境中安装 OpenClaw。

```
Windows10/11 下安装WSL2（Windows Subsystem for Linux 2）

- 以管理员身份运行 PowerShell：wsl --install
  安装成功后，在终端运行：ubuntu，输入用户名和密码登录
  
  
性能优化（.wslconfig） 在 _C:\Users\<用户名>\.wslconfig_ 中添加：
[wsl2]
memory=4GB
processors=4
swap=8GB
autoMemoryReclaim=true
localhostForwarding=true
guiApplications=true

```

安装linubrew：
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
如果安装很耗时，可以使用镜像：
```
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

预装的软件NodeJS（>=22）、git