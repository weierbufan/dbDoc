### 一，准备好shell脚本

    vi /home/zhangy/database_bak.sh  
  
    #!/bin/sh  
    # File: /home/zhangy/database_bak.sh  
    # Database info bakupmysql  TANK 2009/11/04  
    DB_USER="root"                                                              #用户名  
    DB_PASS="********"                                                         ＃密码  
    DB_NAME="myblog"                                                      ＃要备份的数据名  
  
    <span id="more-161"></span># Others vars  
    DATE=`date +%Y_%m_%d`                                            ＃要备份的日期   
    YESTERDAY=`date -d yesterday +%Y_%m_%d`           ＃删除昨天的备份  
  
    BIN_DIR="/usr/local/mysql/bin"  
    BCK_DIR="/home/zhangy/database_bak"                     ＃备份路径      
  
    cd $BCK_DIR  
  
    ＃删除以前该数据库的备份，因为我的linux下面还有2G硬盘空间，郁闷。  
    if [ -f $YESTERDAY$DB_NAME".sql" ]  
    then  
     rm -f $YESTERDAY$DB_NAME".sql"  
    fi  
  
    # 备份  
    ${BIN_DIR}/mysqldump --opt -u${DB_USER} -p${DB_PASS} ${DB_NAME} > ${BCK_DIR}/${DATE}${DB_NAME}.sql  
    
### 二，定期执行

把shell放到crontab里面。

＃查看crond是否已启动

[root@BlackGhost cron]# ps -e|grep crond

21519 ?        00:00:00 crond

＃打开crontab

[root@BlackGhost cron]# crontab -e

＃在里面加上一行

00 18 * * * /home/zhangy/database_bak.sh

＃查看一下是否已加上

[root@BlackGhost cron]# crontab -l

#

# DO NOT EDIT THIS FILE MANUALLY!! USE crontab -e INSTEAD.

#

# <minute> <hour> <day> <month> <dow> <command>

01 * * * *  /usr/sbin/run-cron /etc/cron.hourly

02 00 * * * /usr/sbin/run-cron /etc/cron.daily

22 00 * * 0 /usr/sbin/run-cron /etc/cron.weekly

42 00 1 * * /usr/sbin/run-cron /etc/cron.monthly

56 06 * * * /home/zhangy/www/bb.php

51 23 * * * /sbin/shutdown -h now

00 18 * * * /home/zhangy/database_bak.sh

然后退出

> 转自：http://blog.51yip.com/mysql/161.html    
