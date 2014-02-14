openvpn-run
===========

A shell script that sets up an OpenVPN connection and runs a command over the
connection, with no effect on other processes.

The script connects to the free [VPNBook](http://www.vpnbook.com/freevpn) service as an example.

Technical details
-----------------
The script uses iptables and iproute2 to set up some custom routing rules, that
cause all network activity of a specific user ID to go through the VPN
connection, while other users are routed as before. The process is as follows:

* Every packet owned by the VPN UID is marked, and a routing policy DB rule (`ip rule`) is
  added to use a different routing table for marked packets.
* Every packet received from the VPN TUN interface is also marked, so that
  incoming packets are not dropped due to Reverse Path Filtering.
* The sysctl option `net.ipv4.conf.TUN_DEVICE.src_valid_mark` is enabled so that
  the marking of incoming packets is actually checked.

There are some configuration variables at the top of the script.

Dependencies
------------
* OpenVPN
* iptables utilities (iptables, iptables-save, iptables-restore)
* iproute2
* curl (for vpnbook)
* unzip (for vpnbook)
* [Expect](http://expect.sourceforge.net) is required if OpenVPN was compiled without `enable_password_save`. 
  Not required on Ubuntu.
