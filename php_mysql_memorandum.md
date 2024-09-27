# MySQL

## Strict mode
1. To check either strict mode is enabled for mysql:
`sudo mysql`
`SHOW VARIABLES LIKE 'sql_mode';`

If `STRICT_TRANS_TABLES` appears, then strict mode is active.

2. Otherwise, to enable it, type:
`set global sql_mode='STRICT_TRANS_TABLES;`

To disable the strict mode, run:
`set global sql_mode='';`

3. To make the changes permanent, edit the mysql's configuration file:
in /etc/mysql/mysql.conf.d/mysqld.cnf

4. Below the sub-section [mysqld], add the following:
`sql_mode='STRICT_TRANS_TABLES'`

**Type declarations** are available only for function arguments, return values and class properties.

Passing arguments with GET
==========================

Adding a link <a href="./path/to/target.php?param1=<?=$_GET['param1']?>&param2=<?=$_GET['param2']?>">Link name</a>

How to redirect to a custum 404 not found file
==============================================
```php
<?php
  http_response_code(404);
  include_once './my_404.php'; // provide your own HTML for the error page
  die();  // ensure stoping the normal execution
?>
```
/

1. Run the following into a terminal: `sudo a2enmod rewrite`
2. You can check the newly created symbolik link by typing:
`ls -al /etc/apache2/mods-enabled/rewrite.load`
3. Run: `systemctl restart apache2` / `sudo sv/service restart apache[2]`
4. Edit and add permission to the config Apache's file: `code-oss /etc/apache2/sites-available/000-default.conf`
```conf
<VirtualHost *:80>
  <Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all Granted
  </Directory>
</VirtualHost>
```

5. Check your Apache configuration: `sudo apache2ctl configtest`
6. Restart Apache server: systemctl restart apache2
7. Create a .htaccess at the root directory: `touch /var/www/html/.htaccess`
8. Edit .htaccess file to add: 

```
<IfModule mod_rewrite.c>
  RewriteEngine On
  ErrorDocument 404 http://127.0.0.1/oxus/404.php
  ErrorDocument 500 http://127.0.0.1/oxus/404.php
  ErrorDocument 403 http://127.0.0.1/oxus/404.php
  ErrorDocument 400 http://127.0.0.1/oxus/404.php
  ErrorDocument 401 http://127.0.0.1/oxus/404.php
</IfModule>
```
9. Now try accessing a non-existent web page on the aforementioned address.

Allow HTTP redirection without extension's filename
===================================================

1. Edit the .htaccess file by adding the following:
```
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^([^\.]+)$ $1.php [NC,L]
```

2. My current .htaccess file:
```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^([^\.]+)$ $1.php [NC,L]

  ErrorDocument 404 http://127.0.0.1/learning/serie05/404.php
  ErrorDocument 500 http://127.0.0.1/learning/serie05/404.php
  ErrorDocument 403 http://127.0.0.1/learning/serie05/404.php
  ErrorDocument 400 http://127.0.0.1/learning/serie05/404.php
  ErrorDocument 401 http://127.0.0.1/learning/serie05/404.php
</IfModule>
```


## Configuring PHP

### Updating PHP
```php
sudo add-apt-repository ppa:ondrej/php  
sudo apt update
sudo apt install php8.3 php8.3-cli php8.3-{bz2,curl,mbstring,intl} php8.3-fpm
```

## COMPOSER INSTALLATION
Composer is the recommended PHP dependencies manager.

1. Download the installer
2. Verify the installer SHA-384
3. Run the installer
4. Remove the installer

