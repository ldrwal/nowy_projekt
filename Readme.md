# GIT

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
    

### Podstawy

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
    
### GALEZIE
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
