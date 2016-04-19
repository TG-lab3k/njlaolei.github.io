1. Sublime 2/3 无法输入中文问题
     解决方案
      a. 下载sublime-text-imfix
      git clone https://github.com/lyfeyaj/sublime-text-imfix.git

      b. 将subl移动到/usr/bin/，并且将sublime-imfix.so移动到/opt/sublime_text/（sublime的安装目录）
      sudo cp ./lib/libsublime-imfix.so /opt/sublime_text/
      sudo cp ./src/subl /usr/bin/

      c. 测试; 用subl命令(如下)试试能不能启动sublime，如果成功启动的话，应该就可以输入中文了。
      LD_PRELOAD=./libsublime-imfix.so subl

      d. 经过以上步骤，使用[c]中的命令启动的Sublime已经可以正常输入中文了，但从Sublime的快捷方式启动的Sublime还是无法输入中文.
      那么，我们进行一个整合与Sublime快捷方式衔接.
      1) 创建shell脚本, 内容如下：
      #!/bin/bash
      LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so subl

      2) 保存该脚本(脚本名: sublime)并为其增加执行权限: chmod a+x  sublime
      3) 将该脚本移动到/usr/bin目录
      sudo cp .sublime /usr/bin
      4) 修改sublime快捷方式脚本(/usr/share/applications/sublime_text.desktop)
      将快捷方式脚本中"Exec=/opt/sublime_text/sublime_text" 替换成 “Exec=sublime”
      (tips: 原Exec执行的命令有些是带参数了的，我是保留原参数了的， 如: Exec=/opt/sublime_text/sublime_text --command new_file   --> Exec=sublime --command new_file)
      
      Refer: http://www.jianshu.com/p/bf05fb3a4709


      