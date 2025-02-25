We are provided with a poem, visually is has nothing noticeable but the SNOW in the title reminds me of a specific steg tool called StegSnow. I used it on the poem with "Aakash" as the password since he likes using his name as the password

```bash
❯ stegsnow -C -p "Aakash" poemm.txt
https://pastebin.com/HVQfa14Z%      
```

But now we are presented with a pastebin link `https://pastebin.com/HVQfa14Z`

Lets visit it!

![](attachments/Pasted%20image%2020250224230927.png)

I've seen this before! This is cowcode, an esoteric language made up of only Mooos

I decoded it with https://www.cachesleuth.com/cow.html

![](attachments/Pasted%20image%2020250224231150.png)

And just like that, we got the flag!

Flag: `KashiCTF{Love_Hurts_5734b5f}`