## IceCTFWriteUp
### *Forensics* - Modern Picasso
1. Challenge gives you a gif and each frame of the gif has a like a little mark and it kind of looks like they make out words.

2. Had the idea of splitting the gif into each frame and then joining them together in gimp
3. Simple google search about splitting a give into each individual frame managed to find this tool https://ezgif.com/split
4. Couldn’t find any easy alternative besides magic wanding all 67 layers must be a better way


<details>
    <summary>Flag</summary>
    <code>IceCTF{wow_fast}</code>
</details>
<br>

**After thoughts**
* Using ImageMagick you can remove the white background easily and then merge them together. Example shown below
```bash
# Converting each frame into a separate .png file
convert picasso.gif %02d.png

# Removes the white background of each .png file
ls *.png | while read filename; do convert $filename -transparent white $filename; done

# Overlay images (However, stacks every image onto the first image)
ls *.png | while read filename; do convert $filename 00.png -gravity center -composite 00.png; done 
```

---

### *Forensics* - Lost in the Forest

1. Was given a zip file when you unzip it. it looks like your given a linux directory 
2. Looking through files. It says host name = Centos. Might come in handy later
3. Looking on the desktop of this directory there is a clue.png there is some kind of red fish. Could possibly be alluding to a red herring which is something misleading
4. Lots of files in the Pictures folder. Too many at the moment come back for them later
5. Ok so just go .bash_history and find the wget command in the history and use that then GET READY TO REVERSE ENGINEER BUDDY BOI
6. There is the python code that you get from the wget command. So you have to reverse this code. So somehow thought it would be good to use the bash shell in my tired brain so yes it was painful but I got it eventually
7. So basically I went through the code from end of the function to the start of the function
8. The function basically took the flag took each letter in the flag and turned it into an ordinal number between 1...255 (because of ascii table) then the numbers where add by that list within the function corresponding to their position in each list. The list was joined back up into a string then encoded in utf-8 then encode in base64 then decode in utf-8 the flag the string was then reverse and then multiplied 5 times.
9. So basically wrote did the opposite of that. 
10. Also didn’t realise eventually by filename isn’t referring to the filename of the python code so had to go back to .bash_history and see which file name they used this on.

<details>
    <summary>Flag</summary>
    <code>IceCTF{good_ol_history_lesson}</code>
</details>


---

### *Binary Exploitation* - Simple Overflow

* Basically this challenge is teaching you how to do a simple buffer overflow
The first challenges says can you write “Hello World!”. You type it in first challenge done
* The second challenge says can you put more than 16 characters which can which results in the buffer being overflowed
* The third challenge says can you make the “secret value” created by the four overflowed values equal to “1633771873 (0x61616161)”. a = Ox61 (if you refer to the ascii table) so you just fill the last 4 spots with the letter a
* The fourth challenges says that most architecture, integers are read in reverse order in other words little endian. This challenge is to make the secret value “1633837924 (0x61626364)”. Basically it is saying you have to read in 0x64 then 0x63 then 0x62 and finally 0x61 because it is in reverse order. If 0x61 is ‘a’ then the next numbers are ‘b’ (0x62), ‘c’ (0x63) and ‘d’ (0x64). So the last four characters need to be ‘dcba’
* The final challenge says the secret value needs to 0xcafebabe. If you complete it gives you the points for the flag

<details>
    <summary>Flag/Answer</summary>
    <code>0000000000000000\xBE\xBA\xFE\xCA</code>
</details>
