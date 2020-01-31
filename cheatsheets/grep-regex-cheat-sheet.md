A regular expression is a pattern that describes a set of strings. Regular expressions are constructed analogously to arithmetic expressions, by using various operators to combine smaller expressions. @command{grep} understands two different versions of regular expression syntax: "basic" and "extended". In GNU @command{grep}, there is no difference in available functionality using either syntax. In other implementations, basic regular expressions are less powerful. The following description applies to extended regular expressions; differences for basic regular expressions are summarized afterwards.

The fundamental building blocks are the regular expressions that match a single character. Most characters, including all letters and digits, are regular expressions that match themselves. Any metacharacter with special meaning may be quoted by preceding it with a backslash. A list of characters enclosed by `[` and `]` matches any single character in that list; if the first character of the list is the caret `^`, then it matches any character not in the list. For example, the regular expression `[0123456789]` matches any single digit. A range of ASCII characters may be specified by giving the first and last characters, separated by a hyphen.

Finally, certain named classes of characters are predefined, as follows. Their interpretation depends on the LC_CTYPE locale; the interpretation below is that of the POSIX locale, which is the default if no LC_CTYPE locale is specified.


`[:alnum:]`

Any of `[:digit:]` or `[:alpha:]`

`[:alpha:]`

Any letter:
a b c d e f g h i j k l m n o p q r s t u v w x y z,
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z.

`[:blank:]`

Space or tab.

`[:cntrl:]`

Any character with octal codes 000 through 037, or DEL (octal code 177).

`[:digit:]`

Any one of 0 1 2 3 4 5 6 7 8 9.

`[:graph:]`

Anything that is not a `[:alnum:]` or `[:punct:]`.
`[:lower:]`

Any one of a b c d e f g h i j k l m n o p q r s t u v w x y z.

`[:print:]`

Any character from the `[:space:]` class, and any character that is not in the `[:graph:]` class.

`[:punct:]`

Any one of `! " # $ % & `` ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ' { | } ~.`

`[:space:]`

Any one of CR FF HT NL VT SPACE.

`[:upper:]`

Any one of A B C D E F G H I J K L M N O P Q R S T U V W X Y Z.

`[:xdigit:]`

Any one of a b c d e f A B C D E F 0 1 2 3 4 5 6 7 8 9.


For example, `[[:alnum:]]` means `[0-9A-Za-z]`, except the latter form is dependent upon the ASCII character encoding, whereas the former is portable. (Note that the brackets in these class names are part of the symbolic names, and must be included in addition to the brackets delimiting the bracket list.) Most metacharacters lose their special meaning inside lists. To include a literal `]`, place it first in the list. Similarly, to include a literal `^`, place it anywhere but first. Finally, to include a literal `-`, place it last.

The period `.` matches any single character. The symbol `\w` is a synonym for `[[:alnum:]]` and `\W` is a synonym for `[^[:alnum]]`.

The caret `^` and the dollar sign `$` are metacharacters that respectively match the empty string at the beginning and end of a line. The symbols `\<` and `\>` respectively match the empty string at the beginning and end of a word. The symbol `\b` matches the empty string at the edge of a word, and `\B` matches the empty string provided it`s not at the edge of a word.

**A regular expression may be followed by one of several repetition operators:**

`?`

The preceding item is optional and will be matched at most once.

`*`


The preceding item will be matched zero or more times.

`+`

The preceding item will be matched one or more times.

`{n}`

The preceding item is matched exactly n times.

`{n,}`

The preceding item is matched n or more times.

`{n,m}`

The preceding item is matched at least n times, but not more than m times.
Two regular expressions may be concatenated; the resulting regular expression matches any string formed by concatenating two substrings that respectively match the concatenated subexpressions.



Two regular expressions may be joined by the infix operator `|`; the resulting regular expression matches any string matching either subexpression.

Repetition takes precedence over concatenation, which in turn takes precedence over alternation. A whole subexpression may be enclosed in parentheses to override these precedence rules.

The backreference `\n`, where n is a single digit, matches the substring previously matched by the nth parenthesized subexpression of the regular expression.

In basic regular expressions the metacharacters `?`, `+`, `{`, `|`, `(`, and `)` lose their special meaning; instead use the backslashed versions `\?`, `\+`, `\{`, `\|`, `\(`, and `\)`.

Traditional @command{egrep} did not support the `{` metacharacter, and some @command{egrep} implementations support `\{` instead, so portable scripts should avoid `{` in `egrep` patterns and should use `[{]` to match a literal `{`.

GNU @command{egrep} attempts to support traditional usage by assuming that `{` is not special if it would be the start of an invalid interval specification. For example, the shell command `egrep `{1`` searches for the two-character string `{1` instead of reporting a syntax error in the regular expression. POSIX.2 allows this behavior as an extension, but portable scripts should avoid it.



src: https://ftp.gnu.org/old-gnu/Manuals/grep-2.4/html_node/grep_6.html