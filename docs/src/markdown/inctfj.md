# INCTFJ 2021 Writeup

## Misc

### Askey

Difficulty: `Beginner`

??? info "Content of file given with the challenge"
    ```
    1101001
    1101110
    1100011
    1110100
    1100110
    1101010
    1111011
    1101100
    110011
    1100001
    1110010
    1101110
    110001
    1101110
    1100111
    1011111
    1100001
    1110011
    1100011
    110001
    110001
    1011111
    110100
    1110010
    1110100
    1011111
    111100
    110011
    1111101
    ```

Those numbers are binary. So I converted them to decimals and got some ascii values. So I converted those numbers to characters using python. Here is the python code for doing all these:

```python
with open("chall", "r") as f:  # Opens the file for reading
    lines = (
        f.read().strip().split("\n")
    )  # Reads content of the file and split it by newline character

flag = ""
for num in lines:  # Iters through all of the lines in lines
    flag += chr(
        int(num, 2)
    )  # int(num, 2) converts num to decimal from binary and then chr() turns it into a character
print(flag)
```

### Crack 1t

Message to crack: `bafe38f09170ebc16dad1bbf11fe55a4`

I put it in [crackstation](https://crackstation.net/) and crackstation showed me the message which was `Hackerboy`. Wrap it up with `inctfj{}` to get the flag.

### Glyphy Tweet

Difficulty: `Beginner`

Message given with the challenge:
```
Ｔwіttｅr ⅰs a ｓｏcｉａｌ meｄｉa sⅰｔｅ， aｎd іｔs ｐｒіｍaｒy pｕｒposｅ ⅰｓ ｔо сοｎｎｅϲt ｐeоｐlｅ ａｎｄ allοw pｅοplｅ ｔο ｓｈａｒe thｅіｒ tｈｏｕｇｈｔs ｗｉｔｈ a ｂⅰg ａｕｄіｅｎce． Tｗⅰtｔeｒ ａｌlоwｓ users to discover stories regarding today's biggest news and events, follow people or companies that post content they enjoy consuming, or simply communicate with friends. Additionally, PR teams and marketers can use Twitter to increase brand awareness and delight their audience.
```

This message was encrypted with [*Twitter Secret Message*](/My-Writeups/tools and commands/#twitter-secret-message). So just go to https://injecti0n.github.io/tweet-hidden-message/ and paste the message and voila! you get the decoded message. Just wrap the decoded message you got with `inctfj{}` to get the flag.


### Mimmic

Difficulty: `Beginner`

File provded with the challenge: [0_0.txt](assets/files/inctfj/0_0.txt)\
**Note: You need to download it and view it in a text editor to view it properly.**

The message given with the challenge was encoded with *spam mimic with spaces*. Go to https://www.spammimic.com/decodespace.cgi and decode it. Then you will get a pastebin link. Go to that link and you will get your flag.


### Roter

Difficulty: `Beginner`

Message to decode: `chwnzd{linunyfynnylmzilzoh}`

The message is encrypted with [caesar cipher](/My-Writeups/tools and commands/#caesar-cipher). But it's simple to crack it. Just run this command:
```bash
for i in {1..25}; do echo "chwnzd{linunyfynnylmzilzoh}" | caesar $i; done
```

This would check all the keys and print their result. Here we could see the flag in the output. Now all left for you is to submit it!

### XeroX

??? info "Files given with the challenge"
    ![1.jpg](assets/files/inctfj/xerox/1.jpg)
    ![2.jpg](assets/files/inctfj/xerox/2.jpg)

I stumbled upon this challenge for a little bit thinking the goal was to xor the images. But then the hint said to see the difference. So I made a python script to find the difference between these 2 files and got the flag. Here is the script I wrote:

```python
with open("1.jpg", "rb") as f:
    img_1 = f.read()

with open("2.jpg", "rb") as f:
    img_2 = f.read()

flag = ""
for c1, c2 in zip(img_1, img_2):
    if c1 != c2:
        flag += chr(c1)

print(flag)
```
