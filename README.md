# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## NAME : DHANAPPRIYA S
## REG NO:21224230056
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
## SERVER SIDE:
```
import socket
import os

HOST = '127.0.0.1'
PORT = 8080

def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # Allow quick reuse of the address to prevent WinError 10048
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server_socket.bind((HOST, PORT))
    server_socket.listen(5)
    print(f"Server active. Listening on http://{HOST}:{PORT}...")

    while True:
        client_conn, client_addr = server_socket.accept()
        request = client_conn.recv(4096).decode()
        
        if not request:
            client_conn.close()
            continue

        lines = request.split("\r\n")
        request_line = lines[0]
        
        if not request_line or len(request_line.split(" ")) < 2:
            client_conn.close()
            continue

        method, path = request_line.split(" ")[0], request_line.split(" ")[1]
        filename = path.lstrip("/")

        # GET: Webpage/File Download
        if method == "GET":
            if os.path.exists(filename):
                with open(filename, "r") as f:
                    content = f.read()
                response = f"HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n{content}"
            else:
                response = "HTTP/1.1 404 Not Found\r\n\r\n<h1>404 File Not Found</h1>"
            client_conn.sendall(response.encode())

        # POST: Webpage/File Upload
        elif method == "POST":
            body_start = request.find("\r\n\r\n") + 4
            content = request[body_start:]
            with open(filename, "w") as f:
                f.write(content)
            response = f"HTTP/1.1 201 Created\r\n\r\nFile '{filename}' successfully saved on server."
            client_conn.sendall(response.encode())

        client_conn.close()

if __name__ == "__main__":
    start_server()
```
## CLIENT SIDE:
```
import socket

SERVER_HOST = '127.0.0.1'
SERVER_PORT = 8080

def download_file(filename):
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((SERVER_HOST, SERVER_PORT))
    
    request = f"GET /{filename} HTTP/1.1\r\nHost: {SERVER_HOST}\r\n\r\n"
    client_socket.sendall(request.encode())
    
    response = client_socket.recv(4096).decode()
    print("\n--- [SERVER RESPONSE] ---")
    print(response)
    client_socket.close()

def upload_file(filename, content):
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((SERVER_HOST, SERVER_PORT))
    
    headers = f"POST /{filename} HTTP/1.1\r\nHost: {SERVER_HOST}\r\nContent-Length: {len(content)}\r\n\r\n"
    request = headers + content
    
    client_socket.sendall(request.encode())
    
    response = client_socket.recv(4096).decode()
    print("\n--- [SERVER RESPONSE] ---")
    print(response)
    client_socket.close()

if __name__ == "__main__":
    while True:
        print("\n==============================")
        print("  HTTP Socket Operations Menu")
        print("==============================")
        print("1. Download Webpage/File (GET)")
        print("2. Upload Webpage/File (POST)")
        print("3. Exit")
        choice = input("Enter choice (1-3): ")
        
        if choice == '1':
            fname = input("Enter filename to download (e.g., sample.html): ")
            download_file(fname)
        elif choice == '2':
            fname = input("Enter filename to create/upload (e.g., upload.txt): ")
            data = input("Enter text content to upload: ")
            upload_file(fname, data)
        elif choice == '3':
            break
        else:
            print("Invalid Choice")
```
## OUTPUT
## SERVER SIDE:
<img width="957" height="450" alt="image" src="https://github.com/user-attachments/assets/134a4a6d-acf2-40be-805c-0089a2733d62" />

## CLIENT SIDE:
<img width="960" height="817" alt="image" src="https://github.com/user-attachments/assets/13ec949e-db3a-47e9-a34d-8f4514ec7051" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
