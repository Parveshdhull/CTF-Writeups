#HOME: 
`checkin`

No need to wear a mask at HOME

# Explanation: #

Challenge was supposed to be easy. But it took very long to solve. Because hint was kind of vague. After wasting several hours, I was frustrated and refreshing challenge page again and again. I noticed that CTF logo in left corner was becoming larger after a sec. So I thought what the heck is going on. Because it was fast and small, not visible. I started recording video.

[HOME](files/home.mkv)

It was simple python code to connect to `home.realworldctf.com` on port `1337` using `pwn` library. I started this session in interactive mode using below code and got flag.

```python
from pwn import *
io = remote('home.realworldctf.com', 1337)
io.interactive()
```
[Flag](files/home_flag.mkv)
