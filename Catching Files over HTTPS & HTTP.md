- # Creating a secure web server:
	
	- ### Nginx - Enabling PUT:
		
		- A good alternative for files to Apache is Nginx.
		- The Configuration is less complicated
		- The module system does not lead to security issues as Apache.
		- When allowing HTTP uploads, it is critical to be 100%  positive that users cannot upload web shell and execute them.
		- Configuring Nginx to use PHP.
		
			- ### Create a Directory to Handle Uploaded Files:
	
				- `mkdir -p /var/www/uploads/<directory_name>`
			
			- ### Create the Owner to www-data:
			
				- `chown -R www-data:www-data /var/www/uploads/<directory_name>`
			
			- ### Create Nginx Configuration File:
				
				- create the file under: `/etc/ngnix/sites-available/upload.conf`
				
	```shell-session
	server {
	    listen 9001;
	    
	    location /SecretUploadDirectory/ {
	        root    /var/www/uploads;
	        dav_methods PUT;
	    }
	}
	```
			
	- ### Symlink Site to the sites-enabled Directory:

		- `ln -s /etc/ngnix/sites-available/upload.conf /etc/nginx/sites-enabled/`
	
	- ### Start Nginx:
	
		- `systemctl restart nginx.service`
		- if get any error then check ==/var/log/nginx/error.log==
	
	- ### Verifying Errors:
	
		- `tail -2 /var/log/nginx/error.log` 
		- `ss -lnpt | grep 80`
		- `ps -ef | grep 2811`
	
	- ### Remove NginxDefault Configuration:
	
		- `rm /etc/nginx/sites-enabled/default`
	
	- ### Upload File Using cURL:
	
		- `curl -T <file_path> http://localhost:9001/<direcertory_name>/<file_name>`
		- `tail -1 /<directory_path>/<file_name>`
