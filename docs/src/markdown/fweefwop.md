# Fweefwop CTF Writeup

## General

### Base2

Flag: `11101000`

```{title="Description"}
What is 0xe8 in binary? (use 8 bits)

The number given to us is in hex. So we can convert it into binary using python
```

```python
>> f"{0xe8:0b}"
'11101000'
```

### Base64 Abridged

Flag: `SSBsb3ZlIENURgo=`

```{title="Description"}
What is the result of base64 encoding of "I love CTF"?

We can use the `base64` command for encoding this into base64
```

```bash
echo "I love CTF" | base64
```

### Hex the way in

Flag: `fwopCTF{back_from_hex}`

```{title="Description"}
66 77 6f 70 43 54 46 7b 62 61 63 6b 5f 66 72 6f 6d 5f 68 65 78 7d
```

If you have played CTFs before you know this is hex. So just decode it. I ran it in bash using the command called `unhex` and got the output `fwopCTF{back_from_hex}`

```bash
unhex 66 77 6f 70 43 54 46 7b 62 61 63 6b 5f 66 72 6f 6d 5f 68 65 78 7d
```

### Touch the base 

Flag: `fwopCTF{base64_is_everywhere}`

```{title="Description"}
ZndvcENURntiYXNlNjRfaXNfZXZlcnl3aGVyZX0=
```

