- Begin by running an nmap scan to determine the services that are currently running. I used “nmap -sV -vv <IP address>”.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/753a7bb4-b172-41da-b6c3-bd8220a9579a)

- The webserver appears to be running on port 80 while we can see that port 3389 is running RDP.
- Now that we know there is a webserver running, let’s navigate to the website by searching the IP address in our web browser.
- If we snoop around, we can find some more information on the webserver. The robots.txt page is used to designate which pages of the website web crawlers should not use. Web crawlers essentially “crawl” across the internet and chart web pages to bring up in internet searches. The robots.txt page can prevent web crawlers from going through admin pages or configuration pages, however if someone leaves the robots.txt page accessible to us, we can use it to find out what the backend pages of the website are and check to see if anyone left any sensitive information on the page that shouldn’t be there. We can get to robots.txt by searching “<IP address>/robots.txt”.
- It looks like someone left the password out for us on the robots.txt page, as well as a list of some other pages that might come in handy later.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/d22cb206-9021-43f5-bd74-eee82ed1f46e)

- According to robots.txt, we can also see that a service called Umbraco is being used. If you are not familiar with Umbraco, you can look it up, but essentially, it is an open-source CMS tool, which answers our next question.
- The website domain is pretty easy to find as the website home page lists it for us. It’s just “anthem.com”.
- If we read the “A cheers to our IT department” article we can find a poem that was written for the site administrator. If you are unfamiliar with the nursery rhyme or the Batman villain, it is referencing Solomon Grundy, which is presumably the name of the administrator.

![4d9719cbf81140e666830b87f66b62bd91532e7br1-448-251_hq-238039800-1](https://github.com/calicojack1720/WriteUps/assets/93363006/4833fdc7-f473-471a-9b9d-f434f3539570)


![image](https://github.com/calicojack1720/WriteUps/assets/93363006/d0929d59-ed62-4184-bab1-2826469c3fe3)

- While Solomon Grundy’s email is not explicitly listed on the website, if we check the other article on the site we can find another employee email for Jane Doe which is JD@anthem.com. Assuming that everyone has the same email format, the administrator’s would be SG@anthem.com.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/697cd600-e573-4184-979d-f7772c417d1d)

- Now it’s time for us to spot the flags!
   - Flag 1
      - Found in the metadata of the “We are hiring article”. You can view this by inspecting the page and then looking under the “head” section of the html code.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/cecd3c06-d357-40a0-9252-26f7953270b5)

   - Flag 2
      - Can be found in the html code for the search bar. You can find this by viewing the page’s source (left click > View Page Source).

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/cbc7f9d4-29d0-4085-8955-c9ae62848b69)

   - Flag 3
      - Found on the Author page for Jane Doe. You can get to this by going to the “We are hiring” article and then scrolling to the bottom and clicking on the hyperlink on Jane Doe.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/bb76bd60-c419-496f-ae57-0306dd4cea4e)

   - Flag 4
      - Can be found by inspecting the “A cheers to our IT department” page. The flag is listed under the head for the page with the metadata content.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/40fbeec0-adc0-464b-b038-891b597d6300)

- Now for the final stage. We can use the information we have gathered so far to get into the target machine.
- We know that the machine is running an open RDP port, so we can try to use Remote Desktop to gain access to the machine. If you are using Windows, you could use the Remote Desktop application to do this. I am on Kali Linux, so I’ll be using remmina to remote into the machine. We know the admin email and password from previous tasks, so if we use remmina or similar to get a connection, we can use “SG” as the username and “UmbracoIsTheBest!” as the password.
- The user.txt file is shown on the desktop after logging in. Open it to find the flag.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/2adf0307-29a0-4254-bbfd-2b8903dd353d)

- Next, we need to find the admin password, which has been hidden somewhere on the machine. The hint mentions that is “hidden”, so let’s start by opening up file explorer and selecting View and then checking the box next to “show hidden files”. After doing this, we can find a hidden backup folder under C:\.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/a76e5e96-e0a9-46aa-9a26-f730c2f6432d)

- Unfortunately, we do not have permission to open the restore.txt file inside of it, but we can work around this by changing the permissions of the file. Simply left click on the restore.txt file and select Properties. Than go to the Security tab, hit Edit, and select Add. We can then add the user SG to the file permissions by typing in SG in the box and then selecting “Check Names”. Click OK and then apply your changes. You should now be able to open up the txt file.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/ff62e895-6757-42c0-a0f4-8d367d006f53)

- Now we can use the admin password to get into the Administrator folder under Users. The root.txt file can be found on the Administrator’s Desktop and contains the final flag.

![image](https://github.com/calicojack1720/WriteUps/assets/93363006/23f4c7f9-1f67-446d-aea7-363423975324)
