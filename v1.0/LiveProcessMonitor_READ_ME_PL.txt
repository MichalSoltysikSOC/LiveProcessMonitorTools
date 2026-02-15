Nazwa narzędzia: LiveProcessMonitor.exe

Wersja: 1.0
Suma kontrolna SHA256: 37C53E8DC2F940F137A6B9F50855B81F3C37844EF958255FBF660D0742CF8CD1
Rozmiar pliku: 79,0 KB
Napisane w języku C#, z przeznaczeniem dla platformy .NET Framework.
Skompilowane jako aplikacja Windows GUI w postaci pliku wykonywalnego .exe z nagłówkiem pliku MZ.

Autor: Michał Sołtysik
Cybersecurity Analyst & Consultant | Forensics Examiner | SOC Trainer | Cyber Warfare Organizer
Analityk i Konsultant ds. Bezpieczeństwa Cybernetycznego | Ekspert ds. Informatyki Śledczej | Trener SOC (Centrum Operacji Bezpieczeństwa Cybernetycznego) | Organizator Działań w Cyberwojnie
Oficjalna strona: https://michalsoltysik.com/
LinkedIn: https://www.linkedin.com/in/michal-soltysik-ssh-soc/
Treści z zakresu cyberbezpieczeństwa: https://www.youtube.com/playlist?list=PL0RdRWQWldOAAKBqOVEutxKMP-a6CNoLY
Accredible: https://www.credential.net/profile/michalsoltysik/wallet
Credly: https://www.credly.com/users/michal-soltysik
Email: me@michalsoltysik.com

Cel: Live Live Process Monitor to narzędzie typu GUI dla systemu Windows, przeznaczone do monitorowania procesów oraz aktywności sieciowej punktu końcowego w oparciu o model baseline-driven. Umożliwia ono utworzenie stanu bazowego (baseline) aktualnie uruchomionych procesów wraz z ich aktywnością sieciową, a następnie przejście do ciągłego monitorowania po utworzeniu baseline w celu wykrywania nowych procesów, procesów zakończonych oraz zmian w połączeniach sieciowych TCP i UDP. W przeciwieństwie do narzędzi prezentujących jedynie chwilowy, bieżący stan systemu, Live Process Monitor nie usuwa obserwacji zebranych po utworzeniu baseline. Cała aktywność procesów i sieci po baseline jest stale gromadzona, zachowywana i prezentowana w formie historii audytowej, co pozwala na analizę pełnej sekwencji zdarzeń bez utraty kontekstu. Narzędzie koreluje metadane procesów, linie poleceń, hashe plików wykonywalnych oraz aktywne punkty końcowe sieci w jednym, spójnym widoku, wspierając reagowanie na incydenty, analizę malware oraz bieżącą triage punktu końcowego.
Licencja: Darmowa do użytku prywatnego i komercyjnego.

Zasady projektowe:

(1) Podejście baseline-first, w którym cała aktywność zaobserwowana podczas tworzenia baseline jest traktowana jako zaufany stan referencyjny.
(2) Monitorowanie po baseline, które wyróżnia wyłącznie nowe lub zmienione procesy, przy jednoczesnym zachowaniu pełnej widoczności historycznej.
(3) Retencja dowodowa oparta na sesji - procesy i połączenia pozostają widoczne nawet po zakończeniu, co umożliwia analizę śledczą.

Model monitorowania oparty na baseline:

(1) Narzędzie działa w dwóch wyraźnie oddzielonych fazach: tworzenia baseline oraz monitorowania po baseline.
(2) Dane baseline reprezentują początkowy, znany stan systemu i są wizualnie oddzielone od aktywności po baseline.
(3) Monitorowanie po baseline wyróżnia wyłącznie procesy i połączenia sieciowe zaobserwowane po zakończeniu tworzenia baseline.
(4) Taka architektura umożliwia szybką, wizualną identyfikację podejrzanej lub nieoczekiwanej aktywności bez konieczności wcześniejszej znajomości systemu.
(5) Wszystkie dane istnieją wyłącznie w pamięci bieżącej sesji, o ile użytkownik nie zdecyduje się ich jawnie wyeksportować.

Co robi aplikacja:

(1) Tworzy migawkę baseline uruchomionych procesów z wykorzystaniem natywnych mechanizmów systemu Windows (WMI oraz API systemowe).
(2) Zbiera szczegółowe metadane procesów, w tym relacje rodzic-dziecko, ścieżki do plików wykonywalnych, linie poleceń oraz hashe SHA-256.
(3) Monitoruje cykl życia procesów w czasie rzeczywistym, obejmujący uruchomienie i zakończenie procesu.
(4) Enumeruje aktywne punkty końcowe TCP i UDP oraz koreluje je z procesami właścicielskimi na podstawie PID.
(5) Śledzi historię połączeń sieciowych dla każdego procesu, w tym czas pierwszego zaobserwowania, ostatniego wystąpienia oraz moment zakończenia połączeń TCP.
(6) Opcjonalnie wykonuje odwrotne rozwiązywanie DNS dla publicznych adresów IP zdalnych hostów, dostarczając podstawowego kontekstu.

