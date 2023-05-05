# IDE/Editor的配置

## CLion

如果你选择使用CLion进行开发，那么无需做任何配置，使用CLion打开源代码所在文件夹即可。

## VSCode

- 查看VSCode文档，自行安装
    - 如果你使用VirtualBox一类的虚拟机，安装了带图形界面的Ubuntu，建议将VSCode安装在Ubuntu中
    - 如果你使用WSL，在Windows里安装VSCode然后在VSCode中安装WSL插件
    - 以下默认你是按照上面的方法安装的VSCode
- 打开你的源代码所在的文件夹
    - 虚拟机：直接按`Ctrl+O`打开文件夹
    - WSL：按右下角的Open A Remote Window，选择New WSL Window，然后打开源代码所在文件夹

        ![Open A Remote Window](images/remote-btn.jpeg)
- 打开以后右下角会弹出提示框，点击Yes
        ![Install Recommended Extensions](images/recommended_ext.png)
  等待插件安装完成后，按`F1`输入`CMake: Configure`进行CMake自动配置，配置完成会后自动进行代码索引。
  等待索引完成后进入`storage/db20xx`文件夹，任意打开一个文件测试跳转等功能是否正常。
