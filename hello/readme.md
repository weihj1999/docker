
Container advantages:
1. Same environment
2. Sandbox Project
3. It just works, and everywhere

Image 
--OS
--Software
--Code

Image -- Dockefile --> build Image

Image --> Run --> Container

Build the image

$docker build -t hello-world .

Run the example

$ docker run -p 80:80 hello-world
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
[Thu May 09 22:08:47.995770 2019] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.25 (Debian) PHP/7.0.33 configured -- resuming normal operations
[Thu May 09 22:08:47.995873 2019] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'

OR
$docker run -p 80:80 -v /home/linux/dockertst/src:/var/www/html/ hello-world
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
[Thu May 09 22:13:33.984654 2019] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.25 (Debian) PHP/7.0.33 configured -- resuming normal operations
[Thu May 09 22:13:33.984752 2019] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'

Note:
This specify local directory as volume for the container


Troubleshooting
1. 
$sudo usermod -aG docker $USER
re-login
$docker image ls
It will work.

2. kill all back container once Ctrl + C used
$docker system prune

