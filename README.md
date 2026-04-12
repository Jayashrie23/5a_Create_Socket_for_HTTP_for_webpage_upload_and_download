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

s = socket.socket()
s.bind(("localhost", 8080))
s.listen(1)

print("Server running at http://localhost:8080")

while True:
    c, addr = s.accept()
    c.recv(1024)

    with open("index.html", "r") as f:
        content = f.read()

    response = "HTTP/1.1 200 OK\nContent-Type: text/html\n\n" + content
    c.send(response.encode())
    c.close()
```

Client:
```
import socket

c = socket.socket()
c.connect(("localhost", 8080))

c.send(b"GET / HTTP/1.1\r\nHost: localhost\r\n\r\n")
print(c.recv(4096).decode())

c.close()
```
index.html:
```
<html>
<body>
<h1>Hello from VS Code</h1>
<p>This page is served using socket</p>
</body>
</html>
```
## OUTPUT :
server :
<img width="1117" height="234" alt="Screenshot 2026-04-12 232321" src="https://github.com/user-attachments/assets/b35dcd55-c921-4281-ae88-28c2443a8903" />
Client :
<img width="1281" height="334" alt="Screenshot 2026-04-12 232339" src="https://github.com/user-attachments/assets/a57602ac-0210-45ae-be60-ee6807e82afa" />

## Result :
Thus the socket for HTTP for web page upload and download created and Executed
