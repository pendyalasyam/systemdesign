# What is this repo? 
I have total 12 years experience in software industry. Most of my experience is on client side, once or twice in my experience I touched server side technologies but never deeply worked. If I give interview today, I can crack data structures and algorithms questions and C/C++/Java questions but I am less confident in System Design which is common for this experience. So today I have started System Design. I am reading through "System Design Interview" 2nd edition by Alex Xu. I am taking all my notes here, at the same time noting down all my thoughts along the way.


Any business where owner of the business serving his clients over internet starts with simple architecture like below

![image](https://github.com/user-attachments/assets/7f7ffe18-5ae6-4d99-8a44-d08e3484c7a5)

If I am a business owner and also a computer science studying student, I would have start developing this some thing like below ( Suppose I am developing client-server architecture baseed calculator where clients can pass values and what operation to do, And as Server I do given calculation and respond with answer).

I would have written client and server programs in C language using socket programming and I would have made these completely command prompt driven (because for me its difficult to develop GUI applications in C). Though it seems to be straight forward at first, there are multiple concerns here.

1. Server should parse the command that client is willing to do, understand and calculate the value and send the response. There should be an agreement between client and server that command should follow some pre-agreed format and then only server will understand the command and serve(Like to get he addition of 1 and 2, client should 1 2 1 where last 1 is operation type. If operation type is 2 then server may do subtraction and send the response). Similary there should be also an agreement for response text then only client will understand the response (For example if server sends 1 2 response for the division operation, what is 1 and what is 2. There should be an agreeement that first number is quotient and second number is remainder, or fist number is remainder and second nubmer is quotient). As the number of operations that server supports increases, parsing becomes more and more complex.
2. When client is sending command, there can be problems in the command. Command may not be in the expected format. How server informs the this error.
3. If your calculator supports only integers and client sends floating numbers then how server will send the response.
4. If your server is not able to accept more socket connections, how will server send the response that he is busy and ask client to retry?
5. If server is completely down, how client shows this information to user?
6. If there is some maintainance in your server and you want inform your clients when they requested for service. How will server send the response?
and more

All of these concerns are there for any client <-> server based architecture running over internet. So to address all of these problems, people came up with something called Protocol. Protocol is set of guidelines for particular kind of solution. In order to respond that client requested command is not in expected format, server send "400 Bad Request". Similarly if server is currently under maintainance server can inform its clients with "500 Service Unavailable". When solution developers adhere to protocol, then those solutions will interoperable with any other solutions that follow same protocol. 



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





