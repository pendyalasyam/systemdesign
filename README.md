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

# HTML, HTTP, Web Browser, Web Server
The problem with C based client-side applications is that whenever we change client code we need to compile the application and distribute to clients. All computers are not same, they are different based on their processor types. So to deliver your new client application to all users, you need to compile your client code to all different variations of your clients processor and distribute them accordingly. Users will not able to use your new version of the client until users update the application. In order to solve this problem, we need a technology which doesnt require application compilation to distribute it to the users. One solution is you can develop one base software that is compiled and distribued once based on users processors and all further application logic of can be downloaded on the fly and can be executed by that base software. This base software is called ***Web Browser***. Web browser generally knows how to draw different kinds of Graphics. As per you application needs, in your application you just need to mention what all graphics you need, where you want to place them and what should be their properties. Web Browser on downloading this application logic, draws required graphics on screen as per the configuration.

You can see in application logic, we are just mentioning (or marking) what all graphics we need and their properties. So the language that is used to write this application logic is called markup language. Hyper-Text-Markup-Language (HTML) is one such language.

And the protocol that outlines how HTML is transfered between client and server and what responses are sent in success/failure cases is Hyper-Text-Transport-Protocol (HTTP). 

**Examples of Web Browsers:** Google Chrome, Microsoft Edge, Safari, etc.,

Coming on to server side, it may seem simple to send Resources to clients. But once this is deployed, we will start seeing actual problems.
* Clients may send requests in unexpected format. Server needs to handle this and inform the clients.
* Clients may send wrong data. Server needs to handle this and inform the clients.
* One hacker can send lot of requests to the server to make server busy with serving his requests and not getting enough time to serve legitimate users.
* If your service is becoming popular, you may receive lot of requests and to serve them you may need to bring concepts like multi-threading, event loops etc.
* If your service become popular you may want monetize it. So you want to serve only to paid users. Meaning you need to provide access to only people who have access (authorized users).

See as service becoming more and more popular, you need to implement more and more requirements. And these requirements for every service over the internet. So a reusable software is developed to deal with all the basic requirements. That is called **Web Server**.

**Examples of Web Servers:** Nginx, Apache HTTP Server aka httpd.

***HTTP Protocol:***
HTTP Protocol is the set of rules and guidelines used to transfer resources like html documents and other kind of documents between client and server in client-server architecture based systems. As per the protocol, clients needing resources send REQUEST to server and server responds with RESPONSES either with requested resources or errors in case of failures.

There are different kinds of REQUESTs each designed for serving different kind of request client want to do with resource. Of all the methods supported by HTTP, following 4 are important

* GET: Read resource from server (Safe/Idempotent)
* PUT: Create the resource on the server (Unsafe/Not-Idempotent)
* POST: Update resource on the server (Unsafe/Idempotent)
* DELETE: Delete the resource in server (Unsafe/Idempotent)

_Idempotent: If same operation is done multiple times, the effect is same._

_Safe: Data on the server remains same_

There are different kind of responses each indicating particular of kind of result process the request.
* 1xx informational response – the request was received, continuing process
* 2xx successful – the request was successfully received, understood, and accepted
* 3xx redirection – further action needs to be taken in order to complete the request
* 4xx client error – the request contains bad syntax or cannot be fulfilled
* 5xx server error – the server failed to fulfil an apparently valid request

# Brining up basic web solution
Now that we understand what software is used as Client and what software software is used as Server and what technologies are used to establish communication between them, let us bring up the basic client-server infrastructure 

***Setting up Web Server - Nginx:***
1. sudo apt install nginx -y
2. edit /etc/nginx/sites-available/default such that it has following content for correspoding keys in _server_ section
   ```
   server {
      listen 80;
      listen [::]:80;
      .
      .
      .
      .
   }
   ```

***First Application Written in HTML:***

Write your first index.html page in /var/www/html

***Start Web Server:***

sudo systemctl restart nginx

***Opening with browser:***

Open `http://<your_computer_ipaddress>` from the browser within the same computer and you will be able to see your application.

