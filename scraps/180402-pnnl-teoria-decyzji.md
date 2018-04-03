---
layout: post
title: "Parking nie na lotnisku - Teoria Decyzji"
date: 2018-04-02
---

# {{ page.title }}

Mając tabelkę zgodną z trzema wymiarami Christensena:

| Rozwiązanie          | Funkcjonalny           | Emocjonalny                   | Społeczny                 |
|----------------------|------------------------|-------------------------------|---------------------------|
| zostanie samo w domu | mała szansa na problem | będę się marwił               | wyjdę na wyrodnego ojca   |
| Babcia               | perfekcyjnie!          | wie więcej o dzieciach niż ja | oczekiwane społecznie     |
| opiekunka do dzieci  | zna się na tym         | obca osoba w domu :-(         | poprawne społecznie       |
| **nie idę do teatru** | zadziała              | szkoda teatru, ale zadziała   | oczekiwane społecznie     |
| wujek morderca       | ma swoje, zna się      | zero zmartwień, rodzina       | zabiorą mi dziecko...     |

możemy to teraz połączyć z Teorią Decyzji, a dokładniej z Funkcją Użyteczności. W skrócie, funkcja użyteczności to jest "jak bardzo dla MNIE w TYM KONTEKŚCIE jest to dobre rozwiązanie". 

Dodajmy do powyższej tabelki jeszcze "Koszt", oznaczający "koszt pieniężny, przysługowy itp". Stwórzmy arbitralną funkcję użyteczności (im więcej, tym lepiej):

_fu = Fun * Emo * Społ * Koszt_

I dodajmy arbitralne liczby do powyższej tabelki, by móc określić jak bardzo "wyceniamy" powyższe koszty:

| Rozwiązanie          | Funkcjonalny | Emocjonalny | Społeczny | Koszt | Wynik |
|----------------------|--------------|-------------|-----------|-------|-------|
| zostanie samo w domu | 0.5          | 0.2         | 0.4       | 1     | 0.04  |
| Babcia               | 1            | 1           | 1         | 0.5   | 0.5   |
| opiekunka do dzieci  | 0.75         | 0.75        | 0.8       | 0.5   | 0.225 |
| **nie idę do teatru** | 0.25        | 1           | 1         | 1     | 0.25  |
| wujek morderca       | 1            | 0.9         | 0.1       | 0.7   | 0.06  |

Powyższe pokazuje, że przy tak arbitralnie przyporządkowanych wagach (i funkcji użyteczności):

* najlepszym rozwiązaniem zdecydowanie jest Babcia
* jak Babcia się nie zgodzi, albo poprosić opiekunkę albo nie iść do teatru
* nie można wykorzystać "członka rodziny nie akceptowalnego społecznie" czy "zostawić dziecka samego w domu"

Oczywiście, funkcje użyteczności mają różne krzywe; np:

_fu = norm(Fun) * norm(Emo) * norm(Społ) * norm(Koszt)_

dla funkcji normalizacji takiej jak:

* x < 0.3: return 0.1*x
* x między 0.3 a 0.9: return x
* x > 0.9: return 1.5*x

by móc mocniej zaakcentować nieakceptowalne i pożądane skrajne wartości w powyższym wzorze. Ale to temat na inny artykuł. Takie coś stosuje się np. przy określeniu jak inwestować.
