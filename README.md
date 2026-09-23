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
## Program 

## server
```
import socket
s=socket.socket()
s.bind(("localhost",8080))
s.listen(1)
print("Server running...")
while True:
    c,addr=s.accept()
    req=c.recv(1024).decode()
    print("Request received")
    if "GET" in req:
        f=open("index.html","r")
        res="HTTP/1.1 200 OK\n\n"+f.read()
        f.close()
        c.send(res.encode())
    elif "POST" in req:
        data=req.split("\n\n")[1]
        f=open("upload.txt","w")
        f.write(data)
        f.close()
        c.send("HTTP/1.1 200 OK\n\nFile Uploaded".encode())
    c.close()
```

## client
```
import socket
s=socket.socket()
s.connect(("localhost",8080))
ch=input("1.Download 2.Upload : ")
if ch=="1":
    s.send("GET / HTTP/1.1\nHost: localhost\n\n".encode())
    print(s.recv(4096).decode())
else:
    msg=input("Enter data to upload: ")
    s.send(("POST / HTTP/1.1\nHost: localhost\n\n"+msg).encode())
    print(s.recv(1024).decode())
s.close()
```
## OUTPUT
<img width="1455" height="796" alt="image" src="https://github.com/user-attachments/assets/7fe81acf-21b8-472f-ac1e-af9a69d3c873" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
