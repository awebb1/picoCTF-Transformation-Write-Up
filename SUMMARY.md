## Description

I wonder what this really is... [enc](./enc)

''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])

### Points

20

### Hints

1. You may find some decoders online

## Intro

It's be a while since I've had to understand someone else's code. This problem took me more time than I liked to admit, but working through it I was able to revive my problem solving skills and was very satisfied with this problem. It exposed me to something new and was pretty enjoyable!

## Approach

As I stated this was my first time in a while I needed to understand someone else's code, the first thing I did was look at the hint. "You may need to find some decoders online". Okay great, so I moved back to the description and because of the hint, realized the string of characters I downloaded (灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸弰㑣〷㘰摽) needs to be decoded.

From there I studied the code until I realized this was the encoder itself. The word flag and (after some research) the chr() method, gave it away. I hadn't used ord() or chr() before so I read up and started experimenting in my own code to try and start reversing this.

The first thing I did was isolate the first part of the code, I noticed the addition occuring and wanted to see what would happen if I reversed the first ord portion to the following:

```python
char(ord(flag[i]) >> 8))
```

A 'p' came out, realizing this is probably the first letter of the flag, I focused on the other half:

```python
ord(flag[i + 1])
```
Quickly seeing that this can't simply have an operator swap like the previous portion due to index restraints, I focused back on the fact the 2 halves were being summed. I compared the value of (ord('p') << 8) to to ord('灩'). 

>>(ord('p') << 8) = 28672 and ord('灩') = 28777. 

After seeing how close the values were it clicked, simply subtracting the unicode value of the first solved character from the unicode value of the encoded character should give me a unicode for a second letter. Each encoded letter was just one ASCII letter that had it's unicode value shifted, added to the next character's unicode value, then converted back to ASCII.

I got to coding the decoder, after a few minutes I had the following written:

```python
encodedFlag = '灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸弰㑣〷㘰摽'
decodedFlag = ''

for i in encodedFlag:
	char1 = chr((ord(i) >> 8))
	char2 = chr((ord(i) - (ord(char1) << 8)))
	decodedFlag = decodedFlag + char1 + char2

print(decodedFlag)

```
Very straight forward. All it does is loop through each substring of the encodedFlag (which is the enc file the CTF provided). First it reverses the shift to isolate the first character, then it shifts the result like the original equation, and subtracts it from the unicode value of the current encoded character to build the decoded string!

Once it's ran you get the flag.

## Flag

picoCTF{16_bits_inst34d_of_8_04c0760d}

## Ending Thoughts

This was tricky for me, as I had said. It's dealing with bitwise shifting and methods I had never worked with. I got a good amount of valuable info and want to look more into bitwise shifting and exactly WHY this works the way it does. I was able to come to a good enough understandin just by trial and error. Even though it ended up being rather straight forward it was a great learning tool for me and enjoyable as well!
