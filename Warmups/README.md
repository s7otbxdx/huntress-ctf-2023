# Warmups

## F12

*Remember when Missouri got into hacking!?! You gotta be _fast_ to catch this flag!*

### Walkthrough

![F12 Index Page](/images/f12_index.png)

Clicking `F12` as the challenge suggests and expanding out the HTML, we see a `capture_the_flag.html` endpoint.

Navigating to this page shows an additional alert box, but the flag is actually contained within `span` tags with the `display` attribute set to `none`:

![F12 Your Flag Is](/images/f12_your_flag_is.png)

![F12 Flag](/images/f12_flag.png)

```
flag{03e8ba07d1584c17e69ac95c341a2569}
```

## Technical Support

*Want to join the party of GIFs, memes and emoji shenanigans? Or just want to ask a question for technical support regarding any challenges in the CTF?  *
  
**This CTF uses support tickets to help handle requests. If you need assistance, please create a ticket with the `#ctf-open-ticket` channel. You do not need to direct message any CTF organizers or facilitators, they will just tell you to open a ticket. You might find a flag in the ticket channel, though!**

**Connect here:**  
[Join the Discord!](https://huntress.ctf.games/discord)

### Walkthrough

Joining the [discord](https://huntress.ctf.games/discord) and navigating to the `#ctf-open-ticket` channel, we find the flag:

![Technical Support Flag](/images/technical_support_flag.png)

```
flag{a98373a74abb8c5ebb8f5192e034a91c}
```

## String Cheese

*Oh, a cheese stick! This was my favorite snack as a kid. My mom always called it by a different name though...*

File: [cheese.jpg](challenge_files/cheese.jpg)

![cheese.jpg](challenge_files/cheese.jpg)

### Walkthrough

As the challenge name suggests, running `strings` against the file, filtering on `flag{` returns the flag:

```console
$ strings cheese.jpg | grep 'flag{'
flag{f4d9f0f70bf353f2ca23d81dcf7c9099}
```

## Notepad

*Just a sanity check... you do know how to use a computer, right?*

File: [notepad](challenge_files/notepad)

### Walkthrough

Running `cat` against the provided flag returns the flag:

```console
$ cat notepad
+------------------------------------------------------+
| [✖] [□] [▬]  Notepad                               - |
|------------------------------------------------------|
| File   Edit   Format   View   Help                   |
|------------------------------------------------------|
|                                                      |
|                                                      |
|   New Text Document - Notepad                        |
|                                                      |
|     flag{2dd41e3da37ef1238954d8e7f3217cd8}           |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
|                                                      |
+------------------------------------------------------+
| Ln 1, Col 40                                         |
+------------------------------------------------------+
```

## Layered Security

*It takes a team to do security right, so we have layered our defenses!*

File: [layered_security](challenge_files/layered_security)

### Walkthrough

## Comprezz

*Someone stole my S's and replaced them with Z's! Have you ever seen this kind of file before?*

File: [comprezz](challenge_files/comprezz)
### Walkthrough

## Chicken Wings

*I ordered chicken wings at the local restaurant, but uh... this really isn't what I was expecting...*

File: [chicken_wings](challenge_files/chicken_wing)

### Walkthrough

The provided file contains encoded text:

```console
$ cat chicken_wings
♐●♋♑❀♏📁🖮🖲📂♍♏⌛🖰♐🖮📂🖰📂🖰🖰♍📁🗏🖮🖰♌📂♍📁♋🗏♌♎♍🖲♏❝
```

The challenge name and prompt indicates this is encoded with Wingdings, a method of symbol substitution.

When decoded, the flag is returned (ref: [DCODE - Wingdings Font](https://www.dcode.fr/wingdings-font)):

```
flag{e0791ce68f718188c0378b1c0a3bdc9e}
```

## CaesarMirror

*Caesar caesar, on the wall, who is the fairest of them all?*  
  
*Perhaps a clever ROT13?*

File: [caesarmirror.txt](challenge_files/caesarmirror.txt)

### Walkthrough

## Book By Its Cover

*They say you aren't supposed to judge a book by its cover, but this is one of my favorites!*

File: [book.rar](challenge_files/book.rar)
## BaseFFFF+1

*Maybe you already know about Base64, but what if we took it up a notch*?

File: [baseffff1](challenge_files/baseffff1)

### Walkthrough

The provided file contains encoded text, mostly of Chinese characters.

```console
$ cat baseffff1            
鹎驣𔔠𓁯噫谠啥鹭鵧啴陨驶𒄠陬驹啤鹷鵴𓈠𒁯ꔠ𐙡啹院驳啳驨驲挮售𖠰筆筆鸠啳樶栵愵欠樵樳昫鸠啳樶栵嘶谠ꍥ啬𐙡𔕹𖥡唬驨驲鸠啳𒁹𓁵鬠陬潧㸍㸍ꍦ鱡汻欱靡驣洸鬰渰汢饣汣根騸饤杦样椶𠌸
```

With the challenge being "BaseFFFF+1", we can imply some form of Base encoding has been used against this string.

The hexadecimal `FFFF` is equal to `65,535` which then equals `65,536` when adding `1`.  This gives us Base65536 as the encoding method.

When decoded, the string reads (ref: [Better Converter - Base65536 Decode](https://www.better-converter.com/Encoders-Decoders/Base65536-Decode)):

```
Nice work! We might have played with too many bases here... 0xFFFF is 65535, 65535+1 is 65536! Well anyway, here is your flag:

flag{716abce880f09b7cdc7938eddf273648}
```

## Baking

*Do you know how to make cookies? How about HTTP flavored?*

### Walkthrough

Navigating to the challenge instance, we are shown the following UI:

![EZ Bake Oven - Index](ez_bake_index.png)

Selecting **Magic Cookies** triggers a timer for 121 hours.

![EZ Bake Oven - Magic Cookies](/images/ez_bake_magic_cookies.png)

Analysing the site, we find a cookie `in_oven` is set with a Base64 encoded value which decodes to a dictionary structure, containing the chosen recipe and the time the recipe was chosen:

![EZ Bake Oven - Cookie](/images/ez_bake_in_oven_cookie.png)

```json
{"recipe": "Magic Cookies", "time": "10/24/2023, 22:23:17"}
```

Changing the time to some time in the past, e.g., decreasing the year, encoding it back to Base64 and updating the cookie value returns the flag when the page is refreshed:

![EZ Bake Oven - Flag](/images/ez_bake_flag.png)

```
flag{c36fb6ebdbc2c44e6198bf4154d94ed4}
```

## Dialtone

*Well would you listen to those notes, that must be some long phone number or something!*

File: [dialtone.wav](challenge_files/dialtone.wav)

### Walkthrough

## Read The Rules

*Please follow the rules for this CTF!*

**Connect here:**  
[Read The Rules](https://huntress.ctf.games/rules)

### Walkthrough

Opening the [rules](https://huntress.ctf.games/rules), we find a quote which reads "*If you look closely, you can even find a flag on this page!*".

Looking at the page's source code shows a HTML comment with the flag embedded:

```html
<!-- Thank you for reading the rules! Your flag is: -->
<!--   flag{90bc54705794a62015369fd8e86e557b}       -->
```

## Query Code

*What's this?*

File: [query_code](challenge_files/query_code)

### Walkthrough

The provided file is a QR code in PNG format:

```console
$ file query_code
query_code: PNG image data, 111 x 111, 1-bit colormap, non-interlaced
```

![Query Code QR Code](/images/query_code.png)

Opening the file within [CyberChef](https://gchq.github.io/CyberChef/) using the `Parse QR Code` recipe, we can read its contents and retrieve the flag (ref: [CyberChef - Recipe](https://gchq.github.io/CyberChef/#recipe=Parse_QR_Code(false)&input=iVBORw0KGgoAAAANSUhEUgAAAG8AAABvAQMAAADYCwwjAAAABlBMVEUAAAD///%2Bl2Z/dAAAAAnRSTlP//8i138cAAAAJcEhZcwAACxIAAAsSAdLdfvwAAAEqSURBVDiN1dS7kcQgDAZgeRw4WzegGdogoyW7gbW3gaUlMtrwjBswmQLG/2kf90qA5ILTkHwBAxIShF9B/4MH0RK3GUxkqkzI04CTeEEDA8/YFvA0NPHq%2BGpzI6ehT6GNyHPsEb4vWaDmO0fW9ZV%2BgRopwP8obIGHzaP0p%2BMu1IlgTmvulhYxVR4Dd0Kd9DdBA7GSxu5jnQAvIXeRLs5UeViz2n3Vve%2BMSkySl7BdLF9efVUkojYVbiFfLapMYZucvr6BmCpPMh5ELr8OKjOBZ%2BEOerE6Ib0Hj7L7Z9nLTIFogD7oe8qKfPSVZHImSZ3PjoUXnghV6iws8ZHs0cLnyPio9TEtnKwWM3doou5dnXZjA/XPcTTG/W5R5SNf2cbwOQtF/t2X%2B1f8AIhlHz62EbxkAAAAAElFTkSuQmCC)):

![Query Code Flag](/images/query_code_flag.png)

```
flag{3434cf5dc6a865657ea1ec1cb675ce3b}
```
