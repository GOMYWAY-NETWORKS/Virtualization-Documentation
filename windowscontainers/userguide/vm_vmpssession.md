ms.ContentId: e586a11a-870f-403b-8af8-4c2931589d26
title: VM management using PowerShell  

# Managing Virtual Machines with PowerShell #

## PowerShell Remoting from the Hyper-V host ##
** Windows 10 hosts to Windows 10 VMs only **
These instructions were written against build 10035.  The hope is that it'll all be enabled by default before Technical Preview 2.

Use a VM Session to create a PowerShell session directly between the Hyper-V host and the virtual machines running on that host using the `-VMName` or `-VMguid` flags.

To enable this feature in Technical Preview 2 builds:

1.  Enable the Guest Services IC in the VM settings.
  
  **Using the Hyper-V UI**

  ![Selecting the VM Settings](media\vm_edit_VM_settings.png)

  ![Enabling the Guest Services IC](media\vm_enable_guest_services_ic.png)
	
  **Using PowerShell on the Host**

  Make sure you're running PowerShell as an Administrator.
	
  Run:
	```
	Enable-VMIntegrationService -VMName <YourVM’s Name> -Name "Guest Service Interface"
	```
	
	
2.  Start the VM Session service inside the VM.
  Connect to the virtual machine
  Open the services control window
  ![Starting the VM Session service](media\vm_start_VM_PowerShell_service.png)

3.	Enter a VM Session
  Open a PowerShell window as Administrator.
	
	```	
	$cred = Get-Credential  #provide a valid guest credential
	$session = New-PSSession -VMName PSTest -Credential $cred 
	```

  This also works with Invoke Command.

	```
	$cred = Get-Credential  #provide a valid guest credential
	$job = Invoke-Command -VMName PSTest -Credential $cred  -ScriptBlock {Set-PSBreakpoint C:\script\foo.ps1 -Line 4; C:\script\foo.ps1} -AsJob -Debug
	
	Start-Sleep -Seconds 1
	Get-Job 
	Debug-Job -Job $job
	```
