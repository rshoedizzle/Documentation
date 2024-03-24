# Pi-hole
The Pi-hole is a DNS sinkhole that protects your devices from unwanted content, without installing any client-side software. It resolves IP addresses and domain names (DNS lookup) and checks the address against a block list or not. If the address is on the block list, the request will be blocked.

# Docker Container Installation
## System Preparation

>**Note**
>
>*Ensure that your machine has a static IP address configured as this system is to be used as a constant 24/7 server and that IP needs to remain consistent.*

1. If you are running a Debian-based operating system such as Ubuntu, you will need to update the package list cache before we can install the software needed to run Pi-Hole.

You can update the package list by using the following command.

```console
sudo apt update
```

2. The only package we need to get Pi-Hole running with Docker is the Docker runtime.

Thanks to a convenience script the Docker team provides, installing this runtime is straightforward. While you could install Docker from your package repository, it can be significantly out-of-date.

To install Docker on any supported operating system, run the command below.

```console
curl -sSL https://get.docker.com | sh
```

3. After installing Docker, it is helpful to add your current user to the “`docker`” group it created. Adding your user to the group is as easy as using the usermod command.

We want to add your user to this group because it makes controlling the Pi-Hole Docker container easier. We don’t have to worry about using “`sudo`” to interact with the containers.

```console
sudo usermod -aG docker $USER
```

4. Because we made changes to our user’s groups you will need to log out.

You can easily log out from the terminal by running the following command.

```console
logout
```

Alternatively, you can just restart your machine by using the reboot command.

```console
sudo reboot
```

5. That is all the software you need on your Linux system to run Pi-Hole within a Docker container.

## Installing Pi-Hole Docker Container

This section will show you the process of installing Pi-Hole as a Docker container on your Linux-based system. All we need to do within this section is to write a “`docker-compose`” configuration file.

This file tells Docker what containers it needs to download and what ports it needs to open.

### Creating a Directory for Pi-Hole

1. Start by creating a directory where you will store the configuration file for the Pi-Hole docker container.

```console
sudo mkdir -p /opt/stacks/pihole
```

*Note: mkdir -p flag stands for "parent" which allows for the creation of a directory hierarchy in one step*

2. Move into our newly created directory.

```console
cd /opt/stacks/pihole
```

### Writing the Docker-Compose Configuration File

3. Our next step is writing the “`compose.yaml`” file. This file is where we will define the Pi-Hole docker container and the options we want passed to the container.

```console
nano compose.yaml
```

4. Within this file, you will want to enter the following lines. We will explain the pieces you may want to modify shortly.

```yaml
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
      # WEBPASSWORD: 'set a secure password here or it will be random'
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

### Configuring the Pi-Hole Configuration File

5. Before you save this file, there are three Docker options that you will want to reconfigure for Pi-Hole to suit your setup better.
#### Setting the Password for the Pi-Hole Web Interface

Out of all the things to configure, you will want to set a secure password before running the Pi-Hole container. Pi-Hole will randomly generate the password if you don’t set a value.

Begin by looking for the following line within the configuration file.

```yaml
      # WEBPASSWORD: 'set a secure password here or it will be random'
```

Replace with the following, switching out “`SECUREPASSWORD`” with a secure password of your own. 

```yaml
      WEBPASSWORD: 'SECUREPASSWORD'
```

#### **Configuring the Web Interface Port of Pi-Hole**

By default, we will set up the Docker container so Pi-Hole will be accessible through port `80` on your system. This could be problematic if you already have something operating on port `80`.

To change this, you will want to find the following line and change the number on the left side of the colon (`:`).

```yaml
      - "80:80/tcp"
```

For example, to change the port to “`8080`“, you would replace that line with the following.

```yaml
      - "8080:80/tcp"
```

#### **Setting the Time Zone for the Pi-Hole Docker Container**

By default, the Pi-Hole docker container has been configured to use the “`Chicago`” time zone. It is possible, however, to adjust this to your local time zone.

You can find a list of valid [time zone values on Wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). The value you want to use is in the “TZ database name” column.

To adjust the time zone, find the following line within the file.

```yaml
      TZ: 'America/Chicago'
```

Adjust this value to match your time zone. For example, for Los Angeles, America, we would utilize the line below.

```yaml
      TZ: 'America/Los_Angeles'
