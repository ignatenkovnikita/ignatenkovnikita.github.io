---
layout: post
title: Postgres notes!
---

Postgres notes!

If have error, after create user

sudo -u postgres psql
Создадим тестовую базу данных и тестового пользователя:
CREATE DATABASE test_database;
CREATE USER test_user WITH password 'qwerty';
GRANT ALL ON DATABASE test_database TO test_user;
Для выхода из оболочки введите команду \q.
Теперь попробуем поработать с созданной базой данных от имени test_user:
psql -h localhost test_database test_user


-bash-4.2$ psql -U test_user -W  -d test_database
Password for user test_user: 
psql: FATAL:  Peer authentication failed for user "test_user"

Fix error edit file
vim /var/lib/pgsql/9.6/data/pg_hba.conf

add record
local   all             test_user                               md5
and restart postgres
service postgresql-9.6 restart
