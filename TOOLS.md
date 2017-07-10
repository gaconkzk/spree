How to decompress files:
1. Copy the scr_base.xp from your client folder to the quickbms folder.
2. Open quickbms.exe
3. Point to the “9Dragons Script.txt” inside quickbms folder
4. Point to the file you want to decompress “SCR_BASE.XP”
5. Now create a folder where the files should be stored "SCR_BASE_US_SERVER”
6. Open the folder and click save
7. If it asks you to overwrite something just do it
Before you edit something create a backup of that folder so you don’t have to decompress them again.
I like to copy them to the Xpacker folder.

Decrypt:
You got the files but to edit them you have to decrypt them.

1. First select a file you want to edit, I use the “CharacterCondition.XMS” for example
2. Copy it to the 9DCryptTool_0.1 folder
3. Open command Prompt (Start --> Run --> cmd --> Enter) and go to the 9DCryptTool_0.1 folder
4. Type the name of the exe first “9DCryptTool.exe” then the name of the file “CharacterCondition.XMS” and the name of the decrypted file “CharacterCondition.XMS.dec”
5. Now you can open the “CharacterCondition.XMS.dec” with notepad or something you like.
And here is something funny.
Those are skill effects, you can edit them to zero and you client will ignore some debuffs like paralyze, Disrupted Nerves, Silence or the Effect when you log in (don’t know the name). But you still can get stuned. Tested on P9d, 9D Rus.
Anyway it looks like its selfexplaining

XM_MOVELOCK 1
_XM_TRADELOCK 1
_XM_BATTLELOCK 1
_XM_SKILLLOCK 1
_XM_MEDLOCK 1
_XM_SPELLLOCK 1
_XM_SPEEDLOCK 1
_XM_PARTYLOCK 1
_XM_CHATLOCK 0
_XM_MODELOCK 1

Since I don’t know which one does what I just edit them all to zero lol.

Encrypt:
Now you have to encrypt this file again.
Like before Type the name of the exe first “9DCryptTool.exe” then the name of the decrypted file “CharacterCondition.XMS.dec” and the original name “CharacterCondition.XMS”

Copy the edited and encrypted file back to your scr_base folder, for me its XPacker/SCR_BASE_US_SERVER

Now we have to pack everything
1. Open “XPacker.exe”
2. Right click and select “New Package”
3. Type “SCR_BASE”
4. Right click again and select “New Section”
5. And again type “SCR_BASE”
6. Now move the decompressed, files inside the xpacker window
7. And click “pack”
8. Select a folder where the new SCR_BASE.XP should be and pres open
9. Now copy the just made scr_base.xp inside your client script folder (don’t forget to make backup)

There is not much you can edit coz most is server side but I hope you find something and I hope even more you share what you have found.

Now lets test if what we just done works (most time it doesnt ^^)