# 3c.CREATION FOR FILE TRANSFER USING TCP SOCKETS
## AIM
To write a python program for creating File Transfer using TCP Sockets Links
## ALGORITHM:
1. Import the necessary python modules.
2. Create a socket connection using socket module.
3. Send the message to write into the file to the client file.
4. Open the file and then send it to the client in byte format.
5. In the client side receive the file from server and then write the content into it.
## PROGRAM :
## Server:
```

import socket
import os

# Create TCP socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

host = '127.0.0.1'
port = 12345

# Bind and listen
server.bind((host, port))
server.listen(1)

print("File Transfer Server is waiting...")
client, address = server.accept()

print("Connected to:", address)

# Get file name
filename = input("Enter file name to send: ")

if not os.path.isfile(filename):
    print("File not found!")
    client.sendall(b"ERROR")
else:
    # Read file
    with open(filename, 'rb') as file:
        data = file.read()

    # Send file size first
    client.sendall(b"OK")
    client.sendall(len(data).to_bytes(8, byteorder='big'))

    # Send file content
    client.sendall(data)

    print("File sent successfully!")

# Close connection
client.close()
server.close()
```
## client :
```

import socket

# Create TCP socket
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

host = '127.0.0.1'
port = 12345

# Connect to server
client.connect((host, port))

print("Connected to File Transfer Server!")

# Function to receive exact number of bytes
def receive_exact(sock, size):
    data = b''

    while len(data) < size:
        packet = sock.recv(size - len(data))

        if not packet:
            raise ConnectionError("Connection closed unexpectedly")

        data += packet

    return data

# Receive server status
status = receive_exact(client, 2)

if status == b"OK":

    # Receive file size
    file_size = int.from_bytes(
        receive_exact(client, 8),
        byteorder='big'
    )

    # Receive file content
    with open("received_file.txt", "wb") as file:
        remaining = file_size

        while remaining > 0:
            chunk = client.recv(min(1024, remaining))

            if not chunk:
                raise ConnectionError("File transfer interrupted")

            file.write(chunk)
            remaining -= len(chunk)

    print("File received successfully!")
    print("Saved as received_file.txt")

else:
    print("File not found on server!")

# Close connection
client.close()
```

## OUPUT :
<img width="1917" height="1155" alt="image" src="https://github.com/user-attachments/assets/f438a2ed-b695-4666-bbe5-a893d189e291" />

<img width="1736" height="1102" alt="image" src="https://github.com/user-attachments/assets/b4a70665-005d-40b0-bbb2-a50593766b1d" />


## RESULT
Thus, the python program for creating File Transfer using TCP Sockets Links was 
successfully created and executed.
