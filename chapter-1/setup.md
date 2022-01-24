# NGINX Source List For Debian 10 Server
- Create the sources file 
    
        
    `$ sudo touch /etc/sources.list.d/nginx.list`

    `$ sudo nano /etc/sources.list.d/nginx.list`

- Add the following configurations to the newly created file 

    ```
    deb http://nginx.org/packages/mainline/debain/ buster nginx

    deb-src http://nginx.org/packages/mainline/debian/ buster nginx
    ```
- Add the NGINX GPG signing key to your server

    ```shell
    wget http://nginx.org/keys/nginx_signing.key

    sudo apt-key add nginx_signing.key

    sudo apt-get update

    sudo apt-get install -y nginx

    sudo service nginx start
    ```

## Post Installation Test
- Now that we believe that we have successfully installed nginx. we may need to run the following checks by running the following commands

    - Check the version of nginx installed
    
        `$ sudo nginx -v`
    
        OUTPUT:
        
         ```
         nginx version: nginx/1.14.2
         ```

    - Check if the nginx is running by issuing this command
        
        `$ ps -aux | grep nginx`

        OUTPUT:

        ```
        root      3030  0.0  0.1  65660  1704 ?        Ss   10:50   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
        www-data  3031  0.0  0.3  65976  3372 ?        S    10:50   0:00 nginx: worker process
        root      3427  0.0  0.7  16632  7984 ?        Ss   16:12   0:00 sshd: nginx [priv]
        ```


## Some Important NGINX Commands

    |Command        |Description                                        |
    |:-------------:|:-------------------------------------------------:|
    | nginx -h      | Shows the NGINX help menu                         |
    | nginx -v      | Shows the NGINX version                           |
    | nginx -t      | Tests the NGINX configuration                     |
    | nginx -s      | Sends a signal to the NGINX master process        |
 
## Possible Installation Problems 
### Source directoty does not exist 
`touch: cannot touch '/etc/sources.list.d/nginx.list': No such file or directory`
### Solution
`$ sudo mkdir /etc/sources.list.d/`


# Serving Static Files

We can always serve static files such as the HTML, CSS, JavaScript and some files required by our website to function well.

- Open the NGINX configuration located at `/etc/nginx/conf.d/default.conf`

    `$ sudo nano /etc/nginx/conf.d/default.conf`

- Overite the file with the following new configurations
    ```
    server {
        listen 80 default_server;
        server_name www.example.com;

        location / {
            root /usr/share/nginx/html;
            # alias /usr/share/nginx/html;
            index index.html index.htm;
        }
    }
    ```
    - Restart the NGINX service by running `sudo service nginx restart` 
## Discussion

This configuration serves static files over HTTP on port 80 from the `directory /usr/share/nginx/html/` 


## Possible Static File Serving Problem 
You may experience problem when restarting NGINX service after updating the default configuration file, NGINX by default has a configuration file located at `/etc/nginx/sites-enabled/default`. All you need to do is to remove this fil by running the command ` sudo rm /etc/nginx/sites-enabled/default`



