# PSTerminalServices - UNOFFICIAL CLONE
# **Unofficial Clone of [PSTerminalServices](http://psterminalservices.codeplex.com/) Project by [Shay Levy](http://www.codeplex.com/site/users/view/Shay)**
# <u>**About the Terminal Services Module**</u>
Page content retrieved from http://psterminalservices.codeplex.com/ on July 15, 2017 with minor spelling corrections/edits.

Terminal Services PowerShell Module  

The PSTerminalServices module contains functions to manage Terminal Services (including RemoteDesktop) sessions and processes.  It is based on an open source project named [Cassia](http://code.google.com/p/cassia/) (version 2.0.0.60), a .NET library for accessing the native Windows Terminal Services API.  

The following operations are supported on the local and a remote terminal server:  

1\. Enumerating terminal sessions and reporting session information including connection state, user name, client name, client display details, client-reported IP address, and client build number.  
2\. Logging off a session.  
3\. Disconnecting a session.  
3\. Displaying a message box in a session.  
4\. Enumerating all processes or processes for a specified session.  
5\. Killing a process.  

Supported Platforms  
-------------------  
Cassia has been tested on Windows Server 2008 R2 beta, Windows Server 2008, Windows Server 2003, Windows XP, and Windows Server 2000\. It should work on Windows Vista as well.  

Cassia - Source Files  
---------------------  
Source files of Cassia is included in the module's bin directory (compressed file).  

The module can be installed automatically by downloading the MSI package or manually by downloading a ZIP file. The MSI package will install the module under your Documents folder (%USERPROFILE%\Documents\WindowsPowerShell\Modules\RemoteRegistry). The ZIP file contains the module files only and you need to extract its content to one of two places:  

1.  %USERPROFILE%\Documents\WindowsPowerShell\Modules
2.  %WINDIR%\System32\WindowsPowerShell\v1.0\Modules (need admin privileges)
3.  **If the directory tree (of one of the above) doesn't exist then you should manually create it.**

# <u>**Prerequisites**</u>

Windows PowerShell 2.0 [http://support.microsoft.com/kb/968929](http://support.microsoft.com/kb/968929)  

# <u>**How to use the module**</u>

Check if the module is installed correctly, from your PowerShell session type:  

<pre>PS > Get-Module -Name PSTerminalServices -ListAvailable

ModuleType Name                      ExportedCommands
---------- ----                      ----------------
Manifest   PSTerminalServices        {}
</pre>

If you don't see the above result then the module was not installed correctly. If you choose to download and install the module manually (zip file), make sure the module directory exists under "%USERPROFILE%\Documents\WindowsPowerShell\Modules"  

Importing the module:  

<pre>PS > Import-Module PSTerminalServices
</pre>

Get a list of module functions  

<pre>PS > Get-Command -Module PSTerminalServices

CommandType Name                 Definition
----------- ----                 ----------
Function    Disconnect-TSSession ...
Function    Get-TSCurrentSession ...
Function    Get-TSProcess        ...
Function    Get-TSServers        ...
Function    Get-TSSession        ...
Function    Send-TSMessage       ...
Function    Stop-TSProcess       ...
Function    Stop-TSSession       ...
</pre>

There is also an about_PSTerminalServices_Module help file (under the en_US folder), to review it:  

<pre>PS > Get-Help about_PSTerminalServices_Module
</pre>

The following functions are added to the current session when you import the module:  

*   Disconnect-TSSession - Disconnects any attached user from the session.
*   Get-TSCurrentSession - Provides information about the session in which the current process is running.
*   Get-TSServers - Enumerates all terminal servers in a given domain.
*   Get-TSProcess - Gets a list of processes running in a specific session or in all sessions.
*   Get-TSSession - Lists the sessions on a given terminal server.
*   Send-TSMessage - Displays a message box in the specified session ID.
*   Stop-TSProcess - Terminates the process running in a specific session or in all sessions.
*   Stop-TSSession - Logs the session off, disconnecting any user that might be connected.

# <u>**Some Code Examples**</u>

<pre>For more code examples use Get-Help <cmdletName> –Examples.

# Logs off all the active sessions from remote computer 'comp1', no confirmations
PS > Get-TSSession -ComputerName comp1 -State Active | Stop-TSSession –Force

# Gets all Active sessions from remote computer 'comp1', made from IP addresses that starts with '10'.
PS > Get-TSSession -ComputerName comp1 -Filter {$_.ClientIPAddress -like '10*' -AND $_.ConnectionState -eq 'Active'} 

# Displays a message box inside all active sessions of computer name 'comp1'."}
PS > $Message = "Importnat`n, the server is going down for maintenance in 10 minutes. Please save your work and logoff."
PS > Get-TSSession -State Active -ComputerName comp1 | Send-TSMessage -Message $Message 

# Gets all processes connected to session id 0 from remote computer 'comp1'.
PS>Get-TSSession -ID 0 -ComputerName comp1 | Get-TSProcess 
</pre>
