# PHP for beginners course

## Section 2: Software instalation and set up

#### Xampp setup

Make sure you edit the Apache server, on the config file, by adding this:

```tex
DocumentRoot "/Applications/XAMPP/xamppfiles/htdocs/www"
<Directory "/Applications/XAMPP/xamppfiles/htdocs/www">	
```

and them dump the php files to run inside `/Applications/XAMPP/xamppfiles/htdocs/www`.

Pending: look for the way to put files wherever I want, e.g on /Documents.

## Section 3: variables

### What are web servers?

Software running on a machine that deliver files requested to it. Locally, the address which files are requested is `localhost`.

### Type juggling

it's when converting string into number, e.g

````
"2" * 4; //returns int(8)
````

### Escaping characters

Use double quotes:

````php
"hey\nbuddy"
````

### Assigning indexes manually in arrays

````
$weird_array = [1 => "Hello", 3 => "it's", "me"];

var_dump($weird_array);
// prints array(3) { [1]=> string(5) "Hello" [3]=> string(4) "it's" [4]=> string(2) "me" }
````

### Associatives arrays: strings as indexes

They're like objects

````
$user = ["name" => "tebi", "age" => 32];
````

### An introduction to HTML

If a page is called `index.html` there's no need to add the name to the url, because that will be served by default to the browser by the web server.

To see the address of an image, `right click -> copy image address`, and then past it in `<img src="">`

A local file has the address: 

````
http://localhost/images/collins-lesulie-0VEDrQXxrQo-unsplash.jpeg
````

And that can be used inside `<img src="">`. This will give the same output in the browser:

```html
<img src="http://localhost/images/collins-lesulie-0VEDrQXxrQo-unsplash.jpeg">
<img src="mages/collins-lesulie-0VEDrQXxrQo-unsplash.jpeg" >
```

Need to ouput dynamic data in the middle of html?

`````php
<h1>Hello <?php echo $name ?></h1>
<h2> <?= $day ?> what a nice day</h2>
`````

the second is a shorthand of the first one, ;)

### Mixing PHP and HTML

````php+HTML
<?php if ($authenticated): ?>
<p>Hello authenticated user!</p>
<?php endif; ?>
````

There are no `{}` curly braces on the if block, but, an `endif` keyword needs to be added at the end, to replace the closing curly brace.

### Comments

PHP comments are not sent to the client (HTML comments do)

````php
// single line comments
/* multiple
line comments
*/
````

### Databases

chose `utf8mb4_unicode_ci` encoding.

Password `phpmyadmin` database:

````
o-1cYEYRBiF9Rd8h
````

### Creating a table

for the `id` choose `A_I` which means auto increment.

each row is know as `record`, each column is a `field`

### SQL queries

Important, use single quotes when comparing strings:

````
WHERE title = 'First post';
````

Slow queries when using WHERE and ORDER (spoiler: use the primary keys OR indexes for filtering).

Example of slow query:

````sql
SELECT *
FROM article
WHERE title= 'First post'
````

To fix it,  on `Structure` tab, add an `Index on 1 column` and press `Go`.

The select the column, and the type `Index`. That will expose that column as an index and the query will run much faster, like reading the indexes on a book's index page.

Adding Indexes is not for free, because updating/inserting a value will take longer, because the index will need to be updated,

### Connection PHP to mysql

there are two ways regarding syntax, procedural or object oriented:

````php
// procedural
$connect = mysqli_connect($db_host, $db_username, $db_pass, $db_name);

//object oriented
$connect. = new mysqli($db_host, $db_username, $db_pass, $db_name)
````



### Generating the array

````php
$articles = mysqli_fetch_all($results);
// returns
[0]=>
  array(4) {
    [0]=>
    string(1) "1"
    [1]=>
    string(10) "first post"
    [2]=>
    string(27) "Wow, this is my first post!"
    [3]=>
    string(19) "2022-02-17 14:53:26"
  }
  [1]=>
  array(4) {
    [0]=>
    string(1) "2"
    [1]=>
    string(11) "Second post"
    [2]=>
    string(31) "Another great post content here"
    [3]=>
    string(19) "2022-02-17 14:53:26"
  }
}
````

Not very useful not having the field's names. To fix that:

````php
 $articles = mysqli_fetch_all($results, MYSQLI_ASSOC);
