# Intermediate Nmap #

- I first started out with a couple of different nmap scans, all we really need to do here is run a TCP scan with “nmap -sN -v [target ip]".

- The only open ports appear to be 22, 2222, and 31337. The services listed for ports 22 and 2222 are ssh as would be expected, however port 31337 lists the service as “Elite?”

- A quick google search of port 31337 shows that port 31337 (meaning “Elite” in “hacker slang”) is typically used to establish trojans and backdoors (https://www.speedguide.net/port.php?port=31337). Given the listed service, it appears that port 31337 is some sort of backdoor or service that will allow us to get into the server.

- Use the command “netcat [target ip] 31337” to connect to the port. We can find a message providing a username and password on this port.
  - *ubuntu:Dafdas!!/strong*

- Now all we need to do is ssh into the server using this username and password (“ssh ubuntu@[target ip]”). After some looking around using ‘ls -a’ and ‘pwd’ I navigated to the /home directory where I found another user listed as ‘user’. If we go into the user’s directory, we can find a file called flag.txt, read this file to get the flag.
