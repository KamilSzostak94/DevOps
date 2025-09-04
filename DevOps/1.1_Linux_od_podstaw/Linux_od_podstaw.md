W systemie Linux zaimplementowano hierarchiczną strukturę plików. Każde pliki i katalogi znajdują się na tzw. “/”
```bash
/
├── bin
├── dev
├── etc
├── home
│   ├── bill
│   │   ├── books
│   │   └── school
│   └── patrick
│       └── school
├── tmp
└── usr
```

## Podstawowe polecenia w linux to 

- ls - polecenie służy do listowania bieżącego katalogu
- cd - polecenie służy do zmiany katalogu, wyjścia z katalogu

## Hierarchiczna struktura plików

- `bin` - Zawiera pliki binarne, wykonywalne. Znajdziemy tam polecenia systemowe np. “ls”. Katalog sbin są tam pliki gdzie może je wykonać root tylko
- `boot` - Zawiera niezbędne pliki do uruchomienia systemu
- `dev` - Zawiera urządzenia w postaci plików
- `etc` - Zawiera pliki konfiguracyjne i ustawienia systemowe
- `home` - Zawiera katalogi użytkowników
- `lib` - Zawiera biblioteki które wykorzystywane są na katalogach bin
- `mnt` - Zawiera do montowania dysków
- `opt` - Zawiera do instalacji aplikacji od osób 3
- `root` - Zawiera pliki użytkownika root
- `var` - pliki które często ulegają zmianie

## Podstawowe polecenia

- `touch <nazwa>` - Tworzenie nowego pliku, modyfikacja czasu innego pliku
- `cat <nazwa_pliku>` listowanie danego pliku - jeżeli chcemy  inaczej wyświetlić plik warto skorzystać z potoku “|” przekierowywanie wyjście innego procesu dla nowego procesu np. cat <nazwa_pliku> | more
- `mv <nazwa_stara> <nazwa_nowa>` - Zmiana nazwy pliku / katalogu
- `mkdir <nazwa_katalogu>` - Tworzenie katalogu
- `cp <nazwa_pliku> <nazwa_pliku_nowy>` - Kopiowanie pliku
- `rm` plik - Usunięcie pliku
- `rm -rf <nazwa_katalogu>/` - Usunięcie całego katalogu 

## Dodawanie / Usuwanie użytkownika

```bash
sudo useradd janek
sudo passwd janek
```
1. Tworzy użytkownika o nazwie janek
2. Tworzy hasło "janek"

```bash
sudo userdel janek
sudo userdel -r janek
```
1. Usuwa użytkownika janek
2. przełącznik `-r` usuwa również katalog domowy

## chmod, chown, chgrp

+---------+-------------------+------------------+
| Litera  | Znaczenie         | Parametr liczbowy|
+---------+-------------------+------------------+
| r       | prawo odczytu     | 4                |
| w       | prawo zapisu      | 2                |
| x       | prawo uruchomienia| 1                |
| -       | brak prawa        | 0                |
+---------+-------------------+------------------+

### 3 grupy praw
Do każdego pliku lub katalogu wyszczególniamy zestaw takich praw:

1. prawa właściciela
2. prawa grupy
3. prawa pozostałych użytkowników

```bash
-rw-r--r-- 1 kamil kamil  416 kwi 10 13:09 404.html
```
`rw-` w tym przypadku to prawa właściciela
`r--` prawa grupy
`r--` prawa pozostałych użytkowników

gdzie właścicielem oraz grupą jest kamil

+---------------+------------------+------------------+
| Prawa dostępu | Wartość liczbowa | Znaczenie        |
+---------------+------------------+------------------+
| -rw-------    |       600        | R, W`-O`         |
| -rw-r--r--    |       644        | R, W`-O` + R`-I` |
| -rw-rw-rw-    |       666        | R, W dla wszyst. |
| -rwx------    |       700        | RWX dla -O       |
| -rwxr-xr-x    |       755        | RWX `-O`; R,X`-I`| 
| -rwxrwxrwx    |       777        | Dalej wiadomo :) |
| -rwx--x--x    |       711        | Dalej wiadomo :) |
| drwx------    |       700        | Dalej wiadomo :) |
| drwxr-xr-x    |       755        | Dalej wiadomo :) |
| drwx--x--x    |       711        | Dalej wiadomo :) |
+---------------+------------------+------------------+
`R` - Odczyt
`W` - Zapis
`-O` - właściciel
`-I` - Pozostali

### Polecenie chmod
```bash
$ chmod prawa_dostępu nazwa_pliku

$ chmod u+x plik.sh    # dodaj wykonanie dla właściciela
$ chmod g-w plik.txt   # zabierz zapis grupie
$ chmod o=r plik.txt   # inni mogą tylko czytać
$ chmod a+x skrypt.sh  # wszyscy mogą wykonywać

$ chmod 777 plik.txt   # rwx rwx rwx – wszyscy mają pełne prawa
$ chmod 755 plik.sh    # rwx r-x r-x – właściciel pełne prawa, reszta tylko czyta i wykonuje
$ chmod 644 plik.txt   # rw- r-- r-- – właściciel czyta i pisze, reszta tylko czyta
$ chmod 600 haslo.txt  # rw- --- --- – tylko właściciel $ może czytać i pisać
$ chmod 400 tajne.txt  # r-- --- --- – tylko właściciel może czytać
```
### Typy plików
- `-` dla oznaczenia plików zwykłych
- `d` dla oznaczenia katalogów
- `c` dla oznaczenia plików specjalnych
- `b` dla oznaczenia plików specjalnych przypisanych
- `i` dla oznaczenia łączy symbolicznych

### Polecenie chown


1. Zobaczenie wszystkich grup:
```bash
cat /etc/group
```
2. Zobaczenie grup użytkownika:
```bash
groups janek
id janek
```
3. Utworzenie nowej grupy:
```bash
sudo groupadd developers
```
4. Dodanie użytkownika do grupy:
```bash
sudo usermod -aG developers janek
```
(-aG = dodaj do grupy dodatkowej, bez usuwania z innych)
5. Usunięcie grupy:
```bash
sudo groupdel developers
```
Składnia: chown [właściciel]:[grupa] plik

Zmienia właściciela pliku/katalogu.
```bash
sudo chown janek plik.txt   # zmiana właściciela na janek
sudo chown janek:users plik.txt  # zmiana właściciela i grupy
```

### Polecenie chgrp

Zmienia tylko grupę pliku/katalogu.

```bash
sudo chgrp developers plik.txt
```

## Unzip, zip

### unzip

unzip - służy do rozpakowywania plików .zip
```bash
unzip nazwa_paczki.zip #Rozpakowuje archiwum do bieżącego katalogu.

```
*Najczęściej używane opcje unzip*

`-l` – lista plików w archiwum (bez rozpakowywania)
```bash
unzip -l paczka.zip
```
`-v` – szczegółowa lista zawartości (rozmiary, daty, uprawnienia)
```bash
unzip -v paczka.zip
```
`-d <katalog>` – rozpakowanie do konkretnego katalogu
```bash
unzip paczka.zip -d /home/janek/dokumenty
```
`-n` – nie nadpisuje istniejących plików
```bash
unzip -n paczka.zip
```
`-o` – automatycznie nadpisuje istniejące pliki (bez pytania)
```bash
unzip -o paczka.zip
```
`-j` – rozpakowuje wszystko do jednego katalogu, ignorując strukturę folderów w archiwum
```bash
unzip -j paczka.zip -d /home/janek/test
```
`-q` – tryb cichy (bez wypisywania nazw plików podczas rozpakowywania)
```bash
unzip -q paczka.zip
```
`-t` – testowanie archiwum (sprawdza, czy pliki nie są uszkodzone)
```bash
unzip -t paczka.zip
```
### zip
Tworzenie nowego archiwum .zip:
```bash
zip archiwum.zip plik1.txt plik2.txt
```
Dodanie katalogu do archiwum (rekurencyjnie):
```bash
zip -r archiwum.zip katalog/
```
### zcat

Podgląd zawartości plików w archiwum .zip (bez rozpakowywania):
```bash
zcat plik.zip
```
Rozpakowanie do katalogu 
```bash
unzip projekty.zip -d ~/projekty
```
Dodanie katalog nowy/ do archiwum:
```bash
zip -r projekty.zip nowy/
```

### gzip

Służy do kompresji plików (ale nie tworzy archiwum wielu plików, tylko kompresuje pojedynczy plik → dlatego często używa się go razem z tar) domyślnie zamienia plik na jego skompresowaną wersję .gz

```bash
gzip plik.txt
```
usuwa plik.txt i tworzy plik.txt.gz

### gunzip

odwrotność gzip – rozpakowuje pliki .gz
```bash
gunzip plik.txt.gz
```
→ tworzy z powrotem plik.txt

*Różnica między gzip a tar*

gzip → kompresuje pojedyncze pliki (plik.txt → plik.txt.gz)
tar → tworzy archiwum wielu plików/katalogów (tar -cvf archiwum.tar katalog/)

razem:

tar -czvf archiwum.tar.gz katalog/
tar -xzvf archiwum.tar.gz

+-----------+--------------------------------------------+----------------------------------+
| Polecenie | Do czego służy                             | Przykład                         |
+-----------+--------------------------------------------+----------------------------------+
| zip       | Kompresja i archiwizacja (tworzy .zip)     | zip archiwum.zip plik1 plik2     |
| unzip     | Rozpakowanie archiwum .zip                 | unzip archiwum.zip               |
+-----------+--------------------------------------------+----------------------------------+
| gzip      | Kompresuje pojedynczy plik (.gz)           | gzip plik.txt   -> plik.txt.gz   |
| gunzip    | Dekompresuje plik .gz                      | gunzip plik.txt.gz               |
| zcat      | Podgląd pliku .gz bez rozpakowania         | zcat logi.gz                     |
| zgrep     | Szukanie w pliku .gz bez rozpakowania      | zgrep "hasło" logi.gz            |
+-----------+--------------------------------------------+----------------------------------+
| tar       | Archiwizacja wielu plików/katalogów        | tar -cvf arch.tar katalog/       |
|           | (często razem z gzip → .tar.gz)            | tar -czvf arch.tar.gz katalog/   |
|           | Rozpakowanie .tar lub .tar.gz              | tar -xvf arch.tar                |
|           |                                            | tar -xzvf arch.tar.gz            |
+-----------+--------------------------------------------+----------------------------------+
Legenda:  
-c  = create (twórz)  
-x  = extract (rozpakuj)  
-v  = verbose (wypisz nazwy plików)  
-f  = file (nazwa archiwum)  
-z  = użyj gzip przy tar
