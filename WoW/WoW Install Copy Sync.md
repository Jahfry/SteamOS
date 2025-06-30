# WoW on multiple Linux devices with synchronized install and addons

## Changelog:

* 2025-06-09 - Initial version, install complete, missing last section about copying/syncing to Bazzite

## INITIAL Install of WoW on SteamOS

### Set up ~/Games directory

We're going to use this directory for 2 reasons:

1. Makes it easier to find WoW later on for addon managers, log uploaders, etc
2. Simplifies moving the installed game on individual devices using symlinks (Steam Deck might prefer to have it on an SD card, etc)

* Run a console window (Konsole, etc) as your Gaming user (not root)
* `mkdir -p ~/Games`
* We'll do more here later but for now you're good to keep going

### Install "ProtonPlus" flatpak

* Use ProtonPlus to add the lated "Proton GE" (aka GE-Proton) to SteamOS, 10.4 was current as of this writing
* CLOSE Steam and re-open it to force it to see the new Proton option

### Install Battle.net Launcher

* [Download Battle.net](https://download.battle.net/en-us/desktop)
    * Find it in ~/Documents after download
    * Right-click it (Dolphin file manager) and "Add To Steam"
* Open Steam
* In Steam library find "Battle.net-Setup.exe" > Manage (settings icon) > Properties > Compatibility
    * [X] Force = "Proton Experimental"
        * Not "GE-Proton" (yet)
        * Try Proton Hotfix next if Experimental didn't work for the installer
* 'Play' "Battle.net-Setup.exe" in Steam
    * Language: select best for you (default is already US English) > Continue
    * Uncheck "Launch Battle.net when you start your computer" 
    * **'Change'** the Install Location
        * In the file selection, left panel, choose the bottom "/" folder (this is your Linux file system)
            * Navigate to "/home/deck"
            * Right-panel click "Games" and click 'Select Folder'
            * This will create ~/Games/Battle.net
            * *this doesn't matter to Steam*, we're changing this to make finding the files easier for ***you*** in the future. 
    * Once it finishes installing the main launcher *don't login yet* (won't hurt anything if you do, but will slightly change next instructions)
        * "X" (Close Window) in the top-right of the launcher
        * Give Steam a minute to notice the application closed, if it doesn't, 'X Stop' in Steam
* Steam Library > right-click "Battle.net-Setup.exe" > Manage > "Remove non-Steam Game from your Library"
        
### Configure Battle.net Launcher

* Dolphin file manager > 
    * Delete ~/Downloads/Battle.net-Setup.exe
    * Go to ~/Games/Battle.net/
        * Right-click "Battle.net.exe" > Add to Steam
* Steam Library > "Battle.net.exe"
    * > right-click > Properties
        * Compatibility: [X] Force the use
            * Proton versions are the real gotcha with Linux gaming, you'll have to see what works for you when doing this
                * I usually select the latest GE-Proton here (installed in earlier step)
                * Proton Experimental is also an option
            * If you change your Proton version in the future you will likely need to authenticate your login again each time
        * *Optional:* Shortcut > rename "Battle.net.exe" to "Battle.net" (or, "WoW" or whatever works for you)
    * 'Play'
        * check "[X] Keep me logged in"
        * log in as you normally would
            * Authenticate as needed
            * Captchas don't work well here, if you get one, try the Audio version. If you have to use the visual version, drag your mouse off the image each time before submitting to see what it is really submitting. 
        * "Scan For Games" will complaint it didn't find any games on your system, that's fine, ignore it and click 'Done'
        * Feel free to Skip Tour!
        * Blizzard icon (top-left) > Settings
            * App > General > 
                * "On Game Launch" > "Exit Battle.net completely"
                * "When Clicking X (close window)" > "Exit Battle.net completely" 
            * App > Startup > 
                * [X] Remember Login emails & phone numbers
                * [X] Keep me logged in
                * (optional) "On Startup, View" > "Last Viewed Game Page"

### Install World of Warcraft

