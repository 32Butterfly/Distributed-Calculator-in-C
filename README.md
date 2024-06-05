# Project about distributed calculator

Evaldas Dmitrišin IT 1st year 3rd group student

## **How to use the code** 

1. Clone this repository!
2. In your local machine go into the task3 direcotory
3. Type: 'make' in the terminal to create executable files
4. In the first terminal run ./server portnumber
5. In the other terminals run ./client host portnumber. If you are running this server and client on the same machine run localhost for the host option
6. In order to remove the compiled files run in the terminal 'make clean'

## **How the code works**
This code uses simple socket communication. It communicates via strings and numbers. First the client is given a choice menu. The client has to input the corresponding number. Which is then
validated by the forked child1 server process. If the input was correct aka 1-3 then child1 server sends the client back a Success you chose "option" string. By this option we determine what will be done.
The code distinguishes what needs to be done by using strstr which checks if the response contains the "option" in the response string. Then we proceed with the option and ask for client
input. The input is once again validated by the child1 server. And if the input is correct we fork the parent process again so that we have a child2 responsible for piping between the child1
which is responsible for communicating with the client and it child2. While the main parent is responsible for listening for new connections. 

The basic concept of how the piping works is that the child1 responsible for communicating sends the client inputted numbers which need to be calculated after verifying that they are correct. 
Child2 recieves the number from a pipe and calculates the result which then is piped through a second pipe back to the child1.

The code uses signals as well although primative the client side of things ctrl+c immediately shuts down the client and the child it was communicating with. 
The server signal is slightly more complicated as it takes 3 ctrl+C to shut down and after the third ctrl+c it kills all the parents children before exiting thus hopefully avoiding leaving
deamons roaming in the system. 1 major oversight is that shutting down the main server **DOES NOT** kill the clients so they are left not responding.