That is clearly base64. We can use the same tool used in [Base64 Abridged](#base64-abridged). To decode the base64 we will need to add a `-d` flag at the end of that tool. And if we run it then we will get the flag `fwopCTF{base64_is_everywhere}`.

```bash
echo "ZndvcENURntiYXNlNjRfaXNfZXZlcnl3aGVyZX0=" | base64 -d
```


## Crypto

### Silly Secret Sharing

Flag: `84333`

For this challenge I read a article in wikipedia and took a code from there, and then modified the code a bit and ran it. And tada! you get the flag!

[Wikipedia article on Shamir's Secret Sharing](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing)

Here's the code:
```python                                                                                               
_PRIME = 2 ** 127 - 1
                                                                                             
def _extended_gcd(a, b):
    """                                                                                     
    Division in integers modulus p means finding the inverse of the                                
    denominator modulo p and then multiplying the numerator by this                               
    inverse (Note: inverse of A is B such that A*B % p == 1) this can                                      
    be computed via extended Euclidean algorithm                                                           
    http://en.wikipedia.org/wiki/Modular_multiplicative_inverse#Computation                            
    """                                                                                       
    x = 0                                                                                    
    last_x = 1                                                                                   
    y = 1                                                                 
    last_y = 0                                                            
    while b != 0:                                                                                         
        quot = a // b
        a, b = b, a % b
        x, last_x = last_x - quot * x, x
        y, last_y = last_y - quot * y, y
    return last_x, last_y

def _divmod(num, den, p):
    """Compute num / den modulo prime p

    To explain what this means, the return value will be such that
    the following is true: den * _divmod(num, den, p) % p == num
    """
    inv, _ = _extended_gcd(den, p)
    return num * inv

def _lagrange_interpolate(x, x_s, y_s, p):
    """
    Find the y-value for the given x, given n (x, y) points;
    k points will define a polynomial of up to kth order.
    """
    k = len(x_s)
    assert k == len(set(x_s)), "points must be distinct"
    def PI(vals):  # upper-case PI -- product of inputs
        accum = 1
        for v in vals:
            accum *= v
        return accum
    nums = []  # avoid inexact division
    dens = []
    for i in range(k):
        others = list(x_s)
        cur = others.pop(i)
        nums.append(PI(x - o for o in others))
        dens.append(PI(cur - o for o in others))
    den = PI(dens)
    num = sum([_divmod(nums[i] * den * y_s[i] % p, dens[i], p)
               for i in range(k)])
    return (_divmod(num, den, p) + p) % p

def recover_secret(shares, prime=_PRIME):
    """
    Recover the secret from share points
    (x, y points on the polynomial).
    """
    if len(shares) < 2:
        raise ValueError("need at least two shares")
    x_s, y_s = zip(*shares)
    return _lagrange_interpolate(0, x_s, y_s, prime)

shares = [(20, 161013), (10, 122673)]
print(recover_secret(shares))
```

### Open Secret

Flag: `fwopCTF{I_found_the_alien_his_name_is_paul}`

So first I read [this](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) article on Wikipedia about Diffie Hellman Key exchange. Then I calculated `s` and xored base64 decoded flag with the key as bytes represantation of `s`.

Here's the code:
```python
from Crypto.Util.number import long_to_bytes                                                                                       
import base64                                                                                                       
from pwn import xor                                                       

p = 824717393                                          
g = 150357959    
A = 734947628         
b = 845023462         

B = pow(g, b, p)                       

s = pow(A, b, p)      
flag = 'T6ZBVGqFaF9gjkhLXL9Ke125S3tIvUdBR45GTVqOQEVEtHFNWo5eRVy9Uw=='
print(xor(base64.b64decode(flag), long_to_bytes(s)))
```

### Pretty Safe Password

Flag: `fwopCTF{pa55word}`

For this challenge I wrapped all of the passwords with `fwopCTF{}` in sublime text. Then I ran John the ripper on the hash using the following command:
```shell
$ john hash.txt --wordlist=10k-most-common.txt --format=Raw-MD5
```

## Web

### SQLI

Flag: `fwopCTF{Leaked_data_123}`

This challenge was just a basic SQL injection. I got the flag by just passing `' OR 1=1 --` as both password and username.

### SQLI But Filtered?

Flag: `fwopCTF{f1lt3rs_not_good_3n0ugh}`

This challenge is just advanced version of the [SQLI](#sqli) Challenge. So First I tried the payload on that challenge but it didn't work. So I tried more of them and finally the `' || '1'='1` as admin and password got me the flag.

## Reversing

### Reversing Python 1

Flag: `fwopCTF{no_python_required}`

Code given with the challenge:
```python
print("Enter the flag and I will check it for you.")
enteredFlag = input()
if enteredFlag == "fwopCTF{no_python_required}":
    print("Your flag is correct!")
else:
    print("Your flag is incorrect. :(")
```

The flag is right there in the `if` check. You can just submit that as the flag.

### Reversing Python 2

Flag:  `fwopCTF{perhaps_this_is_the_flag}`

Code given with the challenge:
```python
var1 = "fwopCTF{this_might_be_the_flag}"
var2 = "fwopCTF{this_could_be_the_flag}"
var3 = "fwopCTF{this_potentially_is_the_flag}"
var4 = "fwopCTF{perhaps_this_is_the_flag}"
var5 = "fwopCTF{could_this_be_the_flag?}"
var6 = "fwopCTF{this_probably_isn't_the_flag}"

print("Enter the flag and I will check it for you.")
enteredFlag = input()
if enteredFlag == var4:
    print("Your flag is correct!")
else:
    print("Your flag is incorrect. :(")
```

So the `if` block prints `Your flag is correct!` when the entered flag is equal to `var4`. So we can copy the text in `var4` and submit as the flag.


### Reversing Python 3

Flag: `fwopCTF{see_seesaw_sheshore_see_sheshore_seasells_shells}`

Code given with the challenge:
```python
var1 = "fwopCTF{sheshore_seasore_shells_seasore_shesore_seashells_seashore}"
var3 = "fwopCTF{sheshore_seashore_seesaw_seesaw_shesore_seasore_seasells}"
var2 = "fwopCTF{shesore_seashells_seashells_seasells_shells_seesaw_seasells}"
var1 = "fwopCTF{seashore_see_seashells_shesore_seesaw_sheshore_shells}"
var3 = "fwopCTF{seashells_shells_seasells_seasells_she_shells_see}"
var1 = "fwopCTF{she_seasells_seashore_seashore_seasore_shells_seashore}"
var2 = "fwopCTF{seasore_shells_shells_seashells_sheshore_she_seasells}"
var3 = "fwopCTF{seasore_seesaw_see_seashore_seashells_seashore_seesaw}"
var3 = "fwopCTF{see_seesaw_sheshore_see_sheshore_seasells_shells}"
var1 = "fwopCTF{seashells_she_seasore_seashore_shesore_shesore_seasells}"
var2 = "fwopCTF{seasells_seashells_shesore_seasore_seasore_sheshore_seasore}"
var1 = "fwopCTF{she_seesaw_she_seashore_seasells_she_seesaw}"
var1 = "fwopCTF{she_seashore_shesore_sheshore_sheshore_seesaw_she}"
var2 = "fwopCTF{seasells_she_seasore_she_seashore_seashore_seashells}"
var1 = "fwopCTF{shesore_see_see_seesaw_sheshore_seashells_seashells}"

print("Enter the flag and I will check it for you.")
enteredFlag = input()
if enteredFlag == var3:
    print("Your flag is correct!")
else:
    print("Your flag is incorrect. :(")
```

So in this challenge the `if` block checks if the entered text is equal to `var3` And we can see that `var3` has changed many times in the code. So we can basically just copy the text in the last occurence of `var3` which is `fwopCTF{see_seesaw_sheshore_see_sheshore_seasells_shells}` and we can submit that as the flag.


### Reversing Python 4

Flag: `fwopCTF{then_if_then_else_if_if}`

Code given with this challenge:
```python
print("Enter the flag and I will check it for you.")
enteredflag = input()

var1 = 15
var2 = 4
var3 = 9

if var1 < var3:
    var2 = var1 + var3
elif var3 > var2:
    var1 = (var1 * var2) - var2
else:
    var3 = var2 * var1

if var1 - var2 * var3 >= 10:
    var1 = 15 - var3
    var3 = var1 * var2
else:   
    var2 = var1 / var2
    var3 = var3 * 2


if var1 + var2 > var3:
    if var2 + 2 != var1:
        correctflag = "fwopCTF{else_elif_if_elif_else_else}"
    elif var3 - var1 >= var2 * var2:
        correctflag = "fwopCTF{else_if_then_then_else_elif}"
    elif var3 + var1 + var2 <= var3 + var1 * var2:
        correctflag = "fwopCTF{then_else_else_then_if_if}"

var3 = (var3 * -1) + var1 + var2

if var3 + var2 + var1 * 2 >= 0:
    if var3 == var2 * -2:
        correctflag = "fwopCTF{else_then_if_then_elif_elif}"
    elif var2 * var1 - var3 == 3 * var1 + 5 * var2:
        correctflag = "fwopCTF{then_if_then_else_if_if}"
    else:
        correctflag = "fwopCTF{elif_if_if_then_elif_elif}"

if enteredflag == correctflag:
    print("Your flag is correct!")
else:
    print("Your flag is incorrect. :(")
```

So we can see that there is so many things goin on here. But we can ignore them all and just focus near the `if` statement. So we can see that if the entered text is equal to `correctflag` then it print `Your flag is correct!`. So we can printthe `correctflag` variable just before the if statement and thus we get the flag `fwopCTF{then_if_then_else_if_if}` by inputting anything.

```python
# if enteredflag == correctflag:
#     print("Your flag is correct!")
# else:
#     print("Your flag is incorrect. :(")
print(correctflag)
```


### Reversing Python 5

Flag: `fwopCTF{bonucleicryxriluoxe}`

Code given with the challenge:
```python
print("Enter the flag and I will check it for you.")
enteredFlag = input()
correctflag = "deoxyribonucleic_acid"
correctflag = correctflag[0:4] + correctflag[7:16] + correctflag[5:2:-1]
correctflag = correctflag[4:-4] + correctflag[3*4:-1] + correctflag[-1:0:-2]

if enteredFlag == correctflag:
    print("Your flag is correct!")
else:
    print("Your flag is incorrect. :(")
```

Same trick as [Reversing Python 4](#reversing-python-4). Comment the `if` statement and then print `correctflag` and wrap it with `fwopCTF{}`.

```python
# if enteredFlag == correctflag:
#     print("Your flag is correct!")
# else:
#     print("Your flag is incorrect. :(")
print(correctflag)
```

### Reversing Python 6

Flag: `fwopCTF{also_crypto_lol}`

Code given with the challenge:
```python
print("Enter the flag and I will check it for you.")
enteredFlag = input()
alphabet = "abcdefghijklmnopqrstuvwxyz{}_ABCDEFGHIJKLMNOPQRSTUVWXYZ"
code = [5, 22, 14, 15, 31, 48, 34, 26, 0, 11, 18, 14, 28, 2, 17, 24, 15, 19, 14, 28, 11, 14, 11, 27]
correctflag = ""
for currentNum in code:
    correctflag = correctflag + alphabet[currentNum]

if enteredFlag == correctflag:
    print("Your flag is correct!")
else:
    print("Your flag is incorrect. :(")
```

This is also same as [Reversing Python 4](#reversing-python-4) and [Reversing Python 5](#reversing-python-5). Just print the `correctflag`

```python
# if enteredFlag == correctflag:
#     print("Your flag is correct!")
# else:
#     print("Your flag is incorrect. :(")
print(correctflag)
```

### Reversing Python 7 

Flag: `its_time_t;qfwopCTF{`

Code which we need to reverse:
```python
print("Enter the flag and I will check it for you.")
enteredFlag = input()

semicolonpos = 0
fail = False
if len(enteredFlag) != 20:
    print("Your flag is incorrect. :(")
else:
    enteredFlag = enteredFlag[12:20] + enteredFlag[0:12]
    for i in range(20):
        if enteredFlag[i] == ";":
            semicolonpos = i
        elif enteredFlag[i] == "q":
            break;
    else:
        fail = True
    if not fail:
        enteredFlag = enteredFlag[0:semicolonpos] + "o_dddduel}"
    if enteredFlag == "fwopCTF{its_time_to_dddduel}":
        print("Your flag is correct!")
    else:
        print("Your flag is incorrect. :(")
```

So first of all we can see the that they are adding `o_dddduel}` to the flag. So the first part of the flag will be `fwopCTF{its_time_t`. Then we can see that it removes the semi colon from the entered input. so we can add that and now the flag will look lik `fwopCTF{its_time_;`. And if let's take a look at the for loop. We can see that it will break if the letter `q` is occured. So we now can add that also, and the flag will be `fwopCTF{its_time_;q`. and the final step is to shuffle them. So to reverse that we can use string splicing in python. Using the python code `flag[8:]+flag[:8]` we can see that the flag will be `its_time_t;qfwopCTF{` and that's it we got the flag!

Here's the code for that:
```python
# The flag checked in the if statement
flag = "fwopCTF{its_time_to_dddduel}"
# Where the semicolon was
semicolonpos = 18
# Cut upto the semicolon and then add the semicolon and q
flag = flag[0:semicolonpos] + ";q"
# Now shuffle it
shuffled_flag = flag[8:]+flag[:8]
# And finally print wthe result
print(shuffled_flag)
```
