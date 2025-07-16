# Wireshark Display Filters

This repository is a collection of useful Wireshark display filters to help you navigate the vast amounts of network traffic generated.

A Wireshark filter is a rule used to select specific packets for display in the Wireshark packet capture window. You can use filters to narrow down traffic based on IP address, port number, protocol, and many other properties. This is an essential skill for anyone involved in network troubleshooting, analysis, or security.

Wireshark filters are written in a special language and can be entered directly into the filter bar at the top of the Wireshark window. Let's dive in and make your packet analysis more efficient!

## The Filters

Here is a list of handy filters to get you started. They are organized by category to make them easier to find.

### Basic Protocol Filters

These are your go-to filters for isolating common types of traffic.

* `ip`: Shows all IP packets.
* `tcp`: Displays all TCP packets.
* `udp`: Displays all UDP packets.
* `http`: Shows all HTTP packets.
* `dns`: Displays all DNS packets.
* `arp`: Shows all ARP packets.
* `icmp`: Displays all ICMP (ping) packets.
* `smtp`: Shows all SMTP (email) packets.

### Address and Port Filters

Need to track down a specific device or service? These filters are for you.

* `ip.addr == 192.168.1.100`: Shows all packets with 192.168.1.100 as the source or destination IP.
* `ip.src == 192.168.1.100`: Only shows packets where 192.168.1.100 is the source.
* `ip.dst == 192.168.1.100`: Only shows packets where 192.168.1.100 is the destination.
* `ip.addr == 192.168.1.0/24`: Displays all traffic on the 192.168.1.0/24 subnet.
* `tcp.port == 80`: Shows all TCP packets with a source or destination port of 80 (HTTP).
* `udp.port == 53`: Displays all UDP packets with a source or destination port of 53 (DNS).
* `tcp.port in {80, 443, 8000..8005}`: Looks for TCP traffic on a range of ports.

### Layer 2 Filters (Ethernet)

Sometimes you need to get down to the data link layer.

* `eth.type == 0x88cc`: Filters for Link Layer Discovery Protocol (LLDP) packets.
* `eth.addr == 01:80:c2:00:00:0e`: Also a filter for LLDP.
* `eth.addr == 01:00:0c:cc:cc:cc`: Filters for Cisco Discovery Protocol (CDP) packets.
* `cdp.version == 2`: Shows only CDP version 2 packets.

### TCP Flag Filters

These are fantastic for troubleshooting TCP connection issues.

* `tcp.flags.syn == 1`: Displays all TCP SYN packets, often used to initiate connections.
* `tcp.flags.ack == 1`: Shows all TCP ACK packets.
* `tcp.flags.reset == 1`: Displays TCP packets with the RST flag set, which can indicate a connection being forcibly closed.
* `tcp.flags.fin == 1`: Shows TCP packets with the FIN flag set, used to gracefully close a connection.

### Advanced and Troubleshooting Filters

Ready to level up? These filters can help you spot more subtle issues.

* `!(arp or stp or lldp or cdp or eth.addr==ff:ff:ff:ff:ff:ff)`: A great way to filter out some of the "chatty" network noise.
* `http.time > 1`: Finds HTTP responses that took longer than one second.
* `dns.time > 0.2`: Shows DNS queries that took longer than 0.2 seconds to resolve.
* `tcp.analysis.retransmission`: Shows all TCP retransmissions, which can be a sign of packet loss.
* `tcp.analysis.duplicate_ack`: Displays duplicate acknowledgments, another indicator of potential packet loss.
* `(tcp.analysis.flags) && !(tcp.analysis.window_update)`: This can help you spot "bad TCP" flags without the noise of window updates.
* `http.request.method == "GET"`: Shows all HTTP GET requests.
* `http.response.code == 200`: Displays all HTTP responses with a 200 OK status code.
* `tcp.payload contains "hello world"`: Searches for a specific string within the TCP payload.
* `eth matches "London"`: A case-insensitive search for the string "London" in the Ethernet layer.

### Logical Operators

Combine filters to create powerful and specific queries!

* `&&` (and): Combines two filters. Example: `ip.addr == 192.168.1.100 && tcp.port == 443`
* `||` (or): Matches one filter or another. Example: `tcp.port == 80 || tcp.port == 443`
* `!` (not): Excludes traffic. Example: `!arp`

You can also use parentheses to group expressions for more complex filtering. For instance, `(tcp.flags.syn == 1) && (ip.addr == 192.168.1.100)` will show all SYN packets to or from that specific IP address.
