---
layout: post
title: "Refaktoryzacja - szkielet"
date: 2018-02-09
---

# {{ page.title }}

## Refaktoryzacja

1. Korzyści
    1. Narzędzia
        1. Podejście 'od problemu do rozwiązania' przy refaktoryzacji
        1. Podejście systemowe
        1. Pętla 'koncepcja-cele-implementacja-weryfikacja' w praktyce, w sposób możliwy do powtórzenia.
        1. Celowość, iteracyjność, kryterium końca
    1. Korzyści
        1. Inżynierskie podejście do przebudowania kodu pod kątem celu
        1. Sprawdzenie, czy zmiany jakie chcecie wprowadzić są "lepsze"
        1. Dostaniecie recepturę, która u mnie się sprawdza przy małych i dużych.
        1. Refaktoryzacja nie jest straszna
1. Streszczenie myśli przewodniej
    1. Refaktoryzacja to przebudowa nie zmieniająca wymagań funkcjonalnych.
    1. "Refaktoryzacja" sama w sobie nie ma sensu. Musi być "refaktoryzacja do czegoś", pod kątem celu.
    1. Przebudowa istniejącego kodu wymaga przeanalizowania całości systemu, nie tylko tego elementu systemu.
    1. Sukces refaktoryzacji zależy od zidentyfikowania problemu i zaplanowania działań.
    1. Proponuję podejście: "Kontekst -> problem -> ideał -> (strategia <-> weryfikacja strategii) -> (działanie -> weryfikacja wyniku)"
    1. Iteracyjność, ewolucja a nie rewolucja. Ustawienie warunków sukcesu.
    1. Bez zrozumienia problemu i domeny refaktoryzacja raczej nie przyniesie maksymalnego zwrotu.
    1. Jeśli nie wiesz po czym poznasz sukces, nie osiągniesz sukcesu i nie obronisz rozwiązania.
1. Kontekst refaktoryzacji
    1. Co to: Przebudowa bez zmian wymagań funkcjonalnych
    1. Wyjaśnienie systemu: przykład silnika parowego, który ma działać też na Syberii jak i na pustyni
        1. Część, Całość, Otoczenie
    1. Wszystko się zmienia przez zmianę Otoczenia biznesowego. Refaktoryzacja jest dostosowaniem do CZEGOŚ
    1. Restrukturyzacja w firmie. Firma 3-osobowa, firma 10000-osobowa.
    1. Ten artykuł unika politycznego aspektu refaktoryzacji
1. Jak to robić - pobieżnie?
    1. Kontekst -> problem -> ideał -> (strategia <-> weryfikacja strategii) -> (działanie -> weryfikacja wyniku)
    1. Iteracyjne podejście: Ewolucja, nie rewolucja
    1. Przepisanie lub przeniesienie
    1. Kryterium końca
1. Przykład z regexami
    1. Kontekst: regexy
    1. Problem: nowa osoba ma dużo kopiowania i wklejania kodu by zrobić nowy regex
    1. Poznam sukces po: łatwo dodać nowy element, nawet junior da radę, bez durnego kopiowania
    1. Ideał: losowy junior może łatwo dodać nowy regex
    1. Strategia 1: wydzielenie regexów do stringów
    1. Weryfikacja str 1: PORAŻKA. Regexy są trudne dla juniora
    1. Strategia 2: parametryzacja regexów
    1. Weryfikacja str 2: Sukces, junior potrafi pracować z funkcjami
    1. Działania: zbiór kroków
    1. Weryfikacja wyniku: 
        1. porównanie czasu na dodanie nowego elementu przez weterana (mnie)
        1. sprawdzenie jak łatwo dodać nowy regex
    1. Sami możecie zrobić to ćwiczenie
        1. Link do ćwiczenia
        1. Opisanie ćwiczenia
        1. Link do zbiorczego commita
        1. Link do brancha na rdbutlerze
1. Jak to robić - omówienie?
    1. Kontekst -> problem -> ideał -> strategia <-> weryfikacja strategii -> działanie -> weryfikacja wyniku
    1. Iteracyjne podejście: Ewolucja, nie rewolucja
    1. Przepisanie lub przeniesienie
    1. Kryterium końca
1. Co jest potrzebne?
    1. Domena
    1. Mierzalny sposób weryfikacji
    1. Kontekst: w czym działam (500 milisekund, hardware zajmuje 600 milisekund)
    1. Kontekst: Otoczenie jako System (problem bazy danych z nullami)
1. Podsumowanie
    1. Niektóre cele są sprzeczne; dlatego nie ma refaktoryzacji idealnej
    1. Stożek nieoznaczoności: najlepiej pisać kod pod kątem "łatwo go zmienić", nie "idealny kryształ"
    1. Kod to UI programisty. Skupienie na testowalności i mutowalności
    1. Nie znam żadnych "uniwersalnych technik" - wszystkie są kontekstowe
    1. Obszary ciepłe aplikacji, obszary zimne aplikacji - krystalizacja z czasem
    1. Refaktoryzacja to przede wszystkim problem polityczny
1. Wykazanie korzyści
    1. Przebudowanie kodu pod kątem celu
    1. Czy zmiany są zmianami "na lepsze"?
    1. Receptura jak to robić by działało.
    1. Refaktoryzacja nie jest straszna.