Dane zbierane dla każdego procesu:

- PPID (identyfikator procesu nadrzędnego)					- PPID (parent process ID)
- PID (identyfikator procesu)							- PID (process ID)
- Nazwa procesu (znormalizowana z sufiksem .exe)				- Process name (normalized with .exe suffix)
- Pełna linia poleceń procesu (znormalizowana i, gdy to możliwe, rozwinięta)	- Full process command line (normalized and expanded when possible)
- Ścieżka do pliku wykonywalnego						- Executable file path (when available)
- Hash SHA-256 pliku wykonywalnego na dysku					- SHA-256 hash of the executable file on disk (when accessible)
- Stan procesu (uruchomiony lub zakończony)					- Process state (Running or No longer running)
- Znacznik czasu Win32 First Seen (pierwsza obserwacja)				- Win32 First Seen timestamp (first observed by the tool)
- Znacznik czasu Win32 Last Seen (ostatnia obserwacja)				- Win32 Last Seen timestamp (last observed by the tool)

Dane zbierane dla każdego połączenia sieciowego:

- Połączenia TCP

  * Stan połączenia (np. nawiązane, nasłuchujące, oczekiwanie, zakończone)	* Connection state (for example: ESTABLISHED, LISTENING, TIME_WAIT, ENDED)
  * Czas rozpoczęcia połączenia							* Connection start time (first observed by the tool)
  * Czas zakończenia połączenia							* Connection end time (set when the connection is no longer observed)
  * Lokalny adres IP i port źródłowy						* Local IP address and source port
  * Zdalny adres IP i port docelowy						* Remote IP address and destination port
  * Protokół transportowy: TCP							* Transport protocol (TCP)

- Punkty końcowe UDP

  * Stan zawsze wyświetlany jako bezpołączeniowy				* Connection state always shown as CONNECTIONLESS
  * Lokalny adres IP i port źródłowy						* Local IP address and source port only
  * Zdalny adres IP i port docelowy - niedostępne z założenia			* Remote IP address and destination port are intentionally not available for UDP
  * Protokół transportowy: UDP							* Transport protocol (UDP)

Uwaga: UDP jest protokołem bezpołączeniowym. System operacyjny nie udostępnia informacji o zdalnym punkcie końcowym, dlatego dane te nie są wyświetlane.

Interfejs użytkownika:

Aplikacja prezentuje dynamicznie aktualizowaną tabelę zawierającą:

- Wiersze procesów (jeden wiersz na proces)					- Process rows (one row per process)
- Wiersze połączeń zagnieżdżone bezpośrednio pod procesem właścicielskim	- Connection rows nested directly under the owning process

Tabela zawiera następujące kolumny:

- PPID (identyfikator procesu nadrzędnego)			- PPID
- PID (identyfikator procesu)					- PID
- Nazwę procesu							- Process Name
- Linię poleceń procesu						- Process Command Line
- Ścieżkę procesu						- Process Path
- Hash SHA-256 pliku wykonywalnego na dysku			- Process SHA-256 Hash
- Stan procesu							- Process State
- Znacznik czasu Win32 First Seen (pierwsza obserwacja)		- Win32 First Seen
- Znacznik czasu Win32 Last Seen (ostatnia obserwacja)		- Win32 Last Seen
- Stan połączenia						- Connection State
- Czas rozpoczęcia połączenia					- Connection Start Time
- Czas zakończenia połączenia					- Connection End Time
- Adres IP							- IP Address
- Odwrotne rozwiązywanie DNS					- Reverse DNS
- Port źródłowy							- Source Port
- Port docelowy							- Destination Port
- Protokół transportowy						- Transport Protocol

Uwaga: wiersze połączeń są oznaczone wizualnie jako: " +- [połączenie / connection]".

Semantyka kolorów:

- Jasnozielony - proces baseline nadal uruchomiony		- Light green - baseline process that is still running
- Jasnoniebieski - proces baseline zakończony			- Light blue - baseline process that is no longer running
- Jasnoczerwony - proces po baseline nadal uruchomiony		- Light red - post-baseline process that is still running
- Jasnoróżowy - proces po baseline zakończony			- Light pink - post-baseline process that is no longer running
- Jasnofioletowy - PID 4 (System)				- Light purple - PID 4 (System)

Wiersze połączeń dziedziczą kolor procesu, do którego należą. Stan połączenia (np. TCP zakończone / ENDED) jest widoczny wyłącznie w kolumnach opisowych, a nie poprzez zmianę koloru.

Przepływ operacyjny i kontrolki:

(1) Rozpocznij monitorowanie / Start Monitoring:

- Zbiera pełną migawkę baseline uruchomionych procesów oraz ich aktywnych połączeń sieciowych.
- Oznacza wszystkie zaobserwowane procesy jako baseline.

(2) Wznów monitorowanie po utworzeniu baseline / Resume Monitoring After Baseline Creation:

- Uruchamia ciągłe monitorowanie w czasie rzeczywistym.
- Nowe procesy i połączenia zaobserwowane od tego momentu są klasyfikowane jako aktywność po baseline.
- Umożliwia natychmiastowe wykrywanie zmian względem baseline.

(3) Wstrzymaj monitorowanie / Pause Monitoring:

- Tymczasowo wstrzymuje całą aktywność monitorowania.
- Zachowuje wszystkie dotychczas zebrane dane.
- Pozwala na przegląd lub eksport bez utraty stanu.

(4) Wznów monitorowanie / Resume Monitoring:

- Wznawia monitorowanie dokładnie z miejsca, w którym zostało zatrzymane (pauza).
- Jeżeli tworzenie baseline nie zostało w pełni zakończone, aplikacja najpierw je kontynuuje, a dopiero potem przechodzi do monitorowania po baseline.

(5) Zatrzymaj monitorowanie / Stop Monitoring:

- Całkowicie zatrzymuje monitorowanie.
- Wyłącza wszystkie kontrolki związane z monitorowaniem.
- Dostępna pozostaje wyłącznie opcja Zapisz wyniki / Save The Results.

(6) Zapisz wyniki / Save The Results:

- Eksportuje całą tabelę do pliku CSV na żądanie.
- Zawiera zarówno wiersze procesów, jak i zagnieżdżone wiersze połączeń.
- Używa kodowania UTF-8 oraz nazwy pliku z sygnaturą czasową.
- Domyślnie zapisuje wynik na Pulpicie.

Wykorzystywane kluczowe mechanizmy systemu Windows:

(1) Monitorowanie procesów:

- Migawki procesów realizowane za pomocą Windows Management Instrumentation (WMI), wykorzystywane do enumeracji uruchomionych procesów podczas tworzenia baseline.
- Śledzenie zdarzeń WMI (Win32_ProcessStartTrace oraz Win32_ProcessStopTrace) w celu wykrywania uruchomienia i zakończenia procesów w czasie rzeczywistym.
- Natywne zapytania do systemu Windows służące do ustalania ścieżek do plików wykonywalnych oraz linii poleceń procesów, wraz z mechanizmami zapasowymi dla procesów bardzo krótkotrwałych.

(2) Widoczność sieci:

- Natywne API IP Helper systemu Windows wykorzystywane do enumeracji punktów końcowych TCP i UDP wraz z identyfikatorami procesów właścicielskich.
- Okresowe odpytywanie systemu oraz korelacja migawek w celu śledzenia cyklu życia połączeń, w tym czasu pierwszego zaobserwowania oraz momentu zakończenia połączenia.
- Korelacja sieciowa zorientowana na proces, w której aktywność sieciowa prezentowana jest jako kontekst pod konkretnym procesem, a nie jako niezależna tabela sieciowa.

(3) Integralność plików wykonywalnych i atrybucja:

- Bezpośrednie haszowanie plików wykonywalnych z użyciem algorytmu SHA-256 w celu jednoznacznej identyfikacji obrazów procesów na dysku.
- Buforowanie hashy w pamięci operacyjnej, aby uniknąć wielokrotnego haszowania tych samych plików w trakcie jednej sesji.
- Mechanizmy zapasowe rozwiązywania ścieżek dla procesów systemowych oraz narzędzi krótkotrwałych, zwiększające dokładność atrybucji.

(4) Rozwiązywanie nazw:

- Odwrotne rozwiązywanie DNS dla publicznych adresów IP w celu zapewnienia podstawowego kontekstu celu połączenia.
- Buforowanie wyników zapytań DNS w pamięci, aby uniknąć powtarzających się zapytań w trakcie monitorowania.

Typowy scenariusz użycia:

(1) Uruchom Rozpocznij monitorowanie / Start Monitoring w celu utworzenia baseline uruchomionych procesów oraz ich aktywności sieciowej.
(2) Wykonaj lub obserwuj interesujące działanie w systemie, na przykład uruchomienie próbki lub instalację oprogramowania.
(3) Kliknij Wznów monitorowanie po utworzeniu baseline / Resume Monitoring After Baseline Creation, aby śledzić zmiany po baseline w czasie rzeczywistym.
(4) Zidentyfikuj nowo uruchomione procesy, procesy zakończone oraz nowe lub zakończone połączenia sieciowe przy użyciu wskazówek kolorystycznych.
(5) Przeanalizuj linie poleceń, hashe plików wykonywalnych oraz punkty końcowe sieci w ramach dalszej analizy.
(6) Zapisz wyniki do pliku CSV w celu raportowania lub dalszej obróbki analitycznej.

Znane ograniczenia:

(1) Zdalne punkty końcowe UDP nie są dostępne z założenia i nie są wyświetlane.
(2) Odwrotne rozwiązywanie DNS działa w trybie best-effort i może zwracać puste wyniki.
(3) Bardzo krótkotrwałe procesy nie zawsze dostarczają pełnych informacji o ścieżce pliku wykonywalnego lub jego hashu.
(4) Monitorowanie sieci jest oparte na cyklicznym odpytywaniu systemu - skrajnie krótkotrwałe połączenia mogą nie zostać zarejestrowane.