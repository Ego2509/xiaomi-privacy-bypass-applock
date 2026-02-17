this fork targets the app lock pattern and shows a successful guess attempt. It is intended to recover your files that were once lost because you forgot your lock pattern,
and exploits the "privacy_wrong_attempt_num" property to have as many attempts as you need. 

# IMPORTANT distinction
the original project targets the PRIVACY PASSWORD only, which is different from APP LOCK, an app that allows to restrict access to certain applications that the user
intents to hide/protect. The original bypass only targets the PRIVACY PASSWORD, whereas THIS FORK targets the APP LOCK without killing the activity or crashing the UI. 

---

# Xiaomi Privacy Password and App Lock Bypass (No Factory Reset)

---

Bypass Xiaomi’s Privacy Password/App lock without factory reset.

# DISCLAIMER (What to expect):
Someone reported that they lost old files, but files from the last three months remained untouched. Use this method at YOUR OWN RISK. Personally I didn't have any problem.  
**I'm not responsible for any issues that arise from following this method**.  

# Disclaimer (Intention):
Use this in devices you OWN. As a product customer you should have full permission to try as many times as you like to remember the lock pattern and recover your precious data. I wish you success if that is your intention. If you were patient and stored your phone hoping that one day you'd be able to recover the files maybe this is your tool. You might recover the files that were once locked. If this is not your intention, I am not responsible for any wrongdoings or how this might be used in any other case.  

This guide is for **educational purposes only**. Use it only on **YOUR OWN DEVICE**.


This guide explains how to **disable Xiaomi's Privacy Password** without a factory reset. Tested and confirmed working on:
+ **Xiaomi Note 12** (HyperOS 2.0.9.0). 
+ **Xiaomi Redmi 9** (MIUI Global 12.5.6 Stable) (MIUI Global 12.5.6.0(RJCMIXM)).


## Requirements
- You **must be able to unlock and access your phone** (this method will not work if the phone is completely locked).  
- The phone must be running MIUI with **Developer Options enabled**.
- Internet access required to install apps.  

## Prerequisites 
1. Enable Developer Options:
   - Go to Settings > About Phone
   - Tap OS version 7 times until you see "You are now a developer!"
2. Enable USB Debugging:
   - Go to Settings > Additional Settings > Developer Options
   - Enable USB Debugging
   - Enable USB Debugging (Security Settings) -- This is different from Enable USB Debugging; you will know why later on (from Shizuku Guide).
3. Install Shizuku & SetEdit:
   - Download Shizuku: https://play.google.com/store/apps/details?id=moe.shizuku.privileged.api
   - Download SetEdit: https://play.google.com/store/apps/details?id=by4a.setedit22

## Step-by-Step Guide 
1. Start Shizuku
   - Open Shizuku, follow the instructions, and start the service.
   - Change notification shade / notification style from Classic to Android -- you will know why later on (from Shizuku Guide).
   - Here is their Guide: https://shizuku.rikka.app/guide/setup/#start-shizuku

2. Grant SetEdit Permissions
   - Open Shizuku, go to the "Apps" tab.
   - If you see SetEdit, tap it and grant shell permissions.
   - If SetEdit doesn't appear, you can ignore it or try manually granting permissions using ADB. If you proceed without it and it still doesn't work, consider using ADB.

3. Modify Privacy Password Settings
   - Open SetEdit.
   - Tap the dropdown in SetEdit, change 'System Table' to 'Secure Table' at the top right.
   - Locate these settings:
     - `privacy_password_is_open`
     - `access_control_lock_enabled`   
   - Change their values to `0`.
   - If they don't exist, add them manually.
   - It auto-saves so just exit the app.

4. Reboot Your Phone
   - Restart your phone to apply the changes **and check setEdit first before opening the settings app or the security center**.
   - Check the values of (you can do this by simply doing for example `adb shell settings get secure privacy_wrong_attempt_num`)
     - `privacy_password_is_open`
     - `access_control_lock_enabled`
     - `privacy_wrong_attempt_num` keep track of this one.
   - Exit the app.
   - Check if Privacy Password is disabled: Settings > Passwords & Security > Privacy protection password (Off).
   - Keep going with step 5. 

5. Check if you can trace the app lock attempt counter:
   - In setEdit, change 'System Table' to 'Secure Table' at the top right again,
   - Locate this setting `privacy_wrong_attempt_num`
     - What is the value? is it 15, 5 40? this is the number of attemtps you currently have spent trying unlock app lock.
     - keep this number in mind. Save it or write it down in a piece of paper or notepad. Lets name it `GUESSES`.

6. Check if the attempt number updates:
   - Do a wrong attempt (if you cannot, jumpt to step 7) in App Lock: Settings > Apps > App lock
   - go to step 5 (Check `privacy_wrong_attempt_num`), is it `GUESSES + 1`? Try step 7 
   
7. Try to set a negative wrong attempt num with SetEdit:
   - Just shizuku: `adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh`
   - allow write permission to SetEdit `.\adb shell pm grant by4a.setedit22 android.permission.WRITE_SECURE_SETTINGS`
   - Poke the `privacy_wrong_attempt_num` and put a negative value, say for example -10.
   - Repeat steps 5 and 6.
   - If `privacy_wrong_attempt_num` is `-9`, it means this bypass works, you can brute-force the lock pattern just by executing the script that uses ADB.

8. Run the script lock_pattern_bruteforce.bash 
   - This step depends on how much time you have, but thankfully all the attempts can be done in no more than 3 weeks. 
   - It takes about 5 seconds per attempt, you can optimize it to make it run faster but I warn you that adb sometimes interrumpts
   - the process with Dialogs, the script has a mechanism to just click out. If it doesn't work tune it for your own needs;
     You might want to send a KEYCODE_BACK or something else if the dialog is stubborn. Let it go off, don't remove the sleep there.  
   - I personally think it is better to go from 4, 5, 6 dot lock patterns first and lastly try the 7,8,9 dot patterns; unless you have a good guess.
     That is because You can fully discard simpler patterns first.


## ADB Guide (Optional, if SetEdit Doesn’t Appear and it didn't work)
If SetEdit doesn't appear in Shizuku and the method didn't work, you can try manually granting its permissions via ADB:
### Steps:
1. Install **ADB on your PC**:
   - Download platform tools: https://developer.android.com/studio/releases/platform-tools
   - Extract and open the folder.
2. Connect your phone to the PC via USB or follow this guide https://www.xda-developers.com/install-adb-windows-macos-linux/.
3. Open a command prompt in the platform-tools folder and enter:
   ```sh
   .\adb devices
   ```
   - If your device appears, proceed.
   - If it asks for permission on the phone, accept it.
4. Enter the following command to grant SetEdit permissions:
   ```sh
   .\adb shell pm grant by4a.setedit22 android.permission.WRITE_SECURE_SETTINGS
   ```
5. If successful, continue with the next steps 3 and 4 from Step-by-Step Guide. I personally did this step, but SetEdit still didn't appear in Shizuku. I continued anyway, and it still worked.
   
##  Optional Cleanup
- Turn off Developer Options (optional but recommended).
- Uninstall Shizuku & SetEdit to keep things clean.
- Change notification style back to Classic.


**You're all set!**
