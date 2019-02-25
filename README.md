# SQLreviewing

## T-SQL语句类型
1. DDL（Data Defintion Language）数据定义语言
DDL用于定义和管理数据库及数据库对象，包括CREATE,ALTER,DROP<br>
1.1-1 创建数据库<br>
    ```SQL
    CREATE DATABASE SchoolDB
    ```
1.1-2 创建表<br>
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
1.1-3 修改表-添加Age字段<br>
    ```SQL
    ALTER TABLE Tstudent ADD Age varchar(4)
    ```
1.1-4 修改表-删除Age列<br>
    ```
    ALTER TABLE Tstudent DROP COLUMN Age
    ```

2. DCL（Data Control Language）数据控制语言
3. DML（Data Manipulation Language）数据操纵语言
