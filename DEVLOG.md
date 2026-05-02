# 📓 Devlog - Active Directory Lab



Dziennik postępów, decyzji projektowych i napotkanych trudności w budowie infrastruktury `corp.local`.



---



## [2026-04-27] - Inicjalizacja projektu i fundamenty sieciowe



### 🏗️ Wykonane zadania:

- **Repozytorium:** Utworzenie struktury plików projektu na GitHub.

- **Instalacja OS:** Czysta instalacja **Windows Server 2022 Standard** (Desktop Experience) na maszynie wirtualnej.

- **Konfiguracja Hypervisora:** - Przełączenie karty sieciowej w tryb **Bridged**, aby maszyna była widoczna w sieci lokalnej i miała dostęp do internetu dla aktualizacji.

- **Statyczna adresacja (DHCP Reservation):**

- Skonfigurowano rezerwację adresu IP na routerze na podstawie adresu MAC karty sieciowej.

- Przypisany adres: `192.168.0.106`.



### 🧠 Decyzje i przemyślenia:


- Wybrałem tryb **Bridged** zamiast NAT, aby ułatwić późniejszą integrację z fizycznymi urządzeniami w laboratorium oraz hostem Linux.

- Zastosowałem rezerwację DHCP zamiast sztywnego wpisania IP w systemie na tym etapie, aby uniknąć konfliktów przed podniesieniem roli Domain Controllera.


### 🚩 Napotkane problemy:

- *Brak:* Instalacja przebiegła pomyślnie.


## [2026-04-28] - Konfiguracja post-instalacyjna i standaryzacja DC

### 🏗️ Wykonane zadania:
- **Synchronizacja czasu:** Ustawiono poprawną datę i godzinę (kluczowe dla przyszłej autoryzacji Kerberos w domenie).
- **Optymalizacja pracy w VM:** Włączono dwukierunkowy schowek (Bidirectional Clipboard), ułatwiając transfer kodu między hostem a serwerem.
- **Weryfikacja sieci:**
    - Wykonano `ipconfig /all` – potwierdzono przypisanie oczekiwanego adresu IP: `192.168.0.106` (zgodnie z rezerwacją DHCP).
    - Test łączności: `ping 8.8.8.8` zakończony pomyślnie (potwierdzony dostęp do Internetu dla przyszłych aktualizacji).
- **Zmiana nazwy hosta:** - Zmieniono nazwę serwera na `DC01` za pomocą PowerShell:
      ```powershell
      Rename-Computer -NewName "DC01" -Restart
      ```

### 🧠 Decyzje i przemyślenia:
- **Dlaczego zmiana nazwy na DC01?** - **Identyfikacja:** W środowisku enterprise, gdzie występuje duża liczba maszyn, nazwa `DC01` daje natychmiastową jasność, że serwer pełni rolę pierwszego kontrolera domeny (Domain Controller).
- 
    - **Zarządzanie ryzykiem:** Zmiana nazwy serwera, który już pełni rolę kontrolera domeny, jest operacją wysokiego ryzyka. Nazwa hosta jest głęboko zintegrowana z bazą danych Active Directory, rekordami DNS oraz certyfikatami bezpieczeństwa. Wykonanie tej czynności przed promocją serwera zapobiega późniejszym błędom replikacji i spójności danych.

### 🚩 Napotkane problemy:
- Brak. Wszystkie operacje przebiegły zgodnie z planem.


## [2026-05-02] - Narodziny domeny corp.local i promocja DC

### 🏗️ Wykonane zadania:
- **Instalacja roli AD DS:** Pomyślnie dodano rolę Active Directory Domain Services wraz z narzędziami administracyjnymi (RSAT) przez Server Manager.
- **Konfiguracja DNS:** Zainstalowano rolę DNS Server jako integralną część infrastruktury domeny.
- **Promocja do Domain Controller (DC):**
    - Utworzono nowy las (New Forest) o nazwie `corp.local`.
    - Skonfigurowano hasło DSRM (Directory Services Restore Mode).
    - Przeprowadzono pomyślną walidację wymagań wstępnych (Prerequisites Check).
- **Weryfikacja post-instalacyjna:**
    - Sprawdzono działanie usługi Netlogon.
    - Wykonano test diagnostyczny: `dcdiag /test:NetLogon` (wynik: Passed).
    - Zweryfikowano strukturę stref w DNS Managerze.

### 🧠 Decyzje i przemyślenia:
- **Przejście na pełną statyczną adresację:** Chociaż wcześniej stosowałem rezerwację DHCP, na etapie promocji DC ręcznie skonfigurowałem IPv4 `192.168.0.106` w systemie.
    - **Uzasadnienie:** Kontroler domeny nie może polegać na zewnętrznym serwerze DHCP (np. domowym routerze). W przypadku awarii routera, usługi AD i DNS na serwerze mogłyby nie wystartować poprawnie, paraliżując całą sieć.
- **Konfiguracja DNS Loopback:** Jako preferowany serwer DNS ustawiłem adres lokalny (`127.0.0.1`). Jest to kluczowe, aby DC01 odpytywał samego siebie o rekordy domeny `corp.local`.
- **Zignorowanie ostrzeżenia o delegacji DNS:** Podczas konfiguracji pojawił się komunikat o braku możliwości utworzenia delegacji DNS. Zdecydowałem się kontynuować, ponieważ w przypadku tworzenia nowego lasu z końcówką `.local` (strefa niepubliczna), nie istnieje nadrzędny serwer DNS, który mógłby taką delegację przyjąć.

### 🚩 Napotkane problemy:
- **Długi czas restartu po promocji:** Pierwsze uruchomienie po podniesieniu do roli DC trwało znacznie dłużej (konfiguracja bazy NTDS i zabezpieczeń).
    - **Wniosek:** Cierpliwość jest kluczowa przy pierwszej konfiguracji usług katalogowych – system musi zainicjować strukturę bazy danych i polisy bezpieczeństwa.


- Problem: Wykonanie polecenia dcdiag /test:netlogon zwracało błąd test not found.

- Przyczyna: Narzędzie dcdiag wymaga ścisłego dopasowania do wewnętrznej listy testów. Parametr odpowiadający za weryfikację logowania sieciowego musi zostać podany w liczbie mnogiej.

- Rozwiązanie: Zmieniono składnię na dcdiag /test:NetLogons.

- Wynik: Test został rozpoznany i zakończony statusem Passed, co potwierdziło poprawną konfigurację usługi Netlogon oraz replikacji udziałów SYSVOL i NETLOGON.

---