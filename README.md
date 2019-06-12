# WindowsShells
Today, there are several methods that can be utilized to gain shell access on a Windows Machine once you have verified credentials.
Here I will document the various tools, methods and commands that can be used:


## PSEXEC


## WMI Remoting


## PowerShell Remoting
PowerShell only works if the remote machine already has PowerShell installed and if PowerShell remoting is configured to allow remote access. 
As opposed to that PsExec only requires network access to the machine and administrator privileges. No agents or preinstalled server applications are required. This is especially useful when dealing with older systems such as Windows Server 2003 and Windows Vista, which don't have PowerShell installed by default.
If you need to access the operating system under the system account, PsExec is the simplest solution. A PowerShell session cannot run under a system account, unless I use PsExec for this purpose.

PowerShell Remoting must be enabled on the remote machine for this technique to work.  You can quickly check if a remote machine has powershell with the following command:  
```Test-WsMan COMPUTER```
If PowerShell Remoting is enabled you will see a message like:
```
wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd
ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd
ProductVendor   : Microsoft Corporation
ProductVersion  : OS: 0.0.0 SP: 0.0 Stack: 3.0
```
Otherwise yoi will see an error message like:
```
Test-WSMan : <f:WSManFault xmlns:f="http://schemas.microsoft.com/wbem/wsman/1/wsmanfault" 
Code="2150858770" Machine="My MACHINE"><f:Message>The client cannot connect 
to the destination specified in the request. Verify that the service on the destination is 
running and is accepting requests. Consult the logs and documentation for the WS-Management 
service running on the destination, most commonly IIS or WinRM. If the destination is the WinRM 
service, run the following command on the destination to analyze and configure the WinRM service: 
"winrm quickconfig". </f:Message></f:WSManFault>
```

The following script will check a list of hosts to see if PowerShell Remoting is enabled:  
```
$dev=Import-CSV hosts.csv 
foreach ($element in $dev)
{ 
    &  echo $element.Host
    &  Test-WsMan $element.Host
}
```
The CSV file will need to have the following format with "Host" as the first line:  
_CSV File Format_  
```
Host
COMPUTERNAME1
COMPUTERNAME2
COMPUTERNAME3
```


PSEXEC
