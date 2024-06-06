# 2024新版本如何配置CLion与cubeMX开发STM32

## 1. 为什么我要在网上有很多教程的情况下再做一个新版
各种大佬们给出的配置教程原本很详细，但是在时间过了这么久之后已经不完全符合现在的环境了。
昨天在教群里萌新安装CLION+CUBEMX时我才发现，在配置过程中新出现的一些变化甚至没有清晰的提示报错，如果没有人解释提醒的话，新人要自己解决需要折腾很久也不一定能解决。
遂决定根据自己的经验，把新的安装教程整理如下。
## 2.CLion的下载与“激活”
**商用用户请购入正版，支持软件行业正常发展**
**本教程“激活”方式仅提供个人学习用途使用**
### 2.1 下载安装
[CLion官网](https://www.jetbrains.com/zh-cn/clion/download/)
直接在官网下载最新版即可
安装，看得懂设置就看，看不懂就全程下一步，再不然就去用翻译软件，我没有精力把整个安装界面翻译一遍。
### 2.2 个人学习用途的激活
这里有三种方案
1. JetBrains官网申请学生认证（正版合法）
[申请地址](https://www.jetbrains.com/zh-cn/community/education/#students)
2. Github申请学生包（容易申请，正版合法）[申请地址](https://education.github.com/pack)

在校学生建议使用前两种方案，直接在对应页面申请即可
3. 使用网络补丁+许可证激活码激活（学习版请勿用于商业）

	- 下载工具包
[百度网盘](https://pan.baidu.com/s/1rMfq1coenaHP_EejYsH9qQ?pwd=u7vv) 
提取码：u7vv
	- 解压压缩包，双击`index.html`
	- 点击上面浮动条的蓝字下载`jetbra.zip`，（点击没反应的话就直接去工具包的files文件夹里复制）
	- 放到你想要放的地方，解压出来（这个网络补丁得一直放在那不能删所以别干出配置在桌面或者配置在下载文件夹的逆天操作来）
	- 进入解压出来的文件夹，进入`script`文件夹,在`install-all-users.vbs`上右键，以管理员身份运行
	- 等到运行完成后回到之前的index.html,找到CLion，点击一下下面的****复制激活码
	- 运行CLion，选择`Activate CLion`->`Activation code`,在下面的框里粘贴激活码，点击Activate完成激活

## 3. 工具链的安装
### 3.1 ARM GNU Toolchain的安装
从 Arm GNU Toolchain 的新页面下载最新的

**Windows (mingw-w64-i686) hosted cross toolchains**
**AArch32 bare-metal target (arm-none-eabi)**

- 下载exe格式的那个就行，安装时候**记住自己安在哪里，一会要用**  

- **安装完把完成页面的`Add path to environment variable`勾上！**

## 3.2 STM32CUBEMX的安装
[官网下载安装](https://www.st.com/en/development-tools/stm32cubemx.html)
- 也要记住安在哪

## 3.3 OPENOCD的安装
[Github下载，找个地方解压](https://github.com/openocd-org/openocd/releases)
- 也要记住解压到哪！

## 4.CLion工具链配置
- 打开CLion，选择左边`Customize`,点击右边最下面`All settings`
- 弹出窗口选择左边`Build, Execution, Deployment`里面的`Toolchains`,
- 把`C Compiler`设置为`ARM GNU Toolchain`安装目录下bin文件夹里的`arm-none-eabi-gcc.exe`
- 把`C++ Compiler`设置为`ARM GNU Toolchain`安装目录下bin文件夹里的`arm-none-eabi-g++.exe`
- 点击Apply应用设置
- 还是左边`Build, Execution, Deployment`里面，找到`Embedded Development`
- 里面的`OpenOCD Location`去openocd安装目录的bin文件夹找到`openocd.exe`
- 里面的`Stm32CubeMX Location`去stm32cubemx安装目录的bin文件夹找到`STM32CubeMX.exe`
- 点击`Apply`应用设置

## 5. 新建项目
- 新建项目左边选择STM32CubeMX，右边选择工程存放路径，进入后等待创建完成（此时开发板是STM32F030F4Px）
- 用CubeMX打开ioc文件，修改为自己的单片机或开发板型号，并完成自己要配置的内容
- 生成设置里面`Project Settings`要和CLion里的项目名字一致，`Toolchain/IDE`要选择`STM32CubeIDE`
- 生成代码，完成后回到CLion

## 6. 编译烧录设置
- 回到CLion后会弹出一个板卡选择框，基本上都不能用，需要自己写一个烧录配置
- 例如STM32F1单片机 + ST-Link
```
在工程根目录下新建一个文件夹config，在里面新建一个配置文件stlink.cfg

source [find interface/stlink.cfg]
transport select hla_swd 
source [find target/stm32f1x.cfg] 
adapter speed 10000
```
之后在`OPENOCD运行-配置`选择这个`cfg`文件即可
