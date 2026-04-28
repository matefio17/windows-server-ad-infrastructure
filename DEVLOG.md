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



