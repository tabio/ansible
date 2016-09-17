# playbookの実行
```
ansible-playbook -i host playbook名
```

# mysqlの初期設定に関して
```mysql
# mysqlの初期化
mysql_secure_installation
initial password -> /var/log/mysqld.log

# gitlabのDBとユーザーの作成
CREATE DATABASE gitlabhq_production;
CREATE USER 'gitlab'@'localhost' IDENTIFIED BY 'hogehoge';
GRANT ALL PRIVILEGES ON gitlabhq_production.* TO 'gitlab'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

# redmineのDBとユーザーの作成
CREATE DATABASE redmine 
CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'hogehoge';
UPDATE user SET Select_priv = 'Y', Insert_priv = 'Y', Update_priv = 'Y', Delete_priv = 'Y', Create_priv = 'Y', Drop_priv = 'Y', Index_priv = 'Y', Alter_priv = 'Y' WHERE User = 'redmine'
FLUSH PRIVILEGES;
```
