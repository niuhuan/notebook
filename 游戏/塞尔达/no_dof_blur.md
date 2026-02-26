https://gamebanana.com/mods/14847

---

据回复， NS新版CFW需要修改文件夹名->contents

---

A very simple mod that disables the DOF (Distant blur) in the game! A huge thanks to Rodrigo for helping out making this mod!

Thanks to BSOD Gaming for the screenshots!

Installation Instructions
For Switch: Simply drag & drop the "atmosphere" folder in the root of your SD card, boot the console into CFW, then boot the game, and the mod will be active!

For yuzu: Simply rename the "01006BB00C6F0000" folder to "No DOF" and place it in "C:\Users\{You}\AppData\Roaming\yuzu\load\01006BB00C6F0000\". Restart yuzu and boot up the game!

---

For Ryujinx, extract no_dof_blur.zip\atmosphere\titles\01006BB00C6F0000\romfs
to
{ryujinx}\portable\mods\contents\01006bb00c6f0000

---

For people having problems with this mod not working (like I was), they
have changed how the latest version of Atmosphere works.



It's quite easy to fix. Download the files and
copy/paste the "atmosphere" folder on the root of your memory card.



After that, go inside the "atmosphere" folder and then inside "titles"
folder and find this folder - "01006BB00C6F0000". Simply move this
folder (cut/paste) from the "titles" folder into "contents" folder.



In the end, the setup on your memory card should look like this...



atmosphere\contents\01006BB00C6F0000



And it will work just perfect! Happy Gaming! :D

Also, thanx a lot MelonSpeedruns for creating this mod.  Really appreciate the effort man :)



PS: If you wanna know the specific reason for this, then read below...



Atmosphere's process override layout was changed.


Atmosphere now uses the /atmosphere/contents directory, instead of /atmosphere/titles.
This goes along with a refactoring to remove all reference to "title id" from code, as Nintendo does not use the term.

