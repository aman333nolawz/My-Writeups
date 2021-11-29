# The Few Chosen(TFC) CTF Writeup

## Forensics

### AAAAA

Flag: `TFCCTF{Gr4phic_d35ign_is_my_p455ion}`

File given with this challenge: [AAAAA](../assets/files/tfc ctf/AAAAA/AAAAA)

I tried to look what file it was by running the `file` command. But it showed only `data`. So I checked it in `hexedit` and saw a bunch of `A`s in the beginning of the file and after those `A`s there was some interesting stuffs. So I ran `foremost AAAAA` and got the flag as a png file:

![Flag Image](../assets/files/tfc ctf/AAAAA/flag.png)

## Crypto

### Sea Language 1

Flag: `TFCCTF{WH4T-AR3-Y0U-S1NK1NG-AB0UT?!!!?}`

Cipher given with the challenge:

```
.-- .... ....- - -....- .- .-. ...-- -....- -.-- ----- ..- -....- ... .---- -. -.- .---- -. --. -....- .- -... ----- ..- - ..--.. -.-.-- -.-.-- -.-.-- ..--.
```

I knew it was morse code just by looking at it. So I decoded it and got the flag.

### Sea Language 2

Flag: `TFCCTF{w417_4_m1nu73..._7h15_1s_n07_m0rs3!!!!!r1gh7?}`

Cipher given with the challenge:

```
._._._.. ._...__. ._....__ ._....__ ._._._.. ._...__. .____.__ .___.___ ..__._..
..__..._ ..__.___ ._._____ ..__._.. ._._____ .__.__._ ..__..._ .__.___. .___._._
..__.___ ..__..__ .._.___. .._.___. .._.___. ._._____ ..__.___ .__._... ..__..._
..__._._ ._._____ ..__..._ .___..__ ._._____ .__.___. ..__.... ..__.___ ._._____
.__.__._ ..__.... .___.._. .___..__ ..__..__ .._...._ .._...._ .._...._ .._...._
.._...._ .___.._. ..__..._ .__..___ .__._... ..__.___ ..______ ._____._
```

When I first looked at the cipher, I thought it was morse code, but I was wrong. After some thoughts, I found out that all of them are splitted by 8 characters, so I translated it into binary by replacing `.` with `0` and `_` with `1` and then put it on an online decoder and got the flag. The below code will do the same:

```python
def binary_to_ascii(binaryString):
    return "".join([chr(int(binaryString[i:i+8],2)) for i in range(0,len(binaryString),8)])

encoded = """
._._._.. ._...__. ._....__ ._....__ ._._._.. ._...__. .____.__ .___.___ ..__._..
..__..._ ..__.___ ._._____ ..__._.. ._._____ .__.__._ ..__..._ .__.___. .___._._
..__.___ ..__..__ .._.___. .._.___. .._.___. ._._____ ..__.___ .__._... ..__..._
..__._._ ._._____ ..__..._ .___..__ ._._____ .__.___. ..__.... ..__.___ ._._____
.__.__._ ..__.... .___.._. .___..__ ..__..__ .._...._ .._...._ .._...._ .._...._
.._...._ .___.._. ..__..._ .__..___ .__._... ..__.___ ..______ ._____._
"""
encoded = encoded.replace(".", "0").replace("_", "1")  # Translating it into binary
encoded = encoded.replace(" ", "").replace("\n", "")   # Removing all the spaces and newline characters
print(binary_to_ascii(encoded))
```

### Holiday

Flag: `TFCCTF{don't_come_today_there_are_dogs}`

```{title="Description"}
I've always wanted to visit my friend! He's not fluent in English,
but he does know a few words. This made communicating a problem.
We did, however, talk about each other a lot! He knows I'm allergic to dogs,
and I know that his favourite colour is green! Before leaving for the airport,
he left me a message. He's not much into technology so his crypted message really
confused me!Can you tell me what he said?

Maui: mai hele mai i kēia lā aia nā ʻīlio
```

So I tried putting `mai hele mai i kēia lā aia nā ʻīlio` into google translate and got this: `don't come today there are dogs`. So I tried changing this into the flag format by replacing spaces with underscores and got the flag!

## Misc

### Discord Shenanigans

Flag: `TFCCTF{th1s_5t3g0_fl4g_w45_n0t_h1dden_w3ll}`

```{title="Description"}
We considered giving you a free flag. However, we decided against it. In general, we would never do that! Or would we? That's the beginning of a good CTF! Discord is the new Twitter.

To be able to solve this challenge, you'll need to join our discord. Link in the Rules page.
```

So many people used bot commands in the CTF's server, but that was not how you could get the flag. I had found a suspicious old message from the author of this challenge in the general channel.
![A suspicious message from the author of the challenge](../assets/files/tfc ctf/discord_shenanigans/message.png)

I googled so many decrypters for this but none I could find. Finally, a friend of mine gave me this link https://holloway.nz/steg/ which was a decoder for this message. So i took this message and put it in the decoder and got the flag!

### Weird Friend

Flag: ` TFCCTF{why_ar3_QR_C0d3s_s0-complicated!?o00o0f}`

Cipher given with this challenge:

```
1111111001110110010000111111110000010000110100110101000001101110100111001110010010111011011101010110100000000101110110111010101010001101001011101100000100110001011000010000011111111010101010101010111111100000000010010000000100000000110001110110110101000000110000010010000100011000101011010111000110110110011110010111100101001010011001100000010110111001111101101011101101110011111010100001011111001011111000011101101100011110101101000000010000100000000101111010110001000010100101100110000110000101111010101010111101101110001101101011111001110100001100110011001010100101001111001000100100101110110001011111111000000000010110001001110001001111111110101010010110101011100100000101010100000011000110111011101001010011000111111000010111010010110111110110101001101110100110000111000001111101000001010010010000110101110111111110101111100101000000100
```

At first I thought this was binary, but I was wrong. After a lot of playing around with this challenge, A friend of mine said maybe it will be a QR Code. So I googled for `binary to qr code generator` and got this site: https://bahamas10.github.io/binary-to-qrcode/. So I put the binary in the `binary` field and got a QR Code. Then I decoded it using https://zxing.org/w/decode.jspx and got the flag!