I'm writing this for WoW Retail. Adapt these instuctructions as needed for any other Blizzard games. 

Battle.net should already be running from the previous step, but if not, launch it now

* Battle.net > World of Warcraft > 
    * IF you need a FRESH 'Install' (if you want to copy an existing install, skip to next)
        * 'Install' > 'Change' "Install Location" > 
            * navigate to "/home/deck/Games/"
            * select "Battle.net" > Open (confirm the installer is saying it will create "Z:\home\deck\Games\Battle.net\World of Warcraft")
        * You can uncheck "Create Desktop Shortcut", it won't really do anything
        * Let it install normally for a fresh install
        * *After* fresh install, *If you have a configured WoW on another machine*, you can copy over *only* these 2 directories from your old install to get your UI and addons:
            * ~/Games/Battle.net/World of Warcraft/_retail_/WTF
            * ~/Games/Battle.net/World of Warcraft/_retail_/Interface
    * *IF* you have a working installation copy from another device:
        * Close Battle.net
        * Navigate to ~/Games/Battle.net in you file manager
            * If it doesn't exist, create '~/Games/Battle.net/World of Warcraft'
            * If '~Games/Battle.net/World of Warcraft' ALREADY existed
                * Do you have a Classic or other WoW installation already working? If so, SKIP the next instruction as it will delete your Classic install, instead realize what it was doing and *make changes to fit your needs*. ***I'm not going to make this guide comprehensive enough to handle a situation where this wasn't a fresh install of Battle.net.***
            * You *probably* started the install in Battle.net but stopped it to make your copy. ***If so*** Delete everything in '~/Games/Battle.net/World of Warcraft/' (but don't delete anything above 'World of Warcraft')
        * Copy in your existing WoW folder from another location to '~/Games/Battle.net/World of Warcraft'
            * If this was from a Windows machine default install it would be: 'C:\Program Files (x86)\World of Warcraft'
            * If this was from a different Linux machine it would be similar to 
                * '~/Games/Battle.net/World of Warcraft' (if done similar to my instructions above)
                * **or** maybe '~/Games/WoW/drive_c/Program Files (x86)/World of Warcraft', etc if installed without changing the default location, etc
            * **The important things**:
                * When the copy is done you should see '~/Games/Battle.net/World of Warcraft/_retail_'. If that copied properly to that location then things should be fine.
                * If this copy was from a different machine, you may need to fix permissions on the files (not needed in my case copying from a Bazzite PC)
                    * "deck" should be the owner and group of everything under ~/Games/ (including Battle.net and World of Warcraft) 
                    * permissions should be writable for owner "deck"
            * At this point "Battle.net" launcher can be started
                * Launcher should see "World of Warcraft (retail)" as installed
                * Try to 'Update' if it provides that instead of 'Play'
                    * If it fails to update (mine did after the game copy from another system), try:
                        * Close Battle.net launcher (wait until Steam shows "Play" not "Stop")
                        * Delete '~/Games/Battle.net/World of Warcraft/Launcher.db'
                        * restart Battle.net
                        * 'World of Warcraft (retail)' > Options > Scan and Repair
                            * This takes a bit but is far faster than a fresh install
                            * Once the Scan and Repair is done, try to 'Update' again and it *should* work.
                            * Before giving up (next item), close and restart Battle.net one more time ... it was this restart that cleared up the issues on my system. You might even want to reboot your Deck once. 
                            * If rebuilding 'Launcher.db' and 'Scan and Repair' simply don't work (you can't 'Update' and you can't 'Play'), the simplest option is:
                                * Close Battle.net launcher
                                * Delete everything under '~/Games/Battle.net/World of Warcraft'
                                * Install fresh from the previous section and don't use this section again

# TO DO:

## Future Installs on other Linux Devices

This goes much quicker as you're not doing a fresh install each time. We're going to re-use the installation above. 

### Synchronize folders (~/Games/Battle.net)

### Add to Steam as non-Steam game


