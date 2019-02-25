# SQLreviewing

## T-SQL语句类型
1. DDL（Data Defintion Language）数据定义语言  
    DDL用于定义和管理数据库及数据库对象，包括CREATE,ALTER,DROP
     ****
    1.1 创建数据库<br>
    ```SQL
    CREATE DATABASE SchoolDB
    ```
      ****
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
      ****
    1.3  修改表-添加Age字段<br>
    ```SQL
    ALTER TABLE Tstudent ADD Age varchar(4)
    ```
      ****
    1.4 修改表-删除Age列<br>
    ```SQL
    ALTER TABLE Tstudent DROP COLUMN Age
    ```
     ****
    1.5 删除数据库或数据库对象
    ```sql
    DROP TABLE Tstudent
    ```
     ****
2. DCL（Data Control Language）数据控制语言  
    DCL语句用来控制对数据库和数据库对象的访问，主要是权限控制，包括GRANT,DENY,REVOKE<br>
    - 这三种语句授权的状态为：  
        + GRANT:授予权限
        + DENY:拒绝权限(优先权高于GRANT)
        + REVOKE:消除GRANT,DENY的影响，移除权限
     ****
    2.1 对public经行授权  
    ```sql
    GRANT ALTER ON Tstudent TO public
    DENY DELETE ON Tstudent TO public
    ```
     ****
    2.2 使用REVOKE回收ALTER权限和拒绝DELETE权限
    ```sql
    REVOKE ALTER ON Tstudent TO public
    REVOKE DELETE ON Tstudent TO public
    ```
      ****
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
     ****
    2. 使用INSERT语句插入多行数据
     ```sql
      INSERT INTO Tstudent VALUES
      ('0000000001','顾善明','男','1999-08-08','gu@163.com','计算机科学'),
      ('0000000002','顾善明','男','1999-08-08','gu@163.com','计算机科学'),
      ('0000000003','顾善明','男','1999-08-08','gu@163.com','计算机科学'),
     ```
      ****
    3. 使用UPDATE更新数据
    ```sql
    UPDATE Tstudent SET Sname='JulianGu',Sex='女' WHERE StudentID='0000000001'
    ```
      不写WHERE将更新整张表
      ****
    4. SELECT 查询数据
    ```sql
    SELECT * FROM Tstudent
    ```
    查询指定字段，关键字AS表示为字段取一个别名
     ```sql
     SELECT StudentID AS 学号,Sname AS 姓名 FROM Tstudent
     ```
      ****
    5. DELETE 删除行
    ```sql
    DELETE Tstudent WHERE StudentID='000000001'
    ```
    不使用WHERE将删除整个表
     ****
##T-SQL语法要素
1. SQL语句的批处理符号GO
    GO必须独占一行
    ```sql
    USE SchoolDB
    GO --第一批
    CREATE TABLE TestBatch(ID varchar(3),Name varchar(8))
    GO --第二批
    INSERT INTO TestBatch VALUE('1','韩立刚') --语法错误
    GO --第三批，其余两个批处理正常执行，第三批INSERT语句不执行
    ``` 
     ****
2. EXEC
   执行系统存储过程sp_helpdb 查看SchoolDB数据库相关信息
    ```sql
    EXEC sq_helpdb SchoolDB
    ```
3. 注释符
    行注释符 “--”
    块注释符 “/*...*/”
4. 创建本地临时表#t和一个全局临时表##teacher
    ```sql
    CREATE TABLE #t(Tname nvarchar(10),Tage int)
    CREATE TABLE ##teacher(Tname nvarchar(10),Tage int)
    ```
##变量
变量分为局部变量和全局变量。局部变量名称开头必须是@，作用域仅限于一个批处理语句中
使用 **<font size="4" color="#DC143C" >DECLARE</font>** 定义局部变量，同时指定变量的数据类型，一次性定义多个变量时在每个
变量之间用“,”隔开
使用 **<font size="4" color="#DC143C" >SET</font>** 或 **<font size="4" color="#DC143C" >SELECT</font>** 进行赋值
 ****
1. 局部变量定义和赋值example
    ```sql
    DECLARE @address varchar(20)
    SET @address = 'KunShan,China'
    SELECT @address
    --下面是直接定义变量并赋值
    DECLARE @address = 'Kunshan,China'
    ```
    ****
     ```sql
     USE SchoolDB
     GO
     DECLARE @Sname nvarchar(11),@StudentID nvarchar(20)
     SET @StudentID = '000000001'
     SELECT @Sname = Sname FROM Tstudent WHERE StudentID=@StudentID
     SELECT @Sname
     ```
    上面的语句中，定义了两个局部变量@Snmae,@StudentID，首先为@StudentID赋值为'000000001'
     然后将赋值后的@StudentID传给SELECT语句中的StudentID，查出@StudentID为000000001的Sname
     然后将查询出的Sname值赋值给@Sname,最后输出@Sname的值
    ****
     > 使用 **<font size="4" color="#DC143C" >SET</font>** 为变量赋值，每次只能为一个变量赋值。
     > 使用 **<font size="4" color="#DC143C" >SELECT</font>** 为变量赋值，则每次可以为多个变量赋值
    ****
      ```sql
      DECLARE @Sname nvarchar(11),@StudentID nvarchar(20)
      SELECT @Sname = '潘兆丽',@StudentID = '0000000002' --一次赋多个变量
      SELECT @Sname,@StudentID
      ```
    ****
2. 全局变量
    > 全局变量以@@开头
## 数据类型
1. 字符串类型
   字符串类型用于存储汉字、字母、数字符号和其他符号，包括varchar,char和text等。
      <br>
   1. **<font size="3" color="##DA70D6" >char</font>** (定长字符串)
   > 定长形式为char[(n)]，n的取值范围1~8000，不写 n 默认n=1，长度超过n时将被截断，不足    n时将被空格填充
   2. **<font size="3" color="##DA70D6" >varchar</font>** (变长字符串)
    > 结构与 **<font size="3" color="##DA70D6" >char</font>** 结构一样，区别在于长度小于 n 时，不足 n 的部分不被空格填充，占用的字节长度等于输入长度
   3.  **<font size="3" color="##DA70D6" >text</font>** 
    >  **<font size="3" color="##DA70D6" >text</font>** 数据类型用于存储数据量非常庞大的字符文本数据，**<font size="3" color="##DA70D6" >char</font>**  ，**<font size="3" color="##DA70D6" >varchar</font>** 最多可存储8000字节数据，超过时可以用 **<font size="3" color="##DA70D6" >text</font>** 类型。该类型占用字节长度可变
2. 日期时间类型
   -  datatime
    > 默认显示格式YYYY-MM-DD hh:mm:ss:n* 
    n* 代表 0~000之间的3位数
   - date
    > 存储日期不存储时间，占用3个字节，默认显示格式YYYY-MM-DD
   - time
    > 存储一个24小时制的时间，占用3~5个字节，默认格式为hh:mm:ss:n*。 n*默认位数为7位
3. 数值类型
   
