SQL Injection | Forms

All programmers have read or at least heard about the methods to hack the website’s security. Or even faced this problem. A the same time, the creativity of people who want to break a website is endless, so all bottlenecks should be well secured. That’s why I would like to start a series of short articles where a bunch of basic methods and techniques of hacking websites will be presented.

In the first article I would like to describe and clarify some common methods of hacking one of the most vulnerable parts of a website – the forms. I will elaborate on how to use those methods and how to prevent attacks, as well as put some insight into basic security testing.

## SQL Injection

SQL injection is a technique where malicious users can inject SQL commands into an SQL statement via web page input. And this input may be quite different – the text field in form, _GET or _POST parameter, cookies etc.

This method was really effective before frameworks become so trendy in PHP world. But might be still dangerous if the application doesn’t use ORMs or other data objects extensions. Why? Because of the method of parameters’ binding to the SQL queries.

### Blind Injection

Let’s start with the basic example of SQL statement which will return one matching user by his login and hashed password on a login page.

##### Example 1.

|     |     |
| --- | --- |
| 1   | [mysql_query](http://www.php.net/mysql_query)(‘SELECT id, login FROM users WHERE login = ? and password = hash(?)’); |

I put the question marks by purpose, because this example might be resolved in a couple of different ways. First will be the worst in my opinion and the most vulnerable for SQL Injection.

##### Example 1a.

|     |     |
| --- | --- |
| 1<br>2 | [mysql_query](http://www.php.net/mysql_query)(‘SELECT id, login FROM users WHERE login = “‘  .  $login  .  ‘”<br>    and password = hash(“‘  .  $password  .  ‘”)’); |

In this case, the code doesn’t check the data against some threats or wrong characters. Values are passed from the login form, right into the database query. In optimistic scenario, the user will put there his login and the password. What will happen in the worst scenario? Let’s try to hack this form.

The form can be hacked by passing well prepared data. Our first attempt will login us as a first user from database. In most cases this will be an administrator account.

To do that, a special string should be passed into the login field:

|     |     |
| --- | --- |
| 1   | ” OR 1=1; – |

The first quote mark can be also a semi, so probably it will take more than one attempt to achieve success. The quote is ended by the semicolon and two dashes, because everything what will be executed after the final comparison should be commented.

In result, the SQL will execute:

|     |     |
| --- | --- |
| 1   | SELECT id, login FROM users WHERE login =  “;”  OR  1=1  LIMIT  0,1; *– and password = hash(“;Some password”)* |

This query will return the first user from database and probably log him into the application. A really smart move is to add the limit argument, because this enables us to login as every single user. The only thing needed to complete the task is to go through every offset value.

### More serious stuff

The previous example is not so scary as it seems to be. The options in administration panel are always limited or a lot of work is needed to block and crash the whole site. SQL Injection attack can cause more serious damage to the system.

Think, how many applications have been created with a main table called users and what will happen when an intruder will try to inject a code like this, to the unsecured form:

|     |     |
| --- | --- |
| 1   | My favorite login‘); DROP TABLE users; – |

The users table will be removed. This is one more reason to do database backups often.

### _GET parameters

All parameters from the form have to be passed to the server by one of those methods – POST or GET. Most popular name of the parameter passed by GET method is an id. This will be a very next place vulnerable for attacks. And it doesn’t matter if the id is passed in the url address like `http://example.com/users/?id=1 or http://example.com/users/1`.

What will happen when the following url will be passed to the code from first example?

|     |     |
| --- | --- |
| 1   | http://example.com/users/?id=1  AND  1=0  UNION  SELECT  1,concat(login,password),  3,4,5,6  FROM  users WHERE id =1; *–* |

This query will probably return the user’s login and… hashed password. First part of query AND 1=0 makes the primary segment false and this won’t return any records. The second piece of statement will return prepared data. As a first parameter it will get the id, next will be an user login with hashed password and a couple of more parameters (this number should be guessed).

There are plenty of applications which will decode the password by, in example, brute force method. Unfortunately, most users have the same password to different systems and if one of them can be broken then the password can be used in others.

Curiosity – this example is insusceptible to escaping strings by a method `mysql_real_escape_string or addslashes` etc. Basically, there’s nothing to escape from. So, if the parameters will be passed by:

|     |     |
| --- | --- |
| 1   | ‘SELECT id, login, email, param1 FROM users WHERE id = ‘  .  [addslashes](http://www.php.net/addslashes)($_GET[‘id’]);‘ |

then the attack will still cause troubles.

### Escaping strings

When I was a freshman in programming, character sets were really uncomfortable for me. I didn’t understand the difference between them and why I should use UTF-8, when UTF-16 and why my database always sets the encoding to latin1. When I finally got the point and understood that there might be a little bit of a problem to put every characters into one coding standard then I have found some security issues during text conversions.

Most of previous examples were closing the statement by a quote mark ‘ and continued with prepared data. An attempt of SQL injection attack having single quotes escaped with a backslash may be a bummer, if the system is using addslashes() function to bind parameters. However, the attack may success. All what is needed is to inject a character with a code 0xbf27, and addslashes() modifies this to become 0xbf5c27, a valid multibyte character followed by a single quote.

In other words, one character `뼧` will pass the addslashes function and then, in MySQL mapped by character set conversion into two characters – 0xbf (¿) and 0x27 (‘).

|     |     |
| --- | --- |
| 1<br>2 | “SELECT * FROM users WHERE login = ‘”;<br>    .  [addslashes](http://www.php.net/addslashes)($_GET[‘id’])  .  “;'”; |

This example can be hacked by passing `뼧 or 1=1 –` into login field in the form. In SQL engine the final query will look like that:

|     |     |
| --- | --- |
| 1   | SELECT  *  FROM users WHERE login =  ‘&iquest;’  OR  1=1  LIMIT  1; *–* |

This statement will return first user from database.

### Defence

The question is – how to secure the application. There is a bunch of methods and following them will not make the code unbreakable, but at least, this won’t be so easy.

#### Use mysql_real_escape_string

The `addslashes()` function has been marked as suspicious. This doesn’t take care about every case and is easily breakable. mysql_real_escape_string function doesn’t cause the same problems as addslashes.

#### Use MySQLi

The MySQL Improved extension handles bound parameters.

|     |     |
| --- | --- |
| 1   | $stmt  =  $db->prepare(‘update uets set parameter = ? where id = ?’);<br>$stmt->bind_param(‘si’,$name,$id);<br>$stmt->execute(); |

#### Use PDO

Here’s the long way to do bind parameters.

|     |     |
| --- | --- |
| 1   | $dbh  =  new PDO(‘mysql:dbname=testdb;host=127.0.0.1′,  $user,  $password);<br>$stmt  =  $dbh->prepare(‘INSERT INTO REGISTRY (name, value) VALUES (:name, :value)’);<br>$stmt->bindParam(‘:name’,  $name);<br>$stmt->bindParam(‘:value’,  $value);<br><br>*// insert one row<br>$name = ‘one';<br>$value = 1;<br>$stmt->execute();<br>* |

And a shorter way to pass things in.

|     |     |
| --- | --- |
| 1   | $dbh  =  new PDO(‘mysql:dbname=testdb;host=127.0.0.1′,  $user,  $password);<br>$stmt  =  $dbh->prepare(‘UPDATE people SET name = :new_name WHERE id = :id’);<br>$stmt->execute(  [array](http://www.php.net/array)(‘new_name’  =&gt;  $name,  ‘id’  =&gt;  $id)  ); |

#### Use ORMs

Use ORMs or PDO properly and bind parameters! Really, avoid using SQL statements. If you see the SQL statement in the code then definitely, there’s something wrong with it.

ORM takes care about most of security bottlenecks, escapes the strings and validates params. There’s no need to think about any other methods if you do so.

### Final thoughts

This series is made not to give a full tutorial to hack or break websites, but to secure the applications and prevent attacks from any source. I have tried to prepare this article not only for programmers – they need to be aware of any, any threat in the code and know the way how to prevent them, but also for quality assurance engineers – because their job is to catch and point all issues.