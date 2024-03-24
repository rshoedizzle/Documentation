# Install
## Raspberry Pi OS (32-bit)
### [Install using the convenience script](https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-convenience-script)

Docker provides a convenience script at [https://get.docker.com/](https://get.docker.com/) to install Docker into development environments non-interactively. The convenience script isn't recommended for production environments, but it's useful for creating a provisioning script tailored to your needs. Also refer to the [install using the repository](https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-repository) steps to learn about installation steps to install using the package repository. The source code for the script is open source, and you can find it in the [`docker-install` repository on GitHub](https://github.com/docker/docker-install).

Always examine scripts downloaded from the internet before running them locally. Before installing, make yourself familiar with potential risks and limitations of the convenience script:

- The script requires `root` or `sudo` privileges to run.
- The script attempts to detect your Linux distribution and version and configure your package management system for you.
- The script doesn't allow you to customize most installation parameters.
- The script installs dependencies and recommendations without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
- By default, the script installs the latest stable release of Docker, containerd, and runc. When using this script to provision a machine, this may result in unexpected major version upgrades of Docker. Always test upgrades in a test environment before deploying to your production systems.
- The script isn't designed to upgrade an existing Docker installation. When using the script to update an existing installation, dependencies may not be updated to the expected version, resulting in outdated versions.

> **Tip: preview script steps before running**
> 
> You can run the script with the `--dry-run` option to learn what steps the script will run when invoked:
> 
> ```console
> curl -fsSL https://get.docker.com -o get-docker.sh
> sudo sh ./get-docker.sh --dry-run
> ```

This example downloads the script from [https://get.docker.com/](https://get.docker.com/) and runs it to install the latest stable release of Docker on Linux:

```console
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
```console
Executing docker install script, commit: 7cae5f8b0decc17d6571f9f52eb840fbc13b2737
<...>
```

You have now successfully installed and started Docker Engine. The `docker` service starts automatically on Debian based distributions. On `RPM` based distributions, such as CentOS, Fedora, RHEL or SLES, you need to start it manually using the appropriate `systemctl` or `service` command. As the message indicates, non-root users can't run Docker commands by default.

> **Use Docker as a non-privileged user, or install in rootless mode?**
> 
> The installation script requires `root` or `sudo` privileges to install and use Docker. If you want to grant non-root users access to Docker, refer to the [post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user). You can also install Docker without `root` privileges, or configured to run in rootless mode. For instructions on running Docker in rootless mode, refer to [run the Docker daemon as a non-root user (rootless mode)](https://docs.docker.com/engine/security/rootless/).

---
### [Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

The Docker daemon binds to a Unix socket, not a TCP port. By default it's the `root` user that owns the Unix socket, and other users can only access it using `sudo`. The Docker daemon always runs as the `root` user.

If you don't want to preface the `docker` command with `sudo`, create a Unix group called `docker` and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the `docker` group. On some Linux distributions, the system automatically creates this group when installing Docker Engine using a package manager. In that case, there is no need for you to manually create the group.

> **Warning**
> 
> The `docker` group grants root-level privileges to the user. For details on how this impacts security in your system, see [Docker Daemon Attack Surface](https://docs.docker.com/engine/security/#docker-daemon-attack-surface).

> **Note**
> 
> To run Docker without root privileges, see [Run the Docker daemon as a non-root user (Rootless mode)](https://docs.docker.com/engine/security/rootless/).

To create the `docker` group and add your user:

1. Create the `docker` group.
    
    ```console
    sudo groupadd docker
    ```
    
2. Add your user to the `docker` group.
    
    ```console
    sudo usermod -aG docker $USER
    ```
    
3. Log out and log back in so that your group membership is re-evaluated.
    
    > If you're running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
    
    You can also run the following command to activate the changes to groups:
    
    ```console
    newgrp docker
    ```
    
4. Verify that you can run `docker` commands without `sudo`.
    
    ```console
    docker run hello-world
    ```
    
    This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.
    
    If you initially ran Docker CLI commands using `sudo` before adding your user to the `docker` group, you may see the following error:
    
    ```none
    WARNING: Error loading config file: /home/user/.docker/config.json -
    stat /home/user/.docker/config.json: permission denied
    ```
    
    This error indicates that the permission settings for the `~/.docker/` directory are incorrect, due to having used the `sudo` command earlier.
    
    To fix this problem, either remove the `~/.docker/` directory (it's recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:
    
    ```console
    sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    sudo chmod g+rwx "$HOME/.docker" -R
    ```
---
### [Upgrade Docker after using the convenience script](https://docs.docker.com/engine/install/raspberry-pi-os/#upgrade-docker-after-using-the-convenience-script)

If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There's no advantage to re-running the convenience script. Re-running it can cause issues if it attempts to re-install repositories which already exist on the host machine.

## [Uninstall Docker Engine](https://docs.docker.com/engine/install/raspberry-pi-os/#uninstall-docker-engine)

1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:
    
    ```console
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
    ```
    
2. Images, containers, volumes, or custom configuration files on your host aren't automatically removed. To delete all images, containers, and volumes:
    
    ```console
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
    ```
    

You have to delete any edited configuration files manually.