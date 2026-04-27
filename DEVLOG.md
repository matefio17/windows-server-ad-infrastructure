\# 📓 Devlog - Active Directory Lab



Dziennik postępów, decyzji projektowych i napotkanych trudności w budowie infrastruktury `corp.local`.



\---



\## \[2026-04-27] - Inicjalizacja projektu i fundamenty sieciowe



\### 🏗️ Wykonane zadania:

\- \*\*Repozytorium:\*\* Utworzenie struktury plików projektu na GitHub.

\- \*\*Instalacja OS:\*\* Czysta instalacja \*\*Windows Server 2022 Standard\*\* (Desktop Experience) na maszynie wirtualnej.

\- \*\*Konfiguracja Hypervisora:\*\* - Przełączenie karty sieciowej w tryb \*\*Bridged\*\*, aby maszyna była widoczna w sieci lokalnej i miała dostęp do internetu dla aktualizacji.

\- \*\*Statyczna adresacja (DHCP Reservation):\*\*

&#x20;   - Skonfigurowano rezerwację adresu IP na routerze na podstawie adresu MAC karty sieciowej.

&#x20;   - Przypisany adres: `192.168.0.106`.



\### 🧠 Decyzje i przemyślenia:



\- Wybrałem tryb \*\*Bridged\*\* zamiast NAT, aby ułatwić późniejszą integrację z fizycznymi urządzeniami w laboratorium oraz hostem Linux.

\- Zastosowałem rezerwację DHCP zamiast sztywnego wpisania IP w systemie na tym etapie, aby uniknąć konfliktów przed podniesieniem roli Domain Controllera.



\### 🚩 Napotkane problemy:

\- \*Brak:\* Instalacja przebiegła pomyślnie.



\### 📅 Plan na jutro:

- Zmiana nazwy hosta na `DC01`

\- Instalacja roli AD DS przez Server Manager

\- Promocja serwera do roli kontrolera domeny (AD DS).

\- Konfiguracja podstawowej strefy DNS.

