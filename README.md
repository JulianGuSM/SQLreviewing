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
## T-SQL语法要素
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
## 变量
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
4. 运算符
    - ALL
  如果都为TRUE，那么就为TRUE
  列如，4>ALL(3,2,1)的结果是true，4>ALL（3，2，5）的结果是false
    - AND
  如果两个表达式都为TRUE，那么就是TRUE。
  例如，5>4 AND 5>3的结果是TRUE； 5>3 AND 5>7的结果为false
    - ANY、SOME
  如果比较中任何一个为TRUE，那么就为TRUE
  例如5>ANY(3,6,8)的结果为TRUE，因为5大于3为TRUE
    - BETWEEN
  如果值在某个范围之内，那么就为TRUE
  例如，6 BETWEEN 3 AND 9结果为TRUE，6 BETWEEN 3 AND 5结果为FALSE
    - EXISTS
  如果子查询包含一些行，那么就为TRUE
    - IN
  如果值等于表达式列表里的一个，那么就为TRUE。
  例如，3 IN （2，3，8）的结果为TRUE， A IN (A,B,C)结果为TRUE,3 IN (2,4,5)结果为FALSE
    - LIKE
  如果值与一种模式相匹配，那么结果就是TRUE
  在like中会使用“ **_**”, “ **%**”等使用通配符，“ **_**” 代表一个未知字符，“ **%**”代表任意字符和字符串
  ，例如对LIKE '_立刚'来说，如果是“a立刚”，“韩立刚”，“张立刚”结果都是 TRUE
    - NOT
  对返回结果取反
    - OR
  相当于 ||
  5. 聚合函数
   COUNT(), AVG(), SUM(), MIN(), MAX()
  6. 数值函数
       1. ROUND(value)
   对值进行四舍五入
       2. FLOOR(value)
   地板函数，取小于或等于value的最大整数
       3. CEILING(value)
   天花板函数，取大于或等于value的最小整数
       4. RAND
   取RAND的双边整数值
      ```sql
      SELECT CEILING(RAND()*100 - 1) AS 法一
              FLOOR(RAND()*101) AS 法二
      ```
  7. 字符串函数
       1. 使用LOWER()和UPPER()改变字符串大小写
      ```sql
      SELECT LOWER('ABcde'),UPPER('AbcdE')
      ```
       2. 使用LEN()获取字符串长度
      ```sql
      SELECT LEN('ADBCD 韩立刚')
      ```
       3. 使用LEFT(),RIGHT()和SUBSTRING()截取字符串
   LEFT(string，n)从左边开始截取n个字符
   RIGHT(string ， n)是从右边开始截取n个字符
   SUBSTRING(string，start，length)将对string从start位置开始截取length个字符，start表示开始截取的位置，length表示截取的长度 
       4. LTRIM()删除左端空格，RTRIM()删除右端空格
       5. CHARINDEX()查找字符串 
       6. REPLACE(string1，string2，string3)表示用string1中的字符串string2替换为string3，如果string2不是string1中的字符串则不能替换
       7. STUFF(string1，start，length，string2)表示从字符串string1的start位置开始删除length个字符，然后在删除的位置插入string2
       8. SPACE(n)返回指定数量的空格
       9. REPLICATE(string，n)重复字符串，n是重复次数
  8. 日期时间函数
       1. 使用GETDATE(),CURRENT_TIMESTAMP函数获取当前日期和时间
       2. 使用YEAR(),MONTH(),DAY()获取给定时间的年月日
       3. 使用DATEADD()获取加上指定日期时间后的新日期时间
      ```SQL
      SELECT DATEADD(yy,2,GETDATE()) --加两年
      SELECT DATEADD(qq,2,GETDATE()) --加两季度
      SELECT DATEADD(mm,2,GETDATE()) --加两月
      SELECT DATEADD(wk,2,GETDATE()) --加两周
      SELECT DATEADD(dy,2,GETDATE()) --加两日
      SELECT DATEADD(dd,2,GETDATE()) --加两日
      SELECT DATEADD(dw,2,GETDATE()) --加两日
      -- number 也可以是负数，表示减去指定时间
      ```
       4. 使用DATEDIFF(datetype,startdate,enddate)获取时间差
       5. 使用DATENAME(),DATEPART()得到给定日期的指定部分和指定部分的整数值
 9. 数据类型转换函数
 10. 控制NULL的常用函数
 11. 条件判断语句IF...ELSE和CASE
 12. 循环语句
