## Apache Kafka - Podstawy i Zastosowania
### Czym jest Apache Kafka?
Apache Kafka to platforma do przetwarzania strumieni danych (event streaming). Umożliwia przesyłanie i przetwarzanie dużych ilości danych w czasie rzeczywistym między różnymi systemami w sposób skalowalny i niezawodny.

Kafka działa jako broker wiadomości. Stawia na wysoką wydajność, trwałość danych i możliwość skalowania.

**Najważniejsze pojęcia**
- Topic (temat) – miejsce, gdzie wiadomości są zapisywane. Można to porównać do kanału, do którego nadawcy wysyłają dane, a odbiorcy je czytają.
- Partycja – każdy topic jest podzielony na partycje, które pozwalają na równoległe przetwarzanie i zwiększają wydajność.
- Producent (producer) – komponent, który wysyła wiadomości do Kafki.
- Konsument (consumer) – komponent, który odczytuje wiadomości z Kafki.
- Offset – unikalny numer porządkowy wiadomości w partycji, pozwalający śledzić, która wiadomość została już odczytana.

**Popularne zastosowania Kahki**
Wiadomości (messaging) – jako szybsza i bardziej odporna alternatywa dla tradycyjnych systemów kolejkowania.

Śledzenie aktywności na stronie – zbieranie i przetwarzanie w czasie rzeczywistym działań użytkowników.

Monitorowanie i metryki – przesyłanie danych operacyjnych do centralnego systemu nadzoru.

Logi – zbieranie dzienników zdarzeń z wielu serwerów w formie strumienia danych.

Przetwarzanie strumieniowe – analiza i transformacja danych w locie za pomocą Kafka Streams lub innych narzędzi.

**Kafka Streams**
Kafka Streams to biblioteka do przetwarzania i analizy danych zapisanych w Kafce, zapewniająca:

- Rozróżnienie czasu zdarzenia od czasu przetwarzania
- Obsługę okien czasowych do grupowania danych
- Dokładne przetwarzanie "exactly-once"
- Proste zarządzanie stanem aplikacji


Producenci wysyłają dane do topiców, konsumenci je odczytują, a system dba o ich trwałość i dostępność. Dzięki Kafka Streams można tworzyć aplikacje analizujące dane w czasie rzeczywistym.

## Co oznaczają kluczowe terminy: producent, konsument, partycja, broker

Producent (Producer) – to komponent lub aplikacja, która wysyła (publikuje) dane, czyli wiadomości do Kafki na określony topic (temat). Producent nie musi wiedzieć, kto i kiedy odbierze te dane. Jest odpowiedzialny za ich wysłanie do systemu.

Konsument (Consumer) – to komponent lub aplikacja, która odczytuje (subskrybuje) dane z Kafki, czyli pobiera wiadomości z wybranego topicu. Konsument analizuje lub przetwarza odebrane wiadomości. Może działać niezależnie od producenta.

Partycja (Partition) – każdy topic w Kafce jest podzielony na mniejsze części zwane partycjami. Partycja to uporządkowany, nierozłączny segment danych, który pozwala na równoległe i szybkie przetwarzanie. Każda partycja przechowuje wiadomości w kolejności i ma swój unikalny offset (numer kolejny).

Broker – to serwer lub węzeł w klastrze Kafki, odpowiedzialny za przyjmowanie wiadomości od producentów, przechowywanie ich i udostępnianie konsumentom. Broker zarządza partycjami i replikacją danych, zapewniając ich trwałość i dostępność.


## Przydatne komendy Linux przy pracy z Kafką

W pracy z Apache Kafka w środowisku Linux często przydadzą się podstawowe komendy do zarządzania plikami i procesami:

`ls` – wyświetla listę plików i folderów w katalogu, np. `ls -l` pokazuje szczegółowe informacje.

`cd` – zmiana katalogu, np. cd /ścieżka/do/katalogu.

`cat` – wyświetla zawartość pliku, np. cat kafka.log.

`tail` – pokazuje ostatnie linie pliku, np. tail -f kafka.log służy do śledzenia na bieżąco logów.

`ps` – pokazuje listę uruchomionych procesów, np. ps aux | grep kafka pozwala znaleźć procesy związane z Kafka.

`kill` – kończy proces, np. kill <pid> gdzie pid to numer procesu.

`grep` - przykłady filtrowania logów
Filtrowanie wpisów zawierających określoną datę (np. 2025-09-04):

