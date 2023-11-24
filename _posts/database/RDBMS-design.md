---
title:  "RDBMS Design"
date:   2022-6-17 -0500
author: Pei Tian
categories: database 
tags: SQL RDBMS
header:
    teaser: /assets/img/training-guidence.png
---

### **基本概念**

### **数据模型三要素**

- 数据结构
- 数据操作
    - 关系代数
    - 关系演算
    - SQL
        - DDL，Definition
        - DML，Manipulation
        - DCL，Control
- 完整性约束
    - 实体完整性 Entity Integrity
    - 参照完整性 Referential Integrity
    - 用户自定义的完整性

### **关系模型的基本术语**

- 关系 Relation：二维表实体间联系
- 属性 Attribute：表的一列
- 域 Domain：属性的取值范围
- 元组 Tuple：表的一行
- 度or目 Degree：一个关系中属性的个数
- 基数 Cardinality：一个关系中元组的个数
- 超键 Super Key：关系中唯一标识元组的属性or属性集合，不一定最小
- 候选键 Candidate Key：关系中唯一标识元组的**最小属性集**
    
    唯一性、不可缩减性
    
- 主键 Primary Key
- 外键 Foreign Key：关系表某属性是另一关系表的主键

三种类型的表：基本关系、查询表、视图表（虚表）

### **关系代数运算符**

象集：Zx = \{t[Z]|t\in R, t[X] = x\}, R(X,Z)

1. 集合运算符
    
    \cup 并集、\cap 交集、- 差、\times 笛卡尔积
    
2. 专门的关系运算
    
    \sigma_F(R) = \{t|t\in R~\cap~F(t) = true\}，选择，F为选择条件
    
    \pi_A(R) = \{t[A]|t\in R\}，投影，A为R中属性列
    
    \bowtie：\theta连接（包含等值连接）、自然连接（等值连接去掉重复属性列）
    
    \rho_S(R)：关系R改名为S
    
    R\div S=\{t_r[X]|t_r\in R \cap \pi_y(S)\subseteq Y_X\}, R(X,Y),S(Y,Z)
    
3. 算术比较符
    
    <> 不等于
    
4. 逻辑运算符
    
    \land、\lor、\neg
    

### **数据库体系结构**

- 外模式：数据库用户能够看见和使用的局部数据的逻辑结构和特征
- 模式：数据库中全部数据的逻辑结构和特征
- 内模式：对数据物理结构和存储方式

> 外模式/模式映射：数据逻辑独立性
> 
> 
> 外模式和模式之间的对应关系。当模式改变时(如增加新的关系、新的属性、改变属性的数据类型等)，有数据库管理员对各个外模式/模式映像做出相应的改变，可以使外模式保持不变。应用程序是根据外模式编写的，所以应用程序不需要改变，从而保证了数据与程序的逻辑独立性，简称逻辑数据独立性。
> 
> 模式/内模式映射：数据物理独立性
> 
> 数据库全局逻辑结构与存储结构之间的对应关系。当数据库的存储结构改变了(如选用了另一种存储结构)，由数据库管理员对模式/内模式映像做出相应的改变，可以使模式保持不变。从而保证了数据与程序的物理独立性，简称物理数据独立性。
> 

三级模式、两级映射

---

### **设计数据库**

### **概念模式**

ER图，Entity-Relationship

数据联系：

- 1:1
- 1:N
- M:N

### **逻辑模式**

ER图转关系模式、规范化处理、`CREATE TABLE`

范式：

- 1NF：单独不可拆分的属性
- 2NF：主键与非主键属性间完全依赖
- 3NF：主键与非主键属性间无传递依赖
- BCNF：满足三范式

### **物理模式**

数据库系统实现

---

### **SQL进阶**

### **Transact-SQL**

1. 局部变量
    
    ```sql
     DECLARE @score INT, @name VARCHAR(20) #
     SET @score = 88
     SELECT @score = 90, @name = 'user'
    ```
    
2. 全局变量：SQL Server系统
    
    ```sql
     SELECT @@TOTAL_READ AS 'Reads'
    ```
    
3. 运算符
    
    ```sql
     -- Date
     SELECT STR(YEAR(GETDATE()))
     -- Other DB
     OPENDATASOURCE('SQLOLEDB', 'DataSource=(local); Integrated Security = SSPI;').university.dbo.student
     -- Excel
     OPENDATASOURCE('Microsoft.ACE.OLEDB.12.0', 'Data Source="..."; User ID=Admin; Password=; Extended properties=Excel 12.0')...[Sheet2$]
    ```
    
4. 自定义函数
    - 标量函数
        
        ```sql
         -- Defination
         CREATE FUNCTION get_avg_score(@snum CHAR(4))
         RETURNS INT
         AS
         BEGIN
           DECLARE @temp INT
           SELECT @temp = AVG(score) FROM sc WHERE snum = @snum
           RETURN @temp
         END
        
         -- Operation
         SELECT dbo.get_avg_score('s001')
        ```
        
    - 内嵌表值函数
        
        ```sql
         -- Defination
         CREATE FUNCTION get_avg_ge(@score INT)
         RETURNS table
         RETURN
         BEGIN
           SELECT snum, AVG(score) AS average FROM sc
           GROUP BY snum
           HAVING AVG(score) >= @score
         END
         
         -- Operation
         SELECT * FROM get_avg_ge(80)
        ```
        
    - 多语句表值函数
        
        ```sql
         -- Defination
         CREATE FUNCTION get_sc()
         RETURNS @sc
         TABLE(
           snum CHAR(4),
             cnum CHAR(3),
             score INT
         )
         AS
         BEGIN
           INSERT INTO @sc
           SELECT snum, cnum, score
           FROM sc, sections
           WHERE sc.secnum = sections.secnum
           RETURN
         END
         
         -- Operation
         SELECT * FROM get_sc()
        ```
        
    - 函数操作
        
        ```sql
         DROP FUNCTION dbo.get_sc  # 删除
         ALTER FUNCTION dbo.get_sc  # 修改
        ```
        
