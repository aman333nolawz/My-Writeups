# Winja CTF Writeup

## Cryptography

### Esoteric

Flag: `flag{es2o@t$e4r&i#c_code-most_difficult_to_program}`

We were given with the text
```malbolge
D'`A^?]nIYXWE16Tvus1NpL-KlIH"hgDB{SRba=<)sxqYonsrqj0hPlkdcb(`Hd]#a`_A@VzZY;Qu87MqQP2HlLEiCHG@EDCBA:^!=6543Wx654-Q1qpM'&+*#G'&}|#z@~}_uts9ZYutslqpi/mlNMiha'_d]ba`_^W{[TYXWVOTSLp32HGLKDhBAF?>b%A:?87[543870T.-2+0)Mnm+$H(hg%${zy?}_{t:rq7otmrkj0ngledc)JIedcb[!B^]\UyYXWVU7Mqp3INMLEJCgGFE'=<A@?8\<;4z2V6/4-Q10po'&%I)('g%|{"!x>|{zsxwp6tmrqpi/POkjib(fe^F\"`Y^W{>=YXQu8TMLQPOHlLEJCBAeEDC<;:?8\<;{z8765.R2r*)(L9
```

I recognized it is [malbolge esolang](../tools and commands/#malbolge) because I have seen this in some other CTFs. So I ran it in an [online interpreter](https://malbolge.doleczek.pl/) for malbolge and got the flag.

## Steganography
### Believe Your Eyes

Flag: `flag{849d97fa58871dad45e81027f861739_maYB3_i_SHOULd_BELIeve-7HeM}`

In the site for the challenge, I found this image:
![Image for the challenge](assets/images/winja ctf/believe_your_eyes.jpg)

So I ran some basic tools like `strings` and `exiftool`. When I inspected results got from the `exiftool` I found the flag in the layers like `flag{849d97fa58871dad4, _SHOULd_BELIeve-7HeM}, 5e81027f861739_maYB3_i`. But, they weren't in order so I reordered them and found the flag.

### Hardy

Flag: `flag{Di77icu8tyI9now@!!#-Youdidit!!}`

[Encoded](assets/files/winja ctf/hardy.txt) text given with the challenge

This one was not so hard. It was just ciphers inside ciphers. Here is the order of the ciphers:

1. Base58
2. Morse Code
3. Binary
4. Decimal to ASCII

And [here](https://tinyurl.com/3kweb4vx) is the process in cyberchef.


## Forensics
### Anonymous

Flag: `flag{LJryyYW8IbxuZrOcZ4nd-this-is-a-challenge}`

In this challenge, you will get a zip file with someone's desktop files inside. So first i tried to find any suspicious files in the folders of the user `crazy_crocodile`. So first I checked what files are there in the common directories like `Picture/`, `Documents/`, `Music/`, `Downloads/`, etc. But there wasn't any files in any of those directories. Then I tried to list all of the hidden directories and found `.config/` directory which contained configuration files for google chrome. So I tried to look for any useful files there and I found the `History` file in the `Defaults/` folder in the google chrome folder. So I tried `cat History` and found a google docs link. I tried visiting the link and tried to submit the title of the file wrapped with `flag{}` as the flag and it was right!


## Web
### b74ass

Flag: `flag{D@i7s#o89!0@v&e@_php_master}`

I poked around the website for some times and found that if we supply `?debug` in the URL, we would get the source code of the project. And there I found that we need to make the variable `final_string` to `roseytherehappy` in order to get the flag. But, the PHP code removes every occurrence of `roseytherehappy` with function `preg_replace`. So how do we solve it? Well, the `preg_replace` replaces exact word with nothing. So we could insert a `roseytherehappy` inside any of the letters in the `roseytherehappy` and it will replace it, and we will get the `roseytherehappy` as value. So the payload will look like `roseytherehapproseytherehappyy`. And the final URL will look like: https://jerry.winja.site/?value=roseytherehapproseytherehappyy
