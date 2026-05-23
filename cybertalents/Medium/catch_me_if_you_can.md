Challenge Name:   catch me if you can

I found nothing also after inspecting the page source so I tried if there is a robotst.txt and there was


![image](../media/image25.png)


![image](../media/image26.png)

I found in the S3cr3t.php login page demanding a password


![image](../media/image27.png)

In the source.php I found the code that the login works 

I captured the request of trying to logging in and sent it to repeater


![image](../media/image28.png)


![image](../media/image29.png)

I tried many payloads and searched and the conclusion that I had to url encode the payload and this is the final payload f%0aR_4r3@

the response contained a brainfuck encoding so I used this site https://www.dcode.fr/brainfuck-language to decode the response

![image](../media/image30.png)



![image](../media/image31.png)

FL@g{R3Str1Ct1d_Ar34}