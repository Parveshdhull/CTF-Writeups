## Run Exploit

`python exploit.py`

### exploit.py

```python
import socket, struct, re
from random import randint

connection = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
connection.settimeout(100)
connection.connect(('52.163.228.53', 8080))

def receive(index):
	d = connection.recv(1024)
	d = d.decode('ascii')
	print("Index",index,d)
	return d


def loop(key):
	# Send Mask
	data = receive(1)
	connection.send(b'0\n')

	# send guess
	data = receive(2)
	send_key = str(key) + "\n"
	connection.send(send_key.encode())
	data = receive(3)
	loop(key)


data = receive(0)
key = int(data)
loop(key)
```

