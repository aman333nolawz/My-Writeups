# Foresnics

## Binwalk

Binwalk is a tool for searching a given binary image for embedded files and executable code. Specifically, it is designed for identifying files and code embedded inside of firmware images. You could extract embedded files in a file using binwalk by running the command:
```bash
binwalk -e file
```

## Foremost

Foremost is a forensic data recovery program for Linux used to recover files using their headers, footers, and data structures through a process known as file carving. This tool is often used for CTF challenges for file carving. You could also use binwalk for file carving but foremost is easy and often carves more than binwalk with the default flags. Run this to use foremost:
```bash
foremost file
```

## Zsteg

Zsteg is used for detecting stegano-hidden data in PNG & BMP. You can install it by running `gem install zsteg`. The official github repofor zsteg is https://github.com/zed-0xff/zsteg

## Jsteg

Jsteg is a package for hiding data inside JPEG files with a technique known as steganography. This is accomplished by copying each bit of the data into the least-significant bits (LSB) of the image. The amount of data that can be hidden depends on the file size of the jpeg; it takes about 10-14 bytes of jpeg to store each byte of the hidden data. Install Jsteg using:

```bash
sudo wget -O /usr/bin/jsteg https://github.com/lukechampine/jsteg/releases/download/v0.1.0/jsteg-linux-amd64
sudo chmod +x /usr/bin/jsteg
sudo wget -O /usr/bin/slink https://github.com/lukechampine/jsteg/releases/download/v0.2.0/slink-linux-amd64
chmod +x /usr/bin/slink
```

## Stegsolve

Stegsolve is used to analyze images in different planes by taking off bits of the image. Install it by using:

```bash
wget http://www.caesum.com/handbook/Stegsolve.jar -O stegsolve.jar
chmod +x stegsolve.jar
mkdir bin
mv stegsolve.jar bin/
```

And you could run it by using `java -jar stegsolve.jar` and then opening the image file in the GUI.