//returns
  [0]=>
  array(4) {
    ["id"]=>
    string(1) "1"
    ["title"]=>
    string(10) "first post"
    ["content"]=>
    string(27) "Wow, this is my first post!"
    ["published_at"]=>
    string(19) "2022-02-17 14:53:26"
  }
  [1]=>
  array(4) {
    ["id"]=>
    string(1) "2"
    ["title"]=>
    string(11) "Second post"
    ["content"]=>
    string(31) "Another great post content here"
    ["published_at"]=>
    string(19) "2022-02-17 14:53:26"
  }
}
````



## SQL injection

sql code can be passed as a url param, and destroy the database! a basic approach is to check for url param value's type.



## Importing files

````php
include './some-file.php' // gives a warning if the file wasnt found, but keeps script execution
require('./some-file.php') // stops script execution if the file wasn found.
````

`require` is preferred, because what's the point of running the scripts if there's a missing required bit.



## How to prevent exposing php files in the browser

just add a `.htaccess` file inside the folder you wanna manage

https://www.php.net/manual/en/pdo.construct.php when establishing the connection.



## Forms

if the `<form>` doesn't has the `action` attribute set, then it requests the same page with the form data as url params.

The url params contain the form data when the `<form>` doesn't have a `method` specified, which is `get`.

⚠️ everything submitted from a form, is a **string** when it gets to the server (using get or post methods).



## Avoiding SQL injections (not the best way)

```php
mysqli_escape_string($connect, $_POST['title']) 
  // has $connect to pass the server character set to the function
```

Then if a title is written in the form with a single quote on it, it will be escaped with an `\` symbol:

````
//entering `Hello ` there` in the forms leads to:
'hello \' there' being added to the SQL statement.
````

So the string is printed out but it doesnt act as a string itself for the SQL statement.



## The proper way to avoid SQL injection: Prepare statements

it also lets me get rid of horrible string contatenations inside the SQL statement.

Escaping quites is done automatically by `mysql_stmt_execute()` function.

Downside: I can't print out the SQL string, because the SQL params are inserted to the SQL statement in the database itself, not on PHP. It can be achieved by enabling the query log in the server, intructions here:

https://dev.mysql.com/doc/refman/8.0/en/query-log.html

guide here https://www.php.net/manual/en/mysqli.quickstart.prepared-statements.php

The steps are: Prepare -> Bind - Exectute the SQL statement

floating number is also called `double` in PHP.



## Documenting functions

https://docs.phpdoc.org/guide/references/phpdoc/basic-syntax.html, gives other developers more contenxt about the function usage.



## Connecting to the database using a function

having a require statement inside an `if` block is not a good idea:

```php
// BAD!!
if (condition) {
	require somefile
}
```

and also, using variables declares in the required file might not be present

```php
// BAD!!
// accessing $connect variable declared in database.php
```

Functions to the rescue!!



## Sanitize inputs to avoid code injection in the HTML

Scripts can be injected in textareas for example:
```
<textarea>
	//user input here
	</textarea><script>alert('hacked!');</script>
</textarea>
```

So, after the POST request (+ a GET request at the same time maybe?) when clicking submit button, the template will re-render that form and the script will run in the browser.

That script could steal data from `cookies` and `local storage`. This is known as XSS attack.

The idea would be to replace `< and >` characters by special entities, because they do have meaning in HTML world. We need special entities `&....;`



## Redirecting after interting data into the DB

when checking the `new-article.php` POST request, we see that the server return this on the headers:

````
Request Method: POST
Status Code: 302 Found
// Headers
Location: article.php?id=20
````

that's a relative url that sometimes it doesnt work on older browsers, let's do a redirect to an abosolute url. (see changes in the code).

then, I get this headers:

```Location: 
Location: http://localhost/article.php?id=22
```



Separating logic into a file:
```
// article.php
if (isset($_GET['id']) && is_numeric($_GET['id'])) {
    $sql = "SELECT *
        FROM article
        WHERE id = " . $_GET['id']
        . " ORDER BY published_at;";

    $results = mysqli_query($connect, $sql);

    if ($results === false) { // I wanna make sure $results is the boolean false, and not empty array, etc
        // log error - NOT WORKING- error is preventing if block execution
        echo mysqli_error();
//    echo "if block caught the error";
    } else {
        $article = mysqli_fetch_assoc($results);
    }
} else {
    $article = null;
}
```

Has `is_numeric($_GET['id'])` to prevent sql injection, but it's not the best. Let's move it to another file and `prepare` the statement.

Tip: keep `require` statement to a minimum, they become the code spaghetti, it better to add the `$connect` as a variable of the `getArticle` function.



## Empty()

there's no need to initialize `$errors` as an empty string inside `new-article` as `empty()` returns true if the variable is not defined:

```php+HTML
<?php
if (!empty($errors)){
    foreach ($errors as $error): ?>
        <p><?= $error ?></p>
    <?php endforeach;
} ?>
```

 A variable is considered empty if it does not exist or if its value equals **false**.



## Delete sql statement

make sure you add the `WHERE` clause, and add fields that are indexed, like `id`, otherwise it will take longer.

Although we could redirect users to `delete-article.php?id={id-here}` via an <a> tag, that will be deleting posts using a GET request, not a good practice. 

It's better to replace the <a> tag form a form with method="POST".

http://localhost/article.php?id=22



### $_SESSION

on every request, is possible to store data in an array called $_SESSION, but it's expensive, so store booleans or small things there.

By using the `session_start` function, the sever will create a file with the session id (letters and numbers) and it will add it to the response object. Then, the browser will create a cookie storing that id, so it can be sent back to the server in subsequent requests.

Session_start() should be place before returning data to the server via echo, etc, because the cookies are sent in the headers, before the actual html content.



### Session fixation attacks

https://owasp.org/www-community/attacks/Session_fixation

the session can be hijacked by other user, and use that logged in status to do stuff on our session.

The solution is to change the session `id` when after logging in. The data of the user is being kept, but the `id ` changes, so it can't be stolen, because the attack starts before the user logs in.

Look at the `Cookies` in the chrome dev tool
```
PHPSESSID	s8dnnp5vqi0mu
```

Will change after login



### Classes

although new properties can be added to an object created instantiating a class, it's better to declare all the properties we might need in the class instead.

By default, methods are public, but it's better to add `public` in case the PHP default changes on a new version.



### Setter and getter, why we need them?

if a property is supposed to be of a certain type, but then is assigned another, then a method inside the class could throw an error

don't use magic `__get` and  `__set` magic methods because they make the code more difficult to read and debbug.

Only use setter and getter when you need to do side effects thing when updating a property, or when you need read/write only capabilities.

example of `read only` property:
```
class Item {
	private $price;
	
	function getPrice() {
        return $this->price;
    }
}
```



### Static properties and methods

```
class Item {
	public static $count;
	
	static function getCount(){
        return self::$count;
    }
}

// we initialize the static property
Item::$count = 0;

$banana = new Item(20);

var_dump(Item::getCount()); // prints 1 after one instantiation


```



### Constants

https://www.php.net/manual/en/function.define

```
//normal syntax
define("CONSTANT", "Hello world.");

//inside classes
const CONSTANT = "Hello world.";
```

constants inside classes are accessed like static props:

```
class Item {
	const CONSTANT = 'Hi there';
}

//even more clearer syntax
class Item {
	public const CONSTANT = 'Hi there';
}


Item::CONSTANT
```



An example of global constant usage:

````
mysql_fetch_array($result, MYSQL_ASSOC)
````



Overriding methods in the child class

````
class Parent {
	public function sayHello() {
		return 'hello';
	}
}

class Child extends Parent {
	public function sayHello() {
		return 'hello there!';
	}
}

//or this better way
class Child extends Parent {
	public function sayHello() {
		return parent::sayHello() . ' there!';
	}
}
````



Child classes can't read private properties or methods of parents!

How to make a property of a parent class innaccesible to the world but accessible from a child class?🤔

the `protected` visibility does that!

````
class Item {
	 protected $code = 1234;
}

class Book {
	public function getCode() {
		return $this->code;
	}
}

$ball = new Item();
$ball->code // throws an error

$bible = new Book();
$bible->getCode(); // prints 1234;
````



### PDO

another way to interact with databases, not just mysql, but any relational database system!

https://www.php.net/manual/en/intro.pdo.php

Benefits:

- better statement preparation with named place holders, instead of `?` .

With the OO api, we instantiate a class to connect to the database, instead of calling a function, like on procedural style:
```
$db = new PDO($sdn, $db_username, $db_pass);

VS


```



### Errors

Code can throw exception object that can be caught in code.

https://www.php.net/manual/en/language.exceptions.php, and there's a PDOException built in, so we can grab it and see the error message bein thrown.

```php
try {
            return  new PDO($sdn, $db_username, $db_pass);
        } catch (PDOException $ex) {
            echo $ex->getMessage();
            exit;
        }
```

By default PDO class methods don't throw errors, but we can configure it to do so:

````
 try {
            $db = new PDO($sdn, $db_username, $db_pass);
            $db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
            return $db;
        } catch (PDOException $ex) {
            echo $ex->getMessage();
            exit;
        }
````

