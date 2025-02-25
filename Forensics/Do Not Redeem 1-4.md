We are provided of what looks like a disk image of an Android phone. I will open this in Autopsy and run its analysis.

## Part 1

To get the OTP and exact timestamp, we can look at the mmssms.db that Autopsy classified as a messages database.

![image](https://github.com/user-attachments/assets/b21a0b9b-1a28-46f0-a6b0-1a8b033dbaa8)

Therefore the flag is `kashiCTF{839216_1740251608654}`

## Part 2

I performed an Android Analyzer (aLEAPP) analysis in Autopsy. I saw that in the installed apps, there was an app with package name "com.google.calendar.android" but it had a weird-looking icon and name "NetherGames Vouchers."

![image](https://github.com/user-attachments/assets/03a26573-028d-4c72-afa3-3b804aa4d444)

Flag: `kashiCTF{com.google.calendar.android}`

## Part 3

By searching for "voucher" I found this file with many Discord messages: `/LogicalFileSet1/extracted/data/data/com.discord/cache/http-cache/c0dcba6570747a7c665c2ec31458f1de.1/c0dcba6570747a7c665c2ec31458f1de.1/0`

![image](https://github.com/user-attachments/assets/b6907f42-2d31-4d55-878b-8577cebae5fa)

In one of the messages it included a link and the author's name, "savsch."

![image](https://github.com/user-attachments/assets/204f8414-c5fe-4374-894c-543bd4c72996)

Flag: `kashiCTF{savsch_https://we.tl/t-Ku8Le7js}`

## Part 4

As the previous part mentioned Minecraft, I looked at files in the Minecraft data folder which may contain a poem. In `/LogicalFileSet1/extracted/data/data/com.mojang.minecraftpe/games/com.mojang/minecraftWorlds/0RjavQ==/db/086976.log`, there is a poem:

![image](https://github.com/user-attachments/assets/9f19a641-2b3a-40cc-974d-8ed1c818cbba)

By searching for "author" we can find the author of the poem, iiiORSiii. The voucher code is the pen name of the poem's author, as mentioned in part 3.

Flag: `kashiCTF{iiiORSiii}`
