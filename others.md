## Mysql
***

1. mac os下忘了密码
在/usr/local/mysql/bin下
* ./mysqld_safe --skip-grant-tables &
* mysql -u root
* mysql> alter user 'user1'@'localhost' Identified by "password1";
* exit

1. my.cnf:
* 新建/etc/my.cnf
[mysqld]
bind-address = 127.0.0.1
# mac os 下导入导出数据
secure-file-priv="/最多3层的目录，chmod 777/"
#不区分大小写:
lower_case_table_names=1  