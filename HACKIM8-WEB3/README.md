## HACKIM8 - Web 3 - Blind OS command injection

In this interesting challenge, application is taking user input and passes it to a application code that runs a task on the underlying server. At first glance, I have noticed few things which are below: 

- Command might be executed on the server side but it's not reflecting back in response
- Server was also not accepting few utilities/commands like `wget`,`hping3`,`scapy`,`curl`,`http` and many more. 
