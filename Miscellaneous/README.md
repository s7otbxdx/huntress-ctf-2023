# Miscellaneous

## PRESS PLAY ON TAPE

*While walking home through a dark alley you find an archaic 1980s cassette tape. It has **"PRESS PLAY ON TAPE"** written on the label. You take it home and play it on your old tape deck. It sounds awful. The noise made you throw your headphones to the floor immediately. You snagged a recording of it for analysis.*
  
***WARNING: The audio in this file is very loud and obnoxious. Please turn your volume way down before playing.***

File: [pressplayontape.wav](challenge_files/pressplayontape.wav)

### Walkthrough

When played, the provided WAV file produces an obnoxiously loud noise. A quick search on the challenge/file name reveals a Danish band of the same name:

![PRESS PLAY ON TAPE - Band](/images/ppot_band.png)

From the description we can see this relates to Commodore 64 which used `.tap`  files to hold game data. Searching for a WAV -> TAP convertor, we find [c64tapedecode](https://github.com/lunderhage/c64tapedecode) which has capabilities to both convert WAV files to their TAP equivalent but also to extract any files within the TAP file.

```console
$ git clone https://github.com/lunderhage/c64tapedecode
$ cd c64tapedecode/src
$ make
$ ./wav2tap pressplayontape.wav | ./c64tapedecode -T -v
```

This creates a file `' .p00'` which contains the flag:

```
$ cat ' .p00'
C64File                0
� "FLAG[32564872D760263D52929CE58CC40071]"� 10
```

## Welcome to the Park

*The creator of Jurassic Park is in hiding... amongst Mach-O files, apparently. Can you find him?*

File: [welcomeToThePark.zip](challenge_files/welcomeToThePark.zip)

### Walkthrough


## Discord Snowflake Scramble

*Someone sent [message on a Discord server](https://discord.com/channels/1156647699362361364/1156648139516817519/1156648284237074552) which contains a flag! They did mention something about being able to embed a list of online users on their own website...*
  
*Can you figure out how to join that Discord server and see the message?*
  
***Note: Discord phone verification is NOT required for this challenge.***

*Connect here:*[https://discord.com/channels/1156647699362361364/1156648139516817519/1156648284237074552](https://discord.com/channels/1156647699362361364/1156648139516817519/1156648284237074552)

### Walkthrough



## I Wont Let You Down

*OK Go take a look at this IP:* 

*Connect here*: [http://155.138.162.158](http://155.138.162.158) 

*USING ANY OTHER TOOL OTHER THAN NMAP WILL DISQUALIFY YOU. DON'T USE BURPSUITE, DON'T USE DIRBUSTER. JUST PLAIN NMAP, NO FLAGS!*

### Walkthrough



## Who is Real?

*This is **not** a technical challenge, but it is a good test of your eye!*
  
*Now we live in a world of generative AI, for better or for worse. The fact of the matter is, threat actors can scheme up fake personas to lure you into a scam or social engineering... so, can you determine which profile picture is real and which is fake?*
  
*Play a game to train yourself on identifying what stands out for AI generated people. **After a streak of 10 correct selections, you'll receive the flag!***

### Walkthrough

Navigating to the challenge site, we are presented with two images and asked to select who is real:

![Who is Real? - Homepage](/images/who_is_real_homepage.png)

Clicking on the right image each time seems to give you the flag:

![Who is Real? - Flag](/images/who_is_real_flag.png)

```
flag{10c0e4ed5fcc3259a1b0229264961590}
```

## Operation Eradication

*Oh no! A ransomware operator encrypted an environment, and exfiltrated data that they will soon use for blackmail and extortion if they don't receive payment! _They stole our data!_*
  
*Luckily, we found what looks like a configuration file, that seems to have credentials to the actor's storage server... but it doesn't seem to work. Can you get onto their server and delete all the data they stole!?*

File: [operation_eradication](challenge_files/operation_eradication)
## Rock, Paper, Psychic

*Wanna play a game of rock, paper, scissors against a computer that can read your mind? Sounds fun, right?* 
  
***NOTE: this challenge binary is not malicious, but Windows Defender will likely flag it as malicious anyway. Please don't open it anywhere that you don't want a Defender alert triggering.***

File: [rock_paper_psychic.7z](challenge_files/rock_paper_psychic.7z)

## Walkthrough

Downloading the file and executing it, we are given a prompt to select either rock, paper, or scissors. However, we are playing against a computer which can read our mind, meaning whatever we input, the computer will choose the winning alternative:

![Rock, Paper, Psychic - Prompt](/images/rpp_prompt.png)

For example, if we select `rock`, the computer will choose `paper` and we lose:

![Rock, Paper, Psychic - Computer Wins](/images/rpp_computer_wins.png)

We can open this binary in Cutter with **write mode** enabled. We can see that the debug symbols have been left in, meaning the disassembled output is more clean. More notably, from the symbols we can ascertain that we are working with a Nim-compiled binary.

In particular, Nim has three wrapper functions around what is generally considered the `main` function of a binary. Initially, `NimMain` invokes `NimMainInner` which in turn invokes `NimMainModule` which is where the actual `jmp` to `main` exists.

Opening this up in the graph, we can see the main part of the program where the game logic takes place. Initially, the `getComputerChoice` function is called which chooses the winning choice based on the input, before `determineWinner` with the result of this either calling `computerWins` or `playerWins`.

![Rock, Paper Psychic - Game Function](/images/rpp_game_function.png)

From the above, we can see that `0x00416c6a` is the memory offset location of the `playerWins` function. As such, we can edit the `jne` (jump if not equal) and flip it to `je` (jump if equal). 

![Rock, Paper, Psychic - Patch](/images/rpp_patch.png)

This flipped comparison means that now the `playerWins` function is called rather than `computerWins` which yields the flag:

![Rock, Paper, Psychic - Flag](/images/rpp_flag.png)

```
flag{35bed450ed9ac9fcb3f5f8d547873be9}
```

## MFAtigue

We got our hands on an NTDS file, and we _might_ be able to break into the Azure Admin account! Can you track it down and try to log in? They might have MFA set up though...

File: [NTDS.zip](challenge_files/NTDS.zip)

### Walkthrough

Within the Zip archive, we are given a `ntds.dit` file and a `SYSTEM` file.  Combined, we can use Impacket's `secretsdump.py` to extract the NTLM hashes for the users:

```console
$ python secretsdump.py -ntds ~/ntds.dit -system ~/SYSTEM -hashes lmhash:nthash LOCAL -outputfile ntlm-extract
```

With the outputted file `ntlm-extract.ntds`, we can use `hashcat` along with `rockyou.txt` to crack the hashes:

```console
$ hashcat -m 1000 ~/ntlm-extract.ntds /usr/share/wordlists/rockyou.txt
$ hashcat -m 1000 ~/ntlm-extract.ntds --show
```

![MFAtigue - Hashcat](/images/mfatigue_hashcat.png)

From the above, we see the hash `08e75cc7ee80ff06f77c3e54cadab42a` decodes to password `katlyn99`. From the output file `ntlm-extract.ntds`, we can see this hash belongs to user `huntressctf.local\JILLIAN_DOTSON`:

``` console
$ cat ntlm-extract.ntds | grep '08e75cc7ee80ff06f77c3e54cadab42a'
```

![MFATigue - User Hash](/images/mfatigue_user_hash.png)

With this, we can login as the user:

![MFAtigue - Homepage](mfatigue_homepage.png)

At this point, we are asked to send a push notification to the user's mobile device, prompting them to sign in.

![MFAtigue - Push Notification](/images/mfatigue_push_notification.png)

The best way to get the user to accidentally accept one of these prompts is to spam/fatigue the MFA push, so sending a large number of requests via the **Send Push Notification** button results in the flag:

![MFAtigue - Flag](/images/mfatigue_flag.png)

```
flag{9b896a677de35d7dfa715a05c25ef89e}
```

## Indirect Payload

*We saw this odd technique in a previous malware sample, where it would uncover it's next payload by... well, you'll see.*

### Walkthrough



## Babel

*It's babel! Just a bunch of gibberish, right?*

File: [babel](challenge_files/babel)

### Walkthrough

