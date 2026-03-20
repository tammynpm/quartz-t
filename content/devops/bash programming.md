---
title: bash
tags: [bash, scripting]
draft: true
date: 2026-03-17
---


```shell
read X
read Y
if ((X > Y)); then
    echo "X is greater than Y"
elif ((X == Y)); then 
    echo "X is equal to Y"
else 
    echo "X is less than Y"
fi

```

read 1 character from input, compare it to the characters {'y', 'n'}
trick: use lowercase `"${variable,,}"`

```shell
read -n 1 x
x="${x,,}"

if [[ "$x" == "y" ]]; then
    echo "YES"
elif [[ "$x" == "n" ]]; then
    echo "NO"
fi
```



```shell
read x
read y
read z

if [[ "$x" == "$y" && "$y" == "$z" ]]; then
    echo "EQUILATERAL"
elif [[ "$x" == "$y" || "$y" == "$z" || "$x" == "$z" ]]; then
    echo "ISOSCELES"
else 
    echo "SCALENE"
fi

```