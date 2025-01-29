# What is this repo? 
I have total 12 years experience in software industry. Most of my experience is on client side, once or twice in my experience I touched server side technologies but never deeply worked. If I give interview today, I can crack data structures and algorithms questions and C/C++/Java questions but I am less confident in System Design which is common for this experience. So today I have started System Design. I am reading through "System Design Interview" 2nd edition by Alex Xu. I am taking all my notes here, at the same time noting down all my thoughts along the way.


Any business where owner of the business serving his clients over internet starts with simple architecture like below

![image](https://github.com/user-attachments/assets/7f7ffe18-5ae6-4d99-8a44-d08e3484c7a5)

Assume you are a software developer and a photographer came to you with thousands of photos of his wild photography. He asked you a to develop a client-server based solution where client applications can be given to users and server application he runs. He uploads all his latest photos and users can download them and they can use as wallpapers in their computers.

If I were you and a college going student, I would have start developing this solution some thing like below.

I would have written client and server programs in C language using socket programming and I would have made these completely command prompt driven

1. Client Application: To request all images it sends text "Give me all images"
2. Client Application: To request particular image "Give me image <image_name>"
3. Server Application: To answer "Give me all images", it will send all image names.
4. Server Application: To answer "Give me image <image_name>", it will send image with the name <image_name>.
   
Though this solution seems straight forward, there are multiple complexities here.

1. The client command should parsed properly.
2. If client command is not expected format, server has to inform that command is not proper.
3. If client asked for image that is not there, server has to informat that asked image is not existing.
4. If server is already serving maximum users it can server, server has to send some response that is currently busy.
5. If server is currently under maintainance, server has to send some response that is currently under maintainance.
6. Once you address all these concerns in your solution, tomorrow photographer want to generate money from his photography. So he starts giving premium content only to those who paid him money. So we need a way how server identifies the user to serve the content. There should be a way client can send username. On receiving user name, server checks if user has access to image and then serve the image. If user dont have access, then server should be having a way to inform the user that he doesnt have enough previlages.


All of these concerns are there with any client <-> server based solution serving the content. If multiple business owners(one business owner serving images, one business owner serving videos, one business owner, text articles, and so on) wants similar solutions and goes to different software developers, all of those software developers fianlly has to develope similar modules except that the content is different. Solution to avoid this duplicate effort is software developers identified this duplicacy and all they came togehter and came up with a solution outlining how client request should look like to request a image/video/any other resource and how server should send the response and how server should inform the error. Now that all agreed on some format, if some developer developed a solution for images... now the same can be used for videos with few/some modifications.

The rules that software engineers agreed on how a client request should look and how shold be the response and how an error is informed is collectively called as Protocol.

***A protocol is a set of rules that define how data is transmitted and received over a network***

The problem with C based applications is that whenever we change client code we need to compile the application and distribute to clients. All computers are not same, they are different based on their processor types. So to deliver your new client application to all users, you need to compile your client code to all different variations of your clients processor and distribute them accordingly. Users will not able to use your new version of the client until users update the application. In order to solve this problem, we need a technology which doesnt require application compilation to distribute it to the users. One solution is you can develop one base software that is compiled and distribued once based on users processors and all further application logic of can be downloaded on the fly and can be executed by that base software. This base software is called Web Browser. Since the applications generally containt GUI elements, these elements are mentioned with markup language. Web browser on receiving this application logic written in markup language parses the markup and show the elements to the users. 

HTML is this markup language and HTTP is the protocol that mentions the rules that mentions how to transfer this markup text and what are the responses texts to different kinds of errors while executing this HTML and what are the request/response formats.


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

* one malicious user can impersonate another website just by changing one or two letters and registering to DNS. When users reach to this malicious website because of some small typo, this malicious site can do bad things like credentials stealing/presenting users with incorrect information, etc., In such cases, what can legitimate business owner do? No one to reach out right?

So, in order to prevent these kind of scenarios, DNS delegated domain registartion work to registars and registars do the KYC properly and when things go wrong like above case they will involve into the issue and resolve the conflicts. Registars follow strict privacy rules as well and so they dont share information they collected during KYC until it is very necessary and required by law. For example, in JioHotstar case we still dont know who is that Delhi boy.

# Problems with above simple Clients <-> Server architecture ?
***Single Point Of Failure:*** If the server crashes or power goes off or network gets disconnected, then clients can not reach to the server. This is undesirable for business. Assuem you are a restaurant owner who takes order online on your website and prepared 1000 meals assuming you may get 1000+ orders online. But what if your computer where web server is running crashes? Loss for the business right? So this is undesirable. You want your service to be up and running all the time during your business hours irrespective of crashes/power failures/network bottlenecks/etc. 

***Scalablity Issues:*** Assume your web server maximum 4096 simultaneous connections and 4096 users already connected your service. Now if next person wants to use your service, he can not connect because of the limit. Your client dont want to wait and you dont want to lose him. So as your service is becoming popular, you want your service to accomodate more and more traffic.

***Performance Issues:*** If lot of people start using your service then resources like CPU, memory and network bandwidth of your computer where your webserver is running will be maximum utilized and hence your web server can not serve your clients fastly. Assume yours is a video streaming service like Netflix or Amazon Prime and lot of your clients are consistently seeing spinner animation when they are watching their interesting videos. They will shutdown computer out of frsturation and move to other service provider right? you dont want to lose customers like this. Your service must be performant enough.

***Security Issues:*** Your server is running on public ip. By any chance if a hacker gets into your web server, then he can do all mischevous things he wants to do. For example, he may redirect all the money that your customers are paying to you to his account. So you want your service to be secure.





