# Server set-up with Ubuntu on Vultr VPS

## Generate a SSH key

> [!NOTE]
> Credit to Ben McDonald for his tutorial titled Creating ED25519 SSH keys.

1. Create a new SSH key in the local terminal:

`ssh-keygen -t ed25519 -f $HOME/.ssh/id_ed25519 -C "$(whoami)@$(hostname)-$(date +'%y%m%d')"`

2. Enter a passphrase.

3. View the public key:

`cat ~/.ssh/id_ed25519.pub`

4. Add the public key to the Vultr account.

5. Deploy a new server with Utuntu and select the newly added SSH key in "Server Settings".

## Create a sudo non-root user

> [!NOTE]
> Credit to Jamon Camisso and Anish Singh Walia for their tutorial titled Initial Server Setup with Ubuntu.

6. Log in as the root user:

`ssh root@Server_IP_Address`

7. Enter `yes` and enter the passphrase.

8. Create a new user:

`adduser herman`

9. Enter a password.

10. Grant administrative privileges by adding the new user to the sudo group:

`usermod -aG sudo herman`

## Configure the Firewall

11. Examine UFW profiles:

`ufw app list`

12. Allow SSH connections:

`ufw allow OpenSSH`

13. Enable the firewall:

`ufw enable`

14. Check the status:

`ufw status`

## Log in as the sudo non-root user

15. Configure SSH access for the new non-root user:

`rsync --archive --chown=herman:herman ~/.ssh /home/herman`

16. Log in:

`ssh herman@Server_IP_Address`

17. Rnter the passphrase.

# Install Certbot

> [!NOTE]
> Credit to Alex Garnett for his tutorial titled How To Use Certbot Standalone Mode to Retrieve Let's Encrypt SSL Certificates on Ubuntu 22.04.

19. Set DNS pointing to the server.


