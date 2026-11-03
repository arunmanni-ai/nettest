# nettest

Static page that checks whether HTTP and WebRTC reach the same remote host.

Usage: open `nettest.html?t=<ip-or-host>`

- CHECK 1 - plain HTTPS request to the target
- CHECK 2 - HTTPS request to a reference host, to confirm the network is up
- CHECK 3 - WebRTC (STUN/TURN) to the same target

Run a capture on the target to observe UDP 3478:

    sudo tcpdump -n -i any "udp port 3478 and host <client-ip>"
