---
tags:
  - todo
---
RegEx (Regular Expressions) are powerful text processing patterns.
The most common types are:

| Type                         | Description                   | Used In                                                   |
| ---------------------------- | ----------------------------- | --------------------------------------------------------- |
| PCRE (Perl Compatible RegEx) | Most powerful, modern standar | Python, PHP, R, JavaScript (partially), many modern tools |
| BRE (Basic RegEx)            | Older Unix Standard           | `grep`, `sed` without `-E`                                |
| ERE (Extended RegEx)         | Adds `+`, `?`, `\|`, `()`     | `egrep`, `grep -E`, `sed -E`                              |

## PCRE syntax

Basic Patterns:

| Character | Match                                 | Example                         |
| --------- | ------------------------------------- | ------------------------------- |
| `.`       | any single character (except newline) | `p.ll` -> pill, pull            |
| `\d`      | digit from 0 to 9                     | `\d` -> 1, 2, 5                 |
| `\D`      | NOT digit                             | `\D` -> A, =, i                 |
| `\w`      | word character (a-zA-Z0-9_)           | `\w` -> admin2, web_user, apple |
| `\W`      | NOT word character                    | `\W` ->     , &, ^              |
| `\s`      | whitespace                            | `\s` ->                         |
| `\S`      | NOT whitespace                        | `\S` -> hello, ., =, 22         |

Character Classes:

| Class          | Description                           | Example                |
| -------------- | ------------------------------------- | ---------------------- |
| `[abc]`        | a, b or c                             | `h[ai]` -> ha, hi      |
| `[^abc]`       | NOT a, b or c (**^ means NOT**)       | `h[^ai]` -> ho         |
| `[0-9]`        | any digit from 0 to 9                 | `n[0-9]` -> n0, n2, n7 |
| `[a-z]`        | any lowercase letter                  | `[a-z]0` -> a0, n0     |
| `[A-Z]`        | any uppercase letter                  | `[A-Z]` -> A, O, K     |
| `[\[\]]`       | \[ or ] (need to escape)              | `[\[\]]` -> ], \[      |
| `[.*+?{}_-]`   | you can put any special symbol        | `A[-_]` -> -, _        |
| `[a-zA-Z0-9_]` | you can combine the characters inside | `[aou_+]` -> o, +      |

Basic Quantifiers:

| Quantifier | Description                                |
| ---------- | ------------------------------------------ |
| `A*`       | 0 or more A (as many as possible - GREEDY) |
| `A+`       | 1 or more A (as many as possible - GREEDY) |
| `A?`       | 0 or 1 A (optional)                        |
| `A{3}`     | exactly 3 A                                |
| `A{3,}`    | 3 or more A                                |
| `A{3,5}`   | between 3 and 5 A                          |

Lazy Quantifiers:

| Quantifier | Description                             |
| ---------- | --------------------------------------- |
| A*?        | 0 or more A (as few as possible - LAZY) |
| `A+?`      | 1 or more A (as few as possible - LAZY) |
| `A??`      | 0 or 1 A (prefers 0 - LAZY)             |
| `A{3,5}`   | between 3 and 5 A (prefers 3 - LAZY)    |

Line Anchors:

| Anchor | Description          | Example                   |
| ------ | -------------------- | ------------------------- |
| `^`    | start of line/string | `^[a-zA-Z]{1,5}` -> Hello |
| `$`    | end of line/string   | `END$` -> END             |

Word Boundaries:

| Character | Description                                             |
| --------- | ------------------------------------------------------- |
| `\b`      | Included in a word (between `\w` and `\w` or start/end) |
| `\B`      | NOT word boundary                                       |

Alternation & Groups

| Example          | Result                      |
| ---------------- | --------------------------- |
| cat\|dog         | "cat" or "dog"              |
| red\|blue\|green | "red" or "blue" or "green"  |
| gr(a\|e)y        | "gray" or "grey"            |
| (abc)+           | one or more "abc" sequences |
| (ab\|cd)+        | one or more "ab" or "cd"    |
Non-Capturing Groups `(?: )` does the same but don't save the match, is more efficient.

==Backreferences, named groups TODO.==

Lookarounds:

| Example       | Description                               |
| ------------- | ----------------------------------------- |
| `foo(?=bar)`  | Match "foo" only if followed by "bar"     |
| `foo(?!bar)`  | Match "foo" only if NOT followed by "bar" |
| `(?<=foo)bar` | Match "bar" only if preceded by "foo"     |
| `(?<!foo)bar` | Match "bar" only if NOT preceded by "foo" |
==The differences deeply TODO==

The main difference it's that BRE has only few features, and need to escape symbols to show.
ERE is close to PCRE.