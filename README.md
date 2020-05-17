# Jak zaliczyć projekt z Pythona
## Zalecenia dla studentów przedmiotu Języki Symboliczne

Zamiast powtarzać podobne komentarze w wielu projektach,
spisałem je tutaj. Uwagi do tego dokumentu można zgłaszać
przez *Issues*.

### 1. Pliki

* Repozytorium nie powinno zawierać zbędnych katalogów
`__pycache__`, `.idea`, `venv` itp. ani zbędnych plików,
np. `desktop.ini`. Wskazane jest za to dodanie pliku
`.gitignore` o treści odpowiedniej dla projektów pisanych
w Pythonie. Kto nie dodał tego pliku przy tworzeniu
repozytorium,
[tutaj](https://github.com/github/gitignore/blob/master/Python.gitignore)
znajdzie odpowiednią treść.

* Nazwy bibliotek zewnętrznych niezbędnych do działania
programu należy umieścić w pliku `requirements.txt`.
Dzięki temu przez polecenie `pip install -r requirements.txt`
można je wszystkie naraz zainstalować.
[Tutaj](https://pip.pypa.io/en/stable/reference/pip_install/#example-requirements-file)
znajduje się przykładowa treść tego pliku. Sądzę, że
w większości przypadków wystarczy wymienienie nazw bibliotek
bez uściślania ich wersji, np.

    ```
    numpy
    pygame
    ```

### 2. Uwagi drobiazgowe

Proszę się zapoznać z treścią
[poradnika dla programistów Pythona w firmie Google](https://google.github.io/styleguide/pyguide.html) i stosować do jego zaleceń.
Nie wymagam tylko podawania w docstringach funkcji
i metod sekcji `Args:`, `Returns:` i `Raises:`
ani w docstringach klas sekcji `Attributes:`
([punkty 3.8.3 i 3.8.4](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods)). Najczęściej ignorowane zalecenia
powtarzam poniżej.

* Proszę zainstalować narzędzie
[`pylint`](https://pypi.org/project/pylint/),
przepuszczać przez nie swój kod i poprawiać
wskazane przez nie miejsca
([punkt 2.1](https://google.github.io/styleguide/pyguide.html#21-lint)).

* Proszę używać instrukcji `import` wyłącznie
z nazwami pakietów i modułów, a nie poszczególnych
klas czy funkcji, a już broń Boże nigdy nie pisać
`from spam import *`. Dzięki temu unikną Państwo
zaśmiecania przestrzeni nazw nie wiadomo czym
([punkt 2.2](https://google.github.io/styleguide/pyguide.html#22-imports)).

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
się atrybutami klasy.

* Globalne stałe na poziomie modułów są natomiast
mile widziane, a nawet wymagane zamiast magicznych
liczb, napisów czy większych obiektów. Na przykład
zamiast `7` należy zdefiniować stałą `BOARD_WIDTH = 7`,
zamiast napisów `'left'` i `'right'` — stałe
`LEFT, RIGHT = range(2)`, zamiast krotki `(65, 181, 137)`
— stałą `BACKGROUND_COLOR = (65, 181, 137)`.
Przypominam, że stałe zwyczajowo zapisujemy
`WIELKIMI_LITERAMI_Z_PODKREŚLNIKAMI`
([punkt 3.16.4](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)).
Nie od rzeczy będzie przypomnieć, że XXI wiek
trwa już dość długo i w nazwach można swobodnie
korzystać z polskich liter, jeśli ktoś ma taką ochotę.

* Wymagane są docstringi do modułów, klas, metod
i funkcji, chyba że są jednolinijkowe lub w inny
sposób oczywiste
([punkt 3.8](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations)). Należy się
zapoznać z zasadami pisania docstringów i ich
przestrzegać. W skrócie: pierwszy wiersz ma zawierać
pełne a krótkie zdanie (na końcu zdania stawiamy kropkę)
opisujące działanie metody lub funkcji, np.
`"""Zwraca przycisk naciśnięty przez użytkownika."""`
Jeśli wymagany jest dłuższy opis, należy go podać
poniżej, po pustym wierszu.

* Warto pamiętać, że czytelnik programu nie musi
znać tych samych skrótów, co my. Wniosek: skróty
są niedozwolone, zarówno w nazwach, jak w docstringach
i komentarzach
([punkt 3.16](https://google.github.io/styleguide/pyguide.html#316-naming)).

### 3. Inne uwagi

* Python to nie Java. Niepotrzebne jest tworzenie
osobnych plików na małe klasy w stylu
`PrzechowywaczMonet` tylko po to, żeby później
musieć pisać `import PrzechowywaczMonet` i
`przechowywacz_monet = PrzechowywaczMonet.PrzechowywaczMonet()`.

* Koniec głównego modułu programu powinien się
przedstawiać jak poniżej. Oczywiście nie wszystkie
sekcje funkcji `main()` muszą wystąpić w każdym
programie.

    ```python
    def main():
        ... [inicjalizacja bibliotek zewnętrznych]
        ... [tworzenie obiektów odpowiednich klas]
        ... [wywołanie funkcji/metody wprawiającej te obiekty w ruch]
        ... [sprzątanie]


    if __name__ == '__main__':
        main()
    ```

* Dokładne testy do wszystkich klas/funkcji programu
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
            self.ham = spam.ham(price=42)

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

### 4. Rady wyższego poziomu

* Dobry konstruktor poznaje się po tym, że tylko
łączy w całość przekazane mu inne, wcześniej
skonstruowane obiekty. Dzięki takiemu „wstrzykiwaniu
zależności” (*dependency injection*) znacznie łatwiej
jest testować daną klasę.
* Zły konstruktor poznaje się po:
    * wywołaniach innych konstruktorów;
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
    * niekorzystaniu ze swoich argumentów bezpośrednio,
      tylko używaniu ich po to, żeby się dostać do innych
      obiektów;
    * naruszaniu
      [prawa Demeter](https://pl.wikipedia.org/wiki/Prawo_Demeter),
      czyli przechodzeniu przez więcej niż jeden obiekt,
      np. `self.pies.ogon.merdaj()` zamiast poprawnego
      `self.pies.merdaj()`.
* Złe klasy poznaje się po:
    * opisie zawierającym wyraz „i”;
    * rozłącznych zbiorach metod, które operują
      na rozłącznych zbiorach atrybutów;
    * atrybutach, których wartości są zmieniane
      przez bezpośrednie przypisania do obiektu
      zamiast korzystania po bożemu z jego metod.

### Miłego programowania!
