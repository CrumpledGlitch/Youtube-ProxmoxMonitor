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
