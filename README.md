# Private registry setup

![DPR Logo](/images/docker-registry.png)

A private Docker registry allows you to share your custom base images within your organization, keeping a consistent, private, and centralized source of truth for the building blocks of your architecture.

## Requirements:

* Before you can deploy a registry, you need to install Docker on the host. A registry is an instance of the registry image, and runs within Docker.
* You need to install docker-compose on your machine as well.

## Create the essentials:

Copy the yaml file below on your machine and execute it using:
'''
docker-compose docker-compose.yml up
'''
## Authentification:

Create an authentification file using a **httpasswd** and the commands:
'''
mkdir auth
'''
And:
'''
htpasswd -Bc registry.password <usr>
'''
Now, try login in using:
'''
docker login <your-host>:5000
'''

## SSL certificate:

![DPR Logo](/images/ssl.png)

Create a self-signed SSL certificate using commands:
'''
mkdir -p /certificates
cd certificates
openssl req -newkey rsa:4096 -nodes -sha256 -keyout domain.key -x509 -days 365 -out domain.crt
'''

In the clients machine copy the certificate by first creating a folder:
'''
sudo mkdir -p /etc/docker/certs.d/IP_ADDRESS_OF_YOUR_VM:5000
'''

Then:
'''
sudo cp </certificates/domain.crt> /etc/docker/certs.d/IP_ADDRESS_OF_YOUR_VM:5000/ca.crt
'''

And reload docker:
'''
sudo service docker reload
'''

:tada: Now you're all set!
