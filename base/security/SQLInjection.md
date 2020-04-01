# SQL 注入

SQL 注入攻击是黑客对数据库进行攻击的常用手段之一。随着B/S模式应用开发的发展，使用这种模式编写应用程序的程序员也越来越多。但是由于程序员的水平及经验也参差不齐，相当大一部分程序员在编写代码的时候，没有对用户输入数据的合法性进行判断，使应用程序存在安全隐患。用户可以提交一段数据库查询代码，根据程序返回的结果，获得某些他想得知的数据，这就是所谓的SQL Injection，即SQL注入。

例如：

```sql
// 某个网站的登录验证的SQL查询代码为：
strSQL = "SELECT * FROM users WHERE (name = '" + userName + "') and (pw = '"+ passWord +"');" 
// 恶意填入
userName = "1' OR '1'='1";
// 与
passWord = "1' OR '1'='1";
// 时，将导致原本的SQL字符串被填为
strSQL = "SELECT * FROM users WHERE (name = '1' OR '1'='1') and (pw = '1' OR '1'='1');"
也就是实际上运行的SQL命令会变成下面这样的
strSQL = "SELECT * FROM users;"
// 因此达到无账号密码，亦可登录网站。所以SQL注入攻击被俗称为黑客的填空游戏。
```