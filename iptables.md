sudo iptables -I INPUT 1 -s 127.0.0.1 -j ACCEPT

sudo iptables -n -L --line-numbers

sudo iptables -I INPUT 4 -p tcp --dport 443 -j ACCEPT