```php
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'edb40769019ccf227279e3bdd1f5b2e9950eb000c3233ee85148944e555d97be3ea4f40c3c2fe73b22f875385f6a5155') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

### Make the `composer` command system-wide
sudo mv composer.phar /usr/local/bin/composer

### Composer configuration
1. Initialize the dependency manager:
`composer init`

*Nota Bene*: the key-value pair within "autoload" part stands for "<logical_namespace>: <file_system_directory>"

2. Add the autoloader in your PHP base code:
`sed -i "require_once __DIR__/vendor/autoload.php" ./index.php`
3. Regenerate the autoloader for the local project:
`composer dump-autoload --optimize`


# PHPUnit INSTALLATION OF PHPUNIT

## Remove potentially existing system-wide installation of phpunit
```elvish
vendor/phpunit/phpunit/phpunit --generate-configuration
vendor/phpunit/phpunit/phpunit --migrate-configuration
xbps-remove -R phpunit
```

0. Add some basic php-based dependencies.
`sudo xbps-install -Suy php-cli php-json php-mbstring php-xml php-pcov php-xdebug zip`

1. Install unit test dependency.
`composer [global] require phpunit/phpunit --dev`

2. Install consistent (and nicely implemented) data structure's extension for PHP.
`composer require php-ds/php-ds`

3. List some suggested [transitive] dependencies.
`composer suggest [--all]`

## RUNNING PHPUNIT
1. Execute a suite tests:
`.vendor/bin/phpunit --testsuite units`


### Basic diagnose
1. Run the following to print some useful feed-back related to composer environment.
`composer diagnose`


### Activate cURL to increase performance
1. `php -m | grep curl`
2. If exit code 1, then `sudo hx /etc/php8.3/php.ini`
3. Remove the leading semicolon at line `;extension=curl` to uncomment it
4. sudo sv restart apache


## CONFIGURE PHP MODULE
1. Enable error reporting and specify an error log file.
sed -i "s/'log_errors = Off'/'log_errors = On'/" /etc/php8.3/php.ini
sed -i "s/';error_log ='/'error_log = /var/log/php_errors.log" /etc/php8.3/php.ini

## INSTALL AND CONFIGURE XDEBUG and OPCACHE PHP EXTENSIONS
1. Activate step debugger.

```elvish
sudo xbps-install -S xdebug-8.3_1`
sudo sed -i "zend_extension=xdebug" \
"xdebug.mode = debug,coverage" \
"xdebug.client_host=192.168.1.190" \
"xdebug.client_port=9003" \
"xdebug.start_with_request=trigger" \
"xdebug.trigger_value=StartDebuggerForMe" \
"xdebug.log=/tmp/php_xdebug.log" /etc/php8.3/conf.d/99-xdebug.ini

2. Increase PHP performance avoiding a need to parse each HTTP request sent by web server to PHP module.
sudo sed -i "zend_extension=/usr/lib/20-opcache.ini" \
"opcache.memory_consumption=128" \
"opcache.interned_strings_buffer=8" \
"opcache.max_accelerated_files=4000" \
"opcache.revalidate_freq=60" \
"opcache.enable_cli=1" /etc/php8.3/conf.d/20-opcache.ini

3. Specify env var to match `xdebug.trigger_value` in local `99-xdebug.ini` file.
sed -i ~/.config/elvish/rc.elv "set E:XDEBUG_TRIGGER = StartDebuggerForMe"

4. Regenerate env var for current interactive session and reboot web server.
exec elvish
sudo sv restart apache
php -v
```


# PHP LANGUAGE

## Scope
### Variable's scope

In PHP, a variable's scope is not limited by local code blocks, *viz.* loops and conditional statements create not their
own scope!

## Match conditional

`Match` is an expression (*i.e.* evaluates to a value), unlike `switch` which is a statement.
Branching on a range of integers or on string content requires to match against `true` boolean value.

### Namespace

`use` keyword allows to define an alias to name classes, interfaces or other namespaces.


## Abstract class
An abstract class can not be instantiated. Its behaviour is too generic to deliver a immediate useful business value 
and therefore must be derived in order to be instantiable.

### Parameters and arguments
* Named parameters act like optional arguments.
* Positional parameters act like required arguments.
Both can be named in the callsite.


### mb_string / multibytes sequence / utf-8 encoding
1. Configure Apache web server's UTF-8 encoding and discharge the need to specify it into PHP modules' config or PHP base code.

sed -i 's/;internal_encoding=/internal_encoding="UTF-8"' \
sed -i 's/;input_encoding=/internal_encoding="UTF-8"' \
sed -i 's/;output_encoding=/internal_encoding="UTF-8"' /etc/php8.3/php.ini


## Function

### Closure with arrow notation
An anonymous function using Closure class (*viz.* `fn` notation), automatically captures parent's scope variables by value.
An arrow function allows a single, implicitly returned expression.

### Generator
A generator is a function used to generate a serie of values. Uses `yield` statement to return a value, which saves
the function's state, allowing it to resume later.
A generator behaves like a iterator and can therefore be used in a loop. A `foreach` loop expects an `iterable|object`.


## Class
### Constructor property promotion
Prefixing each constructor parameter with a visibility level (*viz.* public, protected, private) automatically promotes
those parameters into class properties. This declaration is less verbose.

### Anonymous class
An anonymous class can be used when a single, throwaway object is needed.
```php
$obj = new class('Greetings!')
{
  public function __construct(private string $greet)
  {
    $this->greet = $greet;
  }
}
echo $obj->greet; // Greetings!
```

### Object access
In PHP, objects of the same class can access to other's non-public members.

### Final
`final` keyword prevents a child class to override its properties and methods. When applied to a class it can not be
extended.

### Static
`static` keyword makes a method or property available without the need to instantiate its class.
Useful for utilitarian methods or setting up universal constants.
Use static methods when performing a generic function independently of instance variables.
Properties shall be declared static when needing a single instance of the variable.

Instance methods can use both static and instance members.


### Accessor
Exposure of a class' internals make future changes to that class more difficult.
Leave out the set accessor to make a property in read-only mode.
Leave out the get accessor to make a property in write-only mode.