so the db object needs to get those attributes  so PDO throws errors, and we can catch them in a global catcher later on.
````
// from https://www.php.net/manual/en/pdo.setattribute.php
PDO::ERRMODE_EXCEPTION
Throws PDOExceptions.
````

more here https://www.php.net/manual/en/pdo.error-handling.php

There's no point on adding try and catch block everywhere to just log the error. It's better to add a general exception handler intead and catches all uncaught exceptions. That makes the code a lot cleaner! there are no if checks for errors before accessing data, etc

the method `getById` that returns the single article, has been transformed to return an object instead of an associative array, because the object can be modified, like in `edit-article.php`  and then saved back to the db.

```
class Article {
	public statit function getById() {
	$stmt->setFetchMode(PDO::FETCH_CLASS, 'Articles');
}
}


// that code instantiates class on the response!
// a call to this method will instatiate the class Article
    $article = Article::getById($connect, $_GET['id']);
```



In PHP, functions without a return statement return NULL by defaul (JS returns Undefined);



declare a variable inside and if condition ():

```
// user is an object
        $user = $stmt->fetch();
        if ($user) {
            return $user->password === $password;
        }
        
        //replace by
        if ($user = $stmt->fetch()) {
            return $user->password === $password;
        }
```



### Hashing

PHP has this built in function that salts (add random characters) and hashes (replaces the character for other ones) 

use `**`PASSWORD_DEFAULT`**` as argument, to use the latest best algorithm provided by PHP, that can improve over time.

````
<?php

$password = '123';

$hash = password_hash($password, PASSWORD_DEFAULT);

var_dump(password_verify('lala', $hash)); // prints false
````

Hashes can't be reverted via formal functions/methods, like what happens with encrypted password, that can be transformed to the original string.

Although hashed passwords can be reverted by hackers, salting them before hashing is enough.

### Refactoring

All the code that doesn't ouput a template, was moved from `/includes` to `/classes`.

Requiring the most used classes in every php file it's a lot of repetition. 

There's an autoload functionality that will try to load the php file that contains a class that is beeing called in the code.

As the class names are files containing them match, here's a simple script to autoload them
```
spl_autoload_register(function($class) {
    echo $class;
});

//require 'classes/Database.php';

$db = new Database();

// I get `Database` printed
```

so, the final script is:
```
spl_autoload_register(function($class) {
    require "./classes/{$class}.php";
});
```



```
<?php

spl_autoload_register(function ($class) {
echo "Class: {$class}"; 
});

$product = new Product();

echo 'code kept executing'; // never runs

I get `Class: Product` printed, so code didn't keep running, because the Product
class was never required, 
```



Variables inside `./inclues` folder need to be returned, because it makes the code clearer on the file using the code:
```
// db.php
<?php

$db = new Database();
return $db->getConn();

//edit-article.php
$connect = require 'includes/db.php';
```



## Relative paths (bug with chain of requires)

when requiring a file like this:
````
// index.php
require 'includes/init.php';

file structure is
index.php
/includes
 	init.php
````

Php start looking for files where relative to where index.php is located.

The problem is when we have
````
// /admin/index/php
require 'includes/init.php'; 
// throws an error because there's no includes folder at the index level

/admin
		index.php
/includes
		init/php

````

So we need to navigate with dots

```
require '../includes/init.php'; 
```

when requiring files, if the required file also tries to require another file, the relative path is to the origin file that require the first one:
````
/admin.php 
requires init.php
then init.php requires a class file like this
spl_autoload_register(function($class) {
    require "./classes/{$class}.php";
});

RELATIVE to /admin.php
````

So the init.php code needs to be fixed, to be more flexible an reusable from different locations, by calling the classes relative to init.php location instead of relative the the calling file.

👉 the magic constant `__DIR__` is the answer, because we can require the class file relative to init.php and NOT the requiring file 🎉

dirname returns the aboslute path to the parent structure.

````
given var/www/includes/init.php
__DIR__ ouputs var/www/includes/ the file's absolute path
dirname(__DIR__) /var/www /the parent
````

````
// init.php
<?php

spl_autoload_register(function($class) {
    // we make a reference to this file with __DIR__, and then go back to the parent with dirname
    // it doesn't matter who requires init.php file
    require dirname(__DIR__) . "/classes/{$class}.php";
});

session_start();
````



### Relative paths href in anchor tags:

if we require a file that has anchor tags, the href, if used like this:

````
admin/index.php requires header.php

//includes/header.php
href="logout.php" 

Then, clicking that links leads me to 
http://localhost/admin/logout.php

//will be relative to the file that requires this file (top level file in the chain), /admin in this case)❌
````

