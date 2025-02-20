# Introduction

#### Fork Note
This was forked specifically to implement php 8.0 compatibility. For any poor souls that might have to deal with a php version this old...

#### Recomendations
Consider to use https://github.com/rectorphp/rector

#### What is PHP 8 Migration Assistant Report(MAR)?
PHP 8 MAR, or just "php8mar", is a command line utility to generate reports on existing PHP 5.x or 7.x code to assist developers in porting their code quickly to PHP 8.  It will run against invididual files or entire project folders.  Reports contain line numbers, issues noted, and suggested fixes along with documentation links.

#### Will php8mar automatically fix my code?
No, php8mar does implement a full lexer to determine code changes and can not determine the intent of the code.  It uses lexer tokenizing, string matching, and regular expressions to find syntax changes that may cause issues when porting code to PHP 8.  As well, it will detect code snippets in comments and report them as it can not distinguish it as commented code.

#### What has changed in PHP 8? (Read This!)
If you are currently unfamiliar with what will change moving to PHP 8 then read the [UPGRADING file in the current master branch of php-src](https://github.com/php/php-src/blob/PHP-8.0.0/UPGRADING).  Understanding these changes is important to reading the reports generated by php8mar.

#### Does it handle very poorly styled code?
Very poorly styled code will not parse well, but assuming there is at least some structure to the code style it should still parse.  If the code is entirely too poorly formatted there are several web sites that can be used to clean up source code.

* http://www.phpformatter.com/ (Does not support PHP 5.4 [] array syntax.)
* http://phpbeautifier.com/
* http://phpcodecleaner.com/

# Usage
First, start by downloading or cloning this repository.  It does not need to be placed inside the folder containing the source code.

To begin, type on the command line:

	php mar.php

This will produce a list of available arguments and switches.

Typical usage would appear as:

	php mar.php -f="/path/to/file/example.php"

Or:

	php mar.php -f="/path/to/folder/example/"

This would run against the example file or folder and save the resulting report into a reports folder inside the php8mar folder.  When referencing the file or folder to run against it is recommend to use a fully qualified path.  Relative paths are supported, but will be relative to the location of the php8mar folder.

Give a try, use the included `testcases.php` to generate a report:

	php mar.php -f="testcases.php"

## Available Options:
**-f**
```
	Path to the file or folder to run against.
	The location of the file or folder to use for generating the report.  A fully qualified path is recommended.  Relative paths will be based off the php8mar folder.
		*Example: -f="/path/to/folder"*
```

**-e**
```
	Path to the file or folder to exclude.
	The location of the file or folder to ignore for generating the report.  A fully qualified path is recommended.  Relative paths will be based off the php8mar folder.
		*Example: -e="/vendor/"*
```

**-r**
```
	Path to the folder to save the report.
	The location to save the final report.  By default this saves into the reports/ folder inside the php8mar folder.  A fully qualified path is recommended.  Relative paths will be based off the php8mar folder.
		*Example: -r="/path/to/folder"*
```

**-t**
```
	Types of tests to run.
	By default all tests will run.  This option allows tests to be selected using a comma delimited list.  Allowable values: critical, nuance, and syntax.
		*Example: -t="syntax,nuance"*
```

**-x**
```
	List of file extension(s) to process.
	By default *.php files are processed.
		*Example: -x="php,inc"*
```

**--php**
```
	File path to the PHP binary to use for syntax checking.
	If this option is not used syntax checking will use the default PHP installtion to test syntax.
		*Example: --php="/path/to/php/binary/php"*
```

# Test Types
## Critical
Critical tests look for issues that will cause broken code, compilation errors, or otherwise create code that works in unintended manors.

## Nuance
Nuance tests look for issues that might cause silent underisable code behavior.  These tests can report many false positives as they can not determine the intent of the code being checked.

## Syntax
A basic command line based syntax checker that checks all files for standard syntax issues.  This is useful for double checking work after making many mass find and replace operations.  Please note that syntax checking adds a significant increase to processing time especially for large code bases.  To run without syntax checking use the -t option and omit syntax; -t="critical,nuance"
