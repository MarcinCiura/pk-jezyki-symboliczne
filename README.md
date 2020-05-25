# Jak zaliczyć projekt z Pythona
## Poradnik dla studentów przedmiotu Języki Symboliczne

Zamiast powtarzać podobne komentarze w wielu projektach,
spisałem je tutaj. Uwagi do tego dokumentu można zgłaszać
przez *Issues*.

### 1. Pliki

* Repozytorium nie powinno zawierać zbędnych katalogów
`__pycache__`, `.idea`, `venv` itp. ani zbędnych plików,
np. `desktop.ini`.

* Wskazane jest za to dodanie pliku `.gitignore`
o treści odpowiedniej dla projektów pisanych w Pythonie.
Kto nie dodał tego pliku przy zakładaniu repozytorium,
[tutaj](https://github.com/github/gitignore/blob/master/Python.gitignore)
znajdzie odpowiednią treść.

* Nazwy bibliotek zewnętrznych niezbędnych do działania programu
należy umieścić w pliku `requirements.txt`
(zewnętrzne znaczy „nie te, co się zainstalowały z Pythonem”;
nie robić pliku, jak nie ma z czym).
Dzięki temu przez polecenie `pip install -r requirements.txt`
można je wszystkie naraz zainstalować.
[Tutaj](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file)
przykładowa treść tego pliku. Sądzę, że w większości
Państwa projektów wystarczy wymienić nazwy bibliotek
bez uściślania ich wersji, np.
    ```
    numpy
    pygame
    ```

### 2. Uwagi drobiazgowe

Proszę się zapoznać z treścią
[poradnika dla programistów Pythona w firmie Google](https://google.github.io/styleguide/pyguide.html)
i stosować do jego zasad. Nie wymagam tylko podawania
w docstringach funkcji i metod sekcji `Args:`, `Returns:`
i `Raises:` ani w docstringach klas sekcji `Attributes:`
([punkty 3.8.3 i 3.8.4](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods)).
Nie obowiązują również zasady związane z Pythonem 2,
czyli nie należy pisać `from __future__ import ...`,
dziedziczyć z `object` ani korzystać z biblioteki `six`.
Najczęściej ignorowane zasady powtarzam poniżej.

* [Pylint](https://pypi.org/project/pylint/)
jest Państwa przyjacielem. Proszę go zainstalować,
przepuszczać przez niego swój kod
i poprawiać wskazane przez niego miejsca
([punkt 2.1](https://google.github.io/styleguide/pyguide.html#21-lint)).

* Proszę używać instrukcji `import` wyłącznie
z nazwami pakietów i modułów, a nie poszczególnych
klas czy funkcji, a już broń Boże nigdy nie pisać
`from spam import *`. Dzięki temu unikną Państwo
zaśmiecania przestrzeni nazw nie wiadomo czym
([punkt 2.2](https://google.github.io/styleguide/pyguide.html#22-imports)).
Rozumiem, że ta zasada może wywoływać Państwa opór,
ale my tu symulujemy pracę w firmie.
W projektach tworzonych przez zespół
lepsze są zbyt sztywne reguły od braku reguł.
Poza tym standardy firm mniejszego kalibru niż Google
bywają bardziej niedorzeczne. Proszę się przyzwyczajać.

* Można rozdzielać pustymi wierszami sąsiednie sekcje
złożone z instrukcji `import`, które powinnny dotyczyć
kolejno: biblioteki standardowej, bibliotek zewnętrznych
i naszych własnych modułów. Wewnątrz każdej sekcji nazwy
pakietów i modułów należy uporządkować leksykograficznie
([punkt 3.13](https://google.github.io/styleguide/pyguide.html#313-imports-formatting)).

* Proszę się wystrzegać zmiennych globalnych
([punkt 2.5](https://google.github.io/styleguide/pyguide.html#25-global-variables)).
Często da się ich pozbyć przez zmianę architektury
kodu z proceduralnej na obiektową tak, żeby stały
się atrybutami instancji klasy.

* Globalne stałe są natomiast mile widziane,
a nawet wymagane zamiast
[magicznych liczb, napisów czy większych obiektów](https://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants).
Na przykład zamiast `7` należy zdefiniować stałą
`BOARD_WIDTH = 7`, zamiast odróżniania kierunku
przy użyciu zmiennej o wartości `'left'` albo `'right'`
— zdefiniować stałe `LEFT, RIGHT = range(2)`
(miłośnikom nowości polecam
[`enum.Enum`](https://docs.python.org/3/library/enum.html#creating-an-enum)),
zamiast krotki `(0, 43, 54)` — stałą
`BACKGROUND_COLOR = (0, 43, 54)`. Stałe zwyczajowo
zapisujemy `WIELKIMI_LITERAMI_Z_PODKREŚLNIKAMI`
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).
Nie od rzeczy będzie przypomnieć, że XXI wiek
trwa już dość długo i w identyfikatorach można swobodnie
korzystać z polskich liter, jeśli ktoś ma taką ochotę.

* Nazwy funkcji i zmiennych w `styluWielbłądzim`
(*`camelCase`*) są niepytoniczne. Powinny być
w `stylu_wężowym` (*`snake_case`*)
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).

* Ponieważ z nazw plików `*.py` i katalogów z nimi
powstają nazwy modułów i pakietów programu,
do nich też lepiej pasuje `styl_wężowy`
([punkt 3.16](https://google.github.io/styleguide/pyguide.html#316-naming)).

* Warto pamiętać, że czytelnik programu nie musi
znać tych samych skrótów, co my. Wniosek pierwszy:
żadne skróty nie są dozwolone, zarówno w identyfikatorach,
jak w docstringach i komentarzach, chyba że będą
zrozumiałe dla każdego średnio rozgarniętego pięciolatka
([punkt 3.16](https://google.github.io/styleguide/pyguide.html#316-naming)).
Wniosek drugi: tym bardziej niedozwolone są jednoliterowe
identyfikatory
([punkt 3.16.1](https://google.github.io/styleguide/pyguide.html#s3.16-naming)).
Wyjątki to `i`, `j`, `k` jako liczniki pętli;
`x`, `y`, `z` jako współrzędne,
a także litery pasujące do typu danych w funkcjach anonimowych
oraz wyrażeniach listowych, słownikowych, zbiorowych i generatorowych,
czyli po naszemu *lambda expressions*,
*list/dict/set comprehensions* i *generator expressions*
(często trudno wyczuć, jaka litera pasuje,
ale jeszcze nikomu nie stała się krzywda za użycie `x`).

* Wymagane są docstringi do modułów, klas, metod
i funkcji, chyba że jednowierszowych lub w inny
sposób oczywistych
([punkt 3.8](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).
Należy się zapoznać z zasadami pisania docstringów
i ich przestrzegać. W skrócie: pierwszy wiersz ma zawierać
pełne a krótkie zdanie (na końcu zdania stawiamy kropkę)
opisujące działanie metody lub funkcji, np.
`"""Zwraca przycisk naciśnięty przez użytkownika."""`
Jeśli potrzebne jest dłuższe objaśnienie, należy
je podać poniżej, po pustym wierszu.

* Sklejanie napisów przez `+` jest nieładne.
Ładne są za to f-stringi
([punkt 3.10](https://google.github.io/styleguide/pyguide.html#310-strings)).

* Wiersze programu nie powinny być za długie.
Jeśli trzeba je połamać na krótsze kawałki,
dobrze wiedzieć, że kawałki otoczone dowolnym
rodzajem nawiasów (`()`, `[]`, `{}`) same się
ze sobą łączą i wyglądają lepiej niż ze znakiem obciachu
`\` na końcach
([punkt 3.2](https://google.github.io/styleguide/pyguide.html#32-line-length)).

### 3. Rozmaite rady

* [Dobrym zwyczajem](https://pl.wikipedia.org/wiki/Mars_Climate_Orbiter#Utrata_sondy)
są przyrostki — jednostki miary — w identyfikatorach
związanych z wielkościami fizycznymi:
`FRAME_INTERVAL_SECONDS`, `document_age_days`,
`calculate_page_width_mm()`.

* Zamiast `r'spam\ham.png'`, co działa tylko pod Windows,
lepiej pisać `'spam/ham.png'`, co działa również
pod Windows.

* Dłuższe ścieżki do plików dobrze skleja
[`os.path.join()`](https://docs.python.org/3/library/os.path.html#os.path.join).
Nie trzeba pamiętać, które kawałki ścieżki się kończą na `'/'`,
a które nie.

* [System baz danych](https://pl.wikipedia.org/wiki/System_zarz%C4%85dzania_relacyjn%C4%85_baz%C4%85_danych),
którego używają Państwa programy, nazywa się SQLite, nie MySQL.

* Wewnątrz `sqlite3.Cursor.execute()` itp.
zmienne parametry wolno wstawiać do SQL-a tylko przez
[`?`, `?42`, `:spam`, `$spam` lub `@spam`](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor.execute),
a nigdy przez f-stringi ani `.format()`.
Uzasadnienie
[tutaj](https://xkcd.com/327/).
Ku pamięci: w sposobach z pytajnikiem parametry
po stronie Pythona muszą być w krotce lub liście,
więc jak jest jeden, pisać `(spam,)` lub `[spam]`.

* Z drugiej strony przy zapytaniach z dynamicznymi nazwami kolumn
lub z `IN (?, ?,...)` o zmiennej liczbie pytajników
nie da się obejść bez f-stringów lub `.format()`.

* Polecam poniższy szablon czytania z bazy danych.
O metodzie `.fetchall()` najlepiej zapomnieć.
```python
    spam_list = []
    for row in cursor.execute(
            """
            SELECT spam FROM SpamTable
            WHERE ham = ?
            AND eggs = ?
            """,
            (ham, eggs)
    ):
        spam_list.append(row[0])
        # O ile kolumn prosi SELECT,
        # tyle elementów mają krotki |row|.
```

* Python to nie C. Poniżej wzorce i antywzorce pętli.
```python
    # PYTONICZNIE                                  # NIEPYTONICZNIE
    ####                                           ####
    for spam in spam_list:                         for i in range(len(spam_list)):
        frobnicate(spam)                               frobnicate(spam_list[i])
    ####                                           ####
    for i, spam in enumerate(spam_list):           i = 0
        frobnicate(i, spam)                        for spam in spam_list:
                                                       frobnicate(i, spam)
                                                       i += 1
    ####                                           ####
    for spam, ham in zip(spam_list, ham_list):     for i in range(len(spam_list)):
        frobnicate(spam, ham)                          frobnicate(spam_list[i], ham_list[i])
    ####                                           ####
    COINS = [                                      COINS = [
        ('1 grosz', 0.01),                             ['1 grosz', 0.01],
        ('2 grosze', 0.02),                            ['2 grosze', 0.02],
        ...,                                           ...,
    ]                                              ]

    for name, value_pln in COINS:                  for coin in COINS:
        frobnicate(name, value_pln)                    frobnicate(coin[0], coin[1])
    ####                                           ####
    ham = '\n'.join(spam_list)                     ham = ''
                                                   for i, spam in enumerate(spam_list):
                                                       if i:
                                                           ham += '\n'
                                                       ham += spam
    ####                                           ####
    ham = '\n'.join(                               ham = ''
        frobnicate(x) for x in spam_list)          for i, spam in enumerate(spam_list):
                                                       if i:
                                                           ham += '\n'
                                                       ham += frobnicate(spam)
```

* Python to nie Java, odsłona pierwsza.
Zbyteczne jest tworzenie osobnych plików
na małe klasy w stylu `PrzechowywaczMonet`
tylko po to, żeby później było trzeba pisać
`import PrzechowywaczMonet` i
`przechowywacz_monet = PrzechowywaczMonet.PrzechowywaczMonet()`.

* Python to nie Java, odsłona druga. Zamiast pobieraczy
(*getters*) i ustawiaczy (*setters*) robiących tylko
`return self._spam` i `self._spam = spam` wystarczy
nazwać ten atrybut `self.spam` i bezpośrednio go
odczytywać i zapisywać. Jeśli potrzebny jest pobieracz,
który robi coś więcej, pomoże nam dekorator
[`@property`](https://docs.python.org/3/library/functions.html#property):
```python
    @property
    def total_spam(self):
        """Używać bez nawiasów: ham = self.total_spam"""
        return sum(self._spam_list)
```

* Należy odróżniać atrybuty klasy, które są
inicjalizowane tak:
```python
    class Spam:
        ham = 42
```
od atrybutów instancji, które są inicjalizowane
w konstruktorze:
```python
    class Spam:
        def __init__(self):
            self.ham = 42
```
Te pierwsze mają wartość początkową nadawaną tylko raz
i są wspólne dla wszystkich instancji klasy
(można się do nich odwoływać przez `Spam.ham` lub
`self.ham`), a te drugie są osobne w każdej instancji
(`self.ham`). Jeśli atrybut klasy jest stałą,
to wszystko jest w porządku. Natomiast jeśli atrybut
klasy zmienia wartość w trakcie działania programu,
to prosimy się o kłopoty. Nawet gdy przewidujemy
istnienie podczas działania programu tylko jednej
instancji klasy (np. `Game`), to przy testowaniu
zupełnie normalne jest tworzenie jedna po drugiej
coraz nowszych instancji, a każda będzie bazgrać
po jedynym egzemplarzu atrybutu. Dlatego stosowanie
zmiennych atrybutów klas jest prawie zawsze błędem.
Dekorator
[`@dataclasses.dataclass`](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass),
poniekąd powiązany z tym zagadnieniem, jest nowy
i nadobowiązkowy.

* Statyczne nazwy dobre, dynamiczne nazwy złe.
Dlatego zmienne w stylu
```python
    point = {'x': 42, 'y': 56}
```
wyglądają lepiej jako
[`collections.namedtuple`](https://docs.python.org/3/library/collections.html#collections.namedtuple).

* Podobnie stałe w rodzaju
```python
    COLORS = {
        'yellow': (181, 137, 0),
        'orange': (203, 75, 22),
        ...,
    }
```
zyskują po zmianie na
[`enum.Enum`](https://docs.python.org/3/library/enum.html#creating-an-enum).

* Importowanie modułu nigdy nie powinno mieć skutków ubocznych
— takich jak wypisywanie tekstu, otwieranie okien,
odczytywanie (zapisywanie?!) bazy danych, wykonywanie
długich obliczeń, wystrzelenie pocisków balistycznych itp.
Są to zawsze skutki kodu lewitującego poza funkcjami.
Taki kod należy powkładać do funkcji, a te wywoływać
z funkcji `main()`.

* Koniec głównego modułu programu powinien się przedstawiać
jak poniżej. Oczywiście nie wszystkie sekcje funkcji `main()`
muszą wystąpić w każdym programie.

    ```python
    def main():
        [inicjalizacja bibliotek zewnętrznych]
        [tworzenie obiektów odpowiednich klas]
        [wywołanie funkcji lub metody wprawiającej te obiekty w ruch]
        [sprzątanie]


    if __name__ == '__main__':
        main()
    ```

* Dokładne testy do wszystkich klas i funkcji programu
nie są wymagane na zaliczenie, ale trochę testów — tak.
Chodzi o to, żeby Państwo przećwiczyli ich pisanie.
W plikach z testami wystarczy jeden docstring na początku
i nie trzeba zamieniać magicznych liczb itp. na
zdefiniowane stałe. Ogólny wzór pliku `spam_test.py`
do testowania zawartości pliku `spam.py` pokazano
poniżej. Pełna dokumentacja modułu `unittest` znajduje się
[tutaj](https://docs.python.org/3/library/unittest.html).

    ```python
    """Testy modułu spam."""

    import unittest

    import spam


    class HamTest(unittest.TestCase):

        def setUp(self):
            self.ham = spam.Ham(price=42)

        def test_open(self):
            self.assertTrue(self.ham.open())

        def test_default_price(self):
            self.assertEqual(self.ham.price, 42)

        def test_set_price(self):
            self.ham.set_price(39)
            self.assertEqual(self.ham.price, 39)


    class EggsTest(unittest.TestCase):

        ...


    if __name__ == '__main__':
        unittest.main()
    ```

* Widok kolorów w stylu `(255, 0, 0)` przypomina mi czasy
butelek z mlekiem za progiem mieszkania, magnetofonów kasetowych
i 16-kolorowych trybów graficznych. Z 16.777.216 barw da się
wybrać ciekawsze. Ja lubię paletę
[Solarized](https://ethanschoonover.com/solarized/);
jest też mnogość innych.

### 4. Rady wyższego poziomu

* [Kopiuj-wklejoza](https://en.wikipedia.org/wiki/Copy-and-paste_programming)
(w najostrzejszych przypadkach mająca miejsce
między różnymi plikami) nie przystoi prawdziwym programistom.
Po to są moduły, dziedziczenie, metody, funkcje, pętle itp.,
żeby z nich korzystać. W łagodniejszych przypadkach,
kiedy po wklejeniu trzeba coś pozmieniać, można stosować zasadę
[„do trzech razy sztuka”](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)).
* Dobry konstruktor poznaje się po tym, że tylko
łączy w całość przekazane mu inne, wcześniej
skonstruowane obiekty. Dzięki takiemu
[„wstrzykiwaniu zależności”](https://pl.wikipedia.org/wiki/Wstrzykiwanie_zale%C5%BCno%C5%9Bci)
(*dependency injection*) znacznie łatwiej
jest testować daną klasę.
* Zły konstruktor poznaje się po:
    * wywołaniach innych konstruktorów
      (`super().__init__()` jest OK);
    * wywołaniach funkcji, które zmieniają globalny
      stan programu;
    * instrukcjach warunkowych lub pętlach;
    * inicjalizacji atrybutów bardziej skomplikowanej
      niż zwykłe przypisania;
    * tworzeniu obiektu, który potem trzeba jeszcze
      doinicjować inną metodą;
    * dziwnych obejściach, nie korzystających z metody
      `__init__()` po to, żeby móc zwracać kod błędu.
* Złe metody poznaje się po:
    * korzystaniu ze swoich argumentów nie wprost,
      tylko po to, żeby się dostać do innych obiektów;
    * naruszaniu
      [reguły Demeter](https://pl.wikipedia.org/wiki/Prawo_Demeter),
      czyli przechodzeniu przez więcej niż jeden obiekt,
      np. `self.pies.ogon.merdaj()` zamiast poprawnego
      `self.pies.merdaj()`, przy czym zmyłka polega na tym,
      że błąd daje o sobie znać tutaj,
      a leży w niedorobionej klasie `Pies`
      (gwoli jasności: w regule Demeter nie chodzi o liczenie kropek,
      tylko o nierozmawianie z obiektami oddalonymi od `self`;
      takie odwołania jak `constants.SpamEnum.HAM` są koszerne).
* Złe klasy poznaje się po:
    * opisie zawierającym wyraz „i”;
    * rozłącznych zbiorach metod, które operują
      na rozłącznych zbiorach atrybutów;
    * atrybutach zmienianych z zewnątrz obiektu
      przez bezpośrednie przypisania do obiektu
      zamiast korzystania po bożemu z jego metod.

### Miłego programowania!
