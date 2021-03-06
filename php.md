## General

* If a line is broken at a `&&` or `||` operator, the operator must be on the line broken. See `if` example below.
* Comments should generally use `//` and `/** */` should only be used for documentation blocks.
* British spelling is preferred in documentation and comments, while American spelling must be used with identifiers (such as class names and variable names). `analyzer` not `analyser` in identifiers while preferring `behaviour` over `behavior` in comments.
* All documentation within a documentation block must be spelled correctly (prefer British spelling) and have periods at the end of sentences.
* Exception messages may omit periods, but should still follow the same spelling convention.
* Single quotes should be preferred and double quotes should only be used when necessary.
* Heredoc and nowdoc should only be used when absolutely necessary.
* Use closures minimally.

## PHP tags

* Use `<?php` in code. If template files are in PHP, template files can use short tags (`<?=`).
* Never include the closing `?>` at the end of a file.

## Keywords

* The keywords `true`, `false` and `null` should always be in lower-case.
* Control structure keywords such as `if` and `while` must have a space after. They may not be treated like functions.
* The `array` keyword must always be written in lower-case.

### `if`, `else if`, `else`, `try`, `catch`

Note that `elseif` is not mentioned here because it is not allowed.

The opening bracket must always be on the same line as the statement. The end bracket must always be on a separate line. If there is to be an `else` or `else if` that shall be on the next line, aligned with the bracket.

Similarly, with `try`/`catch`, `catch` should always be on the next line after the previous `}` aligned with it.

Brackets are not optional in any case.

This deviates from Squiz control structures standards.

```php
<?php
if ($expr1) {

}
else if ($expr2) {

}
else {

}

try {

}
catch (\Exception $e) {

}

// Example of not doing anything but catching exception
try {
  $this->badMethodCall();
}
catch (\Exception $e) {
}
```

The `:` version of these structures is allowed only in PHP templates.

```php
<?php if ($exp): ?>
  <div class="special">Some text</div>
<?php endif; ?>
```

### `switch`, `case`

The `switch` opening bracket must be on the same line as the statement. A new line must be made after. Each `case` shall be indented 4 spaces and all the case's statements must be indented 4 spaces. Each `case` statement must have a blank line after its final statement. Fall throughs are allowed. `default` is not required.

```php
<?php
switch ($exp) {
  case 1:
    echo 'First case';
    break;

  case 2:
    // Fall through
    echo 'Second case';

  case 3:
    break;

  // Optional
  default:
    break;
}
```

The `:` version of this is allowed in templates.

### `while`, `do while`

Brackets follow the same rule as `switch`. Each statement within this loop is indented 4 spaces.

```php
<?php
while ($exp) {
  // body
}
```

In a `do while` loop, the `while` statement shall be on the same line as the ending bracket of the `do` statement.

```php
<?php
do {
  // body
} while ($exp);
```

The `:` version of this is allowed in templates.

### `for`

Brackets follow the same rule as `switch`. Each statement within this loop is indented 4 spaces.

```php
<?php
for ($i = 0; $i < 10; $i++) {
  // body
}
```

Note the increment operator is after the variable, not before.

The `:` version of this is allowed in templates.

### `foreach`

```php
<?php
foreach ($values as $key => $value) {
  // body
}
```

The `:` version of this is allowed in templates.

Note the placement of the keywords, and the `=>` operator having exactly one space before and one space after.

### `try`,`catch`

In general, the exception variable name in the `catch` block should always be named `$e`.

```php
<?php
try {
  // body
}
catch (FirstException $e) {
  // body
}
catch (SecondException $e) {
  // body
}
```

## Type checking

Note that `==` is allowed in most cases.

* Use `===` for testing `true`, `false` and `null` (do not use `is_null()` or similar functions).
* Prefer using `===` for testing string and numeric values.
* Prefer using `instanceof` for objects.
* Use of `is_string()` is discouraged as this is generally not necessary to do.

## Classes

* A PhpDoc compatible comment block is required and must have at least the description, `@package` and `@license` tags.
* The `@license` tag can be a regular name, but if the license text is available on-line it should be a full URL.
* Must be named in `StudlyCase` style.
* Use of underscores in class names is not allowed.
* The `final` keyword is highly discouraged.
* Dynamically creating classes is not allowed.
* The opening bracket must be on the next line of the class declaration at column 1. It must have a space before its last identifier.
* The closing bracket must be on its own line at column 1.
* For `implements`, all interfaces being implemented must have a space after each comma.
* `extends` and `implements` part of the class declaration line must be all in one line up to 80 characters, at which point breaking is allowed provided the next lines are indented.
* If a class is composed of only static methods and is not intended to be instantiated publically, a private or protected, empty `__construct` method must be declared.

```php
<?php
/**
 * A class that is array-accessible (usable with foreach).
 *
 * @license http://www.opensource.org/licenses/MIT
 * @package MyPackage
 */
class ClassName extends AnotherClassName implements Countable, ArrayAccess
{
  /**
   * Private to prevent instantiation.
   *
   * @return ClassName A description of this is optional.
   */
  private function __construct() {
  }
}
```

## Properties

