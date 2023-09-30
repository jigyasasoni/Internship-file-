# Regex Assignment 

Write a RegEx pattern in python program to check that a string contains only a certain set of characters (in this case a-z, A-Z and 0-9).


```python
import re
def is_allowed_specific_char(string):
    charRe = re.compile(r'[^a-zA-Z0-9]')
    string = charRe.search(string)
    return not bool(string)

print(is_allowed_specific_char("ABCDEFabcdef123450")) 
print(is_allowed_specific_char("*&%@#!}{"))
```

    True
    False
    


```python
# Write a RegEx pattern that matches a string that has an a followed by zero or more b's
import re
def text_match(text):
        patterns = '^a(b*)$'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')
print(text_match("ac"))
print(text_match("abc"))
print(text_match("a"))
print(text_match("ab"))
print(text_match("abb"))
```

    Not matched!
    Not matched!
    Found a match!
    Found a match!
    Found a match!
    


```python
# Write a RegEx pattern that matches a string that has an a followed by one or more b's
import re
def text_match(text):
        patterns = 'ab+?'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')

print(text_match("ab"))
print(text_match("abc"))
```

    Found a match!
    Found a match!
    


```python
# - Write a RegEx pattern that matches a string that has an a followed by zero or one 'b'.
import re
def text_match(text):
        patterns = 'ab?'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')

print(text_match("ab"))
print(text_match("abc"))
print(text_match("abbc"))
print(text_match("aabbc"))
```

    Found a match!
    Found a match!
    Found a match!
    Found a match!
    


```python
# Write a RegEx pattern in python program that matches a string that has an a followed by three 'b'.

import re
def text_match(text):
        patterns = 'ab{3}?'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')

print(text_match("abbb"))
print(text_match("aabbbbbc"))
```

    Found a match!
    Found a match!
    


```python
# - Write a RegEx pattern in python program that matches a string that has an a followed by two to three 'b'.
import re
def text_match(text):
        patterns = 'ab{2,3}'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')
print(text_match("ab"))
print(text_match("aabbbbbc"))
```

    Not matched!
    Found a match!
    


```python
# Write a Python program that matches a string that has an 'a' followed by anything, ending in 'b'.
import re
def text_match(text):
        patterns = 'a.*?b$'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')

print(text_match("aabbbbd"))
print(text_match("aabAbbbc"))
print(text_match("accddbbjjjb"))
```

    Not matched!
    Not matched!
    Found a match!
    


```python
# Write a RegEx pattern in python program that matches a word at the beginning of a string.
import re
def text_match(text):
        patterns = '^\w+'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')

print(text_match("The quick brown fox jumps over the lazy dog."))
print(text_match(" The quick brown fox jumps over the lazy dog."))
```

    Found a match!
    Not matched!
    


```python
# Write a RegEx pattern in python program that matches a word at the end of a string.
import re
def text_match(text):
        patterns = '\w+\S*$'
        if re.search(patterns,  text):
                return 'Found a match!'
        else:
                return('Not matched!')

print(text_match("The quick brown fox jumps over the lazy dog."))
print(text_match("The quick brown fox jumps over the lazy dog. "))
print(text_match("The quick brown fox jumps over the lazy dog "))
```

    Found a match!
    Not matched!
    Not matched!
    


```python
# Write a RegEx pattern in python program to find all words that are 4 digits long in a string.
import re
text = 'The quick brown fox jumps over the lazy dog.'
print(re.findall(r"\b\w{4,}\b", text))
```

    ['quick', 'brown', 'jumps', 'over', 'lazy']
    


```python

```
