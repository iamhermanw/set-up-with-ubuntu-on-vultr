# Server set-up with Ubuntu on Vultr VPS

## Generate a SSH key

> [!NOTE]
> Credit to Ben McDonald for his tutorial titled [*Creating ED25519 SSH keys*](https://www.mebmc.uk/posts/creating_ed25519_ssh_keys).

1. Create a new SSH key in the local terminal:

`ssh-keygen -t ed25519 -f $HOME/.ssh/id_ed25519 -C "$(whoami)@$(hostname)-$(date +'%y%m%d')"`

2. Enter a passphrase.

3. View the public key:

`cat ~/.ssh/id_ed25519.pub`

4. Add the public key to the Vultr account.

5. Deploy a new server with Utuntu and select the newly added SSH key in "Server Settings".

## Create a sudo non-root user

> [!NOTE]
> Credit to Jamon Camisso and Anish Singh Walia for their tutorial titled [*Initial Server Setup with Ubuntu*](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu).

6. Log in as the root user:

`ssh root@Server_IP_Address`

7. Enter `yes` and enter the passphrase.

8. Create a new user:

`adduser herman`

9. Enter a password.

10. Grant administrative privileges by adding the new user to the sudo group:

`usermod -aG sudo herman`

## Configure the firewall

11. Examine UFW profiles:

`ufw app list`

12. Allow SSH connections:

`ufw allow OpenSSH`

13. Enable the firewall:

`ufw enable`

14. Check the status:

`ufw status`

## Log in as a sudo non-root user

15. Configure SSH access for the new non-root user:

`rsync --archive --chown=herman:herman ~/.ssh /home/herman`

16. Exit from the root user's terminal and log in as the new user:

`ssh herman@Server_IP_Address`

17. Enter the passphrase.

# Get Let's Encrypt SSL certificates

> [!NOTE]
> Credit to Alex Garnett for his tutorial titled [*How To Use Certbot Standalone Mode to Retrieve Let's Encrypt SSL Certificates on Ubuntu 22.04*](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-ubuntu-22-04).

18. Set DNS pointing to the server.

19. Install snap:

`sudo snap install core; sudo snap refresh core`

20. Install Certbot:

`sudo snap install --classic certbot`

21. Link the `certbot` command from the snap install directory to your path:

`sudo ln -s /snap/bin/certbot /usr/bin/certbot`

22. Open up ports `80` (HTTP) and `443` (HTTPS):

`sudo ufw allow 80`

`sudo ufw allow 443`

23. Get the certificate:

`sudo certbot certonly --standalone -d yourdomain.com`

24. Handle automatic renewals by adding a `renew_hook`:

`sudo nano /etc/letsencrypt/renewal/yourdomain.com.conf`

25. Add a hook on the last line:

```
renew_hook = systemctl reload your_service
```

26. Run a dry run:

`sudo certbot renew --dry-run`