```bash
grep "2025-09-04" kafka.log # Znajduje i wyświetla linie w pliku kafka.log zawierające datę 2025-09-04.
```
Filtrowanie pod kątem poziomu błędu, np. błędy ERROR:
```bash
grep "ERROR" kafka.log # Wyświetla linie z napisem ERROR.
```
*"|" to potok, łączy wyjście pierwszej komendy do wejścia następnej:*

Łączenie filtrów - wyszukanie błędów z danej daty:
```bash
grep "2025-09-04" kafka.log | grep "ERROR"
# Najpierw wybiera linie z datą, potem spośród nich tylko te z ERROR.
```
Zapis wyników do pliku:
```bash
grep "ERROR" kafka.log > error_logs.txt # Wyniki filtru zapisane do pliku error_logs.txt.
```
Wyszukiwanie z numeracją linii (-n):
```bash
grep -n "ERROR" kafka.log # Pokaże błędy razem z numerami linii.
```
--- 

**awk - przykłady filtrowania logów**
`awk` pozwala na selekcję kolumn lub wierszy na podstawie zawartości.

1. Załóżmy, że logi mają na początku linię z datą:

2025-09-04 12:34:56 INFO Kafka started
2025-09-04 12:35:00 ERROR Connection failed

2. Filtrowanie wpisów z określoną datą (pierwsza kolumna):
```bash
awk '$1 == "2025-09-04"' kafka.log # Wyświetla linie, gdzie pierwsza kolumna to 2025-09-04.
```
3. Filtrowanie wpisów z poziomem błędu `ERROR` (druga lub trzecia kolumna):
Załóżmy że poziom jest w 3-ciej kolumnie (po dacie i czasie):
```bash
awk '$3 == "ERROR"' kafka.log # Wyświetla tylko linie z poziomem ERROR.
```
4. Filtrowanie według daty i błędu razem:
```bash
awk '$1 == "2025-09-04" && $3 == "ERROR"' kafka.log # Wyświetla linie z datą i poziomem ERROR.
```
5. Zapisanie wyniku do pliku:
```bash
awk '$3 == "ERROR"' kafka.log > error_logs.txt
```
---
## Co to jest operator >

Operator to specjalny znak lub symbol w powłoce systemu Linux/Unix, który służy do sterowania przepływem danych między programami, plikami i urządzeniami. Pozwala modyfikować standardowe wejście i wyjście programów, np. przekierowywać wynik komendy do pliku, przekazywać dane z pliku do programu lub łączyć kilka poleceń.

Popularne operatory przekierowania i ich działanie

**Operator >**

Nazwa: Przekierowanie wyjścia (stdout)
Działanie: Przekierowuje wynik działania komendy do pliku. Zastępuje istniejącą zawartość pliku.	
Przykład:
```bash
grep ERROR kafka.log > errors.txt # zapisuje wynik do pliku, nadpisując go
```
**Operator >>**	
Nazwa: Dopisanie do pliku	
Działanie: Dopisuje wynik działania komendy do końca pliku, bez kasowania istniejącej zawartości.	
Przykład
```bash
echo "Nowy log" >> log.txt # dopisuje linię do pliku
```
**Operator <**	
Nazwa: Przekierowanie wejścia `(stdin)`	
Działanie: Pobiera dane do programu z pliku zamiast z klawiatury.	
Przykład
```bash
sort < dane.txt # sortuje zawartość pliku
```
**Operator `**	
Nazwa: Potok (pipe)	
Działanie: Łączy wyjście jednej komendy z wejściem drugiej, pozwalając na przetwarzanie strumieniowe.

**Operator 2>**	
Nazwa: Przekierowanie błędów (stderr)
Działanie: Przekierowuje komunikaty błędów komendy do pliku.	
Przykład:
```bash
cmd 2> error.txt - zapisuje błędy do pliku
```
**Operator 2>&1**
Nazwa:	Przekierowanie błędów i wyjścia	
Działanie: Przekierowuje błędy na standardowe wyjście, żeby oba strumienie trafiały do tego samego miejsca.	
```bash
cmd > out.txt 2>&1 # zapisuje wynik i błędy do tego samego pliku
```

**Przykłady użycia operatorów**
1. Zapisz wynik polecenia ls do pliku, nadpisując zawartość:
```bash
ls -l > lista_plikow.txt
```
2. Dopisz wynik polecenia echo do pliku log.txt:
```bash
echo "Nowy wpis w logu" >> log.txt
```
3. Posortuj zawartość pliku dane.txt:
```bash
sort < dane.txt
```
4. Przejrzyj procesy i wyświetl tylko te zawierające java:
```bash
ps aux | grep java
```
5. Zapisz standardowe wyjście i błędy z polecenia cmd do pliku:
```bash
cmd > wynik.txt 2>&1
```