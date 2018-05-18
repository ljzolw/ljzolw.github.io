---
layout: post
title: "Refaktoryzacja - kolejne artykuły"
date: 2018-05-18
---

# {{ page.title }}

## 5. Selekcja strategii

(Porozmawiajmy o kosztach (surowcach))
(wstęp do IET)
(Skuteczna refaktoryzacja i jej obronienie)
(Sprzedajmy biznesowi refaktoryzację)

## 6. Biznes może nie podjąć refaktoryzacji
    <zaufanie>
    <korzyści - demo 10 mln czy refaktor teraz>
    <używanie argumentów pod kątem korzyści biznesu>


===============

Pamiętacie kilka powodów refaktoryzacji, które wymieniliśmy?

* Wydajność (performance)
* Czytelność kodu
* Utrzymywalność
* Prędkość wdrożenia
* Łatwość zmian kodu

Po czym możemy poznać, że to się udało?

Zacznijmy od czegoś prostego - **wydajność**.

Załóżmy, że mamy aplikację webową i mamy odpowiedzi sięgające od 500 milisekund do 5 sekund, przy założeniu, że mamy internet. [Za raportem gomez](http://www.mcrinc.com/Documents/Newsletters/201110_why_web_performance_matters.pdf), jeśli odpowiedź strony internetowej przekroczy 3 sekundy, około 40% klientów opuści stronę internetową bez czekania.

Więc z perspektywy marketingu, _jak najmniejszy czas ładowania i reakcji strony_ liczony w sekundach jest _skalą wydajności_.

* **wydajność**: skala = sekundy; jak najmniejszy czas ładowania i reakcji strony internetowej na działania użytkownika

To może... **użyteczność / czytelność kodu**?

Hm, czemu zależy nam na czytelności kodu? Czy to _światło_ (wynik) czy _przełącznik_ (środek do celu)? Powiedzmy na potrzeby przykładu, że czytelność kodu jest nam potrzebna, by wiedzieć gdzie mamy wprowadzać zmiany oraz by szybciej móc wdrożyć nowych ludzi do projektu.

Szybsze wdrożenie ludzi do projektu ma potencjalnie prostą skalę - jak najkrótszy czas wdrażania ludzi do projektu, mierzony w godzinach.

Ale jak określimy, czy dany programista jest wdrożony? Może... po prędkości przeprowadzania zmian? Wtedy to by było coś w stylu _"ile czasu (godziny) nowemu programiście zajmuje wprowadzenie Niewielkiej Zmiany do kodu"?_ Nie jest to idealne (i da się lepiej rozpisać), ale pokazuje na czym nam zależy.

Jaka jest skala "gdzie mamy wprowadzać zmiany"?

Może ilość pytań zadanych innym członkom zespołu? Może czas zmarnowany na szukanie miejsca gdzie wprowadzić zmianę? To drugie brzmi sensowniej. Może warto je powiązać? Na potrzeby przykładu zostanę przy tym drugim.

Czyli:

* **czytelność kodu** jest skalą złożoną:
    * **szybsze wdrożenie ludzi do projektu**: skala = godziny; ile czasu zajmuje nowemu programiście wprowadzenie Niewielkiej Zmiany do kodu (cel: jak najmniej)
    * **gdzie wprowadzić zmianę**: skala = godziny; ile czasu zostało zmarnowane na połapaniu się, gdzie wprowadzić zmianę (cel: jak najmniej)

![Dekompozycja - skomplikowana wartość rozkłada się na składowe elementy (to, co powyżej opisałem jako tekst)](/img/1805/180518-scale.png)

Czy to powyższe jest perfekcyjne? Nie. Ale jeśli rozmawiam z kolegą o refaktoryzacji i ów mówi "ta zmiana poprawi czytelność kodu", to ja _nie mam pojęcia o czym on mówi_. Moja czytelność kodu i jego czytelność kodu mogą być zupełnie innymi rzeczami. **To zwłaszcza jest problemem, gdy rozmawiają ze sobą osoba techniczna i nietechniczna**

Nawet tak pobieżne doprecyzowanie jak to co zrobiłem przynosi korzyści. To jest to, co próbuję tu pokazać - **nie musimy zrobić idealnego doprecyzowania**. Wystarczy nam doprecyzowanie lepsze niż to, co mamy, jak długo zmniejsza stożek nieoznaczoności. Wystarczy, że wiemy co jest ważnego w naszej refaktoryzacji.

Co ważne - musimy mieć informację o stanie aktualnym, sprzed refaktoryzacji:

* W pokoju jest ciemno (więc proszę o zapalenie światła, by było jasno).
* Czas odpowiedzi strony to 5 minut (więc proszę o zredukowanie poniżej 2 minut).
* Średnio marnujemy godzinę szukając miejsca do wprowadzenia zmiany (więc proszę o drastyczną redukcję tego czasu).

Posiadanie tej informacji o stanie aktualnym pozwala PORÓWNAĆ wynik po refaktoryzacji ze stanem sprzed refaktoryzacji. Pozwala udowodnić, że to co zrobiliśmy było przydatne i korzystne. Że faktycznie dostarczyliśmy wartość odbiorcy.

Pozwala to obronić naszą refaktoryzację przed kolegami, przełożonymi i klientem na poziomie obserwowalnych wyników, nie "czy ktoś uważa to za ładne, czy nie".

**Jak tego użyć** - Czy wiemy co jest środkiem a co celem? Czy wiemy jak udowodnić, że osiągnęliśmy sukces zgodnie z celem? Czy wiemy jak zbadać, czy poprawiliśmy sytuację czy ją pogorszyliśmy? Czy znamy punkt startowy i potrafimy go mniej więcej umiejscowić na skali?
