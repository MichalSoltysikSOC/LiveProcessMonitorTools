Nazwa narzędzia: LiveProcessMonitorPlus.exe

Wersja: 1.0
Suma kontrolna SHA256: 10972FBECE7EFB6F349C1B377B39F2E77B5BCA8F511F9CAB1E67181B1926735E
Rozmiar pliku: 84,0 KB
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

Cel: Live Process Monitor Plus rozszerza model monitorowania oparty na baseline poprzez korelację natywnej telemetrii systemu Windows z opcjonalnymi danymi zdarzeń Sysmona, zapewniając głębszą widoczność osi czasu zdarzeń przy zachowaniu tego samego przepływu pracy oraz zasad interfejsu użytkownika.
Licencja: Darmowa do użytku prywatnego i komercyjnego.

Wymagania wstępne:

(1) Korzystanie z mechanizmu wzbogacania danych o telemetrię Sysmon wymaga świadomego zapoznania się z warunkami licencyjnymi Sysmona opublikowanymi przez firmę Microsoft oraz ich akceptacji.
(2) Aby włączyć funkcje wzbogacania danych oparte na Sysmonie w narzędziu Live Process Monitor Plus, Sysmon musi być zainstalowany, uruchomiony oraz skonfigurowany z użyciem kompatybilnego pliku konfiguracyjnego.
(3) Poniższa konfiguracja Sysmona jest wymagana do zapewnienia pełnej funkcjonalności Live Process Monitor Plus:

<Sysmon schemaversion="4.90">
  <HashAlgorithms>SHA256</HashAlgorithms>
  <EventFiltering>
    <ProcessCreate onmatch="include">
      <Image condition="contains">.exe</Image>
    </ProcessCreate>
    <ProcessTerminate onmatch="include">
      <Image condition="contains">.exe</Image>
    </ProcessTerminate>
    <NetworkConnect onmatch="include">
      <Image condition="contains">.exe</Image>
    </NetworkConnect>
  </EventFiltering>
</Sysmon>

(4) Alternatywnie do ręcznej konfiguracji można użyć narzędzia SysmonConfigurator.exe, które automatycznie konfiguruje Sysmona. Menu aplikacji udostępnia następujące opcje:

1. Szybkie sprawdzenie stanu Sysmona									1. Quick Sysmon status check
2. Pobranie i rozpakowanie Sysmona (C:\Scripts\Sysmon)							2. Download and extract Sysmon (C:\Scripts\Sysmon)
3. Utworzenie lub nadpisanie konfiguracji Sysmona (C:\Scripts\SysmonLiveProcessMonitorConfig.xml)	3. Create or overwrite Sysmon config (C:\Scripts\SysmonLiveProcessMonitorConfig.xml)
4. Instalacja Sysmona z użyciem konfiguracji (automatyczne wykrywanie Sysmona)				4. Install Sysmon using config (auto-locate Sysmon)
5. Ponowna konfiguracja Sysmona z użyciem konfiguracji (automatyczne wykrywanie Sysmona)		5. Reconfigure Sysmon using config (auto-locate Sysmon)
6. Odinstalowanie Sysmona (automatyczne wykrywanie Sysmona)						6. Uninstall Sysmon (auto-locate Sysmon)
7. Weryfikacja telemetrii Sysmona (Event ID 1, 3, 5 - ostatnie 15 minut)				7. Sysmon telemetry verification (Event ID 1, 3, 5 - last 15 minutes)
8. Wyjście												8. Exit

Nazwa pliku: SysmonConfigurator.exe
Suma kontrolna SHA256: A5E94D062E3B379E0070B39E695342D7FBA4D5EEF23B834BE3AEF9ACDA9C98DE

Uwaga: SysmonConfigurator.exe znajduje się w tym samym repozytorium co Live Process Monitor Plus.

Uwaga: SysmonConfigurator.exe korzysta z aktualnej, oficjalnej lokalizacji pobierania narzędzia Sysmon udostępnianej przez Microsoft Sysinternals oraz z obowiązujących parametrów linii poleceń dla Sysmona. Jeżeli Microsoft zmieni adres URL pobierania (obecnie https://download.sysinternals.com/files/Sysmon.zip) lub zmodyfikuje parametry linii poleceń Sysmona, funkcjonalność automatycznego pobierania lub konfiguracji może przestać działać zgodnie z oczekiwaniami i będzie wymagać aktualizacji tego narzędzia. W takim przypadku problem powinien zostać zgłoszony autorowi narzędzia w celu przeglądu i odpowiedniej aktualizacji implementacji.

Wzbogacanie danych oparte na Sysmonie (opcjonalne):

Gdy Sysmon jest zainstalowany, uruchomiony i poprawnie skonfigurowany, Live Process Monitor Plus wzbogaca dane o procesach i aktywności sieciowej o dodatkową, niskopoziomową telemetrię:

(1) Koreluje zdarzenia Sysmon Event ID 1 (utworzenie procesu) z istniejącymi wierszami procesów.
(2) Koreluje zdarzenia Sysmon Event ID 5 (zakończenie procesu), umożliwiając precyzyjne rejestrowanie czasu zakończenia procesu.
(3) Koreluje zdarzenia Sysmon Event ID 3 (próby oraz nawiązania połączeń sieciowych) z istniejącymi lub nowo zaobserwowanymi połączeniami.
(4) Wyświetla wszystkie zaobserwowane zdarzenia Sysmona (Event ID 1, 3 i 5) przypisane do danego procesu w dedykowanej kolumnie.
(5) Dodaje jednoznaczne znaczniki czasu oparte na Sysmonie dla momentu utworzenia i zakończenia procesu.

Istotne zachowanie aplikacji:

(1) Dane z Sysmona są nieaktywne podczas tworzenia baseline.
(2) Monitorowanie oparte na Sysmonie uaktywnia się dopiero po kliknięciu Wznów monitorowanie po utworzeniu baseline / Resume Monitoring After Baseline Creation.
(3) Wszystkie zdarzenia Sysmona występujące przed rozpoczęciem monitorowania po baseline są traktowane wyłącznie jako kontekst baseline.

Uwaga: Jeżeli Sysmon nie jest dostępny, aplikacja pozostaje w pełni funkcjonalna, korzystając wyłącznie z natywnej telemetrii systemu Windows.

Rozszerzone śledzenie połączeń sieciowych:

Live Process Monitor Plus rozszerza możliwości Live Process Monitor, zapewniając pełniejszy wgląd w cykl życia połączeń sieciowych:

(1) Śledzi czas rozpoczęcia połączenia TCP, czas ostatniej obserwacji oraz moment jego zakończenia.
(2) Rozróżnia:

- połączenia aktywne,
- połączenia zakończone wykryte w wyniku cyklicznego odpytywania systemu,
- próby połączeń zaobserwowane wyłącznie przez Sysmona (stan: próba połączenia / ATTEMPTED).

(3) Zachowuje zakończone połączenia w historii sesji zamiast usuwać je natychmiast.
(4) Koreluje zdarzenia sieciowe Sysmona z migawkami TCP i UDP zbieranymi metodą odpytywania systemu.

Uwaga: Kolory połączeń nadal wynikają z klasyfikacji procesu właścicielskiego. Zmiany stanu połączenia są odzwierciedlane wyłącznie w kolumnach tabeli, a nie poprzez zmianę koloru.

Podsumowanie funkcji charakterystycznych dla wersji Plus:

Live Process Monitor Plus zapewnia dodatkową głębię analityczną poprzez:

(1) Łączenie natywnej telemetrii procesów i sieci systemu Windows z opcjonalnymi danymi zdarzeń Sysmona (Event ID 1, 3 i 5).
(2) Zachowanie szczegółowej osi czasu wykonania procesów oraz aktywności sieciowej, w tym procesów krótkotrwałych i szybko kończących działanie.
(3) Wykorzystanie dwóch niezależnych źródeł dowodowych - telemetrii systemowej opartej na odpytywaniu oraz dziennika zdarzeń Sysmona - w celu wiarygodnego rozróżnienia kontekstu baseline od rzeczywistego zachowania po baseline.

Ograniczenia i uwagi operacyjne:

(1) Wzbogacanie danych oparte na Sysmonie jest opcjonalne i warunkowe. Jeżeli Sysmon nie jest zainstalowany, nie jest uruchomiony lub nie jest skonfigurowany z wymaganymi identyfikatorami zdarzeń, funkcje wzbogacania specyficzne dla wersji Plus nie będą dostępne.
(2) W przypadku braku telemetrii Sysmona, Live Process Monitor Plus działa wyłącznie w oparciu o natywne mechanizmy systemu Windows. Cała podstawowa funkcjonalność monitorowania baseline i monitorowania po baseline pozostaje w pełni dostępna, jednak niedostępne będą:

- znaczniki czasu utworzenia i zakończenia procesu oparte na Sysmonie,
- korelacja zdarzeń sieciowych pochodzących z Sysmona,
- identyfikacja prób połączeń zaobserwowanych wyłącznie przez Sysmona (stan: próba połączenia / ATTEMPTED).

(3) Dane Sysmona są zbierane wyłącznie podczas monitorowania po baseline. Zdarzenia występujące przed kliknięciem Wznów monitorowanie po utworzeniu baseline / Resume Monitoring After Baseline Creation są traktowane jako kontekst baseline i nie są wykorzystywane do korelacji po baseline.
(4) Dostępność i konfiguracja Sysmona pozostają poza kontrolą aplikacji. Odpowiedzialność za instalację, uruchomienie oraz konfigurację Sysmona z rekomendowanymi identyfikatorami zdarzeń spoczywa na użytkowniku.