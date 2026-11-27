# Dziennik pracy — Analiza transkryptomu

## Blok 1: Środowisko pracy, powłoka Linux, conda i Git

### 1. Uruchomienie środowiska wirtualnego
Na początku zainstalowałem VirtualBox oraz Vagrant i pobrałem repozytorium `analiza2026-srodowisko`. Po uruchomieniu maszyny wirtualnej z Ubuntu 24.04 (`vagrant up` i `vagrant ssh`) sprawdziłem wersje podstawowych narzędzi (`git`, `gawk`, `mamba`, `ssh`) oraz przetestowałem działanie katalogu współdzielonego `~/analiza2026` w obie strony, tworząc i usuwając pliki testowe z poziomu maszyny oraz systemu gospodarza.

### 2. Praca w powłoce `bash` (ćwiczenia 1.1 i 1.2)
* **Struktura katalogów i pierwszy skrypt:** W katalogu `cwiczenia01` utworzyłem jednym poleceniem `mkdir -p` z rozwinięciem nawiasów klamrowych strukturę `projekt_demo` (`dane/raw`, `wyniki`, `skrypty`, `logi`). Napisałem skrypt `lista_katalogow.sh` wyposażony w shebang `#!/bin/bash`, komentarz opisowy, flagi bezpieczeństwa `set -euo pipefail` oraz pętlę `for` wypisującą ścieżki utworzonych folderów. Po nadaniu mu uprawnień wykonywania (`chmod +x`) uruchomiłem go, przekierowując standardowe wyjście (`>`) do `wyniki/lista.txt`, a strumień błędów (`2>`) do `logi/lista.log`.
* **Filtrowanie tabeli `geny.tsv`:** Za pomocą poleceń `awk`, `cut`, `sort` oraz `uniq -c` przeanalizowałem przykładową tabelę zliczeń: wyciągnąłem nazwy genów typu `lncRNA`, policzyłem liczbę genów przypadającą na każdy chromosom, znalazłem gen o najmniejszej niezerowej liczbie zliczeń (`LINC0001` — 12 zliczeń, z pominięciem `MIR155` mającego 0) oraz zsumowałem zliczenia dla genów `protein_coding` (łącznie 11 580).
* **Pętla na plikach FASTQ i przekierowania:** Wygenerowałem trzy puste pliki `HBR_{1,2,3}_R1.fq.gz` i uruchomiłem dwukrotnie pętlę `for`, która za pomocą `basename` wycinała same nazwy próbek i dopisywała je do `probki.txt`. Użycie operatora `>>` spowodowało dopisanie nowych danych na końcu pliku, dzięki czemu po dwóch przebiegach pętli znalazło się w nim 6 linii. Gdybym zamiast `>>` użył pojedynczego operatora `>`, plik byłby nadpisywany przy każdej iteracji pętli i na końcu zostałaby w nim wyłącznie ostatnia próbka `HBR_3`.

### 3. Środowiska `conda` i `mamba` (ćwiczenie 1.3)
Z gotowego pliku `qc-env.yml` utworzyłem za pomocą `mamba` izolowane środowisko `qc` i zweryfikowałem działanie programów `fastqc`, `fastp` oraz `multiqc`. Porównałem również pełny eksport środowiska (`mamba env export`) z uproszczonym eksportem `--from-history` zapisanym do `qc-from-history.yml` — bezpośrednio w pliku zdefiniowałem tylko 3 pakiety, a kilkadziesiąt pozostałych (m.in. Pythona, Javę i biblioteki systemowe) `mamba` dobrała automatycznie jako zależności. Następnie napisałem własny plik `seqkit-env.yml` z przypiętą wersją `seqkit=2`, sprawdziłem na nim statystyki odczytów dla pliku `mini.fq` (`seqkit stats`) i po teście usunąłem środowisko poleceniem `mamba env remove -n seqkit`.

### 4. Git, GitHub i konfiguracja dostępu do klastra BEM2 (ćwiczenia 1.4 i 1.5)
Skonfigurowałem lokalnie Gita (`user.name`, `user.email`, domyślna gałąź `main`), wygenerowałem klucz SSH typu `ed25519` i dodałem klucz publiczny do swojego konta na GitHubie. Założyłem publiczne repozytorium `analiza2026-cwiczenia`, sklonowałem je do `~/analiza2026/`, uzupełniłem `README.md` oraz `.gitignore` (ignorujący duże pliki danych i logi) i uporządkowałem pliki z Bloku 1 w katalogu `blok1/`.

* **Dostęp do BEM2:** Konfiguracja `Host bem2` z moim kluczem SSH została dopisana do pliku `~/.ssh/config` — czekam na aktywację konta WCSS, aby przetestować logowanie *(jeśli konto już działa, zamień tę część na: połączenie SSH z klastrem działa poprawnie i utworzyłem katalog `~/analiza2026`)*.
