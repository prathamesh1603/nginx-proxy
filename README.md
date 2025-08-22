Hosting Multiple Applications Inside a VM Using Nginx Reverse Proxy

Introduction:

When hosting multiple applications within a virtual machine (VM), efficiently managing incoming traffic and directing requests to the appropriate application becomes crucial. Nginx, a popular web server and reverse proxy, can help achieve this goal.

In this blog, we will guide you through the process of hosting multiple applications inside a VM using Nginx as a reverse proxy, ensuring seamless routing and optimal resource utilization.

Prerequisites:

Before getting started, ensure that you have the following:

A virtual machine with Nginx installed and running.
Applications running on different ports within the VM.

Step 1: Configure Nginx Reverse Proxy.

To begin, we need to configure Nginx as a reverse proxy to handle incoming requests and forward them to the appropriate application based on the requested URL.

Open the Nginx configuration directory(/etc/nginx/sites-enabled) and run the following command to create new files.

touch app1.example.com
touch app2.example.com

This will create 2 new files where our nginx configuration will be added. Note: You may need to add sudo if you get a permission denied error for the commands

Add the following configuration Blocks inside the app1.example.com by running nano app1.example.com and pasting the following configuration.

server {
    listen 80;

    server_name app1.example.com;

    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

Repeat the process for app2.example.com with the following configuration.

server {
    listen 80;

    server_name app2.example.com;

    location / {
        proxy_pass http://localhost:9000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
Note: Change the server_name and port number as per your application requirement.

In the above example, we configure two server blocks, each with a unique server_name and corresponding proxy_pass directive pointing to the respective application’s URL and port.

Step 2: Test the Nginx Configuration
Before proceeding, ensure that the Nginx configuration is valid. Run the following command:

nginx -t
If the configuration is valid, restart or reload the Nginx service to apply the changes:

sudo service nginx restart
Step 3: Update DNS

To test the implementation, update your DNS records from your DNS providers(ex. GoDaddy, CloudFlare etc.) to add a A record for the sub-domains to map to the server IP address.

For example:

Record Type: A Record
IP Address: Public Ip of the VM
Domain: app1.example.com
ttl: default

Record Type: A Record
IP Address: Public Ip of the VM
Domain: app2.example.com
ttl: default

Step 4: Access Applications Through Nginx Reverse Proxy

Now you can access the applications through the Nginx reverse proxy using the configured URLs:

App1: http://app1.example.com
App2: http://app2.example.com
Ensure that the applications are running on the specified ports within the VM. Check this blog for details.

If you are using a Load Balancer or Firewall solution, you need to forward the request to port 80 with the IP address of the VM.

Summary:

Both applications are available to the public internet over port 80. Internally, using NGINX we are routing the request to the specified port based on the domain names. You can also route based on the URL(path).

Conclusion:

In this blog post, we have learned how to host multiple applications inside a virtual machine using Nginx as a reverse proxy. By configuring Nginx to route incoming requests based on the requested URL, we can efficiently manage multiple applications within a single VM. The Nginx reverse proxy allows for better resource utilization and simplifies access to the hosted applications.

Remember to adapt the configuration to match your specific application URLs and ports. With Nginx as a reverse proxy, you can effectively handle multiple applications, improve scalability, and utilization of your server resources.
