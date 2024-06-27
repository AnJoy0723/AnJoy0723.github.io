1、设置-颜色-打开 透明效果；
2、打开注册表，找到路径：
计算机\\HKEY_CURRENT_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\Advanced；
3、右侧找到名称 TaskbarAcrylicOpacity，如果没有，就右键新建一个DWORD（32位）值D；
4、将值修改为十进制0就是全透明，也可以是十进制0-10之间的效果。
5、不用重启电脑，任务管理器重启windows资源管理器就行。