# 概要
+ ansibleはmacにインストール
+ CentOS 7.5にて実行
+ ruby
  + 2.5.3
  + rbenvを利用
+ mysql 8.0
+ vim 8.1
+ redis
+ nodejs 10.13
  + yarn latest
+ gitlab
  + nginxは自前
  + DBはmysqlを使用するよう変更
  + redisはembededの物を利用
  + 設定の変更はgitlab-ctl reconfigure
  + 再起動はgitlab-ctl restart
  + 初期アカウント root / 5iveL!fe
  + ログイン後path
    + http://xxx/users/sign_in
+ redmine
  + 3.2系
  + nginx
  + unicorn
  + 初期アカウントは admin / admin

# 事前準備
- ansibleインストール
```
brew install ansible
```
- ssh
  - サーバーにログインして公開鍵方式で認証できるようにする
  - rootログイン無効とパスワード認証を禁止
```
vi /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no

systemctl restart sshd
```
  - sshのportを変更
```
vi /etc/ssh/sshd_config # Port 22の部分を変更
systemctl restart sshd # 設定の反映
```
  - firewall側のsshのportも変更
```
vi /usr/lib/firewalld/services/ssh.xml # port="22"の部分を変更
firewall-cmd --reload # 設定の反映
firewall-cmd --list-all --zone=public --permanent # 公開されているポートの確認
```

- webサーバー
```
# 443ポート開放
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
firewall-cmd --list-all --zone=public --permanent
```

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