5. 流程控制
    
    ```sql
     -- IF ELSE
     IF () ***; ELSE ***;
     
     -- WHILE
     WHILE () ***; BREAK; COUTINUE;
     
     -- 等待
     WAITFOR TIME '09:00:00'
     WAITFOR DELAY '00:30:00'
     
     -- CASE
     SELECT RANK =
       CASE score
         WHEN 7 THEN 'GOOD'
         WHEN 10 THEN 'PERFECT'
         ELSE 'SORRY'
     -- SELECT条件计数
     SUM(CASE WHEN s > 90 1 WHEN s < 80 2 ELSE 0)
    
     -- 批处理
     ---------
     GO # 以上批处理结束
    ```
    

### **常用数据库对象**

1. 视图 View
    
    ```sql
    CREATE VIEW V(id, name, desp)
    AS
    	/* ... */
    ```
    
    ❗️❗️ 视图DML操作只能影响一个基表！
    
2. 存储过程 Stored Procedure
    
    特点：减少网络流量、代码复用、加快系统运行速度、提高数据安全性
    
    分类：
    
    - 系统存储过程
    - 用户自定义
    - 扩展存储过程
    - 远程存储过程
    
    ```sql
    -- No Params
    CREATE PROC proc1
    AS
    	SELECT snum, AVG(score) FROM sc GROUP BY snum
    -- Running Test
    EXEC proc1
    
    -- Input Params
    CREATE PROC proc2
    @snum CHAR(4)
    AS
    	SELECT snum, AVG(score) FROM sc
    	WHERE snum = @snum
    -- Running Test
    DECLARE @temp CHAR(4)
    SET @temp = 's002'
    EXEC proc2 @temp
    
    -- Input & Output Params
    CREATE PROC proc3
    @snum CHAR(4),
    @avg INT OUTPUT
    AS
    	SELECT @avg = AVG(score) FROM sc WHERE snum = @snum
    -- Running Test
    DECLARE @temp CHAR(4), @avg_out INT
    SET @temp = 's001'
    EXEC proc3 @temp, @avg_out OUT
    PRINT 's001''s Average Score: ' + CAST(@avg_out AS CHAR(4))
    
    -- Return Status
    CREATE PROC sc_proc4
    @snum CHAR(10)=NULL
    AS
     	IF @snum is NULL
         	RETURN 15
     	ELSE
       		IF EXISTS (SELECT * FROM sc WHERE snum=@snum AND score<60)
             	RETURN 5
       		ELSE
       		BEGIN
                 SELECT *
                 FROM sc
                 WHERE snum=@snum
                 RETURN 0
             END
    -- Running Test
    DECLARE @return_status INT
    EXEC @return_status=sc_proc4 's001'
    IF @return_status=15
    	PRINT '缺少输入参数！'
    IF @return_status=5
       	PRINT '该生有不及格的记录！'
    ```
    
    源码存放：`syscomments`
    
    登记：`sysobjects`
    
    查看自定义存储过程：`sp_helptext proc`
    
    系统存储过程：如`sp_denylogin`
    
3. 触发器 Trigger
    
    Code：
    
    ```sql
    CREATE TRIGGER TG1
    ON [table]|[view]
    FOR|AFTER|INSTEAD OF [INSERT][,][DELETE][,][UPDATE]
    AS
    	/* ... */
    	-- 回滚机制
    	ROLLBACK
    	-- 确认修改
    	COMMIT
    ```
    
    作用：
    
    - 强化约束 Enforce Restriction
    - 跟踪变化 Auditing Changes
    - 级联运行 Cascaded Operation
    - 调用存储过程 Stored Procedure Invocation
    
    构成要素：事件、条件、动作
    
4. 规则 Rule

---

### **管理数据库**

### **数据查询**

关系代数、SQL语言

### **数据库保护技术**

- 完整性控制
    - 实体完整性
    - 参照完整性
    - 域完整性
    - 用户自定义完整性
    
    > CREATE TABLE, Trigger, Rule
    > 
- 安全性控制
    - 用户登录密码
    - 视图：只能更新一张基表有关
    - 存取控制
    - 角色
- 并发控制 [Blog](https://blog.csdn.net/liyunyou/article/details/82806915?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4-82806915-blog-109963032.pc_relevant_antiscanv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4-82806915-blog-109963032.pc_relevant_antiscanv3&utm_relevant_index=6)
    - 三种异常状态：丢失数据、不可重复读、读脏数据
    - 事务(transaction)的ACID性：原子性(atomicity，或称不可分割性)、一致性(consistency)、隔离性(isolation，又称独立性)、持久性(durability)
        - Atomicity：一个事务中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被恢复(Rollback)到事务开始前的状态，就像这个事务从来没有执行过一样。
        - Consistency：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
        - Isolation：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交(Read uncommitted)、读提交(read committed)、可重复读(repeatable read)和串行化(Serializable)。
        - Durability：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。
    - 锁机制
    - 2PL，两段锁协议：所有事务必须分成两个阶段对数据项加锁和解锁
        
        第一阶段是获得封锁，也称为扩展阶段；第二阶段是释放阶段，也称为收缩阶段
        
- 数据库维护
    
    备份
    
- 常用技术
    - 存储过程：无参数、输入参数、输出参数
    - 触发器：insert、delete、update、instead of
        
        `COMMIT`, `ROLLBACK` != `RETURN`
        

### **访问数据库 ADO.NET**

`SqlCommand` 显式连接, `SqlDataAdaptor` 隐式连接