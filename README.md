# Wifi-Unmasker
Windows batch to show wifi SSID and passphrases on a system

   This is a fork from https://github.com/carlospolop/winPE

In winPE.bat the command to enumerate Wifi Profiles is “netsh wlan show profiles” which works well enough in a command-line given user interaction. But the winPE only calls 1 current wifi SSID, and the user needs to specify separately whether they want to call the passphrase.  I was interested to incorporate the SSID and passphrase information into the SYSdump tool, but found that this code was buggy when scripted.
 
    @for /f tokens^=2*delims^=^: %i in ('netsh wlan show profiles') do @set "_ssid=%~i" && @for /f tokens^=2*delims^=: %I in ('cmd/v/c netsh wlan show profiles "!_ssid:~1!" key^=clear^|findstr /vi "absent"^|findstr "Key"') do @echo/ssid=%~i : %I


 
Because the variables need to be named differently in BATCH vs. Windows CMD line, use the following syntax here.  Output will go either to the screen (if using @ECHO ON) or to a previously specified file.

    for /f "tokens=4,* skip=4" %%a in ('netsh wlan show profile') do (for /f "tokens=*" %%c in ('netsh wlan show profile "%%b" key^=clear') do (for /f "tokens=3,*" %%d in ('echo %%c^| find /i "Key Content"') do (echo [SSID: %%b] [PASSWORD: %%e])))
      
