# IMPLEMENTATION-OF-STOP-AND-WAIT-PROTOCOL
# EXP: 1(a)
# DATE:
# REG. No.: 212224040290
# AIM:
To write a python program to perform stop and wait protocol
# ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client otherwise it will sendNACK signal to client.
6. Stop the program
# PROGRAM:
import socket
import time

# Sender function
    def sender():
    sender_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    receiver_address = ('localhost', 12345)

    messages = ["Hello", "How are you?", "Stop-and-Wait", "Protocol", "Done"]
    for i, msg in enumerate(messages):
        while True:
            sender_socket.sendto(msg.encode(), receiver_address)
            print(f"Sent: {msg}")

            sender_socket.settimeout(2)  # Set timeout for ACK
            try:
                ack, _ = sender_socket.recvfrom(1024)
                if ack.decode() == f"ACK {i}":
                    print(f"Received ACK {i}")
                    break
            except socket.timeout:
                print("Timeout! Resending...")

    sender_socket.close()


# Receiver function
    def receiver():
    receiver_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    receiver_socket.bind(('localhost', 12345))

    expected_seq = 0
    while True:
        data, addr = receiver_socket.recvfrom(1024)
        msg = data.decode()

        if msg == "Done":
            print("Transmission complete.")
            break

        print(f"Received: {msg}")

        # Send ACK
        receiver_socket.sendto(f"ACK {expected_seq}".encode(), addr)
        expected_seq += 1

    receiver_socket.close()


# Run sender and receiver separately
    if __name__ == "__main__":
    import multiprocessing
    receiver_process = multiprocessing.Process(target=receiver)
    sender_process = multiprocessing.Process(target=sender)

    receiver_process.start()
    time.sleep(1)  # Give receiver time to start
    sender_process.start()

    sender_process.join()
    receiver_process.join()

# OUTPUT
![image](https://github.com/user-attachments/assets/87c9d392-c537-444f-a107-3f35017fb699)
![image](https://github.com/user-attachments/assets/9a447238-5e20-4075-913d-5569c5bb0cad)

# RESULT:
Thus, python program to perform stop and wait protocol was successfully executed.