* Documentation blocks compatible with PhpDoc are required, regardless of visibility (only exception is callbacks).
* Visibility must always be specified explicitly.
* Constants that are not callbacks must be named in `UPPER_UNDERSCORE` style.
* `public` properties are highly discouraged. Always prefer to make a getter method (and setter method where necessary).
* A dynamic `length` property must be defined using the `__get()` magic method.
* Use of the `var` keyword is not allowed.
* Properties that are not constants must be named in `$camelCase` format.
* No C++-style `$m_memberName` or `$m_member_name` naming is allowed.
* Hungarian notation of any kind is banned.
* Abbreviations are generally discouraged.

```php
<?php
/**
 * A class description.
 *
 * @license http://www.opensource.org/licenses/MIT
 * @package MyPackage
 */
class ClassName {
  /**
   * The string being managed.
   *
   * @var string
   */
  private $str = '';

  /**
   * The __get() magic method.
   *
   * @param string $property_name Property name requested.
   * @return mixed If the property name is 'length', then the length of the
   *   string will be returned.
   */
  public function __get($property_name)
  {
    if ($property_name == 'length') {
      return strlen($this->str);
    }
  }
}
```

Note how the comment block for the method is indented when lines get close 80 characters.

## Methods

* Documentation blocks compatible with PhpDoc are required, regardless of visibility.
* Visibility must always be specified explicitly. Even if the method is public this is not optional.
* If the method is static, `public` must be before the `static` keyword.
* Method names must be in `camelCase`. Use of underscores is not allowed except for internal methods (which must be marked `@internal`).
* Prefixes should be used correctly:
  * `check` and `is` prefixes may only be used for methods that return boolean values.
  * `get` prefix may only be used for methods that return properties of the static class or object.
  * `set` prefix may only be used for methods that set properties/settings of the class properties of the object.
  * `add` prefix may only be to append to an array or similar property type in a class.
  * `remove` prefix may only be used to remove from an array or similar property type in a class.
  * `make` prefix may only be used when the method returns a generated string (such as HTML).
  * `show` and `print` prefixes may be used when the method does output data (uses `echo` or `print` and does not return a value)
* The `final` keyword is highly discouraged.
* The opening bracket must be on the next line of the method declaration aligned with the visibility keyword.
* The closing bracket must be on a separate line aligning up with the first character of the method declaration.
  * The exception to this is empty methods where the closing bracket may be adjacent to the opening bracket (see example above).
* Type hints such as `array` and class names are allowed and encouraged to be used.
* For object classes, methods that allow chaining (`return $this;`) is encouraged.
* Abbreviations are highly discouraged.
* Methods that must be public due to PHP constraints (such as callbacks) but are not intended to be used by the programmer generally must be marked with `@internal`.

```php
<?php
/**
 * A class description.
 *
 * @license http://www.opensource.org/licenses/MIT
 * @package MyPackage
 */
class mClassName {
  const getPropertyName = 'mClassName::getPropertyName';

  /**
   * The property name value.
   *
   * @var string
   */
  private static $property_name = '';

  /**
   * Gets the property name value.
   *
   * @return string The property value.
   */
  public static function getPropertyName()
  {
    return self::$property_name;
  }
}
```

## Functions

Same rules as above. Obviously the `final` rule does not apply here.

## Variables

Must be named in `$camelCase`. Other styles are not acceptable. Hungarian notation of any kind is banned. Abbreviations are discouraged, but are allowed. Examples: `$str`, `$i`, etc.

## Method and function arguments

The rules for variables apply here as well. Default arguments must always be at the end of the declaration.

## Character encoding

UTF-8 with no BOM is the only accepted encoding.

## Indenting

Use an indent of 4 spaces. Do not use tabs.

## Lines

UNIX line feed (LF) is the only accepted line ending.

Each file must have a blank new line at the end.

Lines have a soft limit of 80 characters, and staying within this range by breaking up code is encouraged. Example:

```php
<?php
if (strlen($var) > 80 && in_array($var, $set_of_values, TRUE) && $obj->isThisTooLong($param)) {
}
```

Instead use:

```php
<?php
if (strlen($var) > 80 &&
    in_array($var, $set_of_values, TRUE) &&
    $obj->isThisTooLong($param)) {
}
```

Note that `&&` are not on the next line but instead prior to the line break.

## PHP Templates

Comments should always be in PHP blocks, NOT HTML/XML comments unless the comment is truly intended to be exposed outside of the code.

Use one keyword. I tend to use `print`. `echo` is also allowed.

Always encode text where necessary. If using `fRequest` (Flourish) be sure to always call `fRequest::encode()` and not `fRequest::get()`.

This example is using `sTemplate` (Sutra).

```
<?php
/**
 * Available variables:
 * - $content - string - The content to display, already encoded.
 * - $title - string - Title of this page, already encoded.
 * - $site_name - string - The site name, already encoded.
 */
?>
<!DOCTYPE html>
<html>
  <head>
    <title><?php print $title; ?> | <?php print $site_name; ?></title>
  </head>
  <body>
    <?php // Since short tags are allowed here ?>
    <?= print $content; ?>
    <?= print fRequest::encode('some_parameter', 'string'); ?>

    <!-- A non-allowed comment, generally -->
  </body>
</html>
```
