As penetration tester, often gain access to highly sensitive data such as:
	1. user lists
	2. credentials(i.e., downloading the NTDS.dit file for offline password cracking)
	3. Enumeration Data about the organization's network infrastructure
	4. AD{Active Directory} environment.
	5. Therefore, its essential to encrypt this data or use encrypted data connections such as:
		- SSH
		- SFTP
		- HTTPS
# File Encryption on Windows:

- Many different methods can be used to encrypt files and inforamation on Windows systems. 
- `Invoke-AESEncryption.ps1` PowerShell script method.
- This script is small and provides encryption of files and strings.

## Invoke-AESEncryption.ps1
```powershell-session
.EXAMPLE
Invoke-AESEncryption -Mode Encrypt -Key "p@ssw0rd" -Text "Secret Text" 

Description
-----------
Encrypts the string "Secret Test" and outputs a Base64 encoded ciphertext.
 
.EXAMPLE
Invoke-AESEncryption -Mode Decrypt -Key "p@ssw0rd" -Text "LtxcRelxrDLrDB9rBD6JrfX/czKjZ2CUJkrg++kAMfs="
 
Description
-----------
Decrypts the Base64 encoded string "LtxcRelxrDLrDB9rBD6JrfX/czKjZ2CUJkrg++kAMfs=" and outputs plain text.
 
.EXAMPLE
Invoke-AESEncryption -Mode Encrypt -Key "p@ssw0rd" -Path file.bin
 
Description
-----------
Encrypts the file "file.bin" and outputs an encrypted file "file.bin.aes"
 
.EXAMPLE
Invoke-AESEncryption -Mode Decrypt -Key "p@ssw0rd" -Path file.bin.aes
 
Description
-----------
Decrypts the file "file.bin.aes" and outputs an encrypted file "file.bin"
#>
function Invoke-AESEncryption {
    [CmdletBinding()]
    [OutputType([string])]
    Param
    (
        [Parameter(Mandatory = $true)]
        [ValidateSet('Encrypt', 'Decrypt')]
        [String]$Mode,

        [Parameter(Mandatory = $true)]
        [String]$Key,

        [Parameter(Mandatory = $true, ParameterSetName = "CryptText")]
        [String]$Text,

        [Parameter(Mandatory = $true, ParameterSetName = "CryptFile")]
        [String]$Path
    )

    Begin {
        $shaManaged = New-Object System.Security.Cryptography.SHA256Managed
        $aesManaged = New-Object System.Security.Cryptography.AesManaged
        $aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CBC
        $aesManaged.Padding = [System.Security.Cryptography.PaddingMode]::Zeros
        $aesManaged.BlockSize = 128
        $aesManaged.KeySize = 256
    }

    Process {
        $aesManaged.Key = $shaManaged.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($Key))

        switch ($Mode) {
            'Encrypt' {
                if ($Text) {$plainBytes = [System.Text.Encoding]::UTF8.GetBytes($Text)}
                
                if ($Path) {
                    $File = Get-Item -Path $Path -ErrorAction SilentlyContinue
                    if (!$File.FullName) {
                        Write-Error -Message "File not found!"
                        break
                    }
                    $plainBytes = [System.IO.File]::ReadAllBytes($File.FullName)
                    $outPath = $File.FullName + ".aes"
                }

                $encryptor = $aesManaged.CreateEncryptor()
                $encryptedBytes = $encryptor.TransformFinalBlock($plainBytes, 0, $plainBytes.Length)
                $encryptedBytes = $aesManaged.IV + $encryptedBytes
                $aesManaged.Dispose()

                if ($Text) {return [System.Convert]::ToBase64String($encryptedBytes)}
                
                if ($Path) {
                    [System.IO.File]::WriteAllBytes($outPath, $encryptedBytes)
                    (Get-Item $outPath).LastWriteTime = $File.LastWriteTime
                    return "File encrypted to $outPath"
                }
            }

            'Decrypt' {
                if ($Text) {$cipherBytes = [System.Convert]::FromBase64String($Text)}
                
                if ($Path) {
                    $File = Get-Item -Path $Path -ErrorAction SilentlyContinue
                    if (!$File.FullName) {
                        Write-Error -Message "File not found!"
                        break
                    }
                    $cipherBytes = [System.IO.File]::ReadAllBytes($File.FullName)
                    $outPath = $File.FullName -replace ".aes"
                }

                $aesManaged.IV = $cipherBytes[0..15]
                $decryptor = $aesManaged.CreateDecryptor()
                $decryptedBytes = $decryptor.TransformFinalBlock($cipherBytes, 16, $cipherBytes.Length - 16)
                $aesManaged.Dispose()

                if ($Text) {return [System.Text.Encoding]::UTF8.GetString($decryptedBytes).Trim([char]0)}
                
                if ($Path) {
                    [System.IO.File]::WriteAllBytes($outPath, $decryptedBytes)
                    (Get-Item $outPath).LastWriteTime = $File.LastWriteTime
                    return "File decrypted to $outPath"
                }
            }
        }
    }

    End {
        $shaManaged.Dispose()
        $aesManaged.Dispose()
    }
}
```
### Import module Invoke-AESEncryption.ps1

- `Import-Module .\Invoke-AESEncryption.ps1`

	- After the script is imported, it can encrypt strings or files
	- This creates an encrypted file with the same name as the encrypted file but with the extension "`.aes`"
### File Encryption Example:

- `Invoke-AESEncryption -Mode Encrypt -Key "p4ssw0rd" -Path .\<file_name>`

	- resulting  file: `<file_name.aes>`
	
- Using very strong and unique passwords for encryption for every organization where a penetration test is performed is essential.


# File Encryption on Linux:

- `OpenSSL` is frequently included in Linux distribution, with sysadmins using it to generate security certificates, among other tasks.
- `OpenSSL` can be used to send files "nc style" to ==encrypt files==.

- To encrypt a file using `openssl`
	- select different ciphers {ref from man page of OpenSSL}
	
	- override the default iterations counts with the option `-iter 100000` 
		- It hashes the password 100000 times
		- That makes brute-forcing much slower.

	- `-pbkdf2` to use the Password-Based key Derivation Function 2 algorithm.
		- it is an `aes` type of an encryption.
	When hit enter, then the password is needed.

- ### Encryption /etc/passwd with openssl:

	- `openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc`
	
		- use strong and unique password to avoid brute-force cracking attacks.  
	
- ### Decrypt passwd.enc with openssl
	
	- `openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd`
