# 🏢 Windows Server Active Directory Infrastructure - local.corp



![Windows Server](https://img.shields.io/badge/Windows\_Server-2022-0078D4?style=for-the-badge\&logo=windows\&logoColor=white)

![PowerShell](https://img.shields.io/badge/PowerShell-5.1-5391FE?style=for-the-badge\&logo=powershell\&logoColor=white)

![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04\_LTS-E95420?style=for-the-badge\&logo=ubuntu\&logoColor=white)

![Status](https://img.shields.io/badge/Status-In\_Progress-yellow?style=for-the-badge)



**Kompleksowa implementacja infrastruktury IT dla symulowanego przedsiębiorstwa `corp.local`.**



[🛠️ Architektura](#-architektura) • [⚙️ Automatyzacja](#️-automatyzacja) • [🔐 Security](#-security) • [🐧 Linux](#-integracja-linux) • [🔧 Troubleshooting](#-incident-book)





---



## 📌 O projekcie

Projekt koncentruje się na budowie, hardeningu i automatyzacji środowiska Active Directory. Zamiast standardowej konfiguracji, skupiłem się na metodach stosowanych w dużych organizacjach (Enterprise), takich jak model warstwowy (Tiered Admin) oraz automatyzacja cyklu życia użytkownika za pomocą PowerShell.

> 📓 **Dziennik budowy:** Śledź codzienne postępy i rozwiązane problemy w [DEVLOG.md](./DEVLOG.md).



**Główny cel:** Stworzenie środowiska typu "Production-Ready" z pełną dokumentacją incydentów.



---


## 📂 Struktura Projektu

Poniższa struktura odzwierciedla organizację zasobów projektu, zapewniając przejrzystość dokumentacji oraz łatwość zarządzania skryptami automatyzacji.

```text


windows-server-ad-infrastructure/
├── 📁 documentation/           # Dokumentacja techniczna i projektowa
│   ├── 📁 architecture/        # Diagramy sieci i mapy logiczne AD
│   ├── 📁 incident-book/       # Raporty Root Cause Analysis (RCA)
│   └── setup-guide.md          # Instrukcja wdrożenia środowiska
├── 📁 scripts/                 # Automatyzacja PowerShell
│   ├── 📁 automation/          # Onboarding, bulk import, zarządzanie OU
│   ├── 📁 maintenance/         # Raporty zdrowia systemu, cleanup
│   └── 📁 assets/              # Pliki CSV i dane wejściowe
├── 📁 gpo-backups/             # Eksporty polis (LAPS, AppLocker, Mapowania)
├── 📁 linux-integration/       # Konfiguracja sssd.conf i uprawnień sudo
├── 📁 screenshots/             # Dokumentacja wizualna 
├── LICENSE
├── DEVLOG.md		         # Dziennik postępów
├── .gitignore                  # Wykluczenia plików tymczasowych
└── README.md                   # Ten plik






## 🗺️ Architektura


Środowisko zostało zaprojektowane jako rozproszona infrastruktura korporacyjna, podzielona na warstwy (Tiers) zgodnie z najlepszymi praktykami bezpieczeństwa Microsoft.

### 🖥️ Maszyny Wirtualne

| Hostname | Operating System | Role | IP Address | Resources |


### 🌐 Network Configuration
- **Domain Name:** `corp.local`
- **Network Mode:** Bridged 
- **Subnet:** `192.168.1.0/24`

### 🏗️ Struktura Logiczna (Organizational Units)
Infrastruktura wykorzystuje logiczny podział zasobów, co pozwala na precyzyjne stosowanie polis GPO:
- **`OU=Employees`**: Podział na działy (HR, Sales, IT, Finance).
- **`OU=Servers`**: Dedykowane polisy hardeningu dla serwerów.
- **`OU=Workstations`**: Restrykcje dla stacji roboczych (AppLocker, USB Block).
- **`OU=Admin_Accounts`**: Separacja kont administracyjnych (Tier 0 / Tier 1 / Tier 2).




---



## ⚙️ Automatyzacja (PowerShell)

Wszystkie skrypty znajdują się w folderze `/scripts`.




---



## 🛡️ Polityki GPO i Hardening

Wdrożone mechanizmy zabezpieczające stacje i serwery:



---



## 🐧 Integracja Linux ↔ AD



---



## 🔧 Incident Book (Root Cause Analysis)

*Dokumentacja rozwiązanych problemów technicznych napotkanych podczas budowy labu.\*





---



## 🚀 Jak odtworzyć to środowisko?

1. Zainstaluj Windows Server 2022 na VM (min. 4GB RAM, 2 vCPU).

2. Pobierz skrypty z folderu `/Scripts`.

3. 



---



## 📈 Czego się nauczyłem?



---



## **👨‍💻** Autor





**Mateusz Markiewicz** - Projekt zrealizowany w ramach praktycznej nauki i stanowi część portfolio