# Clients Connecting To Web Server Over Internet?
## Private IP Address vs Public IP Address
Now you have web server up and running and your are able to open your website from web browser with in your local computer. But your clients are not within the same computer and not even in the same private network you are connected. If you have 4 or 5 computers with in your house and all are connected to a home router then this is one private network. Similarly if 100 or so computers are connected your office router then that is another private network. Though these computers have different ip addresses, to the outer world these are all behind one ip address. So whatever the ip addres assigned for your computer is private to your network and only computers within this private network can access your website.  In order for your clients to be able to connect to your website, you need to get one public ip address for your computer.

The ip address provided by local internet service providers are all private ip addresses. If not configured to be static, when you restart your computer your private ip address may also change.

So private ip addresses are not suitable to use for web servers because of their nature that they are not accessbile outside your private network and if not configured properly they may keep changing.

So, in order to make our service always available at the same place and publicly accessible we need public ip address that remains same always for our computer in which Nginx is running. We call this kind of address as static public ip address. 

I called my internet provider to provide me static public ip address, he told me it would cost me around Rs.250/month. For system design I wanted to experiment with 4 different computers but I have only one computer. If buy 3 extra small computers and one public ip address... it would cost me Rs. 30000 initially and Rs.250 every month. So I decided to use free computers provided by AWS. AWS provides 1 small computer with 1GB RAM and 30GB Disk space with public ip for 750hrs per month free per account . So I created 4 gmail accounts and at this time I created one t2.micro computer with one account. Yet to create another 3 computers. Now I have one computer which can run 24hr for free with public ip address and `if I dont restart` this public ip address will not change.

I deployed Nginx and my website in the AWS machine and now I can access my website from around the world with `http://<AWS-IP-ADDR>`

## IP Address vs Domain Name
Human beings are not good with remembering numbers, they are good with remembering names. So we need a system that remebers mapping between to names to ip addresses and when users request for names they get resolved to ip address. DNS is such system existing from 1983 and doing this job effectively for websites all over the world. Once we successfully register <websitename,ipaddress> in DNS then we can access our website like `<http://<websitedomainname>` from clients web browsers.

### DNS Working Mechanism
As of today, there are around 1.1 billion websites around the world. If one computer handles all this traffic, it will be lot of work load on single computer to handle and clients will experience delays in resolution. And also if that computer crashes or if there is power cut for that computer then nobody will be able to access websites with names. So to make this solution robust, DNS decentralized managing this huge mapping data. It divides entire DNS into different parts. Anybody wants to register theire domain(website) into DNS needs to register their domain names with in these parts. These higher level divisions are called `Top-Level-Domains`. One server holds ip addresses of all these Top-Level-Domains and this server is called `Root Server`. 

When you enter `http://www.<websitename>.<TLD_Name>`, 
* it first goes to DNS Resolver `(Called Recursive Resolver)` provided by your ISP to resolve `http://www.<websitename>.<TLD_Name>`. Recursive Resolver then contact Root Server to resolve `www.<websitename>.<TLD_Name>`. To avoid the case of root server being failed, Root Server is replicated on 13 different servers A-M and ip addresses of all these 13  different servers are well-known. If one Root server fails, then Recursive Resolver contacts another Root Server Randomly.
* Since we registered our website in TLD, Root Server dont have information about `www.<websitename>.<TLD_Name>`. So it responds with TLD <TLD_Name>'s ip address to be contacted
* Recursive Resolver then ask TLD <TLD_Name>
* TLD knows the ip address of <websitename>.<TLD_Name>. So it responds with ip of `<websitename>.<TLD_Name>`
* Recursive Resolver now has the ip address of server <websitename>.<TLD_Name>. This server which controls the name resolution for names in domain is called `Authoritative Name Server`. Recursive Resolver asks Authoritative Name Server to resolve `www.<websitename>.<TLD_Name>`. Authoritative Name Server knows the ip address of server which manages `www` for domain <websitename>.<TLD_Name>. So it responds with ip address
* Recursive Resolver responds with this final ip address back to web server
* Web Browser knows for http it needs to use port :80. So it resolves `http://www.<websitename>.<TLD_Name>` to final <IP-Address>:80 and sends the HTTP GET Request.

