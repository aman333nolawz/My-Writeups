# Damncon CTF Writeup

## Web
### Go7 C00k3D

Flag: `DSPH{1_g07_c00kie5}`

This challenge was an easy one. So as the name suggests the challenge is about cookies. So I viewed the cookies and there was a cookie named `Admin` which was set to `False`. So I simply edited it to `True` and got the flag.


## Reversing
### Easy_but_not_blood

Flag: `DSPH{You_are_ON_UPSS}`

This was so an easy one. I ran the strings command on the program they gave and got the flag.

```{hl_lines="1 2" title="strings rev"}
DSPH{You_are_
ON_UPSS}
;*3$"
DSPH{YOU_ARE_FOOL}
GCC: (Debian 10.2.1-6) 10.2.1 20210110
```
