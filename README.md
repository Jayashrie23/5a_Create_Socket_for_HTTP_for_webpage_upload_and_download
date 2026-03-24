# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program :
server:
```
import socket

# DNS records (simulated database)
dns_table = {
    "google.com": "142.250.190.78",
    "yahoo.com": "98.137.11.163",
    "openai.com": "104.18.12.123",
    "example.com": "93.184.216.34"
}
# Create UDP socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Bind server to localhost and port
server_socket.bind(("127.0.0.1", 15353))

print("DNS Server running on port 5353...\n")

while True:
    # Receive domain request from client
    message, client_address = server_socket.recvfrom(1024)
    domain = message.decode()
    print("Request received for:", domain)
    # Check DNS table
    ip = dns_table.get(domain, "Domain not found")
    # Send response back to client
    server_socket.sendto(ip.encode(), client_address)
```
Client:
```
import socket
# Create UDP socket
client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ("127.0.0.1", 15353)

# Get domain name from user
domain = input("Enter domain name: ")

# Send request to server
client_socket.sendto(domain.encode(), server_address)

# Receive response
ip_address, server = client_socket.recvfrom(1024)

print("IP Address:", ip_address.decode())

client_socket.close()
```
httpserver:
```
import socket

s = socket.socket()
s.bind(("localhost",8080))
s.listen(1)

print("Server running...")

while True:
    c,addr = s.accept()
    
    request = c.recv(1024).decode()
    print("Request received")

    if "GET" in request:
        f = open("index.html","r")
        data = f.read()
        f.close()

        response = "HTTP/1.1 200 OK\n\n" + data
        c.send(response.encode())

    elif "POST" in request:
        data = request.split("\n\n")[1]

        f = open("upload.txt","w")
        f.write(data)
        f.close()

        c.send("HTTP/1.1 200 OK\n\nFile Uploaded".encode())

    c.close()
```
httpclient:
```
import socket

s = socket.socket()
s.connect(("localhost",8080))

ch = input("1.Download 2.Upload : ")

# Download webpage
if ch == "1":
    req = "GET / HTTP/1.1\nHost: localhost\n\n"
    s.send(req.encode())

    data = s.recv(4096)
    print(data.decode())

# Upload file
else:
    msg = input("Enter data to upload: ")

    req = "POST / HTTP/1.1\nHost: localhost\n\n" + msg
    s.send(req.encode())

    data = s.recv(1024)
    print(data.decode())

s.close()
```
index.html:
```
<html>
<body>
<h1>Hello HTTP Server</h1>
<p>This page is sent from the Python server.</p>
</body>
</html>
```
## OUTPUT :
server



<img width="887" height="230" alt="server py" src="https://github.com/user-attachments/assets/9a63b305-122f-4c9b-8af5-fb1299cc7de1" />




client
<img width="1146" height="250" alt="client py" src="https://github.com/user-attachments/assets/ec2bb2a7-38eb-4822-934a-b1dcf4b76fc2" />
httpserver
<img width="1002" height="237" alt="httpserver" src="https://github.com/user-attachments/assets/3a8b6d5a-06a3-4bf9-9a4c-b2e969b3a9cc" />
httpclient
<img width="995" height="249" alt="httpclient" src="https://github.com/user-attachments/assets/d102926b-90b6-4c50-a963-9c7efc308d64" />


## Result :
Thus the socket for HTTP for web page upload and download created and Executed
