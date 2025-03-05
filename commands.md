#To deploy the python project#
1. First install the python
   apt install python3
2. Check the python version 
    python3 -V or python3 --version
3. Then update the kernel
    sudo apt update
4. Install the gunicorn (-> it is an python WSCG/http server)
   pip install gunicorn 
5. Setup the virtual environment inside the python file 
    python3 -m venv venv 
6. Activate the virtual environment 
   source venv/bin/activate
7.Make sure the virtual environment is activated 
8. Install flask 
   pip install Flask
9. Run your python file as 
   gunicorn app:app -b 0.0.0.0:8000 (This command will tell Gunicorn to serve your file app on all interfaces at prot 8000)
10. Access your application 
   http://your-ip:8000
11. Creating a reverse proxy 
   install the apache2
12.Enable the required apache modules
   a2enmod proxy
   a2enmod proxy_http

13.Create a virtual host configuration 
   nano /etc/apache2/sites-available/myapp.conf
14. Add the following configuration to the file: 
<VirtualHost *:80>
    ServerName your-server-ip

    ProxyPass / http://127.0.0.1:8000/
    ProxyPassReverse / http://127.0.0.1:8000/

    ErrorLog ${APACHE_LOG_DIR}/myapp_error.log
    CustomLog ${APACHE_LOG_DIR}/myapp_access.log combined
</VirtualHost>

Here replace your-server-ip with the server ip 
15. Enable the new site configuration
    a2ensite myapp.conf
16. Restart the Apache server 
   systemctl restart apache2

For the custom domain 
1. sudo nano /etc/hosts
2. Add the following line at the end of the file 
  127.0.0.1 your_domain.com
3.Update apache configuration 
  sudo nano /etc/apache2/sites-available/myapp.conf
 
Update the ServerName
<VirtualHost *:80>
    ServerName your_domain.com
    ServerAlias www.your_domain.com

    ProxyPass / http://127.0.0.1:8000/
    ProxyPassReverse / http://127.0.0.1:8000/

    ErrorLog ${APACHE_LOG_DIR}/myapp_error.log
    CustomLog ${APACHE_LOG_DIR}/myapp_access.log combined
</VirtualHost>

4.Restart the apache2 