# Laravel + apache 手順

```
% = ローカル
$ = ゲスト
```

## vagrant起動

```shell
% vagrant up
% vagrant ssh
# vagrant upでファイル共有エラーが出た場合
$ sudo yum update -y
$ sudo yum install -y kernel # こっちか
$ sudo yum install -y kernel-devel # こっち
$ exit
% vagrant reload
```

## 依存関係

```shell
% vagrant ssh
$ sudo setenforce 0
$ sudo yum install -y zip unzip wget git vim
$ sudo yum install -y httpd
$ sudo systemctl enable httpd.service
$ sudo systemctl restart httpd.service
$ sudo systemctl status httpd.service
```

## PHP

```shell
$ sudo yum install -y epel-release
$ sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm
$ sudo yum install -y --enablerepo=remi,remi-php73 php php-zip php-devel php-mbstring php-pdo php-xml php-bcmath
```

## Laravel

```shell
$ curl -sS https://getcomposer.org/installer | php
$ sudo mv composer.phar /usr/local/bin/composer
$ composer global require laravel/installer

$ cd /var/www/html
$ sudo chmod 777 -R /var/www/html

$ composer create-project --prefer-dist laravel/laravel dev_laravel
```

## apache

```shell
$ sudo cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.org
$ sudo vim /etc/httpd/conf/httpd.conf
$ sudo systemctl restart httpd.service
```

```conf
DocumentRoot "/var/www/html/dev_laravel/public"
```

## 権限付与

```shell
# SELinuxを無効
$ sudo setenforce 0

$ cd /var/www/html/dev_laravel
$ sudo chmod 775 -R ./storage/
$ sudo chmod 775 -R ./bootstrap/cache
```

# vagrant再起動時にlaravelが動かない場合

```shell
$ sudo vim /etc/selinux/config

# ----
# SELINUX=disabled
# ----
```

http://192.168.33.10/
