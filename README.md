# Wifi-Unmasker
Windows batch to show wifi SSID and passphrases on a system

This is a fork from https://github.com/carlospolop/winPE

In winPE.bat the command to enumerate Wifi Profiles is “netsh wlan show profiles” which works well enough in a command-line given user interaction where one could 
 
Because the variable names need to be treated differently in BATCH, use the following syntax here.  Output will go either to the screen or to a previously specified file.

for /f "tokens=4,* skip=4" %%a in ('netsh wlan show profile') do ( for /f "tokens=*" %%c in ('netsh wlan show profile "%%b" key^=clear') do ( for /f "tokens=3,*" %%d in ('echo %%c^| find /i "Key Content"') do ( echo [SSID: %%b] [PASSWORD: %%e] ) ) )

