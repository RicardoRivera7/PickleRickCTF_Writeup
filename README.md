# Pickle RickCTF Walkthrough
A Tryhack me CTF writeup for the Pickle Rick room

<p align="left">
First Open the attackbox on the site and Navigate to the Target IP they give you (In my case it was 10.201.68.99) <br/>
You Should See this Page <br/>
<img src="https://i.imgur.com/iXLnBuS.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Lets inspect the page elements to see if there's anything that could be useful, right click and select "Inspect" <br/>
We can actually notice some helpful info that was hidden, we now know the username is R1ckRul3s <br/>
<img src="https://i.imgur.com/BdYlrBV.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>
Now we need to do some reconsissance, the first two things that come to mind are doing a nmap scan and a direcorty scan using Dirbuster <br/>
First I'll Spin up Dirbuster in the terminal using the command dirb http://10.201.68.99 /user/share/worlists/dirbuster/directory-list-2.3-medium.txt -w <br/>
Note: I chose a medium sized word list since a full sized one could take a very long time to generate and go through, and the path I chose will depend on where your wordlists are located (the -w gets rid of the warning message of the list being too long) <br/>

Next I'll also run the nmap scan in terminal using the command namp -A -v --top-ports 1000 10.201.68.99 <br/>

I notice a couple interesting directories that were found, the first one I'll navigate to is the robots.txt which gives us a page with a single word <br/>
<img src="https://i.imgur.com/3ZO2mU2.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

The next directory that should be of interest is the login.php, especially since we found the usermame already! <br/>
Once there we see a login page, lets enteer the username R1ckRul3s and try the phrase we saw earlier Wubbalubbadubdub as the password <br/>
<img src="https://i.imgur.com/00SsTJf.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

And we're in! <br/>
<img src="https://i.imgur.com/S7TirAn.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Clicking on any of the other parts of the page such as "potions" or "Creatures" gives us a statement that only the real rick can view this page <br/>
<img src="https://i.imgur.com/ONqYzjZ.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Lets go to the command line page and try a Command Injection by trying to list all directories using ls -alt <br/>
And boom we have results, so we know this command panel is vulnerable <br/>
<img src="https://i.imgur.com/E05PXYd.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

I notice what might be one of the first ingredients we need in the Sup3rS3retPickl3Ingred.txt, so lets try to read it using the command cat Sup3rS3retPickl3Ingred.txt <br/>
Looks like this command was disabled for this CTF <br/>
<img src="https://i.imgur.com/D5HGle8.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>1
<br/>

This makes things much more difficult, but since we know we were able to navigate to things such as robots.txt and login.php we can do the same for this <br/>
Lets naviagte to 10.201.68.99/Sup3rS3retPickl3Ingred.txt, and nice we got the first flag! <br/>
<img src="https://i.imgur.com/US5J6p0.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Lets do the same for clue.txt and wwe get the clue to look around the file system <br/>
<img src="https://i.imgur.com/up2jMS6.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

We don't have any clue about the rest of the file system so wwe can try to make some guesses with common directory names
I try a couple like ls -alt /root and ls -alt /home and luckily home gets me a hit with a rick directory 
<img src="https://i.imgur.com/aqgsAvH.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Lets go into the Rick directory with ls -alt /home/rick <br/>
Looks like we have another directory we can go into called second ingredients <br/>
Lets go a step further and use ls -alt /home/rick/second\ ingredients <br/>
Here we see we're at a dead end since nothing else is listed <br/>
<img src="https://i.imgur.com/Cpkti4p.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

We need to find a way to read this so, let's try to copy the info into a new txt file <br/>
First I try the command cp /home/rick/second\ ingredients flag2.txt and then ls -alt to see if I can find the file <br/>
When I do this nothing happens, so let's see if elevated privileges work <br/>
I run the Sudo -l command to see the list of privileges that are given and looks like pretty much everything is given and can be used <br/>
<img src="https://i.imgur.com/pUcxomc.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

I run the previous cp command again cp /home/rick/second\ ingredients flag2.txt <br/>
I run ls -alt and sure enough its there this time! <br/>
<img src="https://i.imgur.com/rFNrD79.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>


Now using the url to navigate to this new text file we can see the second ingredient appear! <br/>
<img src="https://i.imgur.com/lVRMmMw.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Now that we know we can esclate our privileges with sudo let's see if we can dig around the root directory a bit <br/>
We run the sudo ls -alt /root command and we get a hit! <br/>
<img src="https://i.imgur.com/AFWi6Tb.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

There seems to be an interesting file called 3rd.txt, so lets navigate to it in the url <br/>
Looks like we can't read it that way since it's in the root directory, so let's copy it into the default directory like we did previously!
Run the command sudo cp /root/3rd.txt flag3.txt and run ls -alt next to see if it worked <br/>
<img src="https://i.imgur.com/rJJVT9V.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Looks like it worked, now let's navigate to it in the url <br/>
Looks like we have the final flag! <br>
<img src="https://i.imgur.com/Xp9wTFL.png" height="80%" width="80%" alt="Pickle Rick"/>
<br/>
<br/>

Congrats!











