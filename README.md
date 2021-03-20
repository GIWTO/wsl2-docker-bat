# wsl2-docker-bat

## 配置自启动

## wsl脚本

> 将以下内容添加至 `/etc/init.wsl`

```
service docker start
```

设置可执行权限

```
sudo chmod +x /etc/init.wsl
```

### Windows开机自启

win+r，`shell:startup`，在该目录下创建vbs脚本
`ubuntu-start.vbs`

``` Set ws = WScript.CreateObject("WScript.Shell")
Set ws = WScript.CreateObject("WScript.Shell")
ws.run "wsl -d ubuntu -u root /etc/init.wsl"
```

固定wsl2ip

通过开机启动脚本固定ip地址

新建bat文件`startup.bat`

目录 `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`

```
rem 以管理员权限运行
%1 mshta vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe","/c %~s0 ::","","runas",1)(window.close)&&exit
@echo off
rem 隐藏窗口
if "%1" == "h" goto begin
　　mshta vbscript:createobject("wscript.shell").run("%~nx0 h",0)（window.close)&&exit
:begin

rem set IP for WSL
wsl -d Ubuntu-18.04 -u root ip addr add 172.27.50.110/24 broadcast 172.27.50.255 dev eth0 label eth0:1
rem set IP for windows10
netsh interface ip add address "vEthernet (WSL)" 172.27.50.10 255.255.255.0

pause
```