![image](https://github.com/user-attachments/assets/4729c6fd-7ae8-4915-a970-cbcf875edc01)

For the purpose of performance, resolved names are cached everywhere in the system. So when you browse the website, it is possible that returned ip address is cached ip address(cached in either web browser, cached in either operating system, cached in either TLD name server) and can be wrong not exactly pointing to the right ip address. Every resolved name response has a TTL (time-to-live). Once this time is expired for the record, resolver(web browser, operating system, TLD) must redo the resolution. The one that is correctly resolving the correct ip address of www.google.com is google.com server. Hence google.com server is authoritative name server for www.google.com. Similarly .com TLD server is the authoritative name server for google.com.


To register website with DNS, we need to go registrar that checks if the name we want to give for our website already being used by anyone else or not, and does KYC and then registers our <website name, ipaddress> in DNS. Example DNS Registars: GoDaddy, Namecheap, Google Domains, or Bluehost

You may think why we need to go through registars and why cant DNS system itself give us some software to register our names in the DNS where this software handles the name conflicts. Consider the following case

* one malicious user can impersonate another website just by changing one or two letters and registering to DNS. When users reach to this malicious website because of some small typo, this malicious site can do bad things like credentials stealing/presenting users with incorrect information, etc., In such cases, what can legitimate business owner do? No one to reach out right?

So, in order to prevent these kind of scenarios, DNS delegated domain registartion work to registars and registars do the KYC properly and when things go wrong like above case they will involve into the issue and resolve the conflicts. Registars follow strict privacy rules as well and so they dont share information they collected during KYC until it is very necessary and required by law. For example, in JioHotstar case we still dont know who is that Delhi boy.

For doing all this, we need to pay for Registars while registering our domain names.

### A Note On AnyCast IP Address
We understand that Name resolution start with Root Name Server. But what happens if those 13 Root Servers crash or fail. Since it is only 13 in count, it is highly possible right? So, DNS system designers folllowed one intelligent idea. They used AnyCast IP Addresses for Root Servers. AnyCast ip address makes it possible that multiple computers located at geographically different places share the same ip address. So for example A-Root-Server ip `198.41.0.4` doesnt mean one computer, its collection of computers located at different geographical locations and sharing the same ip address. When Recursive Resolver tries to contact A-Root-Server then it will be actually served by A-Root-Server's replica located near geographic location. This not only handle failures, but also improves system performance as whole. This is another reason for us to pay for DNS resolution, otherwise who will pay for all this infrastructure cost.

AnyCast ip addressed is used by Top-Level-Domain Name Servers and other big companies like Google, Microsoft, etc.

### Buying domain name and configuring the DNS
1. Decide on the name that you want to use for your website
2. Goto `https://www.godaddy.com/`
3. Search for availability of your domain name, if avaialble observe the cost under each TLD
4. Once decided with domain name and TLD you want to use, complete your KYC and buy
5. Goto your domain as Profile -> My Products -> My Account -> Domains -> Select your domain
6. Goto DNS Section
7. For A Record, keep `@` for `Name` and edit `Data` to be your server public ip address
8. Make sure changes are saved.
9. It takes some time for registar to propagate your DNS information to all the DNS servers arounds the world. It may take 1-48hrs. I observed it being completed in 2hrs.
10. Edit your Nginx configuration to make it behave as your `<websitename>`. In order to do this edit /etc/nginx/sites-available/default so that server_name is like below
    `server_name <domainname> www.<domainname>;`
11. Now you should be able to hit your server with `http://www.<domainname>` from any computer in the world.

### DNS Records
* **A Record:** Maps a domain to an IPv4 address.
* **AAAA Record:** Maps a domain to an IPv6 address.
* **CNAME Record:** Aliases one domain to another.
* **MX Record:** Defines mail exchange servers for the domain.
* **TXT Record:** Holds arbitrary text data, often for verification.
* **NS Record:** Specifies authoritative name servers for the domain

# Problems with above simple Clients <-> Server architecture ?
***Single Point Of Failure:*** If the server crashes or power goes off or network gets disconnected, then clients can not reach to the server. This is undesirable for business. Assuem you are a restaurant owner who takes order online on your website and prepared 1000 meals assuming you may get 1000+ orders online. But what if your computer where web server is running crashes? Loss for the business right? So this is undesirable. You want your service to be up and running all the time during your business hours irrespective of crashes/power failures/network bottlenecks/etc. 

***Scalablity Issues:*** Assume your web server maximum 4096 simultaneous connections and 4096 users already connected your service. Now if next person wants to use your service, he can not connect because of the limit. Your client dont want to wait and you dont want to lose him. So as your service is becoming popular, you want your service to accomodate more and more traffic.

***Performance Issues:*** If lot of people start using your service then resources like CPU, memory and network bandwidth of your computer where your webserver is running will be maximum utilized and hence your web server can not serve your clients fastly. Assume yours is a video streaming service like Netflix or Amazon Prime and lot of your clients are consistently seeing spinner animation when they are watching their interesting videos. They will shutdown computer out of frsturation and move to other service provider right? you dont want to lose customers like this. Your service must be performant enough.

***Security Issues:*** Your server is running on public ip. By any chance if a hacker gets into your web server, then he can do all mischevous things he wants to do. For example, he may redirect all the money that your customers are paying to you to his account. So you want your service to be secure.



## Ephemeral Ports
## ulimit -n

/etc/security/limits.conf

* soft nofile 100000
* hard nofile 100000

* Save and exit. Then, restart your terminal or logout/login.




* list of open connections * sudo ss -tnp | grep 17997 | wc -l

# Highly Available, Scalable, Relialbe DB Design
1. WAL based recovery is straight forward. Undo log based recovery can be done but replication is not straight forward
2. WAL/Undo logs are definitely needed because some db writes are done on Virtual memory first and updated on actual physical disks
3. DB backup is for disaster recovery and DB WALs are for system crashes.
4. WALs are sequential so fast. Normal transactions may spread across different tables and indexes and operating on them directly may be slow. So better write to WALs for fastness.
5. Physical DB Replication Vs Logical DB Replication
6. Synchronous Vs Asynchronous Replication : Synchronous replication is slow, Asynchronous synchronous fast. Synchronous replication is consistent, Asynchronous Replication is eventually consistent. For read heavy systems... have more followers
7. Reading your own writes... try to read your own data from leader. But if there are lot of changing data for the user, it may  not scale as lot of reads go to leader only. Use time based forwarding to leader. For example if you asume after 1min all replicas will sync to master, then requests for own data within 1min after update to go to leader and all other requests after 1min to go to followers. Other approach, let the client send last update information in its request and let the request forward to any follower. If follower has data till that time, then request will be processed with follower or follower will wait to catch upto that point.
8. Monotonic Reads / Moving backwards in time.

# Keepalived
**keepalived:
1. Using this software, multiple physical devices share same virtual ip address. Only one of machine will be assigned with this ip address at a time.
2. sudo apt update
3. sudo apt install keepalived
4. sudo apt install libipset13
5. 

vrrp_instance VI_1 {
  state MASTER
  interface eth0
  virtual_router_id 55
  priority 150
  advert_int 1
  unicast_src_ip 172.26.51.216
  unicast_peer {
    172.26.63.47
    172.26.62.191
  }

  authentication {
    auth_type PASS
    auth_pass C3P9K9gc
  }

  virtual_ipaddress {
    172.26.51.215/20
  }
}



vrrp_instance VI_1 {
  state BACKUP
  interface eth0
  virtual_router_id 55
  priority 100
  advert_int 1
  unicast_src_ip 172.26.51.216
  unicast_peer {
    172.26.63.47
    172.26.62.191
  }

  authentication {
    auth_type PASS
    auth_pass C3P9K9gc
  }

  virtual_ipaddress {
    172.26.51.215/20
  }
}


6. sudo systemctl enable --now keepalived.service
7. sudo systemctl start keepalived.service
8. sudo systemctl stop keepalived.service
9. sudo systemctl restart keepalived.service**



# Postgres Commands
1. Starting Postgres Server instance: pg_ctl -D <data_directory> start
2. Stoping Postgres Server instance: pg_ctl -D <data_directory> stop
