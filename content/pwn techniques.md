



arbitrary code execution (shellcode/commands)


library injection (LD_PRELOAD)

hijacking library search path (LD_LIBRARY_PATH)


binary hijacking (PATH)


scripting language exploitation ()



---
ret = getenv("EGG"); --> whatever byte sequences lives inside the EGG environment variable wil be executed as code
ret();


solu: place shellcode into EGG
ensure it runs with preserved SUID privileges
use that shell to access the next level


