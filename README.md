# NTLMRawUnhide.py #

**Author  :**  Mike Gualtieri  
**Blog    :**  https://www.mike-gualtieri.com  
**Twitter :**  https://twitter.com/mlgualtieri  
**GitHub  :**  https://github.com/mlgualtieri/NTLMRawUnhide  

## Intro ##
NTLMRawUnhide.py is a Python3 script designed to parse network packet capture files and extract NTLMv2 hashes in a crackable format.  The tool was developed to extract NTLMv2 hashes from files generated by native Windows binaries like NETSH.EXE and PKTMON.EXE without conversion.

The following binary network packet capture formats are supported: 
* *.pcap 
* *.pcapng 
* *.cap 
* *.etl


## Usage ##
```
Usage: NTLMRawUnhide.py -i <inputfile> [-o <outputfile>] [-f] [-h] [-q] [-v]
```
```
Main options:
  -f, --follow               Continuously "follow" (e.g. "read from")
                             input file for new data
  -h, --help
  -i, --input  <inputfile>   Binary packet data input file
                             (.pcap, .pcapng, .cap, .etl, others?)
  -o, --output <outputfile>  Output file to record any found NTLM
                             hashes
  -q, --quiet                Be a lot more quiet and only output
                             found NTLM hashes. --quiet will also
                             disable verbose, if specified.
  -v, --verbose
```

## Examples ##
Extract NTLMv2 hashes from examples/capture.pcap:
```
python3 NTLMRawUnhide.py -i examples/capture.pcap
```
Same, but with verbose output:
```
python3 NTLMRawUnhide.py -i examples/capture.pcap -v
```
Extract NTLMv2 hashes from examples/capture.pcap and continue to monitor the file for new hashes (like tail -f):
```
python3 NTLMRawUnhide.py -i examples/capture.pcap -f
```
Extract NTLMv2 hashes from examples/capture.pcap and write extracted hashes to /tmp/hashes.txt
```
python3 NTLMRawUnhide.py -i examples/capture.pcap -o /tmp/hashes.txt
```

## Packet Capture Methods ##

To create a compatible packet capture file, any of the following methods can be used:

* Wireshark: 
```
Set capture filter as "tcp port 445"; Save as .pcapng
```
* tcpdump
```
tcpdump -i eth0 -w capture.pcap "port 445"
```
* NETSH.EXE
```
netsh.exe trace start persistent=yes capture=yes TCP.AnyPort=445 tracefile=C:\Users\Public\capture.etl
netsh.exe trace stop
```

* PKTMON.EXE
```
pktmon.exe** filter add SMB -p 445
:: List all filters 
pktmon.exe filter list
:: Find id of the network adapter (example > Id: 9)
pktmon.exe comp list
:: pktmon.exe start --etw -p 0 -c [Adapter ID]     
pktmon.exe start --etw -p 0 -c 9 
:: Will create the file PktMon.etl in current directory
pktmon.exe stop
:: Cleanup
pktmon.exe filter remove
```


## References ##
The following URL was very helpful when building this tool:
	The NTLM Authentication Protocol and Security Support Provider
	http://davenport.sourceforge.net/ntlm.html

