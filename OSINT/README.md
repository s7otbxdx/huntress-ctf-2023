# OSINT

## Under The Bridge

*Can you find this iconic location?*

*Connect here:* [https://osint.golf/HuntressCTF2023-chall2/](https://osint.golf/HuntressCTF2023-chall2)
### Walkthrough



## Operation Not Found

*In the boundless web of data, some corners echo louder than others, whispering tales of innovation, deep knowledge, and fierce competition. On the lush landscapes of [https://osint.golf/](https://osint.golf/), a corner awaits your discovery... where intellect converges with spirit, and where digital foundations stand alongside storied arenas.*

*Connect here:* [https://osint.golf/HuntressCTF2023-chall1/](https://osint.golf/HuntressCTF2023-chall1)

### Walkthrough
## Where am I?

*Your friend thought using a JPG was a great way to remember how to login to their private server. Can you find the flag?*

File: [PXL_20230922_231845140_2.jpg](challenge_files/PXL_20230922_231845140_2.jpg)

![PXL_20230922_231845140_2.jpg](challenge_files/PXL_20230922_231845140_2.jpg)

### Walkthrough

From the challenge prompt, we can infer there is encoded data within this image. Using `exiftool`, we see a Base64 encoded string within the `Image Description` field:

```console
$ exiftool PXL_20230922_231845140_2.jpg
...
Image Description               : ZmxhZ3tiMTFhM2YwZWY0YmMxNzBiYTk0MDljMDc3MzU1YmJhMik=
...
```

Decoding this from Base64 reveals the flag:

```console
$ echo 'ZmxhZ3tiMTFhM2YwZWY0YmMxNzBiYTk0MDljMDc3MzU1YmJhMik=' | base64 -d
flag{b11a3f0ef4bc170ba9409c077355bba2)
```