# TryHackMe - OhSINT

## Description
What information can you possible get with just one photo? 

## Questions
**#1** What is this users avatar of?

**#2** What city is this person in?

**#3** Whats the SSID of the WAP he connected to?

**#4** What is his personal email address?

**#5** What site did you find his email address on?

**#6** Where has he gone on holiday?

**#7** What is this persons password?


---


## Walkthrough
First thing to do is download the task file for this room.

The file is called **WindowsXP.jpg**. The image appears to be the background image for Windows XP operating system.

![WindowsXP.jpg](images/WindowsXP.jpg)

Since I do not see anything of note from viewing the image I run the exiftool command on the image to get more information.

```
~$ exiftool WindowsXP.jpg
ExifTool Version Number         : 10.80
File Name                       : WindowsXP.jpg
Directory                       : .
File Size                       : 229 kB
File Modification Date/Time     : 2021:03:22 15:41:15+00:00
File Access Date/Time           : 2021:03:22 16:05:50+00:00
File Inode Change Date/Time     : 2021:03:22 15:41:15+00:00
File Permissions                : rwxrwxrwx
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
XMP Toolkit                     : Image::ExifTool 11.27
GPS Latitude                    : 54 deg 17' 41.27" N
GPS Longitude                   : 2 deg 15' 1.33" W
Copyright                       : OWoodflint
Image Width                     : 1920
Image Height                    : 1080
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
GPS Latitude Ref                : North
GPS Longitude Ref               : West
Image Size                      : 1920x1080
Megapixels                      : 2.1
GPS Position                    : 54 deg 17' 41.27" N, 2 deg 15' 1.33" W
```

Here I can see 2 pieces of information that stand out.  
A username **OWoodflint** and GPS coordinates **54 deg 17' 41.27" N, 2 deg 15' 1.33" W**.

I focus on the username **OWoodflint** and use google. I found a twitter, wordpress and github accounts associated with the username. Checking the twitter account first I can see there is only 2 tweets.  
A hello world tweet and one containing a BSSID (Basic Service Set Identifier).


![OWoodflint Twitter account](images/OhSINT%20-%20twitter.png)

```
#1 What is this users avatar of ?  
Answer: Cat  
```
Looking into the tweets and their replies I don't see anything of note in the Hello World tweet. There is a base64 message from username PenTestical in the second tweet.

![Twitter base64](images/OhSINT%20-%20twitter%20message.PNG)

Using [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true)&input=UjJsMlpTQjNhV2RzWlM1dVpYUWdZU0IwY25r) I decode the message.  
 
"R2l2ZSB3aWdsZS5uZXQgYSB0cnk" decodes to "Give wigle.net a try"

Checking out wigle.net I see that it collects information on wireless networks and allows querying of that information.

Wigle.net requires a BSSID which is in the tweets and a location which I did not know. So I moved on to the next site I had found wordpress.

https://oliverwoodflint.wordpress.com/author/owoodflint/

![wordpress](images/OhSINT%20-%20wordpress.png)

Looking at the wordpress page I can see a location mentioned **New York**.  

```
#6 Where has he gone on holiday ?  
Answer: New York  
```

Nothing else of note so I checked the source code of the page and found a piece of text coloured white which made it blend into the background on the homepage. 

![wordpress_page_source](images/OhSINT%20-%20wordpress%20source%20code.png)

**pennYDr0pper.!**

I assume this to be the password based on it's complexity, it contains upper case, lower case, number and special character which is typically used for passwords.  

```
#7 What is this persons password ?
Answer: pennYDr0pper.!  
```


I did not see anything else of use on wordpress so I moved onto the github page.

![OhSINT github](images/OhSINT%20-%20github.PNG)

There is 3 bits of information on the github page
 - location: London
 - twitter account: @OWoodflint
 - Email address: OWoodflint@gmail.com

```

#2 What city is this person in ?  
Answer: London  

#4 What is his personal email address ?  
Answer: OWoodflint@gmail.com  

#5 What site did you find his email address on ?  
Answer: Github
```

Now I have enough information to answer all but one of the questions. To find the SSID I went back to wigle.net with the BSSID B4:5D:50:AA:86:41 and 2 locations London and New York.

Finding London on the map and entering the BSSID will cause a point on the map to highlight. 

![wigle.net](images/OhSINT%20-%20wigle.net.PNG)


Zooming in and I could see the SSID is **UnileverWiFi**.

![wigle.net zoomed](images/OhSINT%20-%20wigle.net%20zoom.PNG)

```
#3 Whats the SSID of the WAP he connected to ?  
Answer: UnileverWiFi
```

---
## Solutions

**#1** What is this users avatar of ?  
**Answer:** Cat  
Cat is the avatar for the twitter account.

**#2** What city is this person in ?  
**Answer:** London  
The github page states that OWoodflint is from London.

**#3** Whats the SSID of the WAP he connected to ?  
**Answer:** UnileverWiFi
The SSID is found by using the BSSID and London on wigle.net

**#4** What is his personal email address ?  
**Answer:** OWoodflint@gmail.com  
The email address is listed on github page.

**#5** What site did you find his email address on?  
**Answer:** Github

**#6** Where has he gone on holiday ?  
**Answer:** New York  
New York is mentioned on the wordpress site.

**#7** What is this persons password ?
**Answer:** pennYDr0pper.!  
The password was hidden on wordpress by setting the text colour to the same as the background.



