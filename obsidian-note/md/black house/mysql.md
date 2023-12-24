# 数据库的基本概念
1. 数据库是用来组织、存储和管理数据的仓库
2. 数据组织结构，数据库，数据表、数据行、字段
3. 数据结构之间的关系![image.png](https://s2.loli.net/2023/12/19/rGPvoKjFduJMwxO.png)
# 安装并配置MySQL
1. 对于开发人员来说，安装mysql server 和 MySQL workbench 就足够开发了![image.png](https://s2.loli.net/2023/12/19/hCZJUqaNSAk6gpO.png)
2. 安装教程[[README]]
#  创建数据库、表、行
## 数据库创建
![image.png|650](https://s2.loli.net/2023/12/19/XVatC8slvNPkfSc.png)

## 表的创建
![image.png|700](https://s2.loli.net/2023/12/19/fWHsaBrSqugQhvN.png)
## 向表中写数据
![image.png](https://s2.loli.net/2023/12/19/42SmaLJGiBj7nVX.png)
# 使用sql管理数据库
## 什么是SQL!
![](https://s2.loli.net/2023/12/19/G6BuWCxU9zoXAcZ.png)
## sql能做什么？
增删改查等等，学习目标主要是掌握增(select)删(delete)改(update)查(insert into)命令、还有几个查询语句，where,and和or,count(星号)，order by排序
## select语句
![image.png](https://s2.loli.net/2023/12/19/KkBgt4p6PsnZqf2.png)

`select * from users`
`select username,password form users`

## insert into语句
![image.png](https://s2.loli.net/2023/12/19/jgAlzGQoMt1uJyT.png)
`insert into users(username,password) values('tony',098123)`

## update语句
![image.png](https://s2.loli.net/2023/12/19/8QVgIu4BYoCEPA5.png)
`update users set password = '8888888' where id = 4`
`update users set password = 'admin123',status = 1  where id = 2`
## delete语句
![image.png](https://s2.loli.net/2023/12/19/qgK2cdVyfFzA9E6.png)
`delete from users where id = 4`
## where子句
![image.png](https://s2.loli.net/2023/12/19/HuG1ZWoFdg8r9cf.png)
运算符包括以下
![image.png](https://s2.loli.net/2023/12/19/ZmBEeGhzqo6AISK.png)
示例
![image.png](https://s2.loli.net/2023/12/19/eFZYImvQDrTn12i.png)
## and 和 or 语句
and示例
`select * from users where status = 0 and id > 3`
or示例
`select * from users where status = 1 or username = 'zs' `
## order by
![image.png](https://s2.loli.net/2023/12/19/WFcKZ9qbulBfv2r.png)

`select * from users order by status` 升序排序
`select * from users order by DESC` 降序排序
![image.png](https://s2.loli.net/2023/12/19/LwYJfUjVmFdzZ49.png)
`select * from users order by status DESC , username asc`
## count( * )

![image.png](https://s2.loli.net/2023/12/19/BStLnHT3zNW8If1.png)
`select count(*) from users where status = 0`
## as别名
![image.png](https://s2.loli.net/2023/12/19/Pt6ViY7ZSTMkjNm.png)
`select count(*) as total from users where status = 0`
`select password as uname , password as upwd from users `
# 在项目中操作数据库
## 步骤
1. 示意图
![image.png](https://s2.loli.net/2023/12/19/qZrsRFfYdeAQUNo.png)
2. 安装mysql 和配置
![image.png](https://s2.loli.net/2023/12/19/ixudcnOsL7mfb3V.png)
## 测试
![image.png](https://s2.loli.net/2023/12/19/uMGqBDCJy89zOKE.png)
## 查询数据
![image.png](https://s2.loli.net/2023/12/19/QHjeEk5tXvoaOwK.png)
## 插入数据
![image.png](https://s2.loli.net/2023/12/19/JPWv98Z3o2hVnXg.png)
便捷方式
![image.png](https://s2.loli.net/2023/12/19/BmKuyhPQrCc4V6E.png)
## 更新数据
![image.png](https://s2.loli.net/2023/12/19/gciJHleZbvmVknR.png)
编辑方式
![image.png](https://s2.loli.net/2023/12/19/mL8FIj9WsJ3BXUK.png)
## 删除数据
![image.png](https://s2.loli.net/2023/12/19/2qbkTaCyX4FYotn.png)
## 标记删除
![image.png](https://s2.loli.net/2023/12/19/CR4uqBl28Sod1Dk.png)
