Re-microsoft store

右键以管理员运行 Windows Powershell 

输入
``` 
Get-AppxPackage -allusers Microsoft.WindowsStore | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}
``` 

打开商店 (cmd open microsoft store)
    start ms-windows-store:

无法加载页面
实测临时关闭lan代理即可打开微软应用商店
