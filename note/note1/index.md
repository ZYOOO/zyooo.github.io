# 如何添加右键可选在此处打开命令行窗口


## 如何添加右键可选在此处打开命令行窗口



```
1.win+r   输入 regedit 回车打开注册表

2.切换到HKEY_CLASSES_ROOT\Directory\Background\shell\
    右键shell，新建项 “OpenCMDHere”
    并在该项下，右击新建项 “command”
     直接点击OpenCMDHere将OpenCMDHere中的默认改为 在此处打开命令窗口
    右键 新建字符串名，名字为Icon将值改为cmd.exe 创建 OpenCMDHere 的图片    
3.修改command最终的默认值 输入  cmd.exe /s /k pushd \"%V\"
```