## Object-oriented paradigm
**OOP starts to make more sense than procedural paradigm when you starts using dependencies injection and interfaces.**

You must think that an instance is a way to have a copy of the same object with multiple states that may coexist \
during the lifetime of your script.
A static object can not have some copies. For the entire script lifetime, you merely use the very same object that \
has its own, single state.

That is the reason why you can have static properties inside instantiable objects.
but you can not have any instance properties within static methods.

### OOP principles
* **Favour composition over inheritance.**
* Favour flux architecture in stead of allowing bi-directional flux when some objects can call other's method objects
and *vice-versa*. See https://facebookarchive.github.io/flux/docs/in-depth-overview/ for more details.

Ensure that your data flow in your application is one way directional only: avoid situations when they call each others
in circles. When it happens, created a third one with factored functionality of the previous two.

### Autoloader
Loads automatically each class, interface, trait and enum definitions.

```php
spl_autoload_register(function (string $class_name): void {
  require_once __DIR__ . '/' . strtolower($class_name) . '.php';
});
```

### Type conversion
An explicit cast changes not the type of a variable, it changes only that expression's evaluation.
Use `settype` function to actually alter a variable's type: `settype(<variable>, <target_type_as_string>);`


### Variable testing
Functions to processe user-supplied data:
* isset()
* empty(): true if 0, '', false, null or exists not.
* is_null()
* is_scalar()
* is_callable()
* is_array(); etc.


### Inheritance
When inheriting from a parent class, use `parent::__construct(<all_properties>)` and also pass all those parent \
properties into the child constructor declaration.


## Lexikon
* global: creates a local reference to a global variable in the `$GLOBALS` array.
* Validation: ensure that the data is in an expected form, in terms of type, range and contents.
* Sanitizing: disable potentially malicious code.


## Tips
* Array to string conversion warning: use `implode(' ', $var)` to return a string (*e.g.* when using `echo()`
* Filter an email form field: `if (!filter_var($_POST['user']['email'], FILTER_VALIDATE_EMAIL))`
* Submit an array in a HTML form: add square brackets notation to name property: `name="user[age]"`
* Iterate through a submitted array: suffix the HTML tag with an empty pair of square brackets. Looping expects an \
object|Countable even if only one element exists: `<select name ="user[weapon][]" size="4" multiple>`

### Database
* SQL query sanitization: use `PDO::prepare()`

### User input
* User-fed web data sanitization: `htmlentities($_POST['comment'])`

* **Favour composition over inheritance!** Use readonly properties to avoid an object state manipulated from the outside.
* Get a short name's class: `(new \ReflectionClass($this))->getShortName()`
* Get an error message: `} catch (DomainExceptionError $e) { $e->getMessage(); }`
* Favour exhaustive pattern matching using `match` over `switch`.

### Strings
* Return a substring: `substr(strrchr($haystack, $needle), 1)`: skip the needle and take the rest.

### HTTP header
* Retrive cookie's informations: `curl -I <URI>`


### Benchmark
Measure performance like so:
```php
$start = microtime(true);
// Insert your business layer code here.
$end = microtime(true);
$res = $end - $start;
```


### Reference

<pre><code>
class MyClass
{
  public int $x = 1;  // (*): here is the original object.
}

function modifyVal(MyClass $o): void
{
  // The pointer to object's variable actually changes the original data.
  $o->x = 3;
  // Object variable is altered by an assignment operator, hence creates only a local copy.
  $o = new MyClass;
}

$obj = new MyClass;
modifyVal($obj);  // A pointer to object is passed.
echo $obj->x, PHP_EOL;  // 3

function modifyRef(MyClass &$o): void
{
  $o->x = 8;
  $o = new MyClass;  // (*)
}

$obj2 = new MyClass;
modifyRef($obj2);
echo $obj2->x, PHP_EOL;  // `1`: change has been propagated back to the original object (*)


[//]: #  Return by reference, calling a function prefixed with ampersand.

class YourClass
{
  public int $val = 12;

  public function &getVal(): int
  {
    return $this->val;
  }
}

$obj3 = new YourClass;
$myVal = &$obj3->getVal();  // bind reference
echo $myVal;
</code></pre>


## Error Handling

* In php.ini file:
1. error_reporting = On
2. display_errors = E_ALL & ~E_DEPRECATED & ~E_STRICT
3. log_errors = On
4. error_log = /var/log/php_errors.log

* In base code:
5. trigger_error('message', error_level = E_USER_NOTICE|E_USER_WARNING|E_USER_ERROR)

### Data structure's extension
Navigate through `\Ds` namespace to access new data structures; *e.g.*
<pre><code>
  $vec = new \Ds\Vector();
  $vec->push(...['amlk', 'aurele']);
  $vec->push('myrank' => 4);
</code></pre>
