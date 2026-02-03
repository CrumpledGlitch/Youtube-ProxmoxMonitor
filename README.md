# We  gave YouTube Live Chat full control over a VM via the Proxmox Monitor.


We have been tinkering with the Proxmox API with a friend and decided to see how far I could push a "remote control" concept. I built a Python-based bridge that monitors a YouTube Live chat feed and translates specific commands into real-time keystrokes inside a QEMU VM.

Can be seen here : https://www.youtube.com/watch?v=6-EH__859f0

How it works:

Listener: Uses a threaded pytchat loop to scrape the live feed for commands like !press win+r, !type, and !wait.

The Worker & Queue: To handle multiple users at once, I implemented a FIFO (First-In-First-Out) queue. This prevents the script from hanging when 20 people type at once.

Proxmox API: It uses the proxmoxer library to hit the /nodes/{node}/qemu/{vmid}/monitor endpoint, injecting the keys directly into the VM's hardware monitor.

Commands to try:

!press win+r

!type "notepad"

!press enter

I'd love to hear your thoughts on the implementation—especially if anyone has ideas on how to optimize the sendkey latency!



---------

⌨️ Basic Commands
!press [key] — Press a key or a combination.
Example: !press enter or !press ctrl+alt+del
!type "[text]" — Type words. Note: Use quotes for multiple words.
Example: !type "hello world"
!wait [seconds] — Pause between actions (Max 5s).
Example: !wait 2

🚀 Combo Moves (Copy & Paste)
Try these sequences to get things moving:
Open Notepad: !press win+r !wait 0.5 !type "notepad" !press enter
Open Task Manager: !press ctrl+shift+esc
Search Google: !press win !wait 0.5 !type "chrome" !press enter !wait 2 !type "how to hack a vm" !press enter

🛠 Supported Special Keys
You can use a-z and 0-9, plus these special names:
System: win, ctrl, alt, shift, esc, enter, backspace, tab, spc, del
Arrows: up, down, left, right
Function: f1 through f12
Navigation: pgup, pgdn, home, end
