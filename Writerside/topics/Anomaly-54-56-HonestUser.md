# Anomaly 54-56 - HonestUser

# Anomaly 54
First thing I do to understand a pcap is run the stat of Protocol Hierchy.
![image_49.png](image_49.png)
![image_50.png](image_50.png)
We see a lot of ftp traffic so we can assume that was exploited

# Anomaly 55
Knowing what we know for the previous question we can name that service

# Anomaly 56
This tcp traffic is a mess so i am going to apply a filter to get rid of this junk. I am going to look for tcp packets with data in it. `tcp.len > 0`
Scrolling down we can see some ftp session traffic. Following it reveals some commands and the cat of flag.txt.
![image_51.png](image_51.png)