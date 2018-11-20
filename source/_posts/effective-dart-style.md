---
title: Effective Dart - Style
date: 2018-07-25 17:03:00
tags:
  - Dart
categories:
  - Dart
toc: ture
---

A surprisingly important part of good code is good style. Consistent naming, ordering, and formatting helps code that is the same look the same. It takes advantage of the powerful pattern-matching hardware most of us have in our ocular systems. If we use a consistent style across the entire Dart ecosystem, it makes it easier for all of us to learn from and contribute to each others’ code.

<!---more--->

## Identifiers

Identifiers come in three flavors in Dart.

- `UpperCamelCase` names capitalize the first letter of each word, including the first.
- `lowerCamelCase` names capitalize the first letter of each word, *except* the first which is always lowercase, even if it’s an acronym.
- `lowercase_with_underscores` use only lowercase letters, even for acronyms, and separate words with `_`.

### DO name types using `UpperCamelCase`.

Classes, enums, typedefs, and type parameters should capitalize the first letter of each word (including the first word), and use no separators.

```dart
class HttpRequest { ... }
typedef Predicate = bool Function<T>(T value);
```

This even includes classes intended to be used in metadata annotations.

```dart
class Foo {
  const Foo([arg]);
}

@Foo(anArg)
class A { ... }

@Foo()
class B { ... }
```

If the annotation class’s constructor takes no parameters, you might want to create a separate `lowerCamelCase` constant for it.

```dart
const foo = Foo();

@foo
class C { ... }
```

### DO name libraries and source files using `lowercase_with_underscores`.

Some file systems are not case-sensitive, so many projects require filenames to be all lowercase. Using a separating character allows names to still be readable in that form. Using underscores as the separator ensures that the name is still a valid Dart identifier, which may be helpful if the language later supports symbolic imports.

```dart
library peg_parser.source_scanner;

import 'file_system.dart';
import 'slider_menu.dart';
```

### DO name import prefixes using `lowercase_with_underscores`.

```dart
import 'dart:math' as math;
import 'package:angular_components/angular_components'
    as angular_components;
import 'package:js/js.dart' as js;
```

### DO name other identifiers using `lowerCamelCase`.

Class members, top-level definitions, variables, parameters, and named parameters should capitalize the first letter of each word *except* the first word, and use no separators.

```dart
var item;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}
```

### PREFER using `lowerCamelCase` for constant names.

In new code, use `lowerCamelCase` for constant variables, including enum values. In existing code that uses `SCREAMING_CAPS`, you may continue to use all caps to stay consistent.

```dart
const pi = 3.14;
final urlScheme = RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = Random();
}
```

### DO capitalize acronyms and abbreviations longer than two letters like words.

Capitalized acronyms can be hard to read, and multiple adjacent acronyms can lead to ambiguous names. For example, given a name that starts with `HTTPSFTP`, there’s no way to tell if it’s referring to HTTPS FTP or HTTP SFTP.

To avoid this, acronyms and abbreviations are capitalized like regular words, except for two-letter acronyms. (Two-letter *abbreviations* like ID and Mr. are still capitalized like words.)

good example:

```dart
HttpConnectionInfo
uiHandler
IOStream
HttpRequest
Id
DB
```

bad example:

```dart
HTTPConnection
UiHandler
IoStream
HTTPRequest
ID
Db
```

### DON’T use prefix letters

Because Dart can tell you the type, scope, mutability, and other properties of your declarations, there’s no reason to encode those properties in identifier names. 

good example:

```dart
defaultTimeout
```

bad example:

```dart
kDefaultTimeout
```

## Ordering

To keep the preamble of your file tidy, we have a prescribed order that directives should appear in. Each “section” should be separated by a blank line.

### DO place “dart:” imports before other imports.

```dart
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';

```

### DO place “package:” imports before relative imports.

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'util.dart';
```

### PREFER placing “third-party” “package:” imports before other imports.

If you have a number of “package:” imports for your own package along with other third-party packages, place yours in a separate section after the external ones.

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'package:my_package/util.dart';
```

### DO specify exports in a separate section after all imports.

```dart
import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
```

### DO sort sections alphabetically.

```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'foo.dart';
import 'foo/foo.dart';
```

## Formatting

Like many languages, Dart ignores whitespace. However, *humans* don’t. Having a consistent whitespace style helps ensure that human readers see code the same way the compiler does.

### DO format your code using `dartfmt`.

Formatting is tedious work and is particularly time-consuming during refactoring. Fortunately, you don’t have to worry about it. We provide a sophisticated automated code formatter called [dartfmt](https://github.com/dart-lang/dart_style) that does do it for you. We have [some documentation](https://github.com/dart-lang/dart_style/wiki/Formatting-Rules) on the rules it applies, but the official whitespace-handling rules for Dart are *whatever dartfmt produces*.

The remaining formatting guidelines are for the few things dartfmt cannot fix for you.

### CONSIDER changing your code to make it more formatter-friendly.

The formatter does the best it can with whatever code you throw at it, but it can’t work miracles. If your code has particularly long identifiers, deeply nested expressions, a mixture of different kinds of operators, etc. the formatted output may still be hard to read.

When that happens, reorganize or simplify your code. Consider shortening a local variable name or hoisting out an expression into a new local variable. In other words, make the same kinds of modifications that you’d make if you were formatting the code by hand and trying to make it more readable. Think of dartfmt as a partnership where you work together, sometimes iteratively, to produce beautiful code.

### AVOID lines longer than 80 characters.

Readability studies show that long lines of text are harder to read because your eye has to travel farther when moving to the beginning of the next line. This is why newspapers and magazines use multiple columns of text.

If you really find yourself wanting lines longer than 80 characters, our experience is that your code is likely too verbose and could be a little more compact. The main offender is usually `VeryLongCamelCaseClassNames`. Ask yourself, “Does each word in that type name tell me something critical or prevent a name collision?” If not, consider omitting it.

Note that dartfmt does 99% of this for you, but the last 1% is you. It does not split long string literals to fit in 80 columns, so you have to do that manually.

We make an exception for URIs and file paths. When those occur in comments or strings (usually in imports and exports), they may remain on a single line even if they go over the line limit. This makes it easier to search source files for a given path.

### DO use curly braces for all flow control structures.

Doing so avoids the [dangling else](http://en.wikipedia.org/wiki/Dangling_else) problem.

```dart
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}
```

There is one exception to this: an `if` statement with no `else` clause where the entire `if` statement and the then body all fit in one line. In that case, you may leave off the braces if you prefer:

```dart
if (arg == null) return defaultValue;
```

If the body wraps to the next line, though, use braces:

```dart
if (overflowChars != other.overflowChars) {
  return overflowChars < other.overflowChars;
}
```