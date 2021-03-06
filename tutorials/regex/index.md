---
title: "Regex - regular expressions in PHP"
read_time: "5 min"
updated: "february 22, 2015"
group: "tutorials"
permalink: "/tutorials/regular-expressions/index.html"
layout: "article"
menu:
    - title: "Brief history"
      href: "#brief-history"
    - title: "Common regex usage in PHP"
      href: "#common-regex-usage-in-php"
    - title: "&nbsp;&nbsp;&nbsp;Matching"
      href: "#matching"
    - title: "&nbsp;&nbsp;&nbsp;Replacing"
      href: "#replacing"
    - title: "Common regex usage in JavaScript"
      href: "#common-regex-usage-in-javascript"
    - title: "&nbsp;&nbsp;&nbsp;Matching"
      href: "#matching-1"
    - title: "&nbsp;&nbsp;&nbsp;Replacing"
      href: "#replacing-1"
    - title: "&nbsp;&nbsp;&nbsp;Caveats of regex in JavaScript"
      href: "#caveats-of-refex-in-javascript"
    - title: "Basics of regex patterns"
      href: "#basics-of-regex-patterns"
    - title: "&nbsp;&nbsp;&nbsp;Sockets analogy"
      href: "#sockets-analogy"
    - title: "Using regex for validation"
      href: "#using-regex-for-validation"
    - title: "&nbsp;&nbsp;&nbsp;When not to use regex for validation?"
      href: "#when-not-to-use-regex-for-validation"
    - title: "&nbsp;&nbsp;&nbsp;Regex validation"
      href: "#regex-validation"
    - title: "&nbsp;&nbsp;&nbsp;Finding and replacing"
      href: "#finding-and-replacing"
    - title: "Resources"
      href: "#resources"
---

Regular expressions (abbreviated regex) are sequences of characters that form search patterns. They are mainly used in pattern matching with strings.

## Brief history

* It started in 1940s-60s with lots of smart people talking about regular 
* 1970s g/re/p
* 1980 Perl and Henry Spencer
* 1997 PCRE (Perl Compatible Regular Expressions)
  That's where it really took off and when we talk about regex today that's what we're talking about. PCRE has libraries for almost every language, it looks the same everywhere and it is very useful.

## Common regex usage in PHP

In PHP we have three main regular expression mathes with PCRE not EREG

### Matching

This returns 1 if match is found, 0 if not, false if error occurs:

```php
int preg_match (
    string $pattern,
    string $subject [,
    array &$matches [,
    int $flags = 0 [,
    int $offset = 0
]]])
```

This returns number of matches found:

```php
int preg_match_all (
    string $pattern,
    string $subject [,
    array &$matches [,
    int $flags = PREG_PATTERN_ORDER [,
    int $offset = 0
]]])
```

### Replacing

This returns the replaced string or array (based on the $subject):

```php
mixed preg_replace (
    mixed $pattern,
    mixed $replacement,
    mixed $subject [,
    int $limit = -1 [,
    int $count
    ]])
```

## Common regex usage in JavaScript

In JavaScript regular expressions looks pretty much the same as in PHP.

### Matching

Returns an array of matches or null if no matches were found:

```javascript
string.match(RegExp);
```

### Replacing

Returns the string with the replacements performed:

```javascript
string.replace(RegExp, replacement);
```

### Caveats about JavaScript's regex

* No "single-line" or DOTALL mode. (The dot never matches new line.)
* No lookbehind support
* Same methods for regex and non-regex matching and replacing


## Basics of regex patterns

Problem: finding email addresses in codebase.
Goal: /[\w.+-]+@[a-z0-9-]+(\.[a-z0-9-]+)*/i

### Sockets analogy

Regular expressions are built from two type of characters:

* special characters: .\[]?*+{}()^$/
* literals

Imagine your input strings as bolts and your pattern as a set of sockets (in order).

Let's take a look at what special characters do:

Backslash character `\\` can escape other special character in regular expression:

Square brackets

[abc] - matches any `a`, `b` or `c`.

Quick review of special characters:

* The Dot and the \w - `.`
  Matches everything but new lines. If you want to match a dot and only a dot escape it like \
  \w matches letters, numbers, and the underscore
* Square brackets `[]`
  Matches characters inside the brackets. Supports ranges.
  [abc] 'a', 'b' or 'c'
  [a-z] Lowercase letters
  [0-9] Any single digit
* Optional `?`
  The `?` matches 0 or 1
* The star `*˙
  The star matches 0 or more
* The Plus `+`
  Matches 1 or more
* Curly brackets `{}`
  Min and Max ranges.
  {1,} at least 1
  {1,3} 1 through 3
  {1,64} 1 through 64

Let's put all this together to get regex for email addresses:

```
/[\w.+-]+@[a-z0-9-]+(\.[a-z0-9-]+)*/i
```

![alt text](./image.png "Regex for email addresses")

How this looks in PHP:

```php
preg_match_all(
    "/[\w.+-]+@[a-z0-9-]+(\.[a-z0-9-]+)*/i",
    $input_lines,
    $output_array
);
```

## Using regex for validation

Problem: make sure input is what we expect
Goal 1: /[^\[\]\w$.]/
Goal 2: /^[0-9]{1,2}[dwmy]$/

Regex is great at finding things but you need to know what you're looking for. When you validate you get to determine exactly what you want.

### When not to use regex for validation?

Many cases are better handled with PHP's filter_var. For example validating emails should be done with PHP built-in filters. For example:

```php
filter_var(
    'bob@example.com',
    FILTER_VALIDATE_EMAIL
)
```

### Regex validation

For starting and ending regex you use anchors:

* `^` - the hat that indicates start of the string
* `$` - the dollar sign that indicates end of string

```php
if (!preg_match(
        "%^[0-9]{1,2}[dwmy]$%",
        $_POST["subscription_frequency"])
    ) {
    $isError = true;
    }
)
```

Negated character classes

* `[^abc]` - anything except a,b, or c, including new lines.

Example that ensures input only contains alphanumeric, dash, dot, underscore

```php
if (preg_match("/[^0-9a-z-_.]/i, $productCode)) {
    $isError = true;
}
```

## Finding and replacing

Problem: Link @mentions and #tags
Goal: /\B@([\w]{2,})/i


## Resources

* PHP.net resources:
  * [Pattern syntax](http://www.php.net/manual/en/reference.pcre.pattern.syntax.php)
  * [Modifiers](http://www.php.net/manual/en/reference.pcre.pattern.modifiers.php)
  * [Functions](http://www.php.net/manual/en/ref.pcre.php)
* Regex online tools:
  * [Rexex101](https://regex101.com/) - create, debug, test and have your expressions explained for PHP, PCRE, JavaScript and Python. The website also features a community where you can share useful expressions.
  * [Debuggex](https://www.debuggex.com/) - online regex visualization tool
  * [regexper](http://regexper.com/) - regular expression visualizer using railroad diagrams
  * [PHP Live Regex](http://www.phpliveregex.com/) - live regular expression tester for PHP