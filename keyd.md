# 'keyd' on the Steam Deck

This is a long guide to help people newer to modifying their Steam Deck. Sorry for the folks that just want the nitty gritty, I'd rather just write 1 guide :)

*What are we doing?* Installing 'keyd' on a Steam Deck. 

*Why are we doing that?* So you can remap keys on a physical USB keyboard and have it work *both in Game and Desktop mode*, without affecting the Steam Deck's "volume" and "power" buttons. 

**The Main Problem?** 'keyd' only comes in source code so you'll need to set up your Steam Deck to compile the program first.

***Warning***: Updating SteamOS will overwrite the compiler packages we need to update to do the compiling. I'm not sure if that will affect the 'keyd' binary or not after an update, **but it probably will**. If so you will *need to repeat this process after SteamOS updates*. 

**I'll try to remember to edit this section after the next SteamOS update to answer whether you'll have to do this,** but I think it is a safe assumption. 

***Don't go any farther*** unless you are comfortable with running Linux terminal commands and are willing to take responsibility for any mistakes (at worst you may get to a point where you have to reset your Deck, this won't BREAK it, but mistakes *could* be very inconvenient). Honestly this guide is *meant for people who don't mind tinkering with Linux*. The best thing that could happen would be Valve incorporating 'keyd' into their distribution so that it didn't break on OS updates. If you don't feel like you can keep things straight for each update this process may not be for you. **An aternative is 'kmonad'**, which I've used before, but doesn't (at least not easily) work in Game Mode. 

*Why is Game Mode important?* Simply put ... frames per second. For me World of Warcraft gets about 50% more FPS in Game Mode than in Desktop Mode. If you've been happy with performance playing a game that wants a keyboard in Desktop Mode, look into 'kmonad' for a solution that works through SteamOS updates. I nuked the install that worked with 'kmonad' and forgot to document it so unfortunately (at least for now) I don't have a guide for that. 

## 0. Prep work for compiling 'keyd' on the Steam Deck

* Go into your Steam Deck's "Desktop" mode and connect a USB keyboard. You're not going to want to do this guide with the virtual keyboard.
* For most of this document you'll be working in a Konsole terminal window. Go ahead and open it now.
* If your Steam Deck doesn't have a root password, set one now via `passwd`
  * if you really really don't want a password later on, you can use `passwd -d` after this document to remove it
  * Since you will likely have to redo this after an update ... well ... just keep your password set.
  * If I find a way to set up 'keyd' locally instead of system wide I will update this document, but as it requires systemd to run in Game mode, that probably won't be the case.
  * ***IMPORTANT NOTE:***
     * ***Write this password down / store it in a way you can't lose.***
     * There are ways to recover your password without resetting your Steam Deck to factory but I'm **not** going to be tech support!
* Set your SteamOS to be writable so that following commands will work (we will go back to read only when 'keyd' is installed):
  `sudo steamos-readonly disable`
* Initialize 'pacman' via: 
```
sudo pacman-key --init
sudo pacman-key --populate archlinux
```
* Tell pacman to trust ALL sources (temporary, we'll switch it back after). 
   * This comes from a hint on [this issue](https://github.com/malordin/steamdeck-samba-server/issues/4) where it appears that SteamOS does not ship with complete packages for headers/glibc/base-devel.
   * Edit '/etc/pacman.confg'
      * Use whatever editor you like, example: `sudo nano /etc/pacman.conf` (all of my commands will be for nano, swap if you want)
      * Originally:
        
         > SigLevel    = Required DatabaseOptional
         
      * *Change that to this:*
         ```
         #SigLevel    = Required DatabaseOptional
         SigLevel = TrustAll
         ```
      * Save the file
* Install/repair the compiler/headers needed to compile 'keyd':
  
   `sudo pacman --sync --noconfirm base-devel glibc linux-api-headers`

* **IF** the above command executed properly, do this step. *If you saw errors and nothing was installed, pause this guide now to try and figure out what happened until the step above executes properly*. 
   * Remove the 'TrustAll' so that pacman requires trust again:
 
     `sudo nano /etc/pacman.conf`
     
      * Currently:
         ```
         #SigLevel    = Required DatabaseOptional
         SigLevel = TrustAll
         ```
      * *Change that to this:*
         ```
         SigLevel    = Required DatabaseOptional
         #SigLevel = TrustAll
         ```
      * Save the file

## 2. Download, make and install 'keyd'

These instructions are taken from [the keyd github README](https://github.com/rvaiya/keyd?tab=readme-ov-file#installation). I'm repeating them here with some tweaks (but go read that link to learn about keyd). 

* First, decide where you want to store the source code. I made a directory named '~home/Documents/source'.

  If you want the same directory, do the following. Otherwise adapt the command to your chosen directory:
  
  `mkdir ~home/Documents/source; cd ~home/Documents/source`

* Now clone, build and install 'keyd':

```
git clone https://github.com/rvaiya/keyd
cd keyd
make && sudo make install
sudo systemctl enable keyd && sudo systemctl start keyd
```

## 3. Set your Deck back to 'readonly'

'readonly' can't be on before this point because the 'make install' above needs to write for the last step above. Now that that is done, set it back to readonly: 

`sudo steamos-readonly enable`

## 4. Start remapping keys

Much of the following is also taken directly from [GitHub - rvaiya/keyd: A key remapping daemon for linux.](https://github.com/rvaiya/keyd?tab=readme-ov-file#installation)

* Create the file '/etc/keyd/default.conf'

  `sudo nano /etc/keyd/default.conf`

  and paste in this:
  
  ```
  [ids]
  
  *
  
  [main]
  
  # Maps capslock to escape when pressed and control when held.
  capslock = overload(control, esc)
  
  # Remaps the escape key to capslock
  esc = capslock
  ```

  * This is the default example from the keyd github. We'll be customizing it later for the Deck. 
  * This config will remap your 'escape' key to be 'capslock'. Your capslock key becomes 'escape' when quickly pressed or 'control' if held down. 
* **'keyd' won't do this remapping until you reload it**. If you are ready to swap your escape and capslock keys for testing, do this:

  `sudo keyd reload`

* To revert back to your normal keyboard layout:
  
  `sudo nano /etc/keyd/default.conf`
  
  then comment out the "capslock" and "esc" lines (add a # at the beginning of those lines), save the file, and then:
  
  `sudo keyd reload`

* Your keyboard should be back to normal operation but you have a skeleton file ready for remapping. 
* **If you ever create a bad mapping that prevents you from using your keyboard:** 

   * hold *'backspace' + 'enter' + 'escape'* at the same time to kill the keyd process.
   * Edit your conf file again to fix whatever happened and run keyd again:

     `sudo systemctl start keyd`.

* If you aren't sure what the name of the key you need to remap is, you can watch key events via `sudo keyd monitor`. If you want time stamps, use `sudo keyd monitor -t`.
  * **If keyd daemon is running**, the output of 'monitor' will show the **remapped** keys.
  * To see the keys without remapping just stop keyd (`sudo systemctl stop keyd` or hold 'backspace+enter+escape`) and run
    
    `sudo keyd monitor`

* For more information on using the 'keyd' main program:
   * `keyd --help` or
   * `man keyd` for more details

## 5. Basic Steam Deck configuration

The volume and power buttons on the Steam Deck are handled through a keyboard device. *We don't want keyd to bind to those buttons*, so we're going to blacklist that device from keyd. 

* If you don't do this, the buttons can give different behaviors compared to a stock Steam Deck
* **Example:** *in Game mode you won't be able to hold down the "Power" button to bring up the menu that allows you to switch to Desktop mode* (you can still get there via the Steam Button > Power, but, let's keep things normal)

* **TLDR;** (just fix it)
  * edit '/etc/keyd/default.conf'
  
    `sudo nano /etc/keyd/default.conf`
  
  * Change the [ids] section to match this (replace the section between [ids] and [main], don't change [main] yet):
```
[ids]
# This section determines which devices 'keyd' will be able to remap.
# First line tells 'keyd' to bind to ALL devices except those blacklisted later.
*
# Place devices you DON'T want keyd to remap below.
#   * format example: -<vendor id>:<product id>
#   * "-" at the beginning blacklists the device from keyd
#   * see `man keyd` section on [ids] for more info
# Device: Steam Deck 'AT Translated Set 2 keyboard' - volume and power buttons
-0001:0001
```

   (read the comments in the section above to understand what is going on)

* * stop keyd so you can use 'keyd monitor' to see the base devices instead of the remapped keyd device:
    
    `sudo systemctl`
  
  * `keyd monitor`
     * While this command is running, press your "volume up" button.
     * You should see:
       
        > AT Translated Set 2 keyboard    0001:0001:a38e6885      volumeup down
     
        > AT Translated Set 2 keyboard    0001:0001:a38e6885      volumeup up

      * Read this as:
        
         > <device name>   <vendor id>:<product id>:<? not needed ?>   <key name>   <up or down>
         
      * If 'keyd' were running you would instead see:
   
        > keyd virtual keyboard   0fac:0ade:efba1ddf      volumeup down
        
        > keyd virtual keyboard   0fac:0ade:efba1ddf      volumeup up

  
  * blacklist the Steam Deck buttons (affects power menu and no desire to affect Steam Controls in either mode)

* * Primarily this is for the Power and Volume buttons on the Steam Deck. The other buttons and controls (sticks, pads) appear to purely be game controller functions which aren't affected (or seen) by keyd. *This is good*
  
  * You can see the <vendor id>:<product id> using `keyd monitor` in Desktop mode
  
  * Open a Konsole window
  
  * `sudo systemctl stop keyd` # stop keyd so actual vendor/product IDs are shown
  
  * `sudo keyd monitor`        # see what keys are being pressed in Konsole
  
  * Press the volume up button on the Steam Deck
  

* If instead you see

keyd virtual keyboard   0fac:0ade:efba1ddf      volumeup down
keyd virtual keyboard   0fac:0ade:efba1ddf      volumeup up

## 6. For more information

Please refer to [GitHub - rvaiya/keyd: A key remapping daemon for linux.](https://github.com/rvaiya/keyd?tab=readme-ov-file) for anything beyond what I'm showing you below. But I will give an example of how I configured my keyboard.

* The goal for my keyboard remapping is, well, weird. I use a numpad on the *left* side of my keyboard for playing World of Warcraft. To make it more useful I need to remap some of the numpad keys (like numpadEnter and numLock) so that I can use them for actions in WoW. 

* In addition to this I will attempt to make a configuration that only remaps those keys *while playing WoW* (but until I edit this I'm not sure how that will work on the Steam Deck, especially in Game mode).

* Because the numpad and main keyboard have some keys that are the same function (normally) I will also try to 'blacklist' the remappings to only affect the numpad.
  
  ```
  
  ```
