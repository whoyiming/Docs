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

安装成功后，运行：
```
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.bashrc
source ~/.bashrc
brew -v
```

预装的软件NodeJS（>=22）、git
```
brew install node@24
echo 'export PATH="/home/linuxbrew/.linuxbrew/opt/node@24/bin:$PATH"' >> ~/.profile
source ~/.profile
node -v
```

另外一种按照nodejs，上面用homebrew按照下载依赖包太多了
```
# 下载并安装 nvm：
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# 代替重启 shell
\. "$HOME/.nvm/nvm.sh"

# 下载并安装 Node.js：
nvm install 24

# 验证 Node.js 版本：
node -v # Should print "v24.14.0".

# 验证 npm 版本：
npm -v # Should print "11.9.0".

```

安装openclaw，以后要升级 OpenClaw，还是这条命令。
```
curl -sSL https://openclaw.ai/install.sh | bash
```

安装成功后，若没有出现配置画面，可在终端运行
```
openclaw onboard --install-daemon
```

按照提示来进行选择
![](images/file-20260304201553925.png)

选择相应的大模型，比如qwen时，按照上面提示操作即可
![](images/file-20260304201824991.png)

接下来配置 IM 工具。选择飞书
![](images/file-20260304202715862.png)

Install Feishu plugin，选择 Download from npm
![](images/file-20260304203159086.png)

飞书插件安装好后，进行飞书配置

1、登录飞书开放平台（open.feishu.cn），选择“创建企业自建应用”。
![](images/file-20260304205144437.png)
将内容填写

2、应用创建后，选择“添加应用能力”中的“机器人”。
![](images/file-20260304205351306.png)

3、添加权限管理
![](images/file-20260304205456929.png)

4、复制以下内容，替换对话框中的 JSON 内容，然后点击“下一步，确认新增权限”。
```
{
  "scopes": {
    "tenant": [
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "cardkit:card:write",
      "contact:contact.base:readonly",
      "contact:user.base:readonly",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "docs:document.content:read",
      "event:ip_list",
      "im:chat",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:bot_access",
      "im:chat:create",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.group_msg",
      "im:message.p2p_msg:readonly",
      "im:message:readonly",
      "im:message:send_as_bot",
      "im:resource",
      "sheets:spreadsheet",
      "wiki:wiki:readonly"
    ],
    "user": [
      "aily:file:read",
      "aily:file:write",
      "contact:user.employee_id:readonly",
      "im:chat",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat:read",
      "im:chat:readonly"
    ]
  }
}
```

5、出现新的对话框，点击“申请开通”
![](images/file-20260304205646405.png)

6、出现下面这个界面，不需要做任何修改，直接点击“确认”。
![](images/file-20260304205726094.png)

7、创建版本
![](images/file-20260304205938419.png)

8、输入版本号，并保存
![](images/file-20260304210058173.png)

9、出现对话框，点击“确认发布”。
![](images/file-20260304210124555.png)

10、选择左侧菜单栏中的“凭证与基础信息”，复制 App ID 和 App Secret，后续配置将会用到
![](images/file-20260304212107251.png)


11、进入 API 调试台，创建一个新的飞书群
![](images/file-20260304212418247.png)

12、点击创建群，获取token，然后点击“快速复制 open_id”，选择你的飞书用户，点击“复制成员 ID”
![](images/file-20260304212744794.png)


13、将 JSON 内容中的 owner_id、user_id_list 都替换为刚才复制的用户的 open_id；将 bot_id_list 修改为第10步的应用的 App ID，再点击“开始调试”，显示调用成功
![](images/file-20260304212917748.png)

14、复制响应体中这个群的 chat_id
![](images/file-20260304213339954.png)

15、配置openclaw中的飞书
![](images/file-20260304213927814.png)

16、再回到飞书，选择左侧菜单栏中的“事件与回调”，为应用设置事件。订阅方式选择“使用长连接接收事件”，然后点击“保存”
![](images/file-20260304213619318.png)

17、添加事件，并创建版本（应用发布后，当前配置方可生效”，你都需要创建新版本并且重新发布。）
![](images/file-20260304214916555.png)
![](images/file-20260304215349162.png)


18、回到终端，skills可以先跳过以及后面的都可以先跳过，下面的默认
![](images/file-20260304214527374.png)

19、开始享受喜悦吧
![](images/file-20260304215433110.png)

20、实现飞书机器人的配对，每次当飞书应用重新发布后，都需要执行以下命令重新启动 OpenClaw 的 gateway。
```
openclaw gateway restart
```

21、gateway 重新启动成功后，使用以下命令 发一个消息
```
openclaw message send --channel feishu --target 'ou_466b682099f73a0118ff195b23c02c36' --message "Hello from OpenClaw"
```
注意：其中的 “ou_466b682099f73a0118ff195b23c02c36” 需要替换为在12、13步的用户的 open_id。
![](images/file-20260304220739451.png)



注：
终端交互式命令：openclaw tui
以指定端口启动：openclaw gateway --port 18789 --verbose