## 查询语句
1. 简单查询逻辑处理过程
    ```sql
    SELECT Class，COUNT(*) AS 人数
    FROM dbo.Tstudent
    WHERE Class IN ('网络班','开发班')
    GROUP BY Class
    ORDER BY 人数
    ```
     ****
    **第一阶段：使用FROM确定输入表**
      该步骤先识别被查询的表，如果指定了表操作符,则还要按条件处理这些操作符。
      上面的语句只对一张表进行操作，在语句执行的开始，确定Tstudent表是将要进行操作的表，
      并将Tstudent的所有行输出到虚拟表1中，作为下一阶段的输入表
    **第二阶段：使用WHERE筛选数据**
       在该阶段，将对虚拟表1的所有行使用WHERE筛选器，只有符合Class为‘网络班’和‘开发班’条件的行才会放入到虚拟表2
    **第三阶段：进行数据分组**
       在该阶段，根据GROUP BY子句中指定的分组列Class对虚拟表2进行分组，开发班分为一个组，网络班分为一个组
       得到虚拟表3
    **第四阶段：使用SELECT列表筛选列**
       从虚拟表3中筛选SELECT后的列Class，并得到一个计算列COUNT（*）统计按班级分组后的班级人数，取两列的结果集作为虚拟表4
    **第五阶段：使用ORDER BY子句排序查询结果**
       本例中对别名“人数”代表计算列COUNT（*）进行默认的升序排序

2. 使用WHERE筛选行
   当WHERE子句中使用了多个逻辑运算符时，计算顺序依次为：NOT，AND，OR
3. 使用IN指定条件列表，列表中的值以“ ， ”隔开
4. 使用 **LIKE** 关键字进行模糊匹配
  > % 代表任意字符串
  > _ 代表单个任意字符
  > [] 指定范围（如[韩，马，郭]）内的任意单个字符
  [^] 不在指定范围内的任意单个字符
   ****
   使用%查询“许”姓学生
   ```sql
   SELECT * FROM Tstudent WHERE Sname LIKE '许%'
   ```
   查询姓名中第二个字为“冰”的学生
  ```sql
    SELECT * FROM Tstudent WHERE Sname LIKE '_冰%'
  ```
  查询姓名中含有“许”“发”“冰”的学生
 ```sql
 SELECT * FROM Tstudent WHERE Sname LIKE '%[许，发，冰]%'
 ```
 使用NOT求反，查询不姓许的学生
  ```sql
  SELECT * FROM Tstudent WHERE Sname NOT LIKE '许%'
  ```
5. 使用别名
   1. 定义表别名后，在语句中对该表的引用必须使用别名，而不能使用原表名
   2. 引用别名时注意查询的逻辑处理过程
6. 使用ORDER BY子句对结果排序
   默认升序（ASC），降序（DESC）
   1. 按单列排序
      ```SQL
      SELECT StudentID，subJectID， mark FROM Tscore ORDER BY mark
      ```
      指定列别名进行排序
      ```SQL
      SELECT StudentID，subJectID， mark AS 分数 FROM Tscore ORDER BY 分数
      ```
      使用数字3代表mark
      ```SQL
      SELECT StudentID，subJectID， mark FROM Tscore ORDER BY 3
      ```
   2. 按多列排序
      按多列排序时，列之间用 “，” 隔开。当指定的第一个排序列值相同时，将对相同列值的行按照第二个指定列进行排序，对指定多个排序列以此类推
      下面的语句指定先按mark排序，对mark相等的行按StudentID排序
      ```SQL
      SELECT StudentID，subJectID， mark FROM Tscore ORDER BY mark,StudentID
      ```
      若在指定多列排序时指定排序方式，则需要在每个排序列后指定ASC或DESC
      ```SQL
      SELECT StudentID，subJectID， mark FROM Tscore ORDER BY mark DESC,StudentID
      ```
   3. 使用选择列之外的列排序
      ORDER BY子句可以包括未出现在SELECT选择列表中的列。
      例如下面的语句只查询StudentID和subJectID列，但仍按mark升序排序 
      ```SQL
      SELECT StudentID，subJectID FROM Tscore ORDER BY mark
      ```
      但是如果指定了SELECT DISTINCT或者包含GROUP BY 子句，则排序列必须包含在选择列表中
      ```SQL
      SELECT DISTINCT StudentID，subJectID FROM Tscore ORDER BY mark --报错
      SELECT StudentID，SUM(mark) FROM Tscore GROUP BY StudentID ORDER BY mark --报错
      ```
7. 使用TOP限制结果集
   1. TOP 不与 ORDER BY一起使用返回的数据行时未排序状态下的数据
   2. TOP 与 ORDER BY一起使用
8. 使用DISTINCT消除重复行
## 多表联接查询和数据汇总
1. 联接基础知识
   创造多表查询的环境
    ```sql
    CREATE DATABASE joindb
    GO
    USE joindb 
    GO
    CREATE TABLE Student ( StudentID int, Sname nvarchar ( 10 ), Sex nchar ( 1 ) )
    INSERT Student VALUES(1,'韩立刚','男'),(2,'王景正','男'),(3,'郭淑丽','女'),(4,'韩旭','女'),(5,'孟晓飞','男');
    CREATE TABLE Score (StudentID int,Subjectname nvarchar(20), Mark DECIMAL)
    INSERT Score VALUES (1,'英语',89),(1,'数学',59),(2,'英语',79),(2,'数学',86),(3,'英语',57),(3,'数学',67),(6,'英语',88),(6,'数学',83);
    ```
2. 在FROM 子句中联接
   语法格式：FROM table_1 join_type table_2 [ON join_condition]
   将table_1和table_2通过联接条件join_condition联接在一起，联接条件是许可的
   join_type分为 **交叉连接**、 **内连接**、 **外连接**
   不写联接条件默认交叉连接