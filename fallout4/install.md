# Steam Deck Guide: Fallout 4 > Mod Organizer 2 > F4SE

## 0. **Prepare**

* Update SteamOS (this guide used 3.5.19)
* Switch to Desktop mode
* You *may* need a password on your deck account, I already had one when setting things up

## 1. **Install Fallout 4 via Steam**
   
* Run Fallout 4 from Steam once to set up
   * you can exit from the main menu once it loads
* Once installed, you can find the Fallout4 folder here:
   * `/home/deck/.steam/steam/steamapps/common/Fallout 4`
* If you are using a non-Steam version of Fallout 4 on your Deck, you'll need to google if some instruction below doesn't work. Gamepass versions may be an ongoing problem with Mod Organizer 2

## 2. **Install "ProtonTricks"**

* Open the "Discover" Software Center
* Search for "ProtonTricks" and install it
* Close Discover

## 3. **Install Mod Organizer 2**

**NOTE:** From this point forward I'll be referring to "Mod Organizer 2" as "MO2". 

* Download the latest [Linux installer release](https://github.com/rockerbacon/modorganizer2-linux-installer/releases) 
   * 'mo2installer-5.0.3.tar.gz' was the latest version when writing this
* Extract the download (in your Downloads folder)
   * Run the Dolphin file manager
   * Navigate to your Downloads folder
   * Right-click the 'mo2installer' archive
      * *Extract >*
      * *Extract Archive Here, autodetect subfolder*
* Install the extracted file:
  * Right-click the newly extracted folder (example: `mo2installer-5.0.3`)
  * *Open Terminal Here*
  * Run this command in the terminal:
    `./install.sh`
  * When install is done, Mod Organizer 2 should open when you "Play" Fallout 4 from Steam
    * Go ahead and run MO2 by clicking "Play"
    * If you've never used MO2 you can try going through the tutorial, but, it was a bit of a mess for me so I just learned as I went along
    * You can accept most defaults during the setup wizard. 
      * Choose "Global Instance" when choosing between Global and Portable (Portable probably works, just not how I did things)
      * If you plan to use NexusMods and want to use Mod Manager download links (which is the main reason for the next section) go ahead and set up your Nexus account in MO2. Since that is much of the point of this guide, **take the time to set this up now, including registering a Nexus user account if you don't already have one.**
      * Don't worry about configuring MO2 perfectly once it gets to the main window, do the fix for ProtonTricks in the next step first
      * [This video](https://www.youtube.com/watch?v=-rFwk4tb6Ew) may help you understand MO2 better. It is made for Windows users but most of the information directly applies to running MO2 on the Steam Deck **after** initial install.

## 4. **Modify Mod Organizer 2 to work with the Flatpack version of ProtonTricks**

**NOTE:** This may not be necessary in future versions if mo2installer changes how the .desktop file is generated.  The current version of MO2 won't open Nexus '.nxm' links automatically with the Flatpak version of ProtonTricks. But the fix is (relatively) easy. For more background information you can read [this github issue](https://github.com/rockerbacon/modorganizer2-linux-installer/issues/317).

* Make a backup of the .desktop file for MO2:
  
  ```
  cp /home/deck/.local/share/applications/modorganizer2-nxm-handler.desktop /home/deck/.local/share/applications/modorganizer2-nxm-handler.desktop.orig
  ```

* Edit the .desktop file:
  
  ```
  kate `/home/deck/.local/share/applications/modorganizer2-nxm-handler.desktop
  ```

* Change it to match the following:
  
  ```
  [Desktop Entry]
  Type=Application
  Categories=Game;
  Exec=bash -c 'env "STEAM_COMPAT_CLIENT_INSTALL_PATH=$HOME/.steam/steam" "STEAM_COMPAT_DATA_PATH=$HOME/.steam/steam/steamapps/compatdata/377160" "$HOME/.local/share/Steam/steamapps/common/Proton 9.0 (Beta)/proton" run "$HOME/Games/mod-organizer-2-fallout4/modorganizer2/nxmhandler.exe" "%u"'
  Name=Mod Organizer 2 NXM Handler
  MimeType=x-scheme-handler/nxm;
  ```
  
   * This .desktop file is set up for a default Steam Deck with ProtonTricks installed as a flatpak (through "Discover"). 
   * The modification above makes it so you can download mods on [NexusMods](https://www.nexusmods.com/fallout4/mods/) by clicking on "Mod Manager Download" and have MO2 open automatically rather than you needing to manually install downloaded files.
   * "Exec" line is very long, make sure you copied it correctly.
      * Within the "Exec" line you can change '$HOME/.local/share/Steam/steamapps/common/Proton 9.0 (Beta)/proton' to a different Proton version.
      * If you want to use MO2 to manage a different game, you can change 'STEAM_COMPAT_DATA_PATH=$HOME/.steam/steam/steamapps/compatdata/377160' ... find the correct ID for your other game and replace '377160' in that parameter with the other game's ID. (you're on your own at that point if something isn't working)

## 5. **Test MO2 Downloads**

At this point you should be able to click "Mod Manager Download" on Nexus Mods and have MO2 automatically open for further setup. Assuming you set up a Nexus account in MO2 you can go ahead and test it. 

* Close Fallout 4 and/or MO2 if they are currently open
* Go to [NexusMods > Fallout 4 > Mods](https://www.nexusmods.com/fallout4/mods/) and find a random simple mod to install. I picked [Brighter Settlement Lights](https://www.nexusmods.com/fallout4/mods/2494).
  * Go to the "Files" tab
  * Click "Mod Manager Download" link
  * Select "Slow Download" on the next screen (unless you subscribe to NexusMods) ... if everything worked, MO2 should automatically open and download the mod. 
  * I'm not currently planning to write a full HOW-TO on using MO2 but there will be some stuff added in later sections for the basics. One thing to know is you can change the theme in MO2 to be a dark mode or many other schemes, some of which may render text better on the Steam Deck.

## 6. **Installing Fallout 4 Script Extender (F4SE)**

F4SE is used by many mods as a dependency. It works fine with MO2 but has some extra steps to get it integrated versus other mods. This is how I do it on my installation, including setting up MO2 to use F4SE.

**NOTE:** F4SE has recently been updated to work with Fallout 4's "Next Gen" update. Just grab the latest version and make sure you're using the latest Fallout 4 version (unless you have specific mods that aren't updated yet, in which case you're on your own for this). 

The video linked earlier has a good section on installing F4SE (even though that video uses Windows). Feel free to refer to it [starting at 4 minutes](https://youtu.be/-rFwk4tb6Ew?t=227). The instructions below summarize what to do:

* Close Fallout 4 and/or MO2 if either is currently running
* [manually download the F4SE archive](https://www.nexusmods.com/fallout4/mods/42147?tab=files), just save it to your Downloads folder. 
* Open the Dolphin file manager
* Go to your Downloads directory
* Right-click on the 'Fallout 4 Script Extender (F4SE)-[version].7z' file
   * *Extract* > 
   * *Extract archive here, autodetect subfolder*
* Enter the 'f4se_[version]' directory
* Select all files/directories
* Right-click > Copy
* Go to this directory:
  `/home/deck/.steam/steam/steamapps/common/Fallout 4`
* Paste the files copied above
* (optional) Go back to Downloads and delete the extracted files and (also optional) the F4SE archive. *You might wait until everything is working to clean these up.*
* "Play" Fallout 4 from Steam
   * MO2 should come up, not Fallout 4
   * In MO2, click the Instance Manager button (far left of the top icon toolbar, red and blue arrows)
      * Click 'Create new instance' (this instance will use F4SE)
      * 'Next' on the information screen
      * 'Create a global instance' > 'Next'
      * Name the instance 'Fallout 4 (F4SE)' > 'Next'
      * Keep the default checked for 'Automatic archive invalidation' > 'Next'
      * Accept the default location 'C:\users\steamuser\AppData\Local\ModOrganizer\Fallout 4 (F4SE)' > 'Next'
      * 'Finish' on the Confirmation screen
      * MO2 will restart and look like it has never been run before, that's fine, it's because of the new instance. Skip the Tutorial. 
      * 'Import Nexus Categories'
      * Now is the time to make MO2 look more "Steam Deck Friendly". Click the "Tools" icon (screwdriver & wrench) and go to the "Theme" tab. Pick a theme you like. 'Transparent-Style-FO4-Trosski' does a good job rendering text on an external monitor. 
* Now you can switch between "F4SE" to play Fallout with the F4SE mod, or "Fallout 4" for a stock Fallout. Just select whichever and press "Run" in MO2. 

However while there is a "Fallout Launcher" option in MO2, it currently doesn't work for you. Fix that next. 

## 7. **Fix the ability to run the Fallout Launcher**

SteamOS 3.5.x changed the way it handles many game launchers. Fallout opens directly to the game now when freshly installed. But there are configuration items you may need to do from the launcher (quality settings mostly, or resolution if you are playing on an external monitor). *This section helps you get the Launcher available to you again.* 

**NOTE:** This information should help you bring the Fallout Launcher back whether you use MO2 or not. However there are some differences (the fix is the same, the result is a little different). 

Credit to [How to change Fallout 4 graphics settings on Steam Deck](https://www.ggrecon.com/guides/fallout-4-steam-deck-graphics-options-best-settings/) for the steps, I'm just summarizing them here. 

* Go to your Steam Library
* Right-click the Fallout 4 entry
* Properties > General > Launch Options
* Add this:
   `SteamDeck=0 %command%`
* Close the Properties window

At this point if you **do not have MO2 installed** the Fallout Launcher should load when you click "Play". But that's not what you're here for. 

If you **DO** have MO2 installed then change the instance you are running (drop-down list at the top-right of the window) to 'Fallout Launcher' and click 'Run' to run the launcher (it might seem like that should just work, but for now it does still require the extra Properties or 'Fallout Launcher' will just run the game without the launcher). 

Later on *if you are not using MO2*, you can remove the extra Properties (SteamDeck=0 %command%) from the Launch Options so you don't have to mess with the Launcher to play. **But for MO2 users just leave it set and let MO2 handle whatever it wants to use based on the "Run" selection.**

Now you can also select "Fallout Launcher" > Run in MO2 to configure graphics options. 

## 8. Optimizing Fallout 4 for the Steam Deck

For now just go through the settings and fixes on [this page](https://overkill.wtf/best-settings-fallout-4-on-steam-deck/)

You can install the various mod fixes using MO2. 

NOTES:

* 'Insignificant Object Remover' ... FULL
* 'Achievement Mods Enabled' ... no need for the DLL loader as we have F4SE running
* Step 4 ... no need, you're using MO2 to install the mods
* Step 5 ... to edit the file do this in the terminal:
  
  ```
  kate "/home/deck/.local/share/Steam/steamapps/compatdata/377160/pfx/drive_c/users/steamuser/AppData/Local/ModOrganizer/Fallout 4 (F4SE)/mods/TAA Flicker Fixer/TAAFlickerRemover.ini"
  ```

* Step 6 ... didn't do this (yet anyway)

Added stuff:

* In Fallout Launcher:
  * Anistropic Filtering: 4 samples
  * Remember to set resolution in Game mode later if you changed it to other than 1280x800 in Desktop mode (ie, on an external monitor)

## 9.  Other Performance Mods

* [Best Fallout 4 mods for Steam Deck and ROG Ally | Windows Central](https://www.windowscentral.com/gaming/7-best-mods-to-increase-fallout-4-performance-on-rog-ally-steam-deck-and-legion-go-gaming-handhelds) 

## 10. Mods needing a NG update:

* [Faster Workshop (Workshop Lag Fix) at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/35382?tab=files)
* [Mod Configuration Menu at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/21497?tab=posts) (beta available)
   * [MCM Booster at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/56997)
   * [MCM Settings Manager at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/56195?tab=description)
   * 

## 11. MAYBE mods

* Previsibines Repair Pack ... install before any previs breakers but install previs stuff near end of load order
   * [Immersive Cleaning - Previsibines Repair Pack (PRP) Patch at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/77099) 
* [Light Sources Do Not Cast Dynamic Shadows at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/30253?tab=files)
* [Fog Remover - Performance Enhancer II at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/31976?tab=files)
* [FAR - Faraway Area Reform at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/20713?tab=description)
* [STEAM - Boost Lite at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/85247)
* [Flicker Fixer at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/35720?tab=description) ... fix z-fighting
* [Fallout Priority Next-Gen - CPU Performance FPS Optimizer at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/52515?tab=posts)
* [Settlement Salvage Bot at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/32212?tab=posts) ... see comments about load order with Looted World
* [Effects Texture Reducer - Small Combat FPS Performance Boost at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/75912)

## 12. Mods that change story/gameplay

* [Unofficial Fallout 4 Patch - UFO4P at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/4598?tab=description)

MAYBE:

* [The Machine And Her at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/30488)
   * [The Machine and Her Revival at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/84847)
* [Beantown Interiors Project True Interior Patch at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/63975?tab=description)
* [20 Leagues Under the Sea - Vault 120 at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/58514?tab=description)
* 

Need NG update :( :

* Tales of the Commonwealth
* [Bullet Counted Reload System (BCR) at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/41178?tab=files)

## 13. Windows-only things that might adapt to Steam Deck:

* [VRAMr at Fallout 4 Nexus - Mods and community](https://www.nexusmods.com/fallout4/mods/80305?tab=files)

* 
