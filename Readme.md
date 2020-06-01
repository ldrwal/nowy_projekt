# GIT

## Wstęp

### Dlaczego git
- Praca w trybie rozproszonym.
- Gałęzie.
- Praca z dużymi projektami.
- Szybkość operacji.
- Open Source / Popularność.

### Podstawowa konfiguracja 
Konfiguracja znajduje sie w pliku .gitconfig folderu domowego

    nano ~/.gitconfig
    
Dodajemy do pliku sekceje user w ktorej bedzie znajdowac sie nazwe uzytkownika
oraz adres email.
    
    git config --global user.name "Lukasz Drwal"
    git config --global user.email "drwallukasz@gmail.com"
    
Pozostałe sekcje dodamy poprzez edycje pliku.

    [user]
            name = Lukasz Drwal
            email = drwallukasz@gmail.com
    [core]
            editor = nano
            
Narzedzia do sprawdzania różnicy pomiędzy plikami np. beyond compare
oraz zarzadzania haslami.

    [diff]
            tool = bc3
    [merge]
            tool = bc3
    [credential]
            helper = osxkeychain
    

## Podstawy

#### Git init/ git clone

Komenda tworzenia repozytorium.
    
    git init foo
    albo 
    mkdir foo
    cd foo
    git init
    
Pobranie kopi istniejacego projektu.
Git pobiera całą kopie repozytorium.

    git clone https://github.com/jquery/jquery.git


#### git add + staging area
Dodawanie zmian do repozytorium.
    
    git status

Komenda powiem nam w jakim stanie jest repozytorium.

    git add plik
    # dodanie wszytkich plikow ze zmianami do poczekalni
    git add . 
Komenda doda plik do poczekalni staging area. Ponowna edycja pliku
bez wywolania komnedy commit spowoduje ponowne wykrecie zmian.
Nasz plik będzie miał jedną zmianę znajdującą się w poczekalnia i drugą, która wymaga dodania
do poczekalni. Następnym krokiem będzie potwierdzenie zmian i dodanie ich do repozytorium.

#### git commit
Dodanie zmian do repozytorium.

    git commit
    # dodanie zmian do repozytorium z opisem bez otwierania pliku opisu. 
    git commit -m "opis zmiany"
    # Dodaj automatycznie zmiany do repozytorium zplików, które git już zna.
    git commit -a 
    
    # tryb interaktywny potwierdzania zmian
    git commit --interactive


#### git log
Przeglądanie histori projektu.

    git log
    # wyswietlenie tylko pierwszej lini opisu zmian.
    git log --oneline
    # wyswielenie za pomocą przełącznika --stat informacji, które pliki zostały zmienione
    # oraz oraz ile lini zostało zmienionych
    git log --stat
    # szczegółowe zmiany
    git log --patch
    
    # wyswietlenie 3 ostatnich logow
    git log --oneline -3
    # filtrowanie po dacie
    git log --since="2020-01-02"
    git log --since="2 days ago"
    git log --until="2020-02-00"
    # wyswietlenie prostego graphu 
    git log --graph --online --all
    
#### git diff
Polecenie pokazujące zmiany pomiędzy wpisami w repozytorium.

    # wyswietla zmiany miedzy zmianami w plikach a aktualnym repozytorium
    git diff
    git diff --staged
    # podajemy identyfikatory zmian z git log --oneline
    git diff f941a39 944ff4f index.html
    
    
#### git checkout
Polecenie przełączania pomiędzy gałęziami, przełączanie pomiędzy punktami w histori
lub usuwania zmian.

    git checkout 944ff4f
    git checkout master

    
## GALEZIE

#### git branch / git checkout
Tworzenie nowych gałęzi.
    
    # wsywietla galezie ze wskazaniem aktualnie zuywanej
    git branch
    # tworzy nową gałąz
    git branch new_branch
    # przelaczenie do innej galezi
    git checkout new_branch
    
    git checkout master
    
    # usuwanie galezi
    git branch -d new_branch
    
Uproszczone tworzenie galezi za pomoca laczenia polecen.

    # utworzenie galezi z przelaczeniem
    git checkout -b new_branch

#### git merge
Laczenie galezi.

    # przelaczenie do glownej galezi
    git checkout master
    # scalenie podanej galezi a aktualna galezia.
    git merge new_branch
    
#### git rebase
Sposób scalania gałęzi poprzez dosłownie zmiane bazy.

    # tworzymy zmiany w nowej gałęzi 
    git checkout new_branch
    git commit -a -m 'zmiana 1'
    git commit -a -m 'zmiana 2'
    # teraz wywolamy komende rebase która scali nowa gałąź z główną
    # i przeniesie comity do gałęzi master
    git rebase master
    git checkout master
    
    
#### git cherry-pick
sposób połączenia tylko wybranych commitow z galezi z masterem.
Kopiowany jest commit wraz z opisem ale w masterze tworzony jest pod nowym znacznikiem.

    # tworzymy commity na nowej galezi
    git checkout -b new_branch_3
    git commit -a -m 'zmiana 1'
    git commit -a -m 'zmiana 2'
    # przelaczmay na master
    git checkout master
    # wybieramy commit który chcemy przekopiowac
    git log new_branch_3
    git cherry-pick aaf9623
    

## COFANIE ZMIAN

#### commit -amend
Aktualizacja ostatniego komita.
Przykład.

    # dokonujemy zmian w pliku1 i pliku 2
    # nastepnie dodajmy do staging-area tylko jeden plik
    git add plik1.py
    git commit -m 'modyfikacje w plikach 1-2'
    # tutaj opis comita mowi ze zmienilismy dwa pliki a w komicie jest tylko jeden
    # z pomoca przychodzi komenda --amend
    git add plik2.py
    git commit --amend
    # plik2 zostanie dodany do aktualengo komita
    # ponowne wywolanie komendy umozliwia nam edycje opisu komita
    git commit --amend 
    
#### git reset
Cofanie zmian 

Usuniecie pliku ze staging area.
    
    git add plik1.py
    git reset plik1.py
    
Usuniecie zmian w pliku

    git checkout plik1.py
    
Jesli chcemy usunac comita

    git commit plik1.py -m 'dodana zmiana w plik1'
    
    # usuniecie z podaniem id commita
    git reset 03cf5a5dceaa18e92ada87e78e10460b9cbb1217
    
    # usuniecie ilosciowe HEAD~ilosc comitow wstecz
    git reset HEAD~5
    # usuniecie ostaniego comita
    git reset HEAD~

Polecenia te usuwają comita i przenosza plik do staging area,
czyli nie tracimy tych zmian.
Dopiero komenda 

    git checkout plik1.py 
usunie nasze zmiany w pliku.

Gdy chcemy zrobić to odrazu, uzyjemy parametru --hard
Nalezy pamiętać że polecenie to usunie nam także zmiany w plikach, 
które znajdują się w poczekalni i nie zostały dodane do staging area.

    git commit plik1.py -m 'dodana zmiana w plik1'
    git reset --hard HEAD~
    
    
#### git reflog
Zapis wszystkich operacji które zarejestrował git.
Umożliwia nam to przenosienie w hisori zmian pliku.

    git reflog
    
    git reset --hard HEAD@{3}
    git reset --hard 18b0024
    
#### git revert
Cofanie zmian z dodaniem comita.
Tworzy nam nowy comit z odwrotnościa zmian.
Czyli jak w comicie który cofamy mieliśmy cos dodane to zostanie to usuniete.

    git revert HEAD
    git diff HEAD~2
    

    
    
    

