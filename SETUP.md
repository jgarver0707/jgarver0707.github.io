
Here's how I set up my server...

I was fortunate enough to have an old gaming laptop laying around at home, and decided to install Arch Linux on it. Having this run 24/7 at home would allow me to host various services on it, which I have used for both personal uses (file sharing and video games), as well as academic reasons (this assignment).

I've been using Arch Linux for about five months, and I'm really enjoying it. When I learned about this assignment, and the expectation to deal with Oracle Cloud's clunky interface and less-than reputable business practices, I realized that I had all the means to host my own website, at home, without having to pay anyone or use any middlemen. 

I don't think I'd forgive myself if I didn't at least try to pull it off. Luckily, everything worked out well!

To host a web server from home and access it remotely, you'll need to accomplish the following:
- Hardware at home that is running Linux (ideally), and always on. 
	- In my case, an MSI Katana GF66 laptop. It's using the Arch Linux operating system with the KDE Plasma desktop environment- though the DE will not be used to access it remotely.
- Configuring firewall and port forwarding your server.
	- To open your server to the internet, you need to configure your server's firewall and port forward it. The firewall I use on the Katana is ufw (Uncomplicated FireWall), which integrates nicely with KDE settings. I've tried other firewalls, like firewalld and iptables, but I've found those to be less intuitive. You'll want to pick the ports your services are going to use.
	- The three main services here are SSH, HTTP, and HTTPS. You can use the default ports (22, 80, 443 respectively), or you can change them to custom ones if you feel so inclined. I had concerns about campus IT administrators blocking certain ports for security reasons, and decided to change all of mine to custom ones.
	- To actually connect those ports to the internet, you'll want to port forward. This process is different for every router, and it makes setting up a web server hosted from campus impossible. You basically need to configure your router to accept certain ports from certain devices, and the instructions can sometimes be found on the side of your router. It's possible in home networks where you have access to the router, but not on campus or office buildings.
- Setting up the web server.
	- Initially I used nginx to host my website, but I was asked to use NodeJS to keep my work on par with the rest of the class. NodeJS also allows us to do the echo service, where nginx doesn't.

At this point I was already back on campus, and I got my SSH working so that I could work on the server remotely.

- Remotely accessing the server.
	- Default SSH allows you to connect to a device using a password login method. I didn't think this was very secure, so I disabled password auth and established a password-protected pgp key setup between my laptop and my server. Only this laptop is able to connect to my server remotely, and even then, it's still password locked.
- Monitoring the server.
	- I installed btop to monitor my system resources. It's really pretty and not at all necessary for the assignment, but it makes you look like you know what you're doing. It was either that or cmatrix.
- Running the web service in the background
	- By this point ssh should be a startup service. If your server restarts for any reason (power outage, etc), you'll want it to reboot properly and keep running the sshd service once it can. Otherwise, you'll lose remote access. The same is true of the nodejs web service.
	- In arch, you can use sudo systemctl enable (service) to let systemctl run these on boot. You can also use sudo systemctl start (service) to start them if they are off.

Generative AI helped with relevant configuration files for nodejs. Once you've got those down, you should have a server up.

To log in, I used an SSH alias (stored in .ssh/config) so I could just type "ssh katana" instead of the entire command.

To upgrade my system, I just need to run sudo pacman -Syu, as pacman is the main package manager for Arch linux.
