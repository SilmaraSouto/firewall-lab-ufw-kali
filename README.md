My First Cybersecurity Lab: Ubuntu Firewall Tested with Kali

This is my first practical cybersecurity project.
The idea was simple but powerful: configure a firewall on an Ubuntu server using UFW and test the results from a Kali Linux machine with Nmap.
By doing this, I learned how firewalls control traffic, how to expose only what’s needed, and how to validate the rules from the perspective of an attacker.

What I Built
•	Two virtual machines on VirtualBox:
o	Ubuntu (192.168.100.10) → the target, where I configured the firewall.
o	Kali (192.168.100.20) → the attacker, where I scanned and verified the rules.
•	Set static IPv4 addresses on both so they could communicate on an Internal Network.

--------------------------------------------------

•	Configured UFW on Ubuntu with basic rules:

o	Allow 22/tcp (SSH)
o	Allow 80/tcp (HTTP)
o	Deny 23/tcp (Telnet) (intentionally insecure service)
•	Verified everything using Nmap from the Kali VM.

![UFW Enabled](1 images ufw-enable.png)

--------------------------------------------

Setting up Ubuntu (Defender)
Activate UFW and add rules:

````bash
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw deny 23/tcp

````bash
Check status:sudo ufw status

![UFW Rules](2 images ufw-enable.png)

---------------------------------------------

Testing from Kali (Attacker)
From Kali, I ran:

`````bash
nmap -p 22,23,80 192.168.100.10

Expected result:
•	22/tcp → open (SSH available)
•	80/tcp → open (HTTP available)
•	23/tcp → filtered/closed (blocked by firewall)

![Nmap Open](3 images ufw-enable.png)

------------------------------------------------

Closing the Gates Again
Then I removed the rules and locked everything down:

`````bash
sudo ufw deny 22/tcp
sudo ufw deny 80/tcp

![UFW Deny](4 images ufw-enable.png)

Another Nmap scan showed:

````bash
22/tcp filtered ssh
23/tcp filtered telnet
80/tcp filtered http

![Nmap Filtered](5 images ufw-enable.png)

--------------------------------------------------

Key Takeaways
•	Learned the difference between open, closed, and filtered ports in Nmap.
•	Understood the principle of least privilege in firewall rules.
•	Saw how an attacker’s perspective (Kali + Nmap) confirms defensive configurations.
•	Practiced static IP configuration with Netplan and manual IP assignment in Kali.

---------------------------------------------------

Next Steps
•	Add HTTPS (443/tcp) with a self-signed cert.
•	Configure fail2ban to protect SSH.
•	Use Wireshark on Kali to watch packets being blocked vs. allowed.

----------------------------------------------------

Conclusion
This lab might be small, but it’s meaningful: I built a real defensive setup, tested it from an attacker’s perspective, and documented everything.
It’s a foundation I can build on, and a good first step in my journey into practical cybersecurity.
