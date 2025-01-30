As a software engineer with 12 years of experience when I see students just of out college making youtube videos on System Design, one side I was inspired and on the other side I felt ashamed because still I dont know those topics. Most of experience is on building client side applications, meaning I just need to make sure client application is perfomant with out any crashes. So I became strong in multithreading concepts, data structures and algorithms, mutexes, semaphores, locks and many other concepts that are necessary to build robust performant client side application. But I dont want to lag behind in the industry and I dont want to simply sit before my other colleagues who boast about system design skills with that expression on my face that I dont know. So I decided to learn System Design in a systematic & practical way. All the content of this blog is just the journey of me learning System Design from very basic and my thoughts while learning the System Design. I am preparing this blog for myself to revisit in future, but at the same time I hope it will also help you. Happy learning!!



Solution for any business problem where users of the business connect to owner via internet starts with simple architecture like below.  
![image](https://github.com/user-attachments/assets/7f7ffe18-5ae6-4d99-8a44-d08e3484c7a5)

For the learning purpose, I consider the following business scenario:

Assume you are a software developer and a photographer came to you with thousands of photos of his wild photography. He asked you a to develop a client-server based solution where client applications can be given to users and server application he runs. He uploads all his latest photos and users can download them and they can use as wallpapers in their computers.

If I were you and if I am a college going student, I would have start developing this solution some thing like below.

I would have written client and server programs in C language using socket programming and I would have made these completely command prompt driven

1. Client Application: To request all images it sends text "Give me all images"
2. Client Application: To request particular image "Give me image <image_name>"
3. Server Application: To answer "Give me all images", it will send all image names.
4. Server Application: To answer "Give me image <image_name>", it will send image with the name <image_name>.
   
Though this solution seems straight forward, there are lot of things you need to handle make this solution working and covering aleast mimium scenarios .

1. The client command should parsed properly.
2. If client command is not expected format, server has to inform that command is not proper.
3. If client asked for image that is not there, server has to informat that asked image is not existing.
4. If server is already serving maximum users it can server, server has to send some response that is currently busy.
5. If server is currently under maintainance, server has to send some response that is currently under maintainance.
6. Once you address all these concerns in your solution, tomorrow photographer want to generate money from his photography. So he starts giving premium content only to those who paid him money. So we need a way how server identifies the user to serve the content. There should be a way client can send username. On receiving user name, server checks if user has access to image and then serve the image. If user dont have access, then server should be having a way to inform the user that he doesnt have enough previlages.

Now suppose if another person came and asked your friend to develop client<->server based solution for him for his all of his travelling videos. Users of the solution can download the videos from the server and watch. Your friend, if he is also a college going student, he may approach solving the problem like me 

1. Client Application: To request all images it sends text "Provide all videos"
2. Client Application: To request particular image "Provide video <video_name>"
3. Server Application: To answer "Provide all videos", it will send all video names.
4. Server Application: To answer "Provide video <video_name>", it will send video with the name <video_name>.


If you observe in those use cases just the content type is changing. Both developers approached the problem similarly and solved the problems. The solution provided by first developer for photos case will not work for videos, because Photos client application sends "Give me all images" command  and Videos client application sends "Provide all videos". If both developers decided to use "GET RESOURCES" command as image or video can be called as a resource, then the same implementation done by one developers could be useful for other developer. Similar instead of two different commands for getting one image or one video, if both developers decide to use "GET RESOURCE <RESOURCE_NAME>" then the solution could become reusable. 

You see, if people sit together and agree to follow one single method to solve similar problems, then those solutions become reusable. This way of people coming together and agreeing onto set of guidelines or rules to solve similar problems is called "Drafting Protocol" where "Protocol" is those set of rules and guidelines.

**Protocol: is a set of rules that define how data is transmitted and received over a network**

# HTML, HTTP, Web Browser
The problem with C based applications is that whenever we change client code we need to compile the application and distribute to clients. All computers are not same, they are different based on their processor types. So to deliver your new client application to all users, you need to compile your client code to all different variations of your clients processor and distribute them accordingly. Users will not able to use your new version of the client until users update the application. In order to solve this problem, we need a technology which doesnt require application compilation to distribute it to the users. One solution is you can develop one base software that is compiled and distribued once based on users processors and all further application logic of can be downloaded on the fly and can be executed by that base software. This base software is called ***Web Browser***. Web browser generally knows how to draw different kinds of Graphics. As per you application needs, in your application you just need to mention what all graphics you need, where you want to place them and what should be their properties. Web Browser on downloading this application logic, draws required graphics on screen as per the configuration.

You can see in application logic, we are just mentioning (or marking) what all graphics we need and their properties. So the language that is used to write this application logic is called markup language. Hyper-Text-Markup-Language (HTML) is one such language.

And the protocol that outlines how HTML is transfered between client and server and what responses are sent in success/failure cases is Hyper-Text-Transport-Protocol (HTTP). 

# HTTPS

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





