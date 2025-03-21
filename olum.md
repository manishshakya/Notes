#awk

# Awk

## Introduction

The basic function of awk is to search files for lines (or other units of text) that contain certain patterns. When a line matches one of the patterns, awk performs specified actions on that line. awk continues to process input lines in this way until it reaches the end of the input files.

Programs in awk are different from programs in most other languages, because awk programs are data driven (i.e., you describe the data you want to work with and then what to do when you find it).

Syntactically, a rule consists of a pattern followed by an action.

```
pattern { action }
pattern { action }

```

- Programs in awk consist of pattern–action pairs
- An action without a pattern always runs. The default action for a pattern without one is '{print $0}'
- Comments in awk programs start with ‘#’ and continue to the end of the same line.
- Be aware of quoting issues when writing awk programs as part of a larger shell script (or MS-Windows batch file).
- You may use backslash continuation to continue a source line. Lines are automatically continued after a comma, open brace, question mark, colon ‘||’, ‘&&’, do, and else.

## Example

```
awk '/li/ {print $0};/zo/{print $0w}' mail-list
```

## Shell Quoting issues

For short to medium-length awk programs, it is most convenient to enter the program on the awk command line. This is best done by enclosing the entire program in single quotes.

## An Example with Two Rules

The awk utility reads the input files one line at a time. For each line, awk tries the patterns of each rule. If several patterns match, then several actions execute in the order in which they appear in the awk program. If no patterns match, then no actions run.

# Regular Expression

A regular expression, or regexp, is a way of describing a set of strings. Because regular expressions are such a fundamental part of awk programming, their format and use deserve a separate chapter.
A regular expression enclosed in slashes (‘/’) is an awk pattern that matches every input record whose text belongs to that set.

The two operators ‘~’ and ‘!~’ perform regular expression comparisons. Expressions using these operators can be used as patterns, or in if, while, for, and do statements.
