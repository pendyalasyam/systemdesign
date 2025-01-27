# What is this repo? 
I have total 12 years experience in software industry. Most of my experience is on client side, once or twice in my experience I touched server side technologies but never deeply worked. If I give interview today, I can crack data structures and algorithms questions and C/C++/Java questions but I am less confident in System Design which is common for this experience. So today I have started System Design. I am reading through "System Design Interview" 2nd edition by Alex Xu. I am taking all my notes here, at the same noting down all my thoughts along the way.

A simple Client-Server application serving static content like static html pages start with something like below

![image](https://github.com/user-attachments/assets/7f7ffe18-5ae6-4d99-8a44-d08e3484c7a5)

# Web Server
A web server is computer software and underlying hardware that accepts requests via HTTP (the network protocol created to distribute web content) or its secure variant HTTPS. A user agent, commonly a web browser or web crawler, initiates communication by making a request for a web page or other resource using HTTP, and the server responds with the content of that resource or an error message. A web server can also accept and store resources sent from the user agent if configured to do so.

**Thought: Why cant we simply write a server program using socket program that accepts connections from clients and serve the requests?**

***Counter1:*** Do you want to handle html request parsing?

***Counter2:*** If you require security for your solution, do you want to implement SSL in your application ?

***Counter3:*** Suppose traffic to your server is increasing. You need to improve the performance of your server. So you may need to bring in techniques like Asynchronus IO, Multi-Threading, Event Loops, etc. Are you ready to implement all of this?

***Counter4:*** As your server is implemented from scratch, it needs to undergo heavy testing. Lot of testing cost. Are you ready to pay for this ?

***Counter5:*** You need to adhere to HTML standards to be able to survive in the internet. Do you want to do this maintainance?

***Counter6:*** OR YOU SIMPLY WANT TO USE SOME SOFTWARE THAT IS ALREADY DOING ALL OF THE ABOVE AND BATTLE TESTED?


Obvious answer is I want to use the software that is already doing all this for me. Web Server!!


Web Server Software: Apache HTTP Server, NGINX, etc.,

# How Clients Connect To Web Server ?
Server should be running on public ip. But it is difficult for users to remember ip addresses. So using DNS, we name ip address(like google) and we use name to connect to Web Server. DNS design itself is interesting design that handles name resolution for billions of websites every day. 


![image](https://github.com/user-attachments/assets/4729c6fd-7ae8-4915-a970-cbcf875edc01)

To be able to be resolved by DNS, our website should be registered to DNS using DNS registars like GoDaddy, Namecheap, Google Domains, or Bluehost. DNS doesnt provide any access to users to be able add/remove DNS records as and when they want to do so. All DNS registrations should go through theses registars only. Registars perform KYC. KYC is important because, if Registars are not there and KYC is not done

* one malicious user can impersonate another website just by changing one or two letters and registering to DNS. When users reach to this malicious website because of some small typo, this malicious site can do bad things like credentials stealing/presenting users with incorrect information, etc., In such cases, what can legitimate business owner can do? No one to reach out right?

So, in order to prevent these kind of scenarios DNS delegated domain registartion work to registars and registars do the KYC properly and when things go wrong like above case they will involve into the issue and resolve the conflicts. Registars follow strict privacy rules as well and so they dont share information they collected during KYC until it is very necessary.
