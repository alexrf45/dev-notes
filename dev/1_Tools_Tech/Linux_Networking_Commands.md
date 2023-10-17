
#network #linux #sysadmin 

iptables:

#iptables #network 

```bash

#list all rules
sudo iptables -L

#list rules by line number
sudo iptables -L --line-numbers

#delete rule by line number
sudo iptables -D INPUT 3 (for rule 3)

#flush iptable rules
 sudo iptables -F

#allow all concurrent connections
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

#allow any host to access ssh
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT

#allow specific access to port 80
sudo iptables -A INPUT -p tcp -s F.Q.D.N --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT

#allow specific access to port 22
sudo iptables -A INPUT -p tcp -s F.Q.D.N --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT

#block all other traffic
sudo iptables -P INPUT DROP

#persistant Iptables reconfig
 sudo apt-get install iptables-persistent

#save a copy of the iptable rules
sudo iptables-save > name-of-file.txt
```

tcpdump: 

#tcpdump #network #packetcapture 

```bash
#list interfaces for listening

sudo tcpdump -D

#capture traffic to a capture file
sudo tcpdump -v -w capture.cap

#capture traffic from a certain dest IP
tcpdump -n dst your.target.ip.address -v

#capture specifc port traffic

sudo tcpdump -i eth0 'port 123' -vv

#capture traffic between one host and entire network

sudo tcpdump src host 192.168.0.5 and dst net 192.168.0.0/24

#capture traffic from one host and save it to a capture file
sudo tcpdump -n dst the.destination.ip.address -v -w somefile.cap

#capture specific port traffic from a host
tcpdump src 172.16.46.25 and port not 22 and port not 8834
```

tshark:

#tshark #packetcapture #blueteam 

```bash
for stream in `tshark -r capture.cap -T fields -e tcp.stream | sort -n | uniq`
	do
	    echo $stream
	    tshark -r capture.cap -w stream-$stream.cap -Y "tcp.stream==$stream"
	done

```