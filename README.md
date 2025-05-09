# Lab: Podział Sieci (Subnetting) na 8 Podsieci w Cisco Packet Tracer

**Cel Laba:**

Celem tego laba jest praktyczne przećwiczenie podziału większej sieci IP na 8 mniejszych podsieci oraz skonfigurowanie prostego środowiska sieciowego w programie Cisco Packet Tracer, wykorzystującego te podsieci do zapewnienia komunikacji między różnymi segmentami sieci.

**Scenariusz Zadania:**

Dysponujemy siecią w adresacji **140.55.0.0/16**. Należy ją podzielić na **8 równych podsieci**. Następnie, na podstawie obliczeń, skonfigurować sieć w programie Cisco Packet Tracer, przypisując odpowiednie adresy IP urządzeniom w każdej podsieci oraz konfigurując routing, aby umożliwić komunikację między podsieciami.

**Obliczenia Podziału Sieci (Subnetting):**

Aby podzielić sieć /16 na 8 podsieci, potrzebujemy pożyczyć bity z części hosta adresu IP. Liczba podsieci (N) jest związana z liczbą pożyczonych bitów (n) wzorem $N = 2^n$. W naszym przypadku $8 = 2^3$, więc potrzebujemy pożyczyć **3 bity** z części hosta.

Oryginalna maska /16 to `255.255.0.0` (binarnie `11111111.11111111.00000000.00000000`).
Pożyczając 3 bity z trzeciego oktetu, nowa maska będzie miała 16 + 3 = 19 bitów na część sieciową.
Nowa maska podsieci: `255.255.224.0` (binarnie `11111111.11111111.**111**00000.00000000`).

Rozmiar bloku adresowego (interwał) w trzecim oktecie wynosi $2^{(8-3)} = 2^5 = 32$.

**Wyniki Obliczeń dla 8 Podsieci:**

Poniższa tabela przedstawia kluczowe zakresy adresów dla każdej z 8 wyznaczonych podsieci:

| Podsieć | Adres Sieci     | Maska Podsieci  | Pierwszy Użyteczny Adres IP | Ostatni Użyteczny Adres IP | Adres Rozgłoszeniowy (Broadcast) |
| :------ | :-------------- | :-------------- | :------------------------- | :------------------------ | :----------------------------- |
| 1       | 140.55.0.0      | 255.255.224.0   | 140.55.0.1                 | 140.55.31.254             | 140.55.31.255                  |
| 2       | 140.55.32.0     | 255.255.224.0   | 140.55.32.1                | 140.55.63.254             | 140.55.63.255                  |
| 3       | 140.55.64.0     | 255.255.224.0   | 140.55.64.1                | 140.55.95.254             | 140.55.95.255                  |
| 4       | 140.55.96.0     | 255.255.224.0   | 140.55.96.1                | 140.55.127.254            | 140.55.127.255                 |
| 5       | 140.55.128.0    | 255.255.224.0   | 140.55.128.1               | 140.55.159.254            | 140.55.159.255                 |
| 6       | 140.55.160.0    | 255.255.224.0   | 140.55.160.1               | 140.55.191.254            | 140.55.191.255                 |
| 7       | 140.55.192.0    | 255.255.224.0   | 140.55.192.1               | 140.55.223.254            | 140.55.223.255                 |
| 8       | 140.55.224.0    | 255.255.224.0   | 140.55.224.1               | 140.55.255.254            | 140.55.255.255                 |

**Topologia Sieci w Cisco Packet Tracer:**

Poniższy zrzut ekranu przedstawia zaimplementowaną topologię sieci w programie Cisco Packet Tracer, odzwierciedlającą podział na 8 podsieci. Router pełni rolę centralnego punktu routingowego, łączącego wszystkie podsieci.

![Topologia Laba Subnetting 8 Podsieci](images/topologia_subnetting.png)

*(Pamiętaj, aby nazwać plik ze screenem `topologia_subnetting.png` i umieścić go w folderze `images/` w repozytorium)*

**Implementacja i Konfiguracja:**

Konfiguracja laba polegała na ustawieniu adresacji IP na interfejsach routera oraz na komputerach w każdej z podsieci, zgodnie z wynikami obliczeń. Router, jako punkt styku każdej podsieci, został skonfigurowany z pierwszym użytecznym adresem IP (np. 140.55.0.1 dla Podsieci 1, 140.55.32.1 dla Podsieci 2 itd.) oraz odpowiednią maską podsieci 255.255.224.0.

Komputery PC w każdej podsieci zostały skonfigurowane z adresami IP z zakresu użytecznych adresów danej podsieci, odpowiednią maską podsieci oraz adresem bramki domyślnej wskazującym na adres IP interfejsu routera w tej podsieci.

**Dlaczego adresacja hostów zaczyna się od .3 lub .4?**

W praktyce administracji sieciami często unika się przypisywania pierwszym kilku użytecznym adresom IP (.1, .2) na stacjach roboczych użytkowników.

* Adres **.1** jest niemal zawsze standardowo rezerwowany dla **bramki domyślnej (routera)** w danej podsieci.
* Adres **.2** bywa często rezerwowany dla **serwerów** (np. serwera DNS, małego serwera plików, drukarki sieciowej) w danej podsieci, aby miały łatwo zapamiętywalne adresy na początku zakresu.
* Adresy **sieci** (np. 140.55.0.0) i **rozgłoszeniowe** (broadcast, np. 140.55.31.255) są z definicji **nieużyteczne** dla hostów.

Rozpoczęcie przypisywania adresów IP stacjom roboczym od **.3, .4** lub nawet wyższego numeru jest zatem **dobrą praktyką administracyjną**. Zapewnia to, że krytyczne usługi sieciowe (bramka, potencjalne serwery) mają swoje zarezerwowane, niskie adresy, a adresy stacji roboczych zaczynają się nieco dalej w puli użytecznych adresów. Ułatwia to planowanie i zarządzanie adresacją w sieci.

**Weryfikacja Działania:**

Po skonfigurowaniu wszystkich urządzeń przeprowadzono testy łączności za pomocą komendy `ping` między komputerami znajdującymi się w **różnych** podsieciach (np. PC z Podsieci 1 pingujący PC w Podsieci 8). Poniżej przykładowe wyniki testów (`ping` z podsieci 8 do podsieci 3):

```cli
C:\>ping 140.55.64.3

Pinging 140.55.64.3 with 32 bytes of data:

Request timed out.
Reply from 140.55.64.3: bytes=32 time=7ms TTL=127
Reply from 140.55.64.3: bytes=32 time<1ms TTL=127
Reply from 140.55.64.3: bytes=32 time<1ms TTL=127

Ping statistics for 140.55.64.3:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 7ms, Average = 2ms

C:\>ping 140.55.64.3

Pinging 140.55.64.3 with 32 bytes of data:

Reply from 140.55.64.3: bytes=32 time<1ms TTL=127
Reply from 140.55.64.3: bytes=32 time<1ms TTL=127
Reply from 140.55.64.3: bytes=32 time<1ms TTL=127
Reply from 140.55.64.3: bytes=32 time<1ms TTL=127

Ping statistics for 140.55.64.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
