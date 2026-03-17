- why UEFI bios is so writable? 
- why UEFI bios has lots of free space for the bootkits to take over and live in? 
	- allow for multibooting different OSes? 
	- has to be accessible by each OS, each OS has its own files that instructs the BIOS how to boot the OS
	- what secure boot is supposed to address


UEFI implant vs Bootkit vs rootkit? 

- uefi implant: malware infectes the UEF the modern successor to BIOS . runs before the OS loads, allowing attackers to maintain persistence, compromise OS integrity, evade most security controls
- bootkit: infect bootloader or boot process, executing malicious code before the OS initializes. target the pre-boot environment, bypass standard security measures and remain hiddne, often surviving OS reinstallations 
- rootkit provides continued privileged access to a computer while actively hiding its presence; allows an attacker to maintain command and ctronl over a  system wthout the owner's knowledge, enabling remote file execution, system config changes, spying on user activities




