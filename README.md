# Konfiguracja Dual Boot: Windows 11 & Linux Ubuntu

Profesjonalna instrukcja przygotowania nośnika instalacyjnego oraz rozwiązywania problemów z partycjami typu RAW i blokadą zapisu.

## 1. Cel projektu
Celem jest skonfigurowanie laptopa z systemem **Windows 11** tak, aby umożliwić uruchamianie systemu **Linux Ubuntu** w trybie **dual boot**. Użytkownik zyskuje możliwość wyboru systemu operacyjnego przy każdym starcie urządzenia.

## 2. Wprowadzenie teoretyczne
**Dual boot** to konfiguracja pozwalająca na natywne uruchamianie dwóch systemów operacyjnych na jednym dysku fizycznym. W przeciwieństwie do wirtualizacji (np. VirtualBox), systemy mają bezpośredni dostęp do zasobów sprzętowych, co zapewnia maksymalną wydajność.

## 3. Wymagania wstępne
*   **Pendrive:** minimum 8 GB (zostanie sformatowany!).
*   **Obraz ISO:** System Ubuntu (zalecana wersja LTS).
*   **Oprogramowanie:** [Rufus](https://rufus.ie).

> [!CAUTION]
> **UWAGA:** Proces przygotowania nośnika oraz edycja partycji poleceniem `clean` nieodwracalnie usuwa wszystkie dane z wybranego dysku.

## 4. Przebieg zadania i Troubleshooting

### Problem: Partycja RAW i blokada zapisu
Podczas przygotowania nośnika napotkano partycję o systemie plików **RAW** (nierozpoznawalny/uszkodzony). Próba formatowania systemowego zakończyła się błędem: *„Dysk jest zabezpieczony przed zapisem”*.
![opis obrazka](linux/img/dyski_RAW.png)
![opis obrazka](linux/img/zabezpieczenie_read_only.png)

### Rozwiązanie: Narzędzie DiskPart
Do naprawy struktury logicznej pendrive'a wykorzystano konsolowe narzędzie **DiskPart**:

1.  Uruchomienie wiersza poleceń jako Administrator.
2.  Wyświetlenie listy dysków:
    ```cmd
    list disk
    ```
    ![opis obrazka](linux/img/list_disk.png)

3.  Wybór nośnika (w tym przypadku Dysk 1):
    ```cmd
    select disk 1
    ```
4.  Sprawdzenie atrybutów w celu wykluczenia blokady sprzętowej:
    ```cmd
    attributes disk
    ```
    ![opis obrazka](linux/img/atrybuty.png)

5.  **Kluczowy krok:** Całkowite wyczyszczenie struktury partycji:
    ```cmd
    clean
    ```
    *Polecenie to usuwa tablicę partycji i resetuje dysk do stanu surowego, co jest najskuteczniejszą metodą przy błędach typu RAW.*
    ![opis obrazka](linux/img/clean.png)
### Tworzenie woluminu
Po wyczyszczeniu dysku w narzędziu **Zarządzanie dyskami** utworzono nowy wolumin prosty i sformatowano go do systemu plików **FAT32**.
![opis obrazka](linux/img/nowy_wolumin.png)
![opis obrazka](linux/img/formatowanie_sukces.png)

## 5. Tworzenie nośnika instalacyjnego (Rufus)
Po przywróceniu sprawności pendrive'a, użyto programu **Rufus**:
1. Wybrano urządzenie docelowe.
2. Wskazano obraz ISO Ubuntu.
3. Wybrano schemat partycjonowania (MBR/GPT) zgodny z docelowym komputerem.
4. Po zakończeniu procesu nośnik jest gotowy do instalacji systemu Linux.
![opis obrazka](linux/img/rufus.png)
## 6. Wnioski
Przygotowanie bootowalnego pendrive'a wymaga niekiedy zaawansowanej diagnostyki nośnika. Wykorzystanie narzędzi systemowych takich jak `diskpart` pozwala na naprawę błędów, z którymi nie radzi sobie standardowy interfejs graficzny Windows. Dzięki temu możliwa jest elastyczna praca w środowisku wielosystemowym