```

### Saving the Docker-Compose File

6. Once you have made the above changes to the file, save and quit by pressing CTRL + X, followed by Y, then the ENTER key.

### Disabling the Systemd-Resolve Service (Ubuntu Only)

7. If you are using Ubuntu to run the Pi-Hole Docker container, you may need to disable the Systemd-resolve service.

The operating system uses this service to provide network name resolution. As Pi-Hole will want to operate on the same part the resolve service does, we need to disable it.

Start by stopping the systemd-resolve service by using the following command.

```console
sudo systemctl stop systemd-resolve
```

8. With the service stopped, you will also want to disable it by using the command below.

Disabling the service will stop Ubuntu from starting it back up the next time you restart your device.

```console
sudo systemctl disable systemd-resolve
```

9. With the “`systemd-resolve`” service now disabled, our next step is to modify the “`/etc/resolv.conf`” file to point to a different nameserver. By default, the nameserver will be configured to the systemd service.

Use the command below to begin modifying the configuration file.

```console
sudo nano /etc/resolv.conf
```

10. You will want to find and replace the following line within this file.

```
nameserver 127.0.0.53
```

Replace it with the following. This changes the nameserver to Cloudflare’s 1.1.1.1 service.

```
nameserver 1.1.1.1
```

11. Once you have made changes to this file, save and quit by pressing CTRL + X, followed by Y, then the ENTER key.

### Starting the Pi-Hole Docker Container

12. We can finally start up Pi-Hole’s Docker container on our Linux system.

All you need to do now is run the following command within the terminal.

```console
sudo docker compose up -d
```

## Accessing the Pi-Hole Web Interface

Now that we have the Pi-Hole docker container up and running on your system, we can proceed to use its web interface.

This web interface allows you to control all aspects of Pi-Hole on your system, so you won’t have to mess around with configuration files.

1. Before we begin, you will need to know the IP address of your device so that you can access the web interface.

The easiest way to get the local IP address is to use the hostname command.

```console
hostname -I
```

2. With your local IP address, you will want to go to the following within your web browser.

Ensure you replace “`IPADDRESS`” with the IP you got in the previous step.

```
http://IPADDRESS/admin
```

If you changed the port away from “`80`“, you need to insert the port like shown below.

```
http://IPADDRESS:PORT/admin
```

3. You should now be greeted with the login page for Pi-Hole.

To log in, you must type in the password (**1.**) you set when writing the Docker configuration file earlier.

With your password typed in, click the “`Log In`” button (**2.**)

![Pi-Hole Docker Login Screen](https://pimylifeup.com/wp-content/uploads/2022/10/Pi-hole-docker-container-01-Login-Screen.jpg)

4. You now have access to the Pi-Hole dashboard running from within the Docker container.

![Pi-Hole Dashboard](https://pimylifeup.com/wp-content/uploads/2022/10/Pi-hole-docker-container-02-Pi-Hole-Dashboard.jpg)

5. With access to the dashboard, now is a good time to start changing your device’s DNS to use Pi-Hole.
	
When setting the DNS servers, you must use the IP belonging to the device you are running Pi-Hole on.

## Updating the Pi-Hole Docker Container

1. Change to the directory where we wrote the Compose file earlier.

```console
cd /opt/stacks/pihole
```

2. Tell Docker to download the latest version of the Pi-Hole image.

```console
docker compose pull
```

3. The previous command pulls the latest image, but won’t update your already running container. 

Use the following command within the terminal to tell Docker to start up our Compose file. Docker will detect that Pi-Hole is already running and see that a new image is available. Upon detecting a new image, it will stop the current container and start it back up using the new one.

```console
docker compose up -d
```

---

## Changing Device DNS to Pi-Hole

### Client-Specific Configuration
#### Windows

DNS settings are specified in the **TCP/IP Properties** window for the selected network connection.

1. Go to the **Control Panel**.
2. Click **Network and Internet** > **Network and Sharing Center** > **Change adapter settings**.
3. Select the connection for which you want to configure Google Public DNS. For example:
    
    - To change the settings for an Ethernet connection, right-click the Ethernet interface and select **Properties**.
    - To change the settings for a wireless connection, right-click the Wi-Fi interface and select **Properties**.
    
    If you are prompted for an administrator password or confirmation, type the password or provide confirmation.
    
4. Select the **Networking** tab. Under **This connection uses the following items**, select **Internet Protocol Version 4 (TCP/IPv4)** or **Internet Protocol Version 6 (TCP/IPv6)** and then click **Properties**.
5. Click **Advanced** and select the **DNS** tab. If there are any DNS server IP addresses listed there, write them down for future reference, and remove them from this window.
6. Click **OK**.
7. Select **Use the following DNS server addresses**. If there are any IP addresses listed in the **Preferred DNS server** or **Alternate DNS server**, write them down for future reference.
8. Replace those addresses with the IP addresses of your Pi-Hole server:
    
#### Mac
1. Go to **System Preferences**.
2. Click on **Network**
3. Click on network connection you are connected to and click **Advanced**.
4. Click on the DNS tab.
5. If there are any DNS server IP addresses listed there, write them down for future reference, and remove them from this window.
6. Click the `+` button at the bottom and insert IP address of Pi-Hole server.
7. Click **OK**

### Network-Wide Configuration
This will vary depending on your individual router.

#### General
1. Navigate to DHCP Configuration
2. Change DHCP DNS Server to Pi-Hole server IP address.
3. Save changes.

#### Google Nest Router
1. Open Google Home App
2. Select Wi-Fi
3. Tap on **Network Settings**
4. Tap on **Advanced Networking**
5. Select **DNS Settings**
6. Select **Custom**
7. Enter in IP Address of Pi-Hole as primary DNS server.
8. Select **Save**

## Pi-hole Admin Navigation
### Adding More Block Lists
1. Select Adlists
2. Paste URL block list into Address field
	1. Big Blocklist Collection: https://firebog.net/
3. Click Save
4. Run the command `pihole -g` on your raspberry pi to update current block list
	1. If this command is not found (installation was via Docker) then run\
	   `docker exec -it [name of container] pihole -g`
5. On the admin panel, navigate to **Settings**
6. Click **Restart DNS Resolver**

### Disabling Blocking for Clients
1. In the admin panel navigate to **Groups**
2. Create a new group called "NoBlocking"
3. Navigate to **Clients**
4. Look for client machine to disable blocking for.
5. Select client and select **Add**
6. Change Group Assignment to be the "NoBlocking" group and *not* the "default" group.

## Using Unbound for Secure DNS

If multiple DNS servers are configured, Chrome can likely bypass Pi-Hole DNS servers and use a fallback or alternative DNS server due to the preference of Chrome wanting to use "Secure DNS"

The solution to this is to disable secure DNS in Chrome, but then re-enable it network-wide via Unbound

### Disabling Secure DNS in Chrome
1. In Google Chrome, navigate to the 3 dots in the top right and select Settings
2. Select Privacy and Security
3. Select Security
4. Untick the option for "Use secure DNS" (Encrypt the names of sites you visit)

### Installing Unbound for Network-Wide Secure DNS
1. Install required packages
	  ```console
   sudo apt install -y unbound dnsutils
	```
2. Grab the configuration file
   ```console
   sudo wget https://bartonbytes.com/pihole.txt -O /etc/unbound/unbound.conf.d/pihole.conf
	```

	*Note: This configuration opens port 5533*

3. Make sure that Unbound is running
   ```console
   sudo systemctl restart unbound && sudo systemctl enable unbound
   ```

4. Check that Unbound can resolve DNS names
   ```console
   dig @127.0.0.1 example.com -p 5533
	```
5. Tell Pi-Hole to use Unbound server for all outbound DNS
	1. Navigate to Pi-hole Admin Console
	2. Click **Settings**
	3. Click the **DNS** tab
	4. Unselect current Upstream DNS Servers
	5. Select Custom Upstream DNS Server and enter the localhost Unbound address and port
	   ```console
	   127.0.0.1#5533
		```
	6. Click **Save** 
	7. Can verify Unbound is being used as secure DNS
	   ```console
	   dig @127.0.0.1 example.com
		```

---
## Resources:
**Docker installation:**
{% embed url="https://github.com/pi-hole/docker-pi-hole" %}

**Bare metal basic installation:**
{% embed url="https://docs.pi-hole.net/main/basic-install/" %}

**Installing Unbound:**
{% embed url="https://bartonbytes.com/posts/configure-pi-hole-for-dns-over-tls/" %}