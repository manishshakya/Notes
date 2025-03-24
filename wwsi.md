#c++
#Cplusplus
#variables
#pointer
#references
#string
#array
#vectors

# string

A string is a variable-length sequence of characters.

```C++
#include <string>
using std::string;
```

```C++
string s1; //Default initialization; s1 is the empty string.
string s2(s1); //s2 is a copy of s1.
string s2 = s1; //Equivalent to s2(s1), s2 is a copy of s1.
string s3("value"); //s3 is a copy of the string literal, not including the null.
string s3 = "value"; // Equivalent to s3("value"), s3 is a copy of the string literal.
string s4(n, ’c’);  //Initialize s4 with n copies of the character ’c’.
```

## Operations on strings

```C++
os << s; // Writes s onto output stream os. Returns os.
is >> s; // Reads whitespace-separated string from is into s. Returns is.
getline(is, s); // Reads a line of input from is into s. Returns is.
s.empty(); // Returns true if s is empty; otherwise returns false.
s.size(); // Returns the number of characters in s.
s[n]; // Returns a reference to the char at position n in s; positions start at 0.
s1 + s2; // Returns a string that is the concatenation of s1 and s2.
s1 = s2; // Replaces characters in s1 with a copy of s2.
s1 == s2; //The strings s1 and s2 are equal if they contain the same characters.
s1 != s2; // Equality is case-sensitive.
//<, <=, >, >= Comparisons are case-sensitive and use dictionary ordering.
```

## Assignment of string

```C++
string st1(10, ’c’), st2; // st1 is cccccccccc; st2 is an empty
string st1 = st2; // assignment: replace contents of st1 with a copy of st2
// both st1 and st2 are now the empty string
```

## Processing string

```C++
string str("some string");
// print the characters in str one character to a line
for (auto c : str) { // for every char in str
  c = isupper(c);
  cout << c << endl; // print the current character followed by a newline
}
// str is not modified


for (auto &c : str) { // for every char in str
  c = isupper(c);
  cout << c << endl; // print the current character followed by a newline
}
// str is  modified

for (decltype(str.length()) i=0; i <str.length(); i++) { // for every char in str
  str[i] = toupper(str[i]);
  cout << c << endl; // print the current character followed by a newline
}
```
