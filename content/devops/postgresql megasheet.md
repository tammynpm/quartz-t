---
title: postgresql megasheet
tags: []
draft: true
date: 2026-03-17
---
#postgresl #database
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;

```

equality: single = 
booleanic operators: (literal) AND, OR

divisibility: `mod(a,b) = 0` where a is divisible by b

`length()` for length of strings
UNION select 

aggregation 

`LIKE ''` patterns. one of the most common one is to check the pattern of the first letter of a string. we can use `LIKE 'a%'` . in reverse, to check for the last letter, use `'%a'`. 

an alternative is to use regex but REGEXP is case sensitive. start `^[]`, end `[]$`. a shortcut is to use the `lower()` lowercase function. 