- CPU receive power good signal from power supply
- CPU look at ffff0h to find out the real address of BIOS
http://flint.cs.yale.edu/feng/cos/resources/BIOS/

- BIOS performs POST(power on self test)
  - Once it fails, they produce beep sound to tell you the reason of problem

## Stage 1
- bios look for boot sector inside mbr whichever it finds first
- boot sector contains an address of (a portion of) grub that contains, because 446 byte of sector 0 isn't enough to contain any knowledgeable code

## Stage 1.5
- the address is pointing at between sector 1~63, which is a /boot section that contains file system driver

## Stage 2
- locate and load kernel, which is located at /boot, and handover the control to kernel
- kernel is saved as a self-extracting format, loads systemd

# References
- Linux boot explained: https://opensource.com/article/17/2/linux-boot-and-startup
- A slightly simpler one: https://www.geeksforgeeks.org/what-happens-when-we-turn-on-computer/
- Powering up a computer: http://flint.cs.yale.edu/feng/cos/resources/BIOS/
- MBR: http://www.invoke-ir.com/2015/05/ontheforensictrail-part2.html
- Systemd service file explained: https://www.shellhacks.com/systemd-service-file-example/
- /etc/systemd vs /usr/lib/systemd: https://unix.stackexchange.com/questions/206315/whats-the-difference-between-usr-lib-systemd-system-and-etc-systemd-system
- Boot to rescue/emergency mode: https://www.linuxtechi.com/boot-ubuntu-20-04-rescue-emergency-mode/
- Where the BIOS is stored: 
https://en.m.wikipedia.org/wiki/BIOS#:~:text=Originally%2C%20BIOS%20firmware%20was%20stored,the%20chip%20from%20the%20motherboard.