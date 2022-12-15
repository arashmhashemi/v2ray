# v2ray

This project sets up a VPN service on a Linux server.

## Requirements

To create the VPN server, you will need:

- A server with a compatible Linux distribution. Mainly tested on Debian.
- `root` user access on the server

For users wanting to access the created VPN service, they will need either an Android or iPhone device. On Android, they will need to download the v2RayNG application. On iPhone, they will need to download the Fair VPN application.

## Server Installation

SSH into the server, change to the root user (`sudo su`), and run the following commands (you can copy and paste each line into your server SSH session):

```bash
apt update && apt upgrade -y # ensure system is up to date
apt install curl socat -y # install `socat` program
ufw allow http # allow connections over HTTP to the server
ufw allow https # allow connections over HTTPS to the server
ufw allow 1024:65535/tcp # allow connections over TCP, between ports 1024 and 65535, to the server
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh) # install the VPN software on the server
```

After executing the last command, after a few moments you will be prompted with four questions that you must respond to. The VPN software is written in Chinese, but each of the four prompts, in order, translate to the following:

1. 确认是否继续?[y/n]:
   (translates to: Continue?[y/n])
   Type `y` and press enter.
2. 请设置您的账户名:
   (translates to: Username:)
   Enter an administrator username you will use to manage the VPN service settings. Recommendation is to enter `admin` and press enter.
3. 请设置您的账户密码:
   (translates to: Password:)
   Enter the password for the administrator account that you will use to manage the VPN service settings. This can be anything, as long as you can remember it, and is not easily guessable and not too short. If possible, use a mix of numbers, letters and special characters, and ensure it is at least 10 characters long. Type your desired password and press enter.
4. 请设置面板访问端口:
   (translates to: Dashboard Port:)
   Enter `8080` and press enter.

At this point your VPN service should be online! To make sure it is online, enter your server's IP address followed by `:8080` into your browser. For example, if your server IP is `1.2.3.4`, you would enter `1.2.3.4:8080` into your browser.

If the server is set up to this point correctly, visiting the above URL in your browser should present you a login form. Login to the VPN service using the administrator username and password you set in steps 2 and 3 above. Since this service is written and authored for Chinese users, you may find it useful to have your browser translate the UI for your automatically (Google Chrome should detect the language of the site and offer to translate it for you automatically).

The final step in setting up the service is creating one (or more) user accounts that you can share with people to give them access to your VPN service. Your strategy for how many accounts you want to create, and what each account's individual configuration is, is completely up to you. Perhaps you are setting up a VPN service for your family and friends to use; in this situation, you may want to create an individual account for each person. Alternatively, perhaps you are setting up a VPN service for a large number of individuals and whose access credentials may be shared widely with the public; in this situation, you may want to create just a single user account. It is useful to understand that each account you create in the following steps can be controlled separately. You can place limits on each account separately; for example, you can limit how much data can be transferred using the account, or a specific date by which the account expires.

To set up a user account for your VPN service:

1. Click the second-from-the-top menu option on the left side (inbound list).
2. Click the + button.
3. A form will pop up. In the first input field (labeled "remark") you can enter a label for the account. Perhaps it is a name of a family member or friend you are creating this account for - in this case, label the account appropriately. Otherwise, you can leave it blank.
4. Click the "protocol" drop-down menu and select `vless`. Note that once selected, the form will change, but the selection highlighted in the drop-down menu will stay as the previous option (`vmess`). Do not worry. There is a bug in the UI of this software that prevents the drop-down menus from displaying their current selection.
5. (optional) If you want to limit how much data can be used by the account, enter a number other than 0 into the "Total flow (GB)" input field. For example, enter `2` into this field to limit the account's usage to 2 GB. Otherwise, leave it as `0` to allow the account unlimited use.
6. (optional) If you want the account to expire by a certain date, select a date under the "Expire date" calendar input field. Otherwise, leave this field blank to have the account never expire.
7. In the "transmission" drop-down menu, select `ws`. Note, again, that once selected, the form will change, but the selection highlighted in the drop-down menu will stay as the previous option (`tcp`). Do not worry. There is a bug in the UI of this software that prevents the drop-down menus from displaying their current selection.
8. Under "http masquerade", enter `/gql`. This field is a kind of decoy, or trick, that this service uses to trick network filtering systems into thinking it is accessing common public internet services (in this case, `/gql` refers to a common web technology called GraphQL, so we are "masquerading" our VPN service as a GraphQL service instead of a VPN service - hopefully making it harder to filter).
9. Click the "Add to" button to create the user account.
10. A new user account entry should have been added to the page. On the left side of the newly-added account, click the operate link, and then click QR code.
11. Share the QR Code with users, or click the Copy button to copy the configuration link to share with users. Your users will need to input the configuration (most easily done by scanning the QR code from inside either of the Fair VPN or v2RayNG applications on their mobile phones).

Congratulations! You have just created a VPN service with a user account that can access it.

Install BBR
``` bash
wget -N --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && sudo chmod +x bbr.sh &&  sudo bash bbr.sh
```

Create Cert
``` bash
sudo apt-get install certbot

sudo certbot certonly --standalone --preferred-challenges http --agree-tos --email arash@email.com -d arashvpn.com 
```

DNAT Tunneling all ports (replace IRAN_IP and EU_IP)
``` bash
apt-get update -y && apt-get upgrade -y

sysctl net.ipv4.ip_forward=1
iptables -t nat -A PREROUTING -p tcp --dport 22 -j DNAT --to-destination IRAN_IP
iptables -t nat -A PREROUTING -j DNAT --to-destination EU_IP
iptables -t nat -A POSTROUTING -j MASQUERADE
```

DNAT Tunneling specific ports (replace EU_IP and PORT)
```bash
sudo apt update

sudo sysctl net.ipv4.ip_forward=1
sudo iptables -t nat -A PREROUTING -p tcp --dport PORT -j DNAT --to-destination EU_IP:PORT
sudo iptables -t nat -A POSTROUTING -j MASQUERADE
```