How to fix it?

````
href="/logout.php" 
// always points to the root of the website
// so any file can require this code and the link will work
````

`Url::redirect` follow the same rules as anchors, link wise.



When using requires, we can move up with `../`:
````
require '../includes/init.php';
````



## Pagination

```
👍$paginator = new Pagination($_GET['page'] ?? 1, 4);
```

that the null coallenscense operator in action https://wiki.php.net/rfc/isset_ternary, from php 7

if far better than using asset(), like this
````
👎$paginator = new Pagination(isset($_GET['page']) ? isset($_GET['page']) : 1, 4);
````







Bob's call

```
	add_filter(
		'blaize_filter_subscribe_button_text',
		/**
		 * Update 'Subscribe' button text
		 */
		function () {
			return 'Subscribe';
		}
	);

esc_html( apply_filters( 'blaize_filter_subscribe_button_text', 'Subscribe Today' ) )
```

- Is there any data to filter?
- when is `Subscribe Today` string used? Maybe old code that had more funcitonality and then was deleted? the callback funtion is not expecting any arguments.



## Getting the page from the url

remember that url params are sort of user input, as they can modify it, so needs to be validates somehow.

````
$page = filter_var($page, FILTER_VALIDATE_INT, [
            'options' => [
                'default' => 1
        ]]);
        var_dump($page);
        $this->offset = $items_per_page * ($page - 1);
````

by default, the numbers passed as url param `?page=1` is a string `1`, but PHP converts it into a number when doing `$page - 1`, but what if we have `?page=abc`, then, we need to use the filter.

What if we pass a negative value as url param? it breaks

````
'options' => [
                'default' => 1,
                'min_range' => 1
````

and voila.



## Getting the total number of articles

https://www.php.net/manual/en/pdo.query.php

````
public PDO::query(string $query, ?int $fetchMode = null): PDOStatement|false
````

````
public PDOStatement::fetchColumn(int $column = 0): mixed
````

````
public static function getTotal($connect) {
        // query returns PDO:query, in this case a single column with the number of records;
        return $connect->query('SELECT COUNT(*) FROM article;')->fetchColumn();
    }
````



https://www.php.net/manual/en/pdostatement.fetchcolumn.php



## Putting the navigation into /includes folder

the problem is that the links need to know the location of the file that is calling this include file.

'REQUEST_URI'

The URI which was given in order to access this page; for instance, '`/index.html`'.

https://www.php.net/manual/en/reserved.variables.server.php

'REQUEST_URI' includes the query string, like `?page=3`, so we need to clean it up, before using it on the links. We can split the string at `?` with this https://www.php.net/manual/en/function.strtok.php

Tip: if you `ORDER BY` make sure that column in the table has an index.



## Storing images

databases are not meant to store images (binaries), the filesystem is the place for that.

After the POST request, the data is being picked up with this global variable `$_FILES`, instead of the `$_POST` variable used for grabing text inputs.



## Config time! 

````
phpinfo();
````

let's us see the PHP config in the server.

by default, I see
````
upload_max_filesize	40M	40M
````

but it can be changed here:
````
Loaded Configuration File	/Applications/XAMPP/xamppfiles/etc/php.ini
````

e.g, change it to 50M.

Sometimes, we don't have access to that php.ini file in server because we use a hosting company that locks us from that  (those values set there will affect the whole server), so there a recommended way:

in a hta access file, adding php_value or php_flag and + name 0f the setting + new value, eg 

````
// .htaccess at the root level of the site
php_value upload_max_filesize 2M
````

and the NOT recommended way: ini_set, but is difficult to debug when moving servers.

```
phpinfo prints this
upload_max_filesize	| Local value 2M	| Master value 50M
```



Uploading a super big file:
I get this error:

````
Warning: POST Content-Length of 70550885 bytes exceeds the limit of 41943040 bytes in Unknown on line 0

Warning: Undefined array key "file" in /Applications/XAMPP/xamppfiles/htdocs/www/admin/edit-article-image.php on line 24

Warning: Trying to access array offset on value of type null in /Applications/XAMPP/xamppfiles/htdocs/www/admin/edit-article-image.php on line 24
````

it can't be traped in the try catch block. It's displayed becuse  `display_startup_errors` is ON, but is generally OFF on prod.

Solution to catch it:

disable display_startup_errors, and check if the $_FILE is empty, inside the try and catch block



### To do:

- Try to run php outside`htdocs` folder, and configure the interpreter on `PHP storm`.
- convert double quotes from form to single quotes for SQL values's statement
- 

