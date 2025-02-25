![](attachments/Pasted%20image%2020250222161759.png)

This one may appear hard but was pretty simple.

### Extracting the .vdi

I first started by extracting the file with 7zip and extracting out the file system.

![](attachments/Pasted%20image%2020250222162023.png)

Now that I had the file system locally I could search it. And in `.bash_history` I found this...

![](attachments/Pasted%20image%2020250222162255.png)

And another part of the flag in `.sush_history` like that, but I realised manually searching would be pretty hard. However I noticed all flag parts started like *"fLaG Part"*, this is good because I can search for the flag with just a simple flag command and then thats what I did.

```bash
❯ find . -type f -exec grep -nH "fLaG Part" {} \;
./etc/hosts.allow:7:# fLaG Part 1: 'KashiCTF{r'
./etc/kernel-img.conf:1:# Kernel image management overrides fLaG Part 4: 't_Am_1_Rig'
./etc/sudo.conf:35:# fLaG Part 6: 'r0rs_4ll0w'
./home/kashictf/.bash_history:2:echo "fLaG Part 5: 'ht??_No_Er'"
./home/kashictf/.sush_history:2:echo "fLaG Part 3: 'eserve_roo'"
grep: ./usr/bin/sush: binary file matches

```

Now the command said it found matches in `/usr/bin/sush`, so I looked a lil deeper in Binary Ninja and found this.

![](attachments/Pasted%20image%2020250222162732.png)

Flag: `KashiCTF{rm_rf_no_preserve_root_Am_1_Right??_No_Err0rs_4ll0wed_Th0}`

  