# MIC

My Epson InkJet printer is mysteriously printing blank pages. Is it trying to tell me something?

[scan.pdf](https://github.com/Stirring16/CSAW-CTF-2021/files/7167052/scan.pdf)

## Initial Research

The objective of this challenge is to find the hidden message in the blank pdf files

 The first, these are scanned files and the author of the challenge tells us that they were printed, is MIC dots

I got title `MIC` is: `Machine Identification Code`

## Get Flag

I found [Deda](https://github.com/dfd-tud/deda) which can read the data encoded using these dots.

First, We can use `pdftoppm` to convert the PDF pages to .png images:

```
┌──(kali㉿kali)-[~/Desktop/MIC]
└─$ pdftoppm scan.pdf nsa -png  
```
![image](https://user-images.githubusercontent.com/62060867/133382973-fc99887d-ea57-4f10-843b-d4600d347ac8.png)

Then we need to install Deda 
```
git clone https://github.com/dfd-tud/deda
pushd deda && pip3 install --user deda && popd
sudo apt install poppler-utils
```
Then, I wrote a quick one liner to dump the flag:

``` 
for i in *.png; do deda_parse_print "$i" | grep serial:; done
```
![image](https://user-images.githubusercontent.com/62060867/133384447-136ddb25-89e3-4f0d-a268-d10c1423731f.png)

If we concatenate the serial numbers from all pages, we end up with this decimal sequence

![image](https://user-images.githubusercontent.com/62060867/133384740-2ad150ec-f7f6-415f-a05d-8ac90de0dbfc.png)

So we got the flag

