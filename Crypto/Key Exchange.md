
```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
import hashlib
import json
import random
import socket
import re
import time

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class EllipticCurve:
    def __init__(self, p, a, b):
        self.p = p
        self.a = a
        self.b = b
    
    def add_points(self, P1, P2):
        if P1 is None:
            return P2
        if P2 is None:
            return P1
            
        if P1.x == P2.x and P1.y != P2.y:
            return None
            
        if P1.x == P2.x:
            m = (3 * P1.x * P1.x + self.a) * pow(2 * P1.y, -1, self.p)
        else:
            m = (P2.y - P1.y) * pow(P2.x - P1.x, -1, self.p)
            
        m %= self.p
        
        x3 = (m * m - P1.x - P2.x) % self.p
        y3 = (m * (P1.x - x3) - P1.y) % self.p
        
        return Point(x3, y3)
    
    def multiply(self, k, P):
        result = None
        addend = P
        
        while k:
            if k & 1:
                result = self.add_points(result, addend)
            addend = self.add_points(addend, addend)
            k >>= 1
            
        return result

def decrypt_flag(shared_secret: int, encrypted_data: str) -> bytes:
    data = json.loads(encrypted_data)
    iv = bytes.fromhex(data["iv"])
    ciphertext = bytes.fromhex(data["ciphertext"])
    
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode("ascii"))
    key = sha1.digest()[:16]
    
    cipher = AES.new(key, AES.MODE_CBC, iv)
    return unpad(cipher.decrypt(ciphertext), 16)

def receive_until_prompt(s):
    data = b""
    while True:
        chunk = s.recv(4096)
        if not chunk:
            break
        data += chunk
        if b"coord:" in chunk or b"Message:" in chunk:
            break
    return data.decode()

# Connect to server
HOST = 'kashictf.iitbhucybersec.in'
PORT = 64159

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT))

# Read initial server message
data = receive_until_prompt(s)
print("Server response:", data)

# Extract parameters from server message
point_pattern = r'\((.*?),(.*?)\)'
matches = re.findall(point_pattern, data)
if matches:
    P_A_x, P_A_y = map(int, matches[0])
    print(f"Server public key: ({P_A_x}, {P_A_y})")

# NIST P-384 curve parameters
p = 39402006196394479212279040100143613805079739270465446667948293404245721771496870329047266088258938001861606973112319
a = -3
b = 27580193559959705877849011840389048093056905856361568521428707301988689241309860865136260764883745107765439761230575

E = EllipticCurve(p, a, b)

# Base point G
G = Point(
    26247035095799689268623156744566981891852923491109213387815615900925518854738050089022388053975719786650872476732087,
    8325710961489029985546751289520108179287853048861315594709205902480503199884419224438643760392947333078086511627871
)

# Generate our new key pair
n_B = random.randint(2, p-1)
P_B = E.multiply(n_B, G)

print(f"\nOur private key: {n_B}")
print(f"Our public key: ({P_B.x}, {P_B.y})")

# Send x coordinate and wait for y coordinate prompt
s.send(f"{P_B.x}\n".encode())
data = receive_until_prompt(s)
print("Server response after x:", data)

# Send y coordinate and wait for response
s.send(f"{P_B.y}\n".encode())
data = receive_until_prompt(s)
print("Server response after y:", data)

# Extract encrypted data
encrypted_data = re.search(r'{.*}', data).group(0)

# Calculate shared secret
P_A = Point(P_A_x, P_A_y)
S = E.multiply(n_B, P_A)

try:
    flag = decrypt_flag(S.x, encrypted_data)
    print(f"\nDecrypted flag: {flag.decode()}")
except Exception as e:
    print(f"Decryption failed: {e}")

s.close()
```

From GPT
