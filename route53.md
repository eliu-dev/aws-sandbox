# Route 53

Route 53 is AWS's managed DNS service. It has two core capabilities: (1) domain registration and (2) host zone files on its name servers. It is a global service.

## Domain Registration
Zone files: Database that contains all DNS information for a particular domain. Route53 manages name servers and will typically distribute the zone file to four **hosted servers** (route53 terminology for its name servers).

The name servers exchange information with Top Level Domain servers to get name records added to those servers.

## Hosted Zones

Route53 provides DNS zones and hosting for the zones that contains the zone files. AWS refers to these hosted zone files as **hosted zones**. 

- Zone files are hosted on four managed name servers. 
- Hosted zones can be public or private. If it is private, it will be linked to a VPC.
- Hosted zones host records (known as **recordsets**)

## Nameserver Record Types

1. A **NS** record type allows delegation in DNS. It designates which server is delegated responsibility for information. The NS record is held on each DNS server type (root, TLD, authoritative, etc.) and supports DNS record traversal. At the authoritative server level, one authoritative server can delegate responsibility to another. For example, an `example.com` authoritative server can delegate `ns1.example.com` to another authoritative server.

Example: The .com TLD zone is managed exclusively by VeriSign (**registry**). A **registry** manages the authoritative list of TLDs while a **registrar** supports it by handling the purchase and sale of domains within the zone.
- The .com zone has multiple nameserver records for amazon.com servers.
- The nameservers in amazon.com's zone contain DNS records, such as www which provides access to those records.
- Similarly, the root zone delegates management of the .com zone by having nameserver records in the root zone that point to servers that host the .com zone

2. An **A record type** maps a host name to an IPv4 address

3. An **AAAA record type** maps a host name to an IPv6 address

Both A and AAAA records typically should be created. The client OS will then pick which one to use.

4. A **CNAME record type** (canonical zone) maps hosts to hosts. CNAME effectively allows you to create DNS shortcuts. 


Example (CNAME): A server has an A record and supports FTP, mail, and web services. 3 CNAME records are created for each (e.g., CNAME FTP, CNAME mail, CNAME www) and all point to the A name record. 
- This setup allows resolving all 3 CNAMEs to the same A record IPv4 address.
- If the IP address is updated (i.e., A record updated), all 3 CNAMEs will automatically point at the updated address.
- CNAMEs only point at other names (e.g., an A record). They cannot point directly at an IP address (*exam point*)

5. MX Record (Mail Exchange)
MX records are used for email addressing. Every record has (1) a **value** and (2) a **priority**. Lower values for the priority mean it is higher priority (i.e., 10 is higher priority than 20).

Example (inside `google.com` zone):
- An `A record` with the name `mail` (the name can be anything) that points to some IPv4 address 
- `MX 10 mail` (host)
- `MX 20 mail.other.domain.` (fully qualified domain name)

Two types of MX records:
- Host: if there are no `.`s to the right of the name, it refers to a host. For example a MX record named `mail` inside the `google.com` zone is assumed to be be in the same zone that it is stored in (i.e., `mail.google.com`). This approach is rare nowadays and is not considered best practice (independent research).
- Fully Qualified Domain Name: if there are `.`s to the right of the name, it is a fully qualified domain name. The record may point to the zone it is in or a different zone. For example, if `mail.other.domain.` is the record name, the record refers to the third-party domain. 

Email Example:
The email server examines the domain in the to line. For example, if an email server that sees `hi@google.com` the following will occur:
- The mail server will:parse out `google.com` and send a **MX query** to the google.com zone. 
- The DNS servers for the `google.com` zone will respond with the MX records. 
- The mail server will select the highest priority record and send a follow-up query to determine the IP address (an A Query or AAAA Query). If the MX record contains a CNAME, then a CNAME Query will be used instead.
- The DNS server will respond with the IP address.
- The mail server will connect to the IP address over SMTP and deliver the email.

6. TXT Record
Allows adding arbitrary text to a domain. A common use case is for proving domain ownership.

## Time-to-live (TTL) for DNS
The client gets an **authoritative answer** if it traverses the full hierarchy of DNS servers to reach the authoritative nameserver. The nameserver is the server that is authoritative for the domain; it will hold the relevant record. 

The client can get a cached **nonauthoritative answer** faster by retrieving a cached version from the resolver server. The record holder specifies the TTL for how long the answer will be cached on the resolver server.

Client
- Resolver Server
  - DNS Root server
    - DNS TLD server
      - DNS Authoritative Server


## DNS Background
When a client sends a DNS request, it first queries a recursive DNS resolver to traverse the potential DNS server resources. 

Client Query:
1. A client queries its configured recursive resolver.

2. Recursive Resolution: If the resolver lacks the answer in its cache, it queries the root nameserver.

3. Root Direction: The root nameserver directs the query to the appropriate TLD nameserver.

4. TLD Direction: The TLD nameserver points to the authoritative nameserver for the domain.

5. Authoritative Answer: The authoritative nameserver responds with the requested DNS record (e.g., an A or MX record).

6. Caching: The resolver caches the result for future queries.