EDYTOR ŚRUB I KOTEW ADVANCE STEEL - WERSJA TESTOWA (prototyp)
(English version: README_EN.txt)
==============================================================

CO TO JEST
Zewnętrzny program do edycji katalogów śrub i kotew w bazie Advance Steel, w stylu Tekli:
- zestawy śrub (podkładki, nakrętki) w widoku od łba do końca śruby, nowe normy i klasy, własne podkładki i nakrętki,
- kotwy: nowe długości, średnice i klasy, nowe normy kotew, podgląd kształtu kotwy z wymiarami, blachą i zestawem.
Pracuje na bazie AstorBase (SQL Server LocalDB). Testowany: Advance Steel 2026, wersja krajowa INT.
Inne wersje programu Advance Steel i inne wersje krajowe nie były sprawdzane.

WYMAGANIA
- Windows i Advance Steel z bazą w LocalDB (sprawdzone: Advance Steel 2026 INT)
- Internet przy pierwszym uruchomieniu (doinstaluje Pythona, moduł pyodbc i sterownik ODBC)
- Zgoda administratora, jeśli Windows zapyta o instalację sterownika

PRZED TESTEM: KOPIA ZAPASOWA (zrób obowiązkowo)
1. Zamknij Advance Steel i Management Tools.
2. W wierszu poleceń wpisz:  sqllocaldb stop AdvanceSteel2026
   (nazwę instancji sprawdzisz poleceniem  sqllocaldb i )
3. Skopiuj cały folder  C:\ProgramData\Autodesk\Advance Steel 2026\INT\Steel\Data
   w bezpieczne miejsce. (INT = wersja krajowa, u Ciebie może być inna.)
Program robi też własną kopię (.bak) w folderze "backups" obok programu, ale ręczna kopia jest pewniejsza.

INSTALACJA (zalecana)
1. Rozpakuj CALE archiwum ZIP do dowolnego folderu (np. Pobrane).
2. Kliknij dwukrotnie  Instaluj.bat  - instalator doinstaluje Pythona, pyodbc i sterownik ODBC (jesli ich brakuje; do
   tego potrzebny jest internet, a sterownik moze wymagac zgody administratora), skopiuje program do
   %LOCALAPPDATA%\AS_Edytor_Srub i utworzy skrot "Edytor srub i kotew Advance Steel" na pulpicie i w menu Start.
3. Aktualizacja: rozpakuj nowa paczke i uruchom Instaluj.bat jeszcze raz - ustawienia i kopie zapasowe zostaja.
4. Odinstalowanie: Odinstaluj.bat (usuwa program i skroty; bazy Advance Steel nie zmienia).
Gdy program nie wystartuje ze skrotu, w jego folderze powstaje plik blad.log - przeslij go razem ze zrzutem ekranu.

URUCHOMIENIE (bez instalatora)
1. Rozpakuj paczkę do zwykłego folderu na dysku (nie uruchamiaj z podglądu w archiwum).
   Program składa się z pliku advance_steel_bolt_editor.py i folderu as_editor - muszą leżeć obok siebie.
2. Uruchom Advance Steel lub Management Tools (baza musi być podłączona).
3. Kliknij dwukrotnie  uruchom_edytor_srub.bat  (pierwszy raz może potrwać kilka minut).
4. W oknie programu kliknij "Połącz". Napis u góry zmieni kolor na zielony.
   Jeśli używasz innej wersji krajowej niż INT, kliknij "Wykryj wersje" i wybierz właściwą.
5. Język: przycisk "English" / "Polski" w prawym górnym rogu przełącza język całej aplikacji (wybór jest zapamiętany
   w pliku bolt_editor_settings.json). Po przełączeniu okno buduje się od nowa - połączenie z bazą zostaje, ale
   ostatnio wybrane normy zostają.

UKŁAD OKNA
Trzy zakładki główne:
- Kotwy - konfigurator kotew: wybierz normę, zaznacz kotwę-wzorzec i użyj "+ Długość", "+ Średnica" albo "+ Klasa".
- Zestawy śrub - zestaw nakrętek i podkładek dla normy śruby (widok jak w Tekli).
- Narzędzia - rzadsze funkcje: nowa podkładka / nakrętka, normy i klasy śrub, kopia normy, kopiowanie kotew,
  usuwanie moich kotew, przeglądarki tabel.
Listy norm (kotwy i śruby) możesz zawęzić wpisując tekst, np. "hilti" - zostaną tylko pasujące pozycje; Enter wybiera
pierwszą z nich. W konfiguratorze kotew jest też filtr typu kotwy (Normal, Headed, Threaded, Hooked, J).
Program pamięta ostatnio wybraną normę kotwy, normę śruby i właściciela wpisu (także po zmianie języka).

TEST 1: DODANIE DŁUGOŚCI / ŚREDNICY / KLASY DO KOTWY (zakładka "Kotwy")
1. Wybierz normę kotwy (np. HILTI HSA) i zaznacz kotwę, która ma być wzorcem.
2. Kliknij jeden z przycisków:
   "+ Długość" - otwiera małe okno: wpisz nowe długości i sprawdź wymiary (domyślnie z wybranej istniejącej długości).
     Typ, średnica, klasa i zestaw kotwy zostają bez zmian, a nowe długości są dopisywane do tej kotwy (także fabrycznej:
     jej dotychczasowe wiersze nie są zmieniane, dopisane wiersze mają Twojego właściciela). Dopisane długości usuniesz
     przyciskiem "Usuń zaznaczone moje długości" (zielone wiersze w tabeli pod listą kotew).
   "+ Średnica" - wpisz nową średnicę i długości; klasa, zestaw i wymiary biorą się ze wzorca.
   "+ Klasa" - wpisz nową klasę; średnica, zestaw i wymiary biorą się ze wzorca.
3. Długości wpisz jako listę lub zakresy (np. 300-1000/100, 1200), kliknij "Od-do co..." albo wklej kolumnę z Excela.
4. Schemat kotwy po prawej odświeża się sam. Skład zestawu (nakrętki, podkładki) jest w lewej kolumnie; element,
   którego nie ma w bazie dla tej średnicy, jest czerwony z dopiskiem "(brak)". Zaznacz go i kliknij "Zamień..." albo
   zaznacz "Zapisz bez brakujących elementów".
5. Tabela "Podgląd do zapisu" pokazuje, co zostanie zapisane (Status NOWA / NOWA długość). Kliknij "Zapisz do bazy".
6. W Advance Steel kliknij "Update defaults" (albo uruchom go ponownie) i sprawdź kotwę w Management Tools
   (zakładki Parametry i Zestaw) oraz w modelu.
Inne okna kotew (przycisk "Dodaj kotwę..." i menu "Więcej"): pojedyncza kotwa w normie, kilka średnic naraz
(dwuklik w komórkę tabeli = edycja w komórce, Ctrl+V = wklejenie z Excela: jedna kolumna to długości, kilka kolumn to
długość i kolejne wymiary), nowa norma kotwy. Własne kotwy (zielone) edytujesz przyciskiem "Edytuj zaznaczoną...".

TEST 2: ZESTAW ŚRUB (zakładka "Zestawy śrub")
1. Wybierz normę 4014. Powinien pojawić się rysunek i skład (podkładki, nakrętka).
2. Klasa: kliknij 10.9. Średnica: kliknij jedną, np. Ø 16 (domyślnie zaznaczone są wszystkie).
3. Klucz zestawu: wpisz TEST1 (dla każdego testera inny, np. TEST_JK). Nazwa wyświetlana: Test NaW.
   Aby dodać DODATKOWY zestaw na wzór istniejącego: "Skopiuj istniejący zestaw...", potem wpisz NOWY klucz.
4. Zmień skład (np. dodaj podkładkę z listy dostępnych elementów; "Zamień..." podmienia zaznaczony element).
   Pod tabelą podglądu widać podsumowanie "Do zapisu: ..., pominięte: ..." i powód pominięcia w kolumnie Status.
5. Kliknij "Zapisz zestaw do bazy..." i potwierdź. Pojawi się komunikat o kopii zapasowej i "Zapisano 1 wierszy".
6. Sprawdzenie w programie: "Narzędzia" -> "Przeglądarka tabel (śruby)" -> SetOfBolts -> Filtr: TEST1.
   Powinien być wiersz z OwnerText = MOJA.
7. Sprawdzenie w Advance Steel: zamknij i uruchom ponownie Management Tools / Advance Steel.
   W Management Tools (Bolt -> Set) powinien być "Test NaW". W modelu wstaw śrubę
   (norma 4014, klasa 10.9, średnica 16) i wybierz ten zestaw. Sprawdź model 3D i zestawienie BOM.

TEST 3 (opcjonalnie): NARZĘDZIA
- "Nowa podkładka / nakrętka" - utwórz własną podkładkę na wzór istniejącej (np. mnożnik grubości 1.5).
  Dzięki temu uzupełnisz element, którego brakowało w zestawie.
- "Normy i klasy śrub" (cztery podzakładki):
  1) Słownik klas - wpisz w Filtr np. 5.6 lub A4. Jeśli klasy brak, dodaj ją (Key, RunName).
     Uwaga: A4 istnieje już w bazie jako klasa 1.4401 (nazwa wyświetlana A4).
  2) Nowa norma śruby - wybierz wzorzec (np. 4014 / 8.8), "Wczytaj wzorzec do tabeli", wpisz klucz np. TEST-4014,
     zaznacz klasę, w kolumnie Długości wpisz np. 40-60/10 i kliknij "Utwórz normę".
  3) Dodaj klasę do normy - norma 4014, klasa źródłowa 8.8, nowa klasa 5.6, wybierz jedną średnicę.
  4) Usuń moje wpisy - sprzątanie po teście (usuwa tylko wpisy z właścicielem MOJA).
- "Kopiuj kotwy / dodaj klasę" - kopia kotew do nowej normy albo nowa klasa dla wielu kotew naraz.

SPRZĄTANIE PO TEŚCIE
Śruby: "Narzędzia" -> "Przeglądarka tabel (śruby)": SetOfBolts -> Filtr TEST1 -> zaznacz wiersze -> "Usuń zaznaczone
MOJE wiersze". Potem to samo w tabeli Sets (wpis TEST1). Wpisy z "Normy i klasy śrub": podzakładka 4 (Usuń moje wpisy),
a klasę ze słownika usuniesz w podzakładce 1 (dopiero gdy nie jest już używana).
Kotwy: zakładka "Kotwy" -> przycisk "Usuń zaznaczoną" (kotwa MOJA) albo "Narzędzia" -> "Usuń moje kotwy". Uwaga: wpisy
dodane przy okazji (nowe źródło w tabeli Sources, nowy zestaw w Sets, nowa klasa) zostają w słownikach; zestaw usuniesz
w Przeglądarce tabel, klasę w Słowniku klas. Wpisów fabrycznych (DSC) program nie usuwa - są chronione.

PRZYWRACANIE BAZY (gdyby coś poszło nie tak)
Zamknij Advance Steel, wpisz  sqllocaldb stop AdvanceSteel2026  i podmień pliki w folderze
...\Steel\Data na swoją kopię. Po uruchomieniu Advance Steel baza wróci do stanu sprzed testów.

CO ZGŁASZAĆ
- Wersję Advance Steel i wersję krajową (INT, POL, ...)
- Dokładną treść komunikatu o błędzie (także z czarnego okna wiersza poleceń) albo zrzut ekranu
- Opis: co kliknięto, czego oczekiwano, co się stało

WAŻNE
- To prototyp. Nie testuj pierwszy raz na bazie produkcyjnej bez kopii folderu Data.
- Własne wpisy mają właściciela MOJA. Aktualizacja lub naprawa Advance Steel może je skasować
  (wtedy trzeba je utworzyć ponownie).
- Zapisane zmiany widać dopiero po ponownym uruchomieniu Advance Steel / Management Tools.
