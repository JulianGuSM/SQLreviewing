# SQLreviewing

## T-SQL语句类型
1. DDL（Data Defintion Language）数据定义语言  
    DDL用于定义和管理数据库及数据库对象，包括CREATE,ALTER,DROP<br>
    1.1 创建数据库<br>
    ```SQL
    CREATE DATABASE SchoolDB
    ```
    1.2 创建表<br>
    ```SQL
    USE SchoolDB
    GO
    CREATE TABLE Tstudent
    ( StudentID varchar(10)PRIMARY KEY NOT NULL,
      Sname varchar(10),
      Sex char(2),
      Birthday datetime,
      Email varchar(40),
      Class varchar(20)
    )
    ```
    1.3  修改表-添加Age字段<br>
    ```SQL
    ALTER TABLE Tstudent ADD Age varchar(4)
    ```
    1.4 修改表-删除Age列<br>
    ```SQL
    ALTER TABLE Tstudent DROP COLUMN Age
    ```
    1.5 删除数据库或数据库对象
    ```sql
    DROP TABLE Tstudent
    ```
2. DCL（Data Control Language）数据控制语言  
    DCL语句用来控制对数据库和数据库对象的访问，主要是权限控制，包括GRANT,DENY,REVOKE<br>
    - 这三种语句授权的状态为：  
        + GRANT:授予权限
        + DENY:拒绝权限(优先权高于GRANT)
        + REVOKE:消除GRANT,DENY的影响，移除权限
      
    2.1 对public经行授权  
    ```sql
    GRANT ALTER ON Tstudent TO public
    DENY DELETE ON Tstudent TO public
    ```
    2.2 使用REVOKE回收ALTER权限和拒绝DELETE权限
    ```sql
    REVOKE ALTER ON Tstudent TO public
    REVOKE DELETE ON Tstudent TO public
    ```
3. DML（Data Manipulation Language）数据操纵语言  
    DML用于检索和删除表或者视图等对象的数据，包括对数据的INSERT，DELETE，UPDATE,SELECT
    <br>
    1. INSERT 插入行(关键字：INSERT、INTO、VALUES)
    ```sql
    INSERT INTO Tstudent(StudentID,Sname,Sex,Birthday,Email,Class)
    VALUES('0000000001','顾善明','男','1999-08-08','gu@163.com','计算机科学')
    ```
    上面的语句是标准的ANSI标准语句，每次只能插入一行,若要向表中所有字段都插入数据，则可以
    省略Colum_list，下面的语句可以达到同样的效果
    ```sql
    INSERT INTO Tstudent VALUES('0000000001','顾善明','男','1999-08-08','gu@163.com','计算机科学')
    ```
