# Trojan
Trojan
//----------------------------------------server----------------------------------------//
#!/usr/bin/env python
#

import socket
import sys
import argparse

host='localhost'
data_payload=2048
backlog=5
  """ a simple echo server """
def echo_server(port):
    sock=socket.socket(socket.AF_INET,socket.SOCK_STREAM)                 # Create a TCP socket
    sock.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)              # Enable reuse addr or port
    server_address=(host,port)                                            # Bind socket to port
    print "Starting up echo server on %s port %s" % server_address
    socket.bind(server_address)
    socket.listen(backlog)                        # Listen to clients,backlog argument specifies the max no. of queued connections
    while True:
        print "Waiting to receive message from client "
        client,address=sock.accept()
        data=client.recv(data_payload)
        if data:
            print "Data: %s " % data
            client.send(data)
            print "Send %s bytes back to %s "%(data,address)
            
            # End connection 
            client.close()
if __name__=='__main__':
    parser=argparse.ArgumentParser(description='Socket Server Example')
    parser.add_argument('--port',action="store",dest="port",type=int,required=True)
    given_args=parser.parse_args()
    port=given_args.port
    echo_server(port)
