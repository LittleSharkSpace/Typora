## 数据库概念及使用

> 数据库：存放数据的仓库；表：数据的结构化清单

### MYSQL基本命令

```mysql
# 1. 查看库
show databases;
# 2. 创建库
create database 数据库名;
# 3. 删除库
drop database 数据库名;
# 4. 选择库
use 数据库名;
# 5. 查看表
show tables;
# 6. 查看数据库创建语句
show create database 数据库名;
# 7. 查看选中的数据库
select database();
# 8. 修改数据库字符集
alter database 数据库名 default charset=utf8;
```

 ### 数据类型

#### 整数型

![](https://z3.ax1x.com/2021/10/15/53RPvd.png)

#### 字符型

![](https://z3.ax1x.com/2021/10/15/53RZUf.png)

#### 时间型

![](https://z3.ax1x.com/2021/10/15/53Re58.png)

### 数据库使用

```mysql
数据定义语言DDL （create、drop）
数据操作语言DML（insert、delete、update）
数据查询语言DQL（select、where、group by、order by 、limit）
数据控制语言DCL（grant、revoke）
事务处理语言TPL（commit、rollback）
```

#### show命令

```mysql
show create database 数据库名;	# 查看数据库创建语句
show create table 表名;		 # 查看数据表创建语句
show errors;				  # 显示服务器错误
show warnings;				  # 显示告警信息
show grants;				  # 显示用户权限
```

#### 数据表操作

```mysql
# 创建表
create table student(
	id int auto_increment,
    name varchar(64) not null,
    age int,
    gender varchar(4),
    primary key(id)
)engine=InnoDB default charset=utf8;
# 创建成功后可通过show create table student查看，会自动补全为规范格式

# 查看表结构
desc student;

# 删除表
create table test(id int auto_increment, content varchar(64), primary key(id));
drop table test;

# 修改表
## 1. 修改字段类型 ##
alter table student modify age varchar(64) not null;
desc student;	# 查看表结构
alter table student modify age int;
## 2. 增加字段 ##
alter table student add score float;
## 3. 修改字段名和类型 ##
alter table student change score level varchar(64);
## 4. 删除字段 ##
alter table student drop level;
## 5. 修改表名 ##
alter table student rename students;
## 6. 添加字段并指定字段位置 ##
alter table students add score float after name;

# 插入数据
## 方式一 ##
insert into students (id, name, score, age, gender) value (1, 'cy', 80, 26, '男');
## 方式二 ##
insert into students values(2, 'zqk', 88, 27, '男')
## 方式三 ##
insert into students (id, name, score, age, gender) values
(3, 'job', 99, 26, '男'),
(4, 'jack', 88, 26, '女'),
(5, 'xk', 77, 26, '女');
```

## 数据操作

### 数据检索

```mysql
# 基础查询
select 字段名列表 from 表名;

# 限制查询
select 字段名列表 from 表名 limit n;		    # 取n条数据
select 字段名列表 from 表名 limit n,m;		    # 从第n个开始取m个
select 字段名列表 from 表名 limit n offset m;  # 从第m个开始取n个

# 条件查询
select 字段名列表 from 表名 where 条件;
	关系运算符：> >= <= = != <> between...and
	逻辑运算符: and or not 
	集合运算符：in 、not int
	判空运算：is null、 is not null
	通配符：_  %
	
select id,name, from students where id between 10 and 20;	# 包头包尾
select id,name, from students where name like '孙_';
select id,name, from students where name like '孙%';

# 排序查询
select 字段名列表 form 表名 order by 字段 顺序;
asc 升序(默认)、desc降序

select * from students where id between 10 and 20 order by score desc;

# 集合函数
count() sum() avg() max() min()

select count(*) from students;	# 统计时包含空
select count(1) from students;	# 统计时包含空

# 分组查询
select 字段列表名(包含集合函数) from 表名 group by 字段;
select 字段列表名(包含集合函数) from 表名 group by 字段 having 条件; 	# where作用于全表，having作用于分组

select gender '性别' count(*) '性别计数' from students group by gender having gender='男';
select gender '性别' count(*) '性别计数' from students group by gender having count(gender) > 0;


# 小结
select 字段 from 表名 [where 条件] [group by] [having条件] [order by] [limit]

select gender '性别' count(*) '性别计数' from students where gender is not null group by gender order by count(*);
```

### 数据修改

```mysql
update 表名 set 字段1=值1,字段2=值2,... where 条件
# 更新时不加where条件会修改所有记录，为防止该情况发生可在用户登录时设置--safe-updates
```

### 数据删除

```mysql
delete from 表名 where 条件;		# 如果不加条件，会删除表中所有数据
truncate table 表名;				 # 清空表中所有记录，等价于delete from 表名;
```

## 用户管理

### 权限管理

全局性的管理权限：作用于整个MYSQL 实例级别；

数据库级别的权限：作用于某个指定的数据库上或者所有数据库上；

数据库对象级别的权限：作用于指定的数据库对象上(表、视图等)或者所有的数据库对象上。

user表：

### 用户管理





