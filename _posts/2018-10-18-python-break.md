---
title: Python Break
author: bulafish
date: 2018-10-18 +0800
categories: [Programming]
tags: [Python]
---

What break does is it will break/stop the loop or code from continuing down when certain condition is met!  This is very important and helpful cause when in a loop, if code is not break when condition is met, the loop will just keep running till the end.  Which lead to waste of compute resource.  Where if a break is set, the loop will stop, so the remaining loops will not run, thus saving the compute resource.

Coding example `without` break:<br>
Let's say we want to guess the magic number, which is 3. So in a for loop, it will loop from 1 to 10 and print out designed line at number 3, but the loop will continue loop all the way till the end, which leads to wasting the resources.
![python break](/assets/images/2018101801.png)

```python
magNum=3
print(" ")

for x in range(1,11):
    if x == magNum:
        print(" ")
        print(x, "is the magic number!\n")
    elif x < magNum:
        print("Guess again!")
    else:
        print("unnecessary runs")
```


Coding example `with` break:<br>
Now let's see the difference with break in code.  We can see clearly that the loop breaks at the condition, which is number 3, and the rest of loops are not runned!
![python break](/assets/images/2018101802.png)
```python
magNum=3
print(" ")

for x in range(1,11):
    if x == magNum:
        print(" ")
        print(x, "is the magic number!\n")
        break
    elif x < magNum:
        print("Guess again!")
    else:
        print("unnecessary runs")
```

Therefore, use break wisely and for sure, it will save a lot of troubles and works for your SysOps! XD

REFERENCES:
<br>[Python Programming Tutorial - 10 - Comments and Break](https://www.youtube.com/watch?v=k6rkvgQkW04&list=PL6gx4Cwl9DGAcbMi1sH6oAMk4JHw91mC_&index=10)
