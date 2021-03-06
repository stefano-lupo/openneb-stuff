# OpenNebula New Node Setup stuff
## Setup TCD Proxy
```bash
# Get system to use tcd proxy for HTTP and HTTPS
printf "http_proxy=tsproxy.scss.tcd.ie:8080\nhttps_proxy=tsproxy.scss.tcd.ie:8080" >> /etc/environment

###############################
	Restart Node (just re-ssh in)
###############################

# Double check they work - should end up with index.html, index.html.1
wget http://www.example.com
wget https://www.example.com

# Get OpenBSD version of netcat (has proxy settings options)
apt install netcat-openbsd

# Test ssh into github - note obviously need to set up github ssh key first
# Just follow instructions on github or scp your current key over to the node 
ssh git@github.com -o "ProxyCommand=nc.openbsd -X connect -x $https_proxy %h %p" -i ~/.ssh/<your_github_ssh_key>

# Add github entry in ssh config file so that git always uses it
echo "Host github.com
User git
ProxyCommand nc.openbsd -X connect -x $https_proxy %h %p
IdentityFile ~/.ssh/<your_github_ssh_key>" >> ~/.ssh/config

# Test github again
ssh github.com

```
	
## MongoDB Installation
[Debian Jessie Installation](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/)
```bash
echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

sudo apt-key adv --keyserver-options http-proxy="http://"$http_proxy --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

sudo apt-get update

sudo apt-get install -y mongodb-org

sudo service mongod start
```

