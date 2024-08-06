# :floppy_disk: Simple PHP Logger :floppy_disk:
![License MIT](https://img.shields.io/github/license/advename/Simple-PHP-Logger)

*Simple php logger* is a single file PHP log writer which writes logs into a .txt file using one line of code, e.g.
```php
Logger::info("I'm an info message");
```
outputs inside a *log-28-nov-2019.txt*:
```
[22:50:35 28-Nov-2019] [localhost/test.php] [1] : [INFO] - I'm an info message
```

#### Features
- single file
- singleton pattern
- six log levels (info, notice, debug, warning, error, fatal)
- logs the line where the `Logger` method is executed (good for troubleshooting)
- logs the relative filepath of the source file, not the required one (good for troubleshooting)

### :wrench: Motivation
There are many PHP Logger's out there, but most of them are either very heavy or too basic. Sometimes, you simply want something in the middle, suitable for every situation and project size. Therefore, I've decided to create a simple PHP Logger, inspired by several other logging libraries.

### :white_check_mark: Installation 
Simply download the Logger.php file and require it wherever you need it.
You can also insert it into a *Log* directory and add a namespace to it, for those who want to use it within an OOP project
Make sure that the `logs/` directory exists and is writable, else the logger throws an error that it cant read/write/create the log files.

### :mag_right: Log levels 
The simple php logger uses six log levels:

|Level   |Description   | Example |
|---|---|---|
| INFO  |  Used for general informations | "The server has been running X hours" |  
| NOTICE  |  Used for interesting events | "The user $userName has logged in." |  
| DEBUG | Used for debugging | Could be used instead of echo'ing values |
| WARNING | Used for anything that might/ might not be a problem. | "The image XX is 30MB big. This can cause slow web performance" | 
| ERROR | Any error which is fatal to the operation, but not shutting down the application| Can't open a required file, missing data, etc. | 
| FATAL | Any error which is shutting down the application| Database unavailable | 

### :books: How to use 

#### Installation using Composer

Install the package through [composer](http://getcomposer.org):

```
composer require advename/simple-php-logger
```

Make sure, that you include the composer [autoloader](https://getcomposer.org/doc/01-basic-usage.md#autoloading) somewhere in your codebase.



#### Installation without Composer

If you prefer not to use the Composer autoloader, you can manually include the Logger class in your project. 

Follow these steps:
Download the `Logger.php` class

``` 
<?php
require 'Logger.php';

use advename\Logger;
```


#### Basic example
Here's a basic example how you could use simple php logger
```php
<?php
require __DIR__ . '/../vendor/autoload.php';

use advename\Logger;


function connectToDatabase()
{
    $dbhost = 'localhost';
    $dbname =  'post_website';
    $dbuser = 'root';
    $dbport = '3306';
    $dbpass = '';
    try {
        $pdo = new PDO("mysql:host=$dbhost;port=$dbport;dbname=$dbname", $dbuser, $dbpass);
    } catch (PDOException $e) {
        
        Logger::fatal("Database connection failed", [$e->getMessage()]); // <- Log a fatal error with details
        
        die("Oh snap, looks like something didn't work. Please retry again later"); // <- UX friendly die() message
    }
}
```

#### Define log directory

Define the root log directory where the Logger will output the logs. If not specified, the logs will be saved in the same directory as the Logger.php file.

```php
<?php
require __DIR__ . '/../vendor/autoload.php';

use advename\Logger;

Logger::setLogDirectory(__DIR__);
```


#### Logging methods
```php
<?php
// Info log
Logger::info("The article XX has YY comments");

// Notice log
Logger::notice("The user XX has created the YY article");

// Debug log
Logger::debug("This is where the code breaks");

// Warning log
// $file would be a e.g. data about an image received from a POST request
Logger::debug("The file XX is 100GB big", $file);

// Error log
Logger::error("The file XX is missing");

// Fatal log
$information = ["Very bad database", "I didnt feed him"];
Logger::error("Database connection failed", $information);
```

### :book: Log output 
The simple php logger outputs the logs in the following blueprint:
```
[Hour:Minutes:Seconds Date-Month-Yearh] [file-path] [line-of-execution] : [Log level] Descriptive message (optional = The array/object of additional information)
```

You can change the default time output using the `setOptions()` method.
```codes
Logger::setOptions([
    'logFormat' => 'Y-M-d H:i:s'
]);
```

**Logger outputs**
```codes
[15:45:33 27-Nov-2019] [localhost/test.php] [4] : [INFO]- The article XX has YY comments 
[15:45:33 27-Nov-2019] [localhost/test.php] [8] : [NOTICE]- The user XX has created the YY article 
[15:45:33 27-Nov-2019] [localhost/test.php] [12] : [DEBUG]- This is where the code breaks 
[15:45:33 27-Nov-2019] [localhost/test.php] [16] : [WARNING]- The file XX is 100GB big 
[15:45:33 27-Nov-2019] [localhost/test.php] [19] : [ERROR]- The file XX is missing 
[15:45:33 27-Nov-2019] [localhost/test.php] [23] : [FATAL]- Database connection failed ["Very bad database","I didnt feed him"]
```

##### Note
**Big thanks to** reddit user [u/mferly](https://www.reddit.com/user/mferly) for his support and guidance!

This logger is not a replacement for a PSR-3 compliant logger library such as [Monolog](https://github.com/Seldaek/monolog), [KLogger](https://github.com/katzgrau/KLogger) or [Analog](https://github.com/jbroadway/analog). This Logger should simply be used to quickly implement a simple logger with several methods.

**Inspired by**
- [Simple PHP Text Logging Class \| drew.d.lenhart](https://www.drewlenhart.com/blog/simple-php-logger-class)
- [PSR-3](https://www.php-fig.org/psr/psr-3/)
