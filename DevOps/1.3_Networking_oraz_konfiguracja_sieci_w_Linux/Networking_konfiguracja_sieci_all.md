# Sieć i konfiguracja w Linux dla DevOpsa – najważniejsze informacje

## Typy sieci wirtualnych
- **NAT (Network Address Translation)**  
  Sieć izolowana od hosta. VM nie ma dostępu do hosta i odwrotnie.
- **Bridge**  
  VM podłączona bezpośrednio do sieci lokalnej, widoczna jak samodzielna maszyna w sieci.
- **Host-Only**  
  Komunikacja jedynie pomiędzy hostem a VM, bez dostępu do internetu.

---

## Podstawowe polecenia sieciowe
- `ifconfig` lub `ip a` / `ip address show` – pokazują konfigurację interfejsów sieciowych.
- `nmcli` – zarządzanie i status połączeń sieciowych.
- `ip route show` oraz `routel` – wyświetlenie tablicy routingu (trasy przesyłania pakietów).
- `traceroute <adres>` – śledzi trasę pakietów od hosta do celu.

---

## Adres IP, maska podsieci i CIDR
- Adres IPv4 składa się z 32 bitów (np. 192.168.1.10).
- Notacja CIDR `/n` mówi, ile bitów to część sieci (np. `/24` to 24 bity na sieć, czyli maska 255.255.255.0).
- Hostów w sieci: \(2^{(32-n)} - 2\) (odejmujemy adres sieci i broadcast).
- Przykład:  
  - Adres IP: 192.168.1.100/24  
  - Sieć: 192.168.1.0  
  - Broadcast: 192.168.1.255  
  - Hosty: 192.168.1.1 – 192.168.1.254 (254 adresy dostępne)

---

## Przykładowe maski i liczba hostów

| Notacja | Maska podsieci      | Liczba hostów| Typowa rola                  |
|---------|---------------------|--------------|------------------------------|
| /8      | 255.0.0.0           | ~16 mln      | Sieci bardzo duże (klasa A)  |
| /16     | 255.255.0.0         | ~65 tys.     | Sieci średnie (klasa B)      |
| /24     | 255.255.255.0       | 254          | Standard domowy/firmowy      |
| /30     | 255.255.255.252     | 2            | Połączenia punkt-punkt       |

---

## Narzędzia do diagnostyki i zarządzania siecią
- `nmcli` – zarządzanie siecią w konsoli.
- `ip a` / `ifconfig` – wyświetlanie interfejsów i adresów.
- `ip route show` / `routel` – tablica routingu.
- `tcpdump` – przechwytywanie ruchu sieciowego (np. `sudo tcpdump -i eth0 port 80`).
- `dig` – zapytania i diagnoza DNS (np. `dig -4 example.com`).
- `ping` – test łączności.
- `traceroute` – śledzenie trasy pakietów.
- `telnet` i `nc (netcat)` – testowanie portów i komunikacji.

---

## DNS i lokalne rozwiązywanie nazw
- DNS tłumaczy nazwy domenowe na adresy IP.
- Lokalna zamiana nazw przez plik `/etc/hosts`:
127.0.0.1 localhost
192.168.1.100 serwer.local

## NAT i port forwarding
- **NAT**: translacja adresu prywatnego na publiczny (np. router domowy).
- **Port forwarding**: przekierowanie ruchu z zewnętrznego portu na port w sieci wewnętrznej.
---

## Model OSI - uproszczony przegląd

| Warstwa | Funkcja               | Typowe protokoły            |
|---------|-----------------------|-----------------------------|
| 7       | Aplikacji             | HTTP, DNS, SMTP             |
| 6       | Prezentacji           | SSL, TLS, MIME              |
| 5       | Sesji                 | LDAP, RPC, SMB              |
| 4       | Transportu            | TCP, UDP                    |
| 3       | Sieci                 | IP, ICMP, RIP               |
| 2       | Łącza danych          | Ethernet, WiF               |
| 1       | Fizyczna transmisja   | Kable, fale radiowe         |

---

## TCP vs UDP
- **TCP**: połączeniowy, potwierdza dostarczenie danych.
- **UDP**: bezpołączeniowy, szybki, bez gwarancji.

---

## ARP (Address Resolution Protocol)
- Mapuje adres IP na MAC i odwrotnie.
- ARP Request – rozsyłany broadcast.
- ARP Reply – odpowiedź unicast.
- Tablica ARP ma ograniczoną pojemność.

---

## Firewall – podstawowe operacje
Narzędzia: `firewall-cmd` (firewalld), `ufw`, `iptables`.

Przykłady firewalld:

sudo firewall-cmd --state
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
sudo firewall-cmd --list-all

sudo ufw allow 22/tcp
sudo ufw deny 3306
sudo ufw enable
sudo ufw status

## SSH Forwarding (tunelowanie portów)

| Typ forwarding | Opis                                                     | Przykład użycia                              |
|----------------|----------------------------------------------------------|----------------------------------------------|
| Local (-L)     | Przekierowanie portu lokalnego do portu na serwerze zdalnym | `ssh -L 15432:localhost:5432 user@server`    |
| Remote (-R)    | Udostępnienie portu lokalnego na serwerze zdalnym       | `ssh -R 9000:localhost:3000 user@server`     |
| Dynamic (-D)   | Utworzenie lokalnego serwera proxy SOCKS5                | bezpieczne przeglądanie przez serwer zdalny |

Przykład Local Forwarding pozwala na dostęp do bazy danych PostgreSQL działającej na zdalnym serwerze przez lokalny port.

---

## Docker Networking
- Domyślna sieć to mostek (bridge).
- Port mapping przypisuje port kontenera do portu hosta (np. `-p 80:80`).
- Można tworzyć własne sieci wewnątrzkontenrowe.

## Sieć Web - kluczowe koncepcje

### Domeny oraz DNS

Domain Name System (DNS) - Jest to jak książka adresowa która tłumaczy nam adresy stron internetowych z adresu numeru IP na nazwy czytelne np.

google[.]pl - 142.250.203.131

Polecenie ping jest zawodnym poleceniem ze względu na zablokowanego protokoły. On może działać lecz nie odpowiadać na pinga.

### Kluczowe protokoły

1. **HTTP / HTTPS**
- protokół sieci www
- komunikacja klienta (przeglądarki) z serwerem
- wersje HTTP/1.1, HTTP/2
- HTTPS: szyfrowana wersja protokołu
2. **FTP**
- Dwukierunkowy transfer plików
- Tradycyjnie pełnił ważną rolę podczas wdrażania aplikacji
- Zazwyczaj potrzebne jest uwierzytelnienie użytkownika
3. **SSH - Secure Shell**
- Służy do łączenia ze zdalnym systemem
- W praktyce potrzebny do administrowania serwerem
- Transmisja jest szyfrowana

### Porty protokołu

Port protokołu (sieciowy) - używany do identyfikacji procesów na zdalnych systemach.

**Przykładowe porty**

| HTTP  | 80 |
| HTTPS | 443 |
| FTP   | 21 |
| SSH   | 22 |

## 3. Praca nad serwerem

**Typy serwerów:**

- Serwer aplikacji
- Serwer bazy danych
- Serwer do przechowywania

**Serwer a hosting**

**Hosting współdzielony**

- Najprostsza opcja
- Wiele aplikacji na jednej fizycznej maszynie
- Konieczność dzielenia zasobów na wiele kont
- Narzucone z góry technologie
- Relatywnie niska wydajność
- Ograniczenia administracyjne

**Serwer dedykowany**

- Kompletna maszyna do twojej dyspozycji
- Dowolna konfiguracja
- Duża wydajność
- Konieczność budowania od zera
- Wysoki koszt

**VPS - Wirtualny serwer prywatny**

- Wydzielone środowiska w jednej maszynie
- Dowolność konfiguracji
- Duża dostępność niedrogich ofert

### Dodajemy statyczne IP

**Prywatny adres IP** - Służy do wewnętrznej komunikacji między instancjami

**Publiczny adres IP** - Dostępny adres dla różnych klientów. 

**Statyczny adres IP** - Stały adres IP, który jest raz definiowane i się nie zmienia. Możemy je przepinać na różne instancje. 


### **Logujemy się za pomocą skróconej nazwy**

**Jeśli masz wpis np. `lab1` w pliku konfiguracyjnym SSH** — czyli `~/.ssh/config`.

**Krok po kroku — jak to zrobić**

1. **Stwórz (lub edytuj) plik `~/.ssh/config`**

```bash
vim ~/.ssh/config
```

2. **Dodaj wpis:**

```bash
Host lab1
    HostName 192.168.1.100  # lub domena, np. lab1.mojserwer.pl
    User kamil              # twoja nazwa użytkownika na serwerze
    IdentityFile ~/.ssh/id_rsa
```

Wyjaśnienie: 
> - `Host lab1` — to właśnie skrót, którego później użyjesz (`ssh lab1`)
> - `HostName` — IP lub domena serwera
> - `User` — użytkownik na serwerze
> - `IdentityFile` — klucz prywatny (zwykle `~/.ssh/id_rsa`)

### Blokujemy dostęp za pomocą hasła

```bash
sudo vim /etc/ssh/sshd_config
```

