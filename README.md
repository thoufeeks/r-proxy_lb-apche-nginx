This demo covered the implementation of how to setup up loadbalancer using nginx webserver and revers proxy for the backend apache webapp
------------------------------------
Demo Architecture

              Client (Browser)
                     │
             ┌──────▼──────┐
             │   Nginx LB  │  ← VM1 (EC2)
             └────┬───────┘
                  │
      ┌───────────┴────────────┐
      ▼                        ▼
Apache Server 1         Apache Server 2
   (VM2)                     (VM3)

   
-------------------------------------------
   COMMANDS
-------------------------------------------

   # On VM2 and VM3
sudo apt update -y      # For Ubuntu
sudo apt install apache2 -y

# Customize home page for identification
echo "Hello from Apache Server 1" | sudo tee /var/www/html/index.html   # VM2
# On VM3
echo "Hello from Apache Server 2" | sudo tee /var/www/html/index.html   # VM3

sudo systemctl enable apache2
sudo systemctl start apache2
-------------------------------------------

Set Up Nginx on VM1 (Reverse Proxy + Load Balancer)
SSH into VM1, then:

sudo apt update -y
sudo apt install nginx -y
--------------------------------------------------

Edit the default Nginx config or create a new one:

sudo nano /etc/nginx/sites-available/

Under the above dir please create a new file and paste the content of nginx-block file : eg: sudo nano /etc/nginx/sites-available/reverse-lb.conf

Enable the site
(Create a symbolic link to sites-enabled:)

sudo ln -s /etc/nginx/sites-available/reverse-lb.conf /etc/nginx/sites-enabled/

sudo nginx -t
sudo systemctl reload nginx

Optionally: Disable the default site
(To avoid conflicts with the default config:)

sudo rm /etc/nginx/sites-enabled/default
sudo systemctl reload nginx
----------------------------------











