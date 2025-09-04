# Monitoring i porty

## nmap – skanowanie portów
sudo nmap -sV 192.168.1.10
sudo nmap -O 192.168.1.10
sudo nmap -p 22 192.168.1.10
sudo nmap 192.168.1.0/24

## netstat – nasłuchujące porty
netstat -plnt
# -p = PID/proces
# -l = nasłuch
# -n = numeryczne adresy
# -t = TCP

## Przykład
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1234/sshd
tcp        0      0 127.0.0.1:5432          0.0.0.0:*               LISTEN      2345/postgres
tcp6       0      0 :::80                   :::*                    LISTEN      3456/nginx