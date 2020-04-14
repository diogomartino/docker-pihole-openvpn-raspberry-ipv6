
## Clone this repository
    git clone https://github.com/bruxo00/docker-pihole-openvpn-raspberry-ipv6.git

## How to setup IPv6 on docker
Get your host IPv6 by running `ifconfig`. Locate your IPv6:

    inet6 fe80::ba27:ebff:fee0:df97  prefixlen 64
Next, open `sudo nano /etc/docker/daemon.json` and type:

    {
          "ipv6": true,
          "fixed-cidr-v6": "fe80::ba27:ebff:fee0:df97/64"
    }
Enter your IPv6 and your prefix in front of it. Now you need to restart docker by doing `sudo service docker restart`. Make sure it restarted successfully by doing `service docker status`.
Also, make sure you have your raspberry/device set to a static IP in your router.

## Setup PiHole
Configure your .env file

    sudo nano .env

It's explained in the file what each option does. Now open the docker compose file:

    sudo nano docker-compose.yml
Here you can change all the ports as you wish. In my case, I don't want pihole to run lighttpd on port 80, so I changed it to 8080.

**If** PiHole somehow has trouble setting up your IP's on the first start, go to the root folder that you chose in the .env file and open the file. If you didn't change anything, it will be on:

    sudo nano ./data/pihole/etc-pihole/setupVars.conf
And add/edit the following:

    DHCP_IPv6=true
    IPV4_ADDRESS=192.168.1.64 # Host IPv4
    IPV6_ADDRESS=fe80::ba27:ebff:fee0:df97 # Host IPv6
And then restart the container.

## Create the IPV6 network

    docker network create --ipv6 --driver bridge --subnet "fd01::/64" ipv6
    
## Setup OpenVPN
Open the  docker-compose file:

    sudo nano docker-compose.yml
Change the port if you want. In my case, I use 443 via UDP to avoid my university blocks on the most common ports.
**To generate certificates, follow the** [official documentation](https://github.com/kylemanna/docker-openvpn/blob/master/docs/docker-compose.md).

## Start the containers

    sudo docker-compose up -d




###### Based on [this tutorial](https://gist.github.com/zottelbeyer/c47b1a48b9c5c69796a712466e7fb71f).
