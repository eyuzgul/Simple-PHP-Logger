# :floppy_disk: Simple PHP Logger :floppy_disk:
[![PyPI license](https://img.shields.io/pypi/l/ansicolortags.svg)](https://pypi.python.org/pypi/ansicolortags/) ![](https://img.shields.io/github/languages/code-size/advename/Simple-PHP-Logger)

*Simple php logger* is a single file PHP log writer which follows various PSR-3 standards and writes logs into a .txt file using one line of code, e.g.
```php
Logger::info("I'm an info message");
```

The simple php logger's features are
- following various PSR-3 standards
- single file
- singleton pattern
- six log levels (info, notice, debug, warning, error, fatal)
- logs the line where the `Logger` method is executed (good for troubleshooting)
- logs the relative filepath of the source file, not the required one (good for troubleshooting)
- can be used with or without OOP (Add/remove namespaces)
- no composer needed :tada:

### :wrench: Motivation
There are many PHP Logger's out there, but most of them either follow all PSR-3 requirements or are too basic. Sometimes, you simply want something in the middle which can be used in every situation. Therefore, I've decided to create a basic PHP Logger, based on many PSR-3 standards using a singleton design pattern and inside a single file. This makes it an accessible and powerful logger.

### :white_check_mark: Installation
Simply download the Logger.php file and require it wherever you need it.
You can also insert it into a *Log* directory and add a namespace to it, for those who want to use it within an OOP project
Make sure that the directory `logs/` exists and is writable, else the logger throws an error that it cant read/write/create the log files.

### :mag_right: Log levels
The simple php logger uses six log levels:

|Level   |Description   | Example |
|---|---|---|
| INFO  |  Used for general informations | "The server has been running X hours" |  
| NOTICE  |  Used for interesting events | "The user $userName has logged in." |  
| DEBUG | Used for debugging | Could be used instead of echo'ing values |
| WARNING | Used for anything that might/ might not be a problem. |  | 
| ERROR | Any error which is fatal to the operation, but not shutting down the application| Can't open a required file, missing data, etc. | 
| FATAL | Any error which is shutting down the application| Database unavailable | 

### :books: How to use

#### Basic example
Here's a basic example how you could use simple php logger
```php
<?php
include_once(__DIR__ . '/log/Logger.php');

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
        // Log a fatal 
        Logger::fatal("Database connection failed", [$e->getMessage()]);
        die();
    }
}
```

#### All methods
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

**Example of the outputs of all methods above**
```codes
[15:45:33 27-Nov-2019] [localhost/boatie_template/test.php] [4] : [INFO]- The article XX has YY comments 
[15:45:33 27-Nov-2019] [localhost/boatie_template/test.php] [8] : [NOTICE]- The user XX has created the YY article 
[15:45:33 27-Nov-2019] [localhost/boatie_template/test.php] [12] : [DEBUG]- This is where the code breaks 
[15:45:33 27-Nov-2019] [localhost/boatie_template/test.php] [16] : [WARNING]- The file XX is 100GB big 
[15:45:33 27-Nov-2019] [localhost/boatie_template/test.php] [19] : [ERROR]- The file XX is missing 
[15:45:33 27-Nov-2019] [localhost/boatie_template/test.php] [23] : [FATAL]- Database connection failed ["Very bad database","I didnt feed him"]
```

##### Notice
This logger is based on Drew's LogWriter class [Simple PHP Text Logging Class \| drew.d.lenhart](https://www.drewlenhart.com/blog/simple-php-logger-class)

> **UPDATE:** The readme file will be updated with more informations and examples.
