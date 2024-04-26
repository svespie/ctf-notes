[Baby Time Capsule](https://app.hackthebox.com/challenges/Baby%2520Time%2520Capsule)

CHALLENGE DESCRIPTION

*Qubit Enterprises is a new company touting it's propriety method of qubit stabilization. They expect to be able to build a quantum computer that can factor a RSA-1024 number in the next 10 years. As a promotion they are giving out "time capsules" which contain a message for the future encrypted by 1024 bit RSA. They might be great engineers, but they certainly aren't cryptographers, can you find a way to read the message without having to wait for their futuristic machine?*

![Baby_Time_Capsule.zip](../../../../_attachments/Baby%20Time%20Capsule.zip)

pw: hackthebox

This challenge includes both a download and a live website to interact with. First, the provided source code:

``` python
from Crypto.Util.number import bytes_to_long, getPrime
import socketserver
import json

FLAG = b'HTB{--REDACTED--}'

class TimeCapsule():

    def __init__(self, msg):
        self.msg = msg
        self.bit_size = 1024
        self.e = 5

    def _get_new_pubkey(self):
        while True:
            p = getPrime(self.bit_size // 2)
            q = getPrime(self.bit_size // 2)
            n = p * q
            phi = (p - 1) * (q - 1)
            try:
                pow(self.e, -1, phi)
                break
            except ValueError:
                pass
        return n, self.e

    def get_new_time_capsule(self):
        n, e = self._get_new_pubkey()
        m = bytes_to_long(self.msg)
        m = pow(m, e, n)
        return {"time_capsule": f"{m:X}", "pubkey": [f"{n:X}", f"{e:X}"]}

def challenge(req):
    time_capsule = TimeCapsule(FLAG)
    while True:
        try:
            req.sendall(
                b'Welcome to Qubit Enterprises. Would you like your own time capsule? (Y/n) '
            )
            msg = req.recv(4096).decode().strip().upper()
            if msg == 'Y' or msg == 'YES':
                capsule = time_capsule.get_new_time_capsule()
                req.sendall(json.dumps(capsule).encode() + b'\n')
            elif msg == 'N' or msg == "NO":
                req.sendall(b'Thank you, take care\n')
                break
            else:
                req.sendall(b'I\'m sorry I don\'t understand\n')
        except:# Socket closed, bail
            return

class MyTCPRequestHandler(socketserver.BaseRequestHandler):

    def handle(self):
        req = self.request
        challenge(req)

class ThreadingTCPServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
    pass

def main():
    socketserver.TCPServer.allow_reuse_address = True
    server = ThreadingTCPServer(("0.0.0.0", 1337), MyTCPRequestHandler)
    server.serve_forever()

if __name__ == '__main__':
    main()

```



