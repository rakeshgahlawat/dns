# Domain Name System

DNS answer sequence - Host file > DNS cache > DNS server. Host file in Windows C:\Windows\System32\drivers\etc\hosts and to display DNS cache C:\>ipconfig /displaydns and to flush DNS resolver cache C:\>ipconfig /flushdns

Root Hints is information about Root DNS servers and is present in Windows Server at Administrative tools > DNS > DNS properties > Root Hints and same information is also present at systemroot [C:\ drive on Windows Server]\System32\dns\CACHE.DNS

Authoritative Server is the authority of its zone and Wireshark response will indicate as "Authority RRs: 1" | Non-authoritative Server does not contain original source files of domain's zone and and Wireshark response will indicate as "Authority RRs: 0"

Client to DNS server is Recursive query and is seen in Wireshark as DNS Flag "Recursion desired: Don't do query recursively set to 1. DNS server to all other DNS servers [Root, TLD or Domain Servers] is Iterative query and the process of these Iterative queries is known as Recursion.

The Internet Corporation for Assigned Names and Numbers ICANN | Internet Assigned Numbers Authority IANA

DNS Zones. Forward lookup zones are for Host names to IP address resolution and contain authoritative data. Reverse lookup zones are for IP address to Host name resolution, a use case is for SMTP.

Resource Records. A or AAAA is a Host record | PTR is Pointer record in reverse lookup zone that points to a Host record in forward lookup zone | CName is a canonical name record for alias hostname | SRV is a service locator record to determine the services a resource is running | MX is mail exchange record, denotes a mail server or exchange server | NAPTR is name authority pointer record | NS is a name server record and denotes that DNS is running on the server which is what makes it a name server | SOA state of authority record is the DNS server providing authoritative information.

Forward lookup zone -> host name to IP address mapping | Reverse lookup zone -> IP address to host name mapping | Network ID is 192.168.0 for a class C IP address 192.168.0.1 and Host ID would be 1

Primary DNS zones -> have authoritative zone information which contains read/write copy of zone data and accepts dynamic updates from clients (ddns) | Secondary DNS zone -> has a copy of the primary DNS zone, is the option that can be configured on clients and clients can go to it for DNS resolution when primary is unavailable. 

Zone transfers -> are used to copy DNS zone data from primary or master DNS server to secondary. First packet is over TCP port 53 during zone transfer and is an Asynchronous Full Transfer Zone AXFR which contains all the information. Second or subsequent packets are UDP port 53 [if update fits in the packet] and is a Incremental Zone Transfer IXFR which contains only the information that has updated.

Stub zone -> contains the list of authoritative DNS servers for a zone (domain) and host records that contain their IP addresses (known as glue records) | Cache only -> install DNS on this server and configure clients to send DNS requests to it, hence it builds its cache overtime and is flushed on reboot.

SOA record serial number increments after every change on primary/master DNS server and only then a zone transfer is initiated.

Forwarding is when a DNS server follows the recursion process to get an answer. Conditional forwarding is when a DNS server has the IP address of the DNS server that the client has requested the answer for. Disabling recursion shall cause the DNS server to not send any iterative queries and answer to the client will be host name not found.

DNS server looks up for answers in order of conditional forwarder > forwarder > recursion starting with root hints. Disabling recursion also disables forwarders.

DNS load balancing methods are netmask ordering and round robin. The netmask ordering feature is used to return addresses for type A DNS queries to prioritize local resources to the client, based on IP address of the client. The round robin feature is used to randomize the results for type A DNS queries to provide basic load-balancing functionality.

DNS server looks for answers in the order -> local cache > local zone > conditional forwarder > forwarder > root hints.

The utility nslookup can be run interactively and non interactively. To clear DNS cache on DNS server right click it and choose clear cache option. To debug logging on DNS server right click and choose properties > in properties enabled debug logging under the tab with same name.

When dealing with multiple Wireshark captures in order to understand the order of packets/events choose to view time column in UTC time zone for all wireshark capture files. Pings can be used as bookmarks in Wireshark capture files.
