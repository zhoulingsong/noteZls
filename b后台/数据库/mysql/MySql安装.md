## Mac 下安装 MySql

### 一、 从官网下载 MySql

1. 打开 MySql 官网下载页 https://www.mysql.com/downloads/
2. 找到 MySQL Community Edition （社区版），点击下面的 Downloads 
3. 找到 [MySQL Community Server](https://dev.mysql.com/downloads/mysql/) 点击看下面的 [DOWNLOAD](https://dev.mysql.com/downloads/mysql/)
4. 现在才是真正的下载页面，有多种文件类型可选择 .tar.gz / .tar / .dmg
5. 选择最下面的这句话开始下载 [No thanks, just start my download.](https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.12-macos10.13-x86_64.dmg)



![image-20180908121112603](/var/folders/k2/gsbbv4y57w71m_3s_8d_927h0000gn/T/abnerworks.Typora/image-20180908121112603.png)

### 二、 安装

1. 双击解压下载文件 mysql-8.0.12-macos10.13-x86_64.dmg
2. 双击 pkg 文件，一路点击 继续 向下安装。
3. 最后记得保存最后弹出框中的密码（它是你mysql root账号的密码），也可以在生成密码的页面选择自定义密码（我的密码是 aa123123)

### 三、 系统配置

1. 进入系统偏好设置
2. 点击 MySql
3. 开启 MySql 服务，（我的设置为 开机自动开启）
4. 设置环境变量
   1. 进入 /usr/local/mysql/bin 查看是否有 mysql 命令文件存在
   2. 将上面的路径添加到用户下的 ~/.bash_profile 中 PATH=$PATH:/usr/local/mysql/bin
   3. 让环境变量生效 source ~/.bash_profile
   4. 测试下是否成功输入 mysql -uroot -p密码 看是否能链接 mysql  



## MySQL Workbench 工具

1. **问题描述**：Mac版MySQL Workbench出现异常强制退出后，再次进入后左侧的navigator消失，左侧整个导航条消失了，只显示Object info和Session。

   ![image-20190119152222938](/var/folders/k2/gsbbv4y57w71m_3s_8d_927h0000gn/T/abnerworks.Typora/image-20190119152222938.png)

   **问题根源：**MySQL Workbench出现异常强制退出，配置文件发生一场，需要重置配置文件即可恢复原样。

   **解决方案：**

   1. 关闭 MySQL Workbench

   2. 打开Mac的终端Terminal，输入一下命令：

   ~~~ 
   cd ~/Library/Application\ Support/MySQL/Workbench/
   rm wb_state.xml
   rm wb_options.xml
   ~~~

   3. 重启 MySQL Workbench

