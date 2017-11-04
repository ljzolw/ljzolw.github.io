---
layout: post
title: "Szkolenie ze wzorców, wersja 2"
date: 2017-08-15
---

## 1. Zakres artykułu

Rozpatruję tematy takie jak: 

* 

Wartość dodana dostarczona przeze mnie:

* 

## 1. Jak przejść przez to szkolenie samemu?

Dla osób, które chcą same spróbować zrobić sobie to autoszkolenie, kroki poniżej:

1. Wymagania
    1. Podstawowa znajomość języka C#. Brak dodatkowych bibliotek.
    2. Visual Studio Community Edition (pisanie na 2017).
    3. Najnowsza wersja .NET.
    4. Podstawowa znajomość narzędzia mstest do testów automatycznych, wbudowane w VS Community.
    5. Umiejętność czytania komentarzy w kodzie w języku angielskim.
2. Jak przejść przez to szkolenie?
    1. Link do głównej strony szkolenia [TUTAJ](https://gitlab.com/lj_zolw/CasinoMeowMeow),
    2. Ściągnij sobie ćwiczenia z brancha exercises_100_200 (jeśli nie znasz gita, ściągnij [TEGO ZIPA](https://gitlab.com/lj_zolw/CasinoMeowMeow/repository/exercises_100_200/archive.zip))
    3. Przeczytaj dokument CasinoMeowMeow_Modeling; powinien być w folderze Docs.
        1. Zwróć uwagę na domenę, z jaką pracujesz - co to za program i co robi
        2. Spróbuj zastanowić się, jak taki program powinien być napisany by działać.
    4. Odpal w VS Community plik CasinoMeowMeow.sln; powinien być w folderze CasinoMeowMeow.
    5. Sprawdź czy wszystko działa jak powinno:
        1. Projekty się budują poprawnie.
        2. Testy są czerwone poza kilkoma zielonymi.
    6. Przejdź przez wszystkie testy po kolei, od T101 (powinien działać) do T213.
        1. Idź po kolei czytając komentarze w testach. 
        2. Twoim celem jest sprawienie, by wszystkie testy o nazwie 'Txxx' były zielone jednocześnie.
        3. Podczas rozwiązywania nie zapominaj o domenie programu nad którym pracujesz. Czemu to tu jest? Jaką to pełni rolę?
    7. Gdy wszystkie testy Txxx działają:
        1. Niekoniecznie testy nie-Txxx będą działać. Testy są zaprojektowane tak, byś pracował z kilkoma wzorcami projektowymi a do pełnego działania programu potrzeba czasem tyci innego ułożenia kodu.
    8. Gdy już wszystkie testy Txxx działają lub utkniesz, przełącz się na branch master (lub ściągnij [TEGO ZIPA](https://gitlab.com/lj_zolw/CasinoMeowMeow/repository/master/archive.zip))
        1. Tam masz dokładnie tą samą solucję, ale rozwiązaną. Możesz porównać swoje rozwiązanie z tym, które jest zaproponowane.
    9. Jeśli nadal Ci mało i chcesz poćwiczyć
        1. Przeczytaj jeszcze raz ten dokument. Zauważysz, że w dodatku jest propozycja gry w Oczko.
        2. Przełącz się na branch master (tam gdzie wszystko działa)
        3. Zaimplementuj grę w Oczko. Dodaj odpowiednie komponenty tak, by dało się grać w GuessACard lub w Oczko.

## 2. Czemu przebudowałem szkolenie?


## 5. Obserwacje i wnioski

