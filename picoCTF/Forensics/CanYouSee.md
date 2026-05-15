# CanYouSee

🌀 [**Question Link**](https://learn.cylabacademy.org/library/408)
🔥 **Difficulty: Easy**


## Description
You will get an image file.

## Analysis
Because the question only offers an image, I guess it might be related to steganography.

## Solution

### Basic information
First, we use the command `file` to check the basic info and there is no useful information.

```shell
syrup@DESKTOP-G1ALONG:~/ctf$ file ukn_reality.jpg
ukn_reality.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 72x72, segment length 16, baseline, precision 8, 4308x2875, components 3
```


### Try steghide
By using steghide, we can see that there is an embedded file `flag`, so we can extract this file and cat it. But it shows that the flag is not here.

```shell
syrup@DESKTOP-G1ALONG:~/ctf$ steghide info ukn_reality.jpg
"ukn_reality.jpg":
  format: jpeg
  capacity: 120.8 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase:
  embedded file "flag":
    size: 76.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes

syrup@DESKTOP-G1ALONG:~/ctf$ steghide extract -sf ukn_reality.jpg
Enter passphrase:
the file "flag" does already exist. overwrite ? (y/n) y
wrote extracted data to "flag".

syrup@DESKTOP-G1ALONG:~/ctf$ cat flag
The flag is not here maybe think in simpler terms. Data that explains data.
```

### Try exiftool
By using exiftool, we can see there is something special at `Attribution URL`. It looks like base64 encoding (ending with `==`), so we decode it using base64 and we can get the flag.   

```shell
syrup@DESKTOP-G1ALONG:~/ctf$ exiftool ukn_reality.jpg
ExifTool Version Number         : 12.76
File Name                       : ukn_reality.jpg
Directory                       : .
File Size                       : 2.3 MB
File Modification Date/Time     : 2026:05:05 00:08:12+08:00
File Access Date/Time           : 2026:05:05 00:08:58+08:00
File Inode Change Date/Time     : 2026:05:05 00:08:44+08:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 72
Y Resolution                    : 72
XMP Toolkit                     : Image::ExifTool 11.88
Attribution URL                 : cGljb0NURntNRTc0RDQ3QV9ISUREM05fZGVjYTA2ZmJ9Cg==
Image Width                     : 4308
Image Height                    : 2875
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 4308x2875
Megapixels                      : 12.4
```

## Flag
    picoCTF{ME74D47A_HIDD3N_deca06fb}