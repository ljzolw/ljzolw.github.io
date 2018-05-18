---
layout: post
title: "Opłacalna biznesowo refaktoryzacja: co chcemy osiągnąć?"
date: 2018-05-18
---

# {{ page.title }}

## 1. Streszczenie myśli przewodniej artykułu

1. Refaktoryzacja jest TYLKO i wyłącznie narzędziem umożliwiających osiągnięcie celu, nie celem samym w sobie.
2. Sukces czy porażka refaktoryzacji są bardzo zależne od zrozumienia kontekstu projektu oraz celów projektu.
3. Pozytywny wynik dobrze przeprowadzonej refaktoryzacji da się udowodnić w sposób biznesowy.
4. Refaktoryzacja jest wielowymiarowym problemem. Nie jest możliwa jednoczesna optymalizacja wszystkich wymiarów.
5. W niektórych kontekstach X jest lepsze, w niektórych Y. Dla Ciebie ważne jest to, co jest lepsze w Twoim kontekście.

## 2. Historia pewnej refaktoryzacji

Paweł Programista wrócił do pokoju szczęśliwy. Udało mu się przekonać biznes do tego, by w ciągu dwóch tygodni zespół mógł posprzątać kod. A biznes się na to zgodził!

![Śpiewają ptaki, radość i szczęście. I w ogóle.](/img/1805/180518-happiness.png)

Paweł wiedział co robić. Największym problemem jaki zaobserwował Paweł były monolityczny charakter aplikacji. Wniosek był prosty - trzeba wprowadzić **mikroserwisy**!

Przekonał biznes, że z tym kodem nie da się już pracować; jest za duży, jest to nieczytelne, kod to jedno wielkie spaghetti i jakakolwiek zmiana będzie kosztowna. Słono płacić będzie musiał za to właśnie biznes.

W tym świetle refaktoryzacja o którą poprosił zespół była logiczną konsekwencją obecnego stanu aplikacji.

![Paweł wie co robić, widzi Prawdę. Jest też rysunek upadającego monolitu i budowanych mikroserwisów](/img/1805/180518-pawel.png)

Refaktoryzacja była oddana na czas. Wszystko się udało.

Dwa lata później do biznesu przyszła Daria Developerka. Powiedziała biznesowi, że z tym kodem nie da się pracować; nie da się niczego znaleźć, jest to nieczytelne, rozproszone i jakakolwiek zmiana zajmuje za dużo czasu. Zespół prosi o możliwość refaktoryzacji kodu...

Zaproponowała spojenie _mikroserwisów_ do czegoś bardziej **monolitycznego**.

![Daria wie co robić, widzi Prawdę. Jest też rysunek niszczonych mikroserwisów i budowanego monolitu](/img/1805/180518-daria.png)

Co się tu stało? I jak myślicie - co odpowie biznes na prośbę Darii po tym, jak propozycja Pawła nie rozwiązała problemu?

## 3. Analiza sytuacji

* Czy spotkaliście się z podobną sytuacją w waszych projektach? Refaktoryzacja, która "miała pomóc", ale czy naprawdę pomogła?
* Niby wiem, że idę w dobrym kierunku, ale kolega proponował inną refaktoryzację... może to jego rozwiązanie było lepsze?
* Z tyłu głowy pojawia się często cichy szept - czy niczego nie zepsuję? Czy na pewno idę w dobrą stronę?
* Jak udowodnić, że moje rozwiązanie jest faktycznie lepszego od tego co zaproponowała inna osoba? Nie chcę, by się ze mnie śmiali czy mnie atakowali...
* Jak zatem przeprowadzić SKUTECZNĄ refaktoryzację, którą da się obronić? I to w taki sposób, by "nic nie zepsuć"?

## 4. Wielowymiarowość refaktoryzacji

Dlaczego w ogóle próbujemy coś refaktoryzować?

Zacznijmy od tego, że refaktoryzacja to zmiana kodu bez zmieniania obserwowalnego zachowania ze strony użytkownika; zgodnie z tą definicją _optymalizacja_ jest formą _refaktoryzacji_.

Więc... po co nam to? Jest kilka potencjalnych powodów...

* **Czytelność kodu**: "Bo za wolno wprowadzamy zmiany; kilka godzin zajmuje znalezienie miejsca, w którym można coś zmienić"
* **Utrzymywalność**: "Ech, spróbuj tylko coś zmienić i zaraz pojawi się kilkanaście błędów. Dużo kopiowania i wklejania"
* **Prędkość wdrożenia**: "Ten kod? Nikt nie wie, jak to działa; nowy programista zaczyna się zwracać po roku"
* **Wydajność (performance)**: "Cóż; prędkość tej operacji zajmuje kilkanaście minut; w sam raz, by zjeść obiad"
* **Łatwość zmian kodu**: "W aktualnym systemie nie da się tego TAK zrobić. Mamy hack na hacku, no i używamy refleksji"

Powyższe powody to nie jedyne powody, ale powinny pokazać coś istotnego - mamy bardzo wiele różnych celów refaktoryzacji. A optymalizacja niektórych z tych celów może wpływać negatywnie na inne.

_Przykład: Jedną z metod optymalizacji aplikacji pisanych w C++ jest wprowadzenie fragmentów kodu pisanych w języku Assembler w odpowiednich miejscach. Wpływa to bardzo pozytywnie na wydajność (krytyczny fragment kodu działa szybciej), ale utrudnia czytelność kodu oraz łatwość wprowadzania zmian (lepiej tego nie dotykać) jak i spowalnia prędkość wdrożenia (nikt - poza autorem - tego kodu nie rozumie)._

![Część parametrów idzie do góry, część idzie jednocześnie w dół](/img/1805/180518-multiparam.png)

**Jak tego użyć** - zanim zaczniemy refaktoryzować, zastanówmy się, dlaczego próbujemy coś zmienić. Na co próbujemy wpłynąć. Co próbujemy naprawić.

## 5. Refaktoryzacja to Narzędzie, nie Cel

Problem polega na tym, że z mojego doświadczenia ludzie rzadko mówią "mamy problem z wdrażaniem nowych ludzi; zróbmy refaktoryzację pod kątem czytelności". Zwykle mówią coś w stylu "zróbmy refaktoryzację".

Jest to zupełnie inny komunikat, a druga strona może zrozumieć intencję stojącą za propozycją refaktoryzacji w dowolny sposób, albo nie odkryć jej w ogóle.

Zauważcie, że refaktoryzacja nie jest celem sama w sobie. Refaktoryzacja jest tylko **środkiem** prowadzącym do **jakiegoś konkretnego celu**.

_Przykład: Mamy do czynienia z dość trudnym do zrozumienia modułem. Jedną ze strategii przyspieszenia wdrożenia nowych ludzi do projektu może być przebudowa tego kodu. Inną strategią może być zbudowanie testów automatycznych, których jedyną funkcją będzie wdrażanie ludzi._

Oba zaproponowane w przykładzie rozwiązania mają swoje zalety i wady - ale znając cel, jesteśmy w stanie merytorycznie porozmawiać o korzyściach i kosztach. Następnie wybrać to rozwiązanie, które gwarantuje nam osiągnięcie postawionych celów przy akceptowalnych kosztach.

Zgodnie z powyższym, jesteśmy w stanie wybrać refaktoryzację, bo jest najlepszym rozwiązaniem danego problemu - nie dlatego, bo "trzeba refaktoryzować kod". Innymi słowy, możemy podjąć racjonalną decyzję na bazie tego, co doprowadzi nas do sukcesu mając na uwadze wcześniej określone cele.

![Refaktoryzacja czy szkolenie juniorów?](/img/1805/180518-choices.png)

**Jak tego użyć** - zanim zaczniemy refaktoryzować, pomyślmy. Czy na pewno refaktoryzacja jest najlepszą metodą rozwiązania tego problemu? Nie ma innej? Czy rozpatrywaliśmy w ogóle inne możliwości?

## 6. "Skuteczna" Refaktoryzacja jest pochodną kontekstu

Weźmy jako przykład "czytelność kodu". Wydawałoby się, że kod może być "czytelniejszy" lub "mniej czytelny", prawda?

Otóż, ta sama technika może być uznana za czytelniejszą lub mniej czytelną w zależności od tego, jaki zespół pracuje nad kodem.

Przykładowo, rozpatrzmy dwie techniki:

* Technika 1:
    * Każda czynność (funkcja, metoda) przypisana jest do jakiegoś obiektu.
    * Stan aplikacji także przypisany jest do jakiegoś obiektu.
    * Obiekt sam zarządza swoim stanem i czynnościami zmieniającymi ów stan.
    * Programista chcąc zmienić stan aplikacji, musi użyć danego obiektu oraz wywołać przypisaną doń metodę.
* Technika 2:
    * Każda czynność (funkcja, metoda) jest 'luźna'; znajduje się w pliku w folderze Functions.
    * Stan aplikacji przypisany jest do jakichś struktur, które znajdują się w folderze State.
    * Do zarządzania stanem aplikacji i czynnościami zmieniającymi ów stan budowane są byty dodatkowe.
    * Programista chcąc zmienić stan aplikacji, musi wykorzystać właściwą funkcję z Functions na właściwej strukturze ze State.

Obie techniki mają swoje zalety i wady. Ważne jest to, że **dla programistów, którzy przywykli do Techniki 1, Technika 2 jest mniej czytelna - i na odwrót**.

![Rysunek Techniki 1 po lewej, rysunek Techniki 2 po prawej](/img/1805/180518-techniques.png)

Co więcej, w niektórych kontekstach wybiorę Technikę 1, w innych Technikę 2. Tak więc "lepsza" jest pochodną celów i kontekstu.

**Jak tego użyć** - Czy refaktoryzacja, o której myślimy jest tym, co zespół w którym pracuję uzna za "lepsze"? Może refaktoryzacja o której myślę wymaga dodatkowego doszkolenia członków zespołu. Może technika o której myślę jest "lepsza", ale czy jest "lepsza w moim kontekście użycia"?

## 7. Jak niczego nie zepsuć

Nie da się wprowadzić zmiany jednocześnie _niczego_ nie poświęcając. To jest praktycznie niemożliwe.

Nawet najprostsza i najkrótsza refaktoryzacja, która ma dać najwięcej wartości - nawet taka refaktoryzacja najpewniej zmieni część parametrów:

* Wykorzystanie bardziej zaawansowanych technik programowania sprawi, że osoby mniej znające ten język uznają kod za nieczytelny i trudniej im się z nim będzie pracować.
* Wykorzystanie tylko prostych technik programowania sprawi, że zaawansowani programiści są zmuszeni do pracy wolniejszej niż by mogli.
* Stworzenie kodu zawsze prostego do zmiany (np. wszystko na Eventach, Szynie i luźnych obiektach) sprawi, że kod stanie się bardzo trudny do debugowania; też nie będzie widać przyczynowo-skutkowości.
* Zwiększenie bezpieczeństwa aplikacji może wpłynąć negatywnie na interfejs użytkownika.
* Każda zmiana kodu kosztuje czas i pieniądze.
* Skrajnie: nawet zmiana _nazw zmiennych na czytelniejsze dla zespołu_ sprawia, że dla twórcy oryginalnych nazw - te nowe nazwy mogą być mniej czytelne.

Wielowymiarowość zadań programistycznych oznacza, że podczas refaktoryzacji musimy określić na czym nam zależy. I, niestety, co jesteśmy w stanie w jakimś stopniu poświęcić.

Nie "po mojej refaktoryzacji wszystko jest lepsze" a "po mojej refaktoryzacji usprawniliśmy X i Y, ale kosztem Z".

![Rysunek przedstawiający aplikację sformowaną z luźnych obiektów, komunikującą się tylko eventami. Obserwator nie ma pojęcia co komunikuje się z czym i kiedy. One po prostu TO ROBIĄ.](/img/1805/180518-szyna.png)

**Jak tego użyć** - Czy zastanowiliśmy się nad tym, w jakim stopniu ta refaktoryzacja wpłynie na różne aspekty aplikacji? Jakie skutki uboczne wprowadzi? Czy określiliśmy, co jest akceptowalnym kosztem a co nie? Czy wzmacniamy coś, co jest ważne dla biznesu?

## 8. Po czym poznamy sukces

### 8.1. Problem zapalonego światła

Wyobraźcie sobie, że znajdujemy się w sali szkoleniowej. Pada deszcz, więc jest lekki półmrok. Proszę o włączenie światła. Kolega po Twojej prawej stronie mówi "zrobione". Ja patrzę na delikwenta i mówię "no nie, nie włączyłeś światła". On się upiera, że włączył.

Skąd Ty wiesz, czy ów kolega włączył światło? Innymi słowy, skąd wiemy, czy zadanie zostało wykonane?

...

**Bo przedtem światło nie świeciło, a teraz świeci.** Ewentualnie, bo przełącznik jest w innej pozycji niż przed przełączeniem przez kolegę ;-).

Powyższy przykład jest dość istotny z dwóch powodów:

* Powiedzieć można wszystko. Inżynier musi umieć udowodnić, że coś zostało zrobione prawidłowo (dostarczyło odpowiednie wyniki).
* Bardzo ważne jest określenie, czy zależy mi na świecącym świetle, czy na przełączonym przełączniku. Co jest środkiem, co jest celem.

![Prośba o włączenie światła spotyka się ze stwierdzeniem, że nie ma żarówki. Manager jednak twierdzi, że zadanie będzie wykonane, jeśli przełącznik przełączymy; żarówka jest nieważna](/img/1805/180518-lightbulb.png)

### 8.2. Sukces refaktoryzacji

Zapalenie czy zgaszenie światła jest stosunkowo prosto udowodnić. Jak jednak udowodnić, że refaktoryzacja się udała? Po czym poznamy sukces refaktoryzacji?

_A czemu robimy tą refaktoryzację? Na które parametry chcemy wpłynąć? Na czym nam zależy?_

Spójrzmy na taki parametr jak **wydajność**. Tutaj sprawa jest stosunkowo prosta - załóżmy, że mamy aplikację webową i mamy odpowiedzi sięgające od 500 milisekund do 5 sekund, przy założeniu, że mamy internet. [Za raportem gomez z mcrinc.com](http://www.mcrinc.com/Documents/Newsletters/201110_why_web_performance_matters.pdf), jeśli odpowiedź strony internetowej przekroczy 3 sekundy, około 40% klientów opuści stronę internetową bez czekania.

Więc z perspektywy marketingu, _jak najmniejszy czas ładowania i reakcji strony_ liczony w sekundach jest _skalą wydajności_.

Jak zatem udowodnić sukces refaktoryzacji? Ano, można doprowadzić do tego, że odpowiedź strony będzie trwała nie "do 5 sekund" a "do 2 sekund". W tym momencie mamy prosty, mierzalny, twardy dowód. Nasze działanie przyniosło korzyść biznesową pod kątem wydajności.

Dużo trudniej jest udowodnić coś takiego jak **Łatwość zmian kodu**. Spróbujmy jednak pomyśleć na ten temat.

Gdy ja myślę o łatwości (czy trudności) zmian kodu, widzę takie cztery rzeczy:

* ile czasu zajmuje mi odszukanie miejsca, gdzie chcę wprowadzić zmianę?
* ile czasu zajmuje mi dostosowanie kodu do tego, bym mógł zacząć pracę nad tym, co chcę zrobić?
* ile czasu zajmuje mi dopisanie testów i przetestowanie, czy to nadal działa?
* ile wprowadziłem błędów przy dostarczeniu nowej funkcjonalności?

Jak można na to spojrzeć "całościowo"?

Jeżeli po refaktoryzacji:

* będziemy w stanie udowodnić, że średnia ilość błędów znajdowana po kolejnych zmianach się zmniejszyła
* będziemy w stanie udowodnić, że podobnej klasy zmiana (np. na bazie zdania eksperta) zajęła zdecydowanie mniej czasu:
    * na etapie odnalezienia miejsca
    * na etapie dostosowania kodu
    * na etapie testowania i dodania analogicznej ilości sensownych testów

To jesteśmy w stanie w miarę sensownie argumentować, że refaktoryzacja była udana pod kątem **łatwości zmian kodu**. Pozwala to obronić naszą refaktoryzację przed kolegami, przełożonymi i klientem na poziomie obserwowalnych wyników, nie "czy ktoś uważa zmianę za ładną, czy nie".

**Jak tego użyć** - Czy wiemy co jest środkiem a co celem? Czy wiemy jak udowodnić, że osiągnęliśmy sukces zgodnie z celem? Czy wiemy jak zbadać, czy poprawiliśmy sytuację czy ją pogorszyliśmy? Czy znamy punkt startowy i potrafimy go mniej więcej umiejscowić na skali?

## 9. Przykład całościowy - historia innej refaktoryzacji

W styczniu 2018 roku moja ukochana Żona powiedziała, że powoli ma dość implementowania kolejnych modułów ekstraktujących dane z tekstu w naszej aplikacji (dalej zwanych Ekstraktorami).

Ogólnie rzecz biorąc, był to stary fragment aplikacji (gdy jeszcze uczyliśmy się Pythona), zmieniany często "na szybko". Miał trochę kopiowania i wklejania (ze zmianami). Z uwagi na częste zmiany, kod nadgnił - nie był czytelny ani logiczny. Trzeba było zatem coś z tym zrobić, bo przed nami było (i będzie) sporo pracy z Ekstraktorami.

Aplikacja miała między innymi następujące cechy:

* łatwość dodawania nowego kodu; program wiecznie się rozwija
* całkowicie niestabilna domena; musi być możliwość całkowitej przebudowy
* aplikacja pracuje na danych, które będą błędne i musi je prawidłowo obsłużyć
* nie ma szans na nowych programistów, więc kod musi być zrozumiały dla aktualnych developerów

Z tego udało się nam wyciągnąć główne cele tej refaktoryzacji:

1. musi być możliwość szybkiego dodania nowego Ekstraktora
2. nie możemy zdecydowanie pogorszyć czytelności i elastyczności kodu

Sprawdziliśmy więc ile czasu zajmuje dodanie Ekstraktora. 3-5 godzin. Niefart.

Niestety, z uwagi na sytuację refaktoryzacja była najlepszym rozwiązaniem - nie da się ustabilizować danych czy nie dodać nowych funkcjonalności do aplikacji. Więc - działamy.

Zidentyfikowaliśmy główny problem - wyrażenia regularne. Dodanie nowego wyrażenia regularnego, które tylko trochę różni się od pozostałych jest żmudne, łatwo popełnić błąd oraz zajmuje sporo czasu. Drugim problemem była konieczność obsługi specjalnych warunków przy podobnym szkielecie, czyli "kopiuj, wklej, trochę zmień". Te dwa miejsca zajmowały najwięcej czasu (no i wymagały napisania testów sprawdzających, czy nic nie zepsuliśmy).

Po czym poznamy sukces? Ano, jeśli będzie dało się dodać Ekstraktor dużo szybciej. Aha, i nie będziemy budować wyrażeń regularnych z palca - to był inny warunek nienegocjowalny.

Przeprowadziłem refaktoryzację właśnie pod kątem zmniejszenia czasu dodawania Ekstraktorów (i usunięcia budowania regexów). **To jest cel.**

By to osiągnąć, sparametryzowałem regexy, dodałem budowanie ich z poziomu kodu oraz ujednoliciłem szkielet, parametryzując specjalne warunki. **To jest środek.**

Jaki był wynik? **Dodawanie Ekstraktora zajmuje do 1 godziny.** Rekordem było 15 minut (łącznie z testami).

* wszystko ma odpowiednie funkcje, więc łatwo je po prostu wywołać; nie ma kopiowania i wklejania -> ogromna oszczędność czasu
* nie musimy dodawać nowych testów do szkieletów, bo jest jeden (dobrze przetestowany) ujednolicony szkielet -> oszczędność czasu
* wyrażenia regularne są parametryzowane i generowane przez odpowiednie funkcje -> mniej błędów i oszczędność czasu
* jest mniej kodu, więc łatwiej się połapać co i jak -> łatwiejsza przebudowa

Jak udowodnić, czy się opłacało? Włożyłem w tą refaktoryzację paręnaście godzin, a na każdym Ekstraktorze oszczędzam ~2 godziny. Jako, że zmiana Ekstraktorów też jest dużo szybsza, refaktoryzacja się już dawno zwróciła, patrząc na zmiany na przestrzeni ostatniego roku.

## 10. Podsumowanie

Refaktoryzacja dla refaktoryzacji jest jak kopanie rowu dla kopania rowu. Sympatyczne ćwiczenie intelektualne (lub fizyczne), które jednak zamiast pomóc - potencjalnie powoduje zgryzotę.

Jeżeli chcecie przeprowadzić refaktoryzację dającą wartość, rozważcie wykorzystanie poniższej procedury:

1. Jakie wymiary ma mój problem? Jakie wymiary w ogóle są ważne w mojej aplikacji?
2. Na których wymiarach zależy mi najbardziej? W kierunku których wymiarów chcę refaktoryzować?
3. Czy refaktoryzacja na pewno jest najlepszym narzędziem wobec celu, który chcę osiągnąć? Zależy mi na wymiarach, nie refaktoryzacji.
4. Czy w kontekście (otoczeniu), w którym się znajduję - czy to, o czym myślę faktycznie pomoże?
5. Czy to, nad czym chcę pracować pomoże w jakiś sposób biznesowi? Czy efekty uboczne są akceptowalne?
6. Czy wiem, po czym poznać sukces? Czy _rozumiem_ wymiar, w kierunku którego próbuję refaktoryzować?

Dla większości pytań "czy X jest lepsze czy Y" odpowiedź brzmi "jest taki kontekst, w którym X jest lepsze niż Y. Jest taki kontekst, w którym Y jest lepsze niż X". A naszą rolą jest to, byśmy doprecyzowali w jakim kontekście się znajdujemy i co mamy zrobić.

**Jak zatem obronić refaktoryzację?** Na bazie racjonalnych argumentów. Przedstawcie wymiary które optymalizujecie - oraz przewidywane efekty uboczne. Rozmawiajcie o konkretach, o oczekiwanych wynikach. Nie rozmawiajcie na poziomie dogmatów, tylko obserwowalnych wyników. Uwzględnijcie otoczenie, dane historyczne oraz priorytety biznesu.

Od razu będzie przyjemniej się pracowało.

Powodzenia!

## 11. Metadane artykułu

| Akcja                                                         | Czas lub Ilość |
|---------------------------------------------------------------|----------------|
| Czas poświęcony na budowę planu artykułu i samego szkicu      |  3 godziny |
| Liczba osób korygujących szkic (poza autorem)                 |  0 osób   |
| Ilość zmian szkicu po konsultacjach                           |  0 zmian  |
| Czas poświęcony na napisanie i korektę artykułu               |  8 godzin |
| Liczba osób korygujących artykuł (poza autorem)               |  1 osoba  |
| Czas poświęcony na przebudowę artykułu później                |  2 godziny |
| Liczba osób korygujących artykuł po publikacji (poza autorem) |  osoby  |
| Liczba błędów / modyfikacji artykułu po wydaniu               |  błędów |
| Czas poświęcony na korekcję błędów                            |  godziny |
