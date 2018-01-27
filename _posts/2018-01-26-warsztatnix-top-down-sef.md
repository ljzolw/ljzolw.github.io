---
layout: post
title: "'Warsztatnix', czyli top-down w praktyce"
date: 2018-01-26
---

# {{ page.title }}

## 1. Zakres artykułu
### 1.1. Rozpatrywane narzędzia

1. Podejście 'top-down' w praktyce, podczas analizowania i budowania produktu (tu: spotkania).
1. Pętla 'koncepcja-cele-implementacja-weryfikacja' w praktyce, w sposób możliwy do powtórzenia.

### 1.2. Korzyści dla Was

Kiedy warto to przeczytać?

1. Chcesz zaprojektować COŚ (produkt, organizację, program komputerowy), co ma pełnić jakąkolwiek funkcję. Acz - musi być celowy.
1. Chcesz umieć określić "czym jest sukces", lub "czy to co robię, pozwoli mi osiągnąć ten sukces".
1. Uważasz, że celowość nie jest szczególnie istotna i chcesz przeczytać kontrprzykład.

## 2. Streszczenie myśli przewodniej artykułu

Jeżeli nie macie czasu przeczytać tego artykułu, lub jeśli chcecie sobie przypomnieć najważniejsze fragmenty artykułu, przeczytajcie ten punkt. To nie jest spis treści; to ekstrakt reszty artykułu (który stanowi jedno wielkie _case study_):

1. Jeżeli nie wiecie, co budujecie - nie zrobicie tego dobrze i uda Wam się raczej przez przypadek (lub do momentu zmiany Otoczenia na mniej korzystne).
1. Jeżeli wiecie, co robicie, możecie to określić i docelowo - zmierzyć.
1. W zależności od tego co budujecie, użyjecie innych środków. Stąd podejście 'top-down' - od góry (koncepcji) do dołu (działania).
1. Jeżeli wykonujecie działania bez ostrej weryfikacji pod kątem celów i mierników, robicie rzeczy niekoniecznie prowadzące do sukcesu. Marnujecie swój czas.
1. Dla różnych produktów sukces jest definiowany inaczej - ale zawsze prowadzi do materializacji jakichś korzyści u kogoś.
1. Percepcja jest bardzo istotna. Nikt nie siedzi w Waszych głowach - tylko Wy wiecie co chcecie osiągnąć. Komunikujcie to.

Jeśli jednak chcecie zapoznać się z całością i zobaczyć, jak to wykorzystać... zapraszam do Czelimina.

## 3. Trzy lata w Czeliminie
### 3.1. Krótka historia trzech ostatnich lat

_Wydarzenia tu prezentowane są całkowicie fikcyjne, acz nieco inspirowane światem rzeczywistym. Nieco. W sumie, 100% fikcji._

Chciałbym Wam przedstawić Zygmunta Zapaleńca. 

Zygmunt mieszka w _fikcyjnym_ mieście Czeliminie. W Czeliminie, ogólnie rzecz biorąc, nic się nie dzieje jeżeli chodzi o działania okołobranżowe - nie ma konferencji, nie ma niczego. Programiści są pozamykani w swoich firmach i tak naprawdę nie rozmawiają ze sobą.

I Zygmunt zdecydował się to zmienić. Trzy lata temu zorganizował własną mikro-konferencję i nazwał ją Warsztatnix. Dwóch prelegentów, mała (acz sympatyczna) sala... standardy branżowe zachowane. Rozmawiał też z przedstawicielami innych firm i w ten sposób pozyskiwał prelegentów.

Czelimin ożył. Coś zaczęło się dziać. Zygmunt był szczęśliwy.

...a potem minęło trochę czasu...

W pewnym momencie różne okoliczne firmy zaczęły się orientować w tym, że przecież takie wydarzenie to świetna okazja na promocję zarówno swoich pracowników jak i ich samych. Zamiast występować na Warsztatnixie, mogą przecież założyć własne wydarzenia. Postawiło się odpowiednie zadania, uruchomiono działy marketingu i w Czeliminie (i okolicy) zaczęły wyrastać różne Spotkania jak grzyby po deszczu.

A Zygmunt... cóż, jedna osoba nie jest w stanie dorównać czemuś takiemu. Warsztatnix nadal istniał, ale coraz trudniej było o nowych prelegentów. Coraz mniej ludzi zaczęło przychodzić. Co gorsza, Warsztatnix zaczął tak jakby... trącić myszką.

Gdy w styczniu Warsztatnixowi wybiła trzecia rocznica istnienia, Zygmunt zaczął się zastanawiać. W sumie, jedyne, czego chciał to to, by w Czeliminie coś się działo. A teraz "coś się dzieje". Może czas pozwolić Warsztatnixowi odejść do historii?

Oczywiście, że nie. Wtedy nie byłoby tego artykułu ;-).

### 3.2. Czelimin? Warsztatnix? Nieważne, rozwiążmy to.

Warsztatnix pojawił się w świecie, w którym nie było innych Spotkań. Nie było konkurencji. Potem konkurencja się pojawiła i zrobiło się trudniej. 

Czyżby?

Stawiam tu hipotezę, że Warsztatnix jest czymś innym niż pozostałe Spotkania. Cele Warsztatnixu od samego początku były inne: 

* Spotkania sponsorowane przez różne Firmy mają za zadanie przyciągnąć do nich świetnych ludzi i budować markę 
* Warsztatnix próbował łączyć ludzi między firmami.

Jeżeli tak, Zygmunt zrobił błąd. Nie powinien był od samego początku robić "konferencji" takiej jak inne Spotkania. Bez działu marketingu i promocji Warsztatnix będzie zawsze "jak każdy inny, ale mniejszy i nie ma tylu atrakcji". Większość tego typu problemów po prostu można zalać złotem i dolarami (podpowiedź: Zygmunt nie jest bogaty, bo nie kupił bitcoinów w 2007 roku).

Co zatem można zrobić?

Spróbujmy przeprojektować Warsztatnix zgodnie z techniką "po co to robię - co robię - po czym poznam sukces".

Tak się przypadkowo składa, że da się do tego typu wykorzystać istniejący framework do budowania organizacji - SEF (Strategic Execution Framework). Jako, że nie zamierzam pisać książki, wybierzemy jedynie podzbiór SEF by pokazać Wam technikę Top-Down i kilka innych rzeczy. I tak będzie długo i trudno.

Na tym etapie artykułu, z naszego punktu widzenia istotne jest to, że SEF ma następujące własności:

* Skupia się na podejściu "z góry do dołu", czyli "od koncepcji wysokopoziomowej i idei do rozwiązania"
* Zakłada mierzalność i nacisk na wykonywanie działań operacyjnych a nie "gdybanie"
* Jest narzędziem uniwersalnym przy budowaniu jakiegokolwiek _produktu_ (organizacji, grupy, firmy, przedmiotu, oprogramowania...).
* W ramach tego artykułu możemy użyć 25% tego narzędzia i osiągnąć widoczny sukces ;-).

To powyższe jest dokładnie tym, czego nam w tym wypadku potrzeba. Innymi słowy, SEF powinien nam umożliwić ustrukturyzowaną przebudowę Warsztatnixu z uwzględnieniem jego silnych i słabych stron.

Przejdźmy więc do pierwszego elementu SEF - Koncepcji.

## 4. Różnice w koncepcji...
### 4.1. Koncepcja

Każda organizacja (produkt, program...) istnieje z _jakiegoś_ powodu. Rozwiązuje jakiś problem. Powoduje jakąś zmianę w rzeczywistości. To jest właśnie **Koncepcja** zgodnie z SEF; z angielskiego "Ideation". 

Koncepcja jako całość odpowiada na pytanie "Kim my jesteśmy?".

Koncepcję rozdziela się na Przyczynę ("Purpose"), Tożsamość ("Identity") oraz Zamierzenia ("Long-Range Intention"). W dużym skrócie:

* **Przyczyna** odpowiada na pytanie "czemu istniejemy". Jaki jest sens istnienia tej organizacji? Jest to zwięzłe, ambitne i zawiera ładunek emocjonalny.
* **Tożsamość** odpowiada na pytanie "czym chcemy być". Jest to krótki opis tego, jak taka organizacja widzi ideał samej siebie.
* **Zamierzenia** odpowiadają na pytanie "gdzie chcemy dotrzeć". Ta kategoria ma pokazać, jak świat się zmieni, jeśli ta organizacja zwycięży. Świat idealny z perspektywy Przyczyny.

Dobrze; mając powyższe - spójrzmy na Warsztatnix oraz Spotkania.

### 4.2. Koncepcja Warsztatnixa
#### 4.2.1. Przyczyna

Czemu tu jesteśmy? 

"Rozwiążmy wspólnie swoje problemy i się poznajmy lepiej".

W Czeliminie jest mnóstwo mądrych ludzi i większość wymyśla koło na nowo. Każdy ma czasem jakiś problem - może nie znamy dobrej ekipy do remontu mieszkania, może nie umiemy poradzić sobie z połączeniem Django z Angularem. Czasem ktoś chce zmienić pracę... ale która firma do tej osoby najbardziej pasuje? Czasem ktoś chce zainwestować, a czasem ktoś szuka inwestora.

Sęk w tym, że jakkolwiek w Czeliminie jest ta wiedza "ogólnie", jest ona rozproszona. I przez to nikt nie jest w stanie z niej skorzystać.

Hasło _"Rozwiążmy wspólnie swoje problemy i się poznajmy lepiej"_ jest właśnie odpowiedzią na powyższy Ból. Gdyby istniały osoby, z którymi można po prostu pogadać... gdyby dało się kogoś zapytać... gdyby ktoś pomógł w moim projekcie... gdyby było coś, co ja mogę zrobić by było lepiej...

I właśnie po to powstał Warsztatnix.

#### 4.2.2 Tożsamość

Czym chcemy być?

"Platforma spotkań pasjonatów, wspólnie rozwiązujących problemy i wspólnie się od siebie uczących".

Warsztatnix nie chce być firmą. Chce być neutralną _platformą_ - miejscem komunikacji między ludźmi. Ma to z jednej strony być fizyczne miejsce, z drugiej strony komunikacja między ludźmi ma przekraczać sam Warsztatnix. 

Jakkolwiek dziwnie to może brzmieć, czelimińskie firmy rzadko rywalizują ze sobą bezpośrednio. Firma Firmex nie traci na tym, że firma Informatikos ma ludzi, którzy rozwiązali swój problem w dwie godziny a nie dwa tygodnie. Każde z nas uczy się czegoś w firmie, w której pracujemy - więc niech Warsztatnix będzie takim miejscem wymiany idei, dobrych praktyk i pomocy sobie nawzajem.

Chcemy być miejscem, gdzie ludzie z różnych firm mogą się spotykać i mogą współpracować zgodnie z tym, co ich interesuje. Gdzie mogą się poznawać i rozmawiać. Bez przymusu, bez jakichś rozkazów.

#### 4.2.3. Zamierzenia

Gdzie chcemy dotrzeć?

Docelowo Warsztatnix ma stworzyć tzw. "luźną sieć społeczną". Sam Warsztatnix ma być taką agorą; rynkiem, gdzie może każdy przyjść ze swoim problemem czy chęcią do pracy i znajdzie tam coś, co może zrobić czy komuś pomóc. Taka "sieć sąsiedzka - to jest mój sąsiad, on mi pomoże czy pożyczy cukier".

Na poziomie konkretnych życzeń, można użyć następujących haseł:

* Nie tylko technologie, też problemy inne; rozwiązujemy problemy interdyscyplinarne.
* Luźna ("słaba") sieć kontaktów - każdy zna kogoś. Każdy może i chce pomóc każdemu.
* Podnośmy swoje umiejętności krzyżowo, nie zamykajmy się w silosach firm.
* Nie walczymy ze sobą, to nie wojny klanów. Wszyscy gramy w tą samą grę. Pomóż ile umiesz, pytaj ile potrzebujesz.
* Miejsce do wymiany opinii bez ściemy. Krytyka, ale konstruktywna i życzliwa.

Można to też podsumować sloganem: "Warsztatnix - wyjdziesz wiedząc więcej"

### 4.3. Koncepcja innych Spotkań
#### 4.3.1. Przyczyna

Czemu tu jesteśmy?

"Pokażmy, jacy jesteśmy świetni i zrekrutujmy najlepszych ludzi na świecie".

W Czeliminie jest sporo naprawdę dobrych, mądrych ludzi. Nasza firma jest w stanie dać im dużo lepsze zadania i warunki niż ta firma, w której oni teraz pracują. Problem polega na tym, że tamci ludzie nie wiedzą o istnieniu naszej firmy - lub nie wiedzą, że nasza firma drastycznie potrzebuje kogoś takiego, jak oni. Podobny problem dotyczy klientów - nie zawsze wiedzą, że nasza firma może im najbardziej pomóc.

Przez ogromną konkurencję między świetnymi firmami na rynku programistycznym w Czeliminie jest trudno o "wybicie się" spośród innych firm.

Stąd konieczność _"pokazania, jacy jesteśmy świetni"_. Niech wszyscy wiedzą, co tracą nie współpracując z nami - czy to klienci, czy pracownicy. A dzięki temu - _"zrekrutujemy najlepszych ludzi na świecie"_ i zapewnimy pracownikom projekty i zadania odpowiednie do ich poziomu, a jednocześnie klientom zapewnimy rozwiązania na jakie zasłużyli.

Musimy tylko im pokazać jacy jesteśmy dobrzy. Stąd to Spotkanie.

#### 4.3.2. Tożsamość

Czym chcemy być?

"Najlepsze merytorycznie spotkania w ważnych dla nas dziedzinach, ściągające ludzi z całej Polski".

Może jest wiele spotkań, ale _nasze_ będzie najlepsze. Nasza firma pracuje przede wszystkim w Pythonie (_tu: podstawcie swój ulubiony język_), więc nasze wydarzenie będzie najlepszym merytorycznie wydarzeniem dookoła tego języka. Będzie o nas mówił sam Guido van Rossum.

Chcemy być tak dobrzy, że stwierdzenie "prezentował na Spotkaniu" będzie powodem do chwały dla prelegentów.

#### 4.3.3. Zamierzenia

Gdzie chcemy dotrzeć?

Docelowo Spotkanie ma budować świetną opinię o naszej firmie zarówno wśród potencjalnych pracowników jak i klientów. Samo Spotkanie ma być rozpoznawalnym na całym świecie wydarzeniem, o którym mówi się w gazetach. Ma ściągać klientów z całego świata i potencjalnych pracowników z Czelimina i okolic. Nasza Firma może wybierać wśród najlepszych - tak stała się dzięki Spotkaniu prestiżowa.

Na poziomie konkretnych życzeń, można użyć następujących haseł:

* Przede wszystkim pojawiają się tematy w obszarach potrzebnych firmie, na bardzo wysokim poziomie.
* Wystąpienia ściągają potencjalnych klientów oraz pracowników. Są bardzo popularne.
* Pokazać firmę organizującą jako najlepszą zarówno merytorycznie jak i budować dobre skojarzenia z marką.
* Zapewnić możliwość rekrutacji najlepszych dla firmy ludzi.
* Maksymalizować zasięg. O naszej firmie wiedzą wszyscy i wszyscy tą firmę lubią, cenią i szanują.

Można to też podsumować sloganem: "Najlepsze Spotkanie - Najlepsza Firma"

### 4.4. Implikacje

Analizując powyższe, można wyraźnie zauważyć:

* Warsztatnix oraz Spotkanie to dwa _zupełnie różne_ wydarzenia z _zupełnie różnymi_ celami.
* Warsztatnix oraz Spotkanie będą różniły się formami, działaniami... ogólnie, wszystkim.
* Warsztatnix nie konkuruje z żadnym Spotkaniem.
* Spotkania konkurują między sobą - tylko jedna firma może być "najlepsza" (uwaga: Spotkanie z Javy nie rywalizuje ze spotkaniem z C++; obie firmy mają _mniej więcej_ różną populację klientów i pracowników)

Powyższe pozwala mi powiedzieć, że  Warsztatnix miał potencjalnie niewłaściwą formę. Struktura "prelegenci wykładają przed publicznością" jest świetna, jeśli chcemy promować prelegentów. Jest jednocześnie fatalna jak chodzi o współpracę czy wymianę myśli. 

Wykład nie daje pretekstu do rozmowy. Posłuchamy, pokiwamy mądrze głowami i się rozejdziemy do domów.

Trzeba zrobić dalszy krok. Wiemy już, czym Warsztatnix i Spotkanie chcą być. Jednak jak powinny to realizować? Szczęśliwie, SEF ma na to odpowiedź.

Różnice w koncepcji...

## 5. ...przekładają się na różnicę Wizji...
### 5.1. Wizja

Opracowaliśmy poziom Koncepcji. Dzięki Zamierzeniom mamy w głowie wyraźny obraz tego, jak ma wyglądać rzeczywistość, gdy wygramy - jak wygląda idealna przyszłość. Dzięki Tożsamości wiemy, czym chcemy być. Dzięki Przyczynie wiemy, po co istniejemy.

Wizja zgodnie z tym modelem odpowiada jako całość na pytanie "Gdzie idziesz?". Próbuje przekształcić Koncepcję w coś namacalnego. Składa się z Celów (Goals), Miar (Metrics) i Strategii (Strategy).

Bardzo ważne jest tu angielskie tłumaczenie: [Goal to nie jest to samo co Objective](https://www.diffen.com/difference/Goal_vs_Objective). 

* Goal: "chcę osiągnąć sukces w ramach genetyki i stworzyć zwierzę nie powodujące alergii"
* Objective: "chcę skończyć pracę magisterską w ramach genetyki do końca przyszłego miesiąca"

Tłumacząc:

* Goal jest "celem strategicznym" - czymś, co jest konkretnym, obserwowalnym **wynikiem**. Nie jest działaniem.
* Objective jest "celem operacyjnym" - konkretnym działaniem prowadzącym do celu strategicznego.

Wiedząc powyższe, wyjaśnijmy sobie elementy Wizji:

* **Cele** odpowiadają na pytanie "co jest dla nas ważne". Które elementy Koncepcji są dla nas ważne?
    * Cele są formułowane na bazie tego, co było wcześniej w Zamierzeniach, pod kątem spełnienia Koncepcji i nie zaburzenia Tożsamości.
* **Miary** odpowiadają na pytanie "czy udało nam się osiągnąć cele". Bardzo często służą też jako weryfikacja tego, czy cele są postawione poprawnie.
    * Miary mają móc udowodnić, że osiągnęliśmy (lub nie) Cel. Na tym etapie wystarczy nam "więcej, mniej".
* **Strategia** jest elementem trzech kategorii SEF. Jako, że omawiamy jedynie trzy z sześciu, Strategię omówimy osobno.

Ja osobiście jestem zwolennikiem podejścia prezentowanego przez [Toma Gilba](https://www.gilb.com/) w połączeniu z kilkoma bolesnymi lekcjami ;-)

1. Wszystko da się zmierzyć. Bez wyjątku.
2. Jeżeli czegoś nie da się zmierzyć, to znaczy, że nie dość dobrze to rozumiemy lub tego nie doprecyzowaliśmy.
3. Dostajemy to, co mierzymy. Jeżeli nam na czymś zależy, musimy móc to zmierzyć.
4. Nie wszystko naraz. Po kolei - od największego problemu do najmniejszego.

Jeśli już wiecie, co to Wizja - przejdźmy do Warsztatnixa oraz Spotkania.

### 5.2. Wizja Warsztatnixa - Cele i Miary

Dezagregując Zamierzenia na elementy kluczowe oraz pilnując, by nic tu nie było sprzeczne z Tożsamością i by wpływało pozytywnie na Koncepcję dostajemy to:

1. Istnieje grupa ludzi współpracujących ze sobą.
1. Ci ludzie rozwiązują swoje problemy
1. Skupienie na różnorodności - różni ludzie, różne pomysły
1. Każdy: zawsze wychodząc z Warsztatnixa wie więcej
1. Istnieje między nimi luźna sieć kontaktów

Z punktu widzenia Koncepcji Warsztatnixa to powyższe jest tak naprawdę istotne. Wszystkie te rzeczy są obserwowalne, a przynajmniej na to wygląda. Wszystkie pasują do SMART. Sprawdźmy więc, czy da się je sensownie zmierzyć:

1. Współpraca:
    1. _rozumiem jako:_ poziom zainteresowania i zaangażowania uczestników warsztatu. Praca z ludźmi "spoza swojej firmy".
    1. _miara_: ilość osób w ogóle współpracujących i nie wycofanych. Ilość osób współpracujących tylko w ramach "swojej grupy znajomych" (minimalizujemy)
1. Różnorodność:
    1. _rozumiem jako:_ nie ma "gwiazd", każdy ma prawo głosu, nie ma silosów, rozbijanie grupek
    1. _miara_: ilość osób wycofanych i nieaktywnych (minimalizujemy). Ilość różnych ludzi prezentujących rzeczy. Ilość różnych firm się angażujących. Ilość tematów nieinformatycznych.
1. Swoje problemy:
    1. _rozumiem jako:_ przynoszone są problemy i koncepty "z sali". Te problemy są rozwiązywane przez grupę lub podgrupy.
    1. _miara_: ilość osób, które przyniosła własne problemy. Ilość osób pracujących nad takimi problemami. Ilość takich problemów.
1. Rozwiązywanie:
    1. _rozumiem jako:_ postęp zrozumienia i skończenia prac nad danym obserwowalnym problemem rośnie. Po ludzku: przyszedłem i miałem mniej zrozumienia/zrobienia, niż gdy wyszedłem.
    1. _miara_: ilość osób pracujących nad rozwiązaniem, ilość potencjalnych rozwiązań problemu, ilość osób która zadeklarowała rozwiązanie czy otrzymaną pomoc
1. Luźna sieć:  
    1. _rozumiem jako:_ ludzie komunikują się między sobą podczas spotkania, ale też poza nim gdy jest taka konieczność. Powstają zespoły celowe międzyfirmowe.
    1. _miara_: Ilość bezwzględna osób, które robią rzeczy jak opisane w "rozumiem".

Powyższe nie jest idealne, ale nie jest też tragiczne. W jakimś stopniu wiążemy to, co chcemy osiągnąć z jakąś formą oceny, czy udało nam się to osiągnąć. Problem jednak leży w parametrze poniżej:

1. przyrost wiedzy: 
    1. _rozumiem jako:_ podczas szkolenia wszyscy uczą się coraz więcej, poznają nowe rzeczy. Szybciej i skuteczniej rozwiązują problemy.
    1. _miara_: PROXY: ilość osób zaangażowanych, PROXY: ilość osób mówiących że nic im to nie daje

Normalnie przyrost wiedzy można obserwować pracując z kimś w projekcie, albo wykorzystując drugi poziom modelu Kirkpatricka np. w formie testów przed ćwiczeniem i po ćwiczeniu. Z oczywistych przyczyn, osoby obecne na Warsztatniksie nie są w stanie pracować razem w jednej firmie. Nie da się też dać im dwa razy tego samego zadania, bo się znudzą. Co więcej, niekoniecznie mamy tą samą populację osób.

To powoduje, że nie możemy sensownie zmierzyć przyrostu wiedzy. To, co możemy jednak zmierzyć to wskaźnik pośredni - proxy - często występujący przy uczeniu się. W naszym wypadku to będzie ilość osób MÓWIĄCYCH, że spotkanie nie miało sensu / nic im nie dało oraz ilość osób zaangażowanych i zainteresowanych działaniami.

Priorytetyzacja. Lepiej mieć miary średnie, które jednak nie robią krzywdy (ich wypełnienie nie koreluje się pozytywnie z Koncepcją, lub - co gorsza - ich wypełnienie koreluje się negatywnie z Koncepcją) niż nie mieć żadnych miar. A lepiej nie mieć żadnych miar niż mieć miary szkodliwe.

### 5.3. Wizja Spotkania - Cele i Miary

Dezagregujemy ponownie:

1. Na Spotkaniu pojawia się jak najwięcej osób w naszej grupie docelowej.
1. O naszej firmie mówi się dobrze i jest ona szeroko znana.
1. Merytorycznie dobre osoby aplikują do naszej firmy.
1. Klienci zauważają Spotkanie i są nim zainteresowani.

Ustawiamy miary:

1. na Spotkanie przychodzą osoby z grupy docelowej: 
    1. _rozumiem jako:_ na Spotkaniu pojawiają się przede wszystkim osoby z interesującej nas grupy - potencjalni klienci lub pracownicy.
    1. _miara_: stosunek osób z interesującej nas grupy do wszystkich osób. Też: ilość osób przychodzących często (na potem: zdefiniować i zmierzyć "często").
1. nasza firma (i Spotkanie) jest szeroko znana:
    1. _rozumiem jako:_ sporo osób, też poza tym regionem rozpoznaje to Spotkanie jako wpływowe. I rozpoznaje naszą Firmę.
    1. _miara_: z bólem przyznaję się do niekompetencji. Najpewniej mierzyłbym zasięg [bazując na czymś takim jak w tym artykule](https://www.kaushik.net/avinash/brand-measurement-analytics-metrics-branding-campaigns/). Na przykład: różnica w ilości odsłon witryny "Kariera w Firmie" po zakończeniu Spotkania (zarówno jednorazowo jak i stała zmiana z powracającymi gośćmi czy marketingiem szeptanym).
1. odpowiednie osoby aplikują do Firmy:
    1. _rozumiem jako:_ dzięki Spotkaniu aplikują do nas kandydaci, których chcemy pozyskać.
    1. _miara_: ilość osób aplikujących do naszej Firmy oraz ilość osób decydujących się na to przez Spotkanie.
1. dzięki Spotkaniu w Firmie pojawiają się nowi klienci:
    1. _rozumiem jako:_ dzięki Spotkaniu klienci wiedzą o naszej Firmie i są zainteresowani współpracą z naszą firmą
    1. _miara_: ilość klientów, którzy słyszeli o naszej Firmie dzięki Spotkaniu.

Także i w tym wypadku nie jestem w 100% pewny swoich miar, ale na potrzeby artykułu jest to w miarę wystarczające.

### 5.4. Komentarz

Oczywiście, gdybym _naprawdę_ miał budować miary, to poszukałbym więcej takich artykułów jak [ten załączony wcześniej](https://www.kaushik.net/avinash/brand-measurement-analytics-metrics-branding-campaigns/). Dobrą strategią określenia miary jest użycie przeglądarki - najpewniej jakaś sugestia będzie na pierwszej stronie Google.

Tak czy inaczej, powyższe miary spełniły swoją rolę - widzimy różnicę w celach strategicznych i w tym, na czym chcemy się skupić budując Warsztatnix oraz Spotkanie.

### 5.5. Implikacje

Po przejściu przez poziom Wizji, Warsztatnix oraz Spotkanie są dwoma zupełnie różnymi wydarzeniami, skupiającymi się na zupełnie różnych rzeczach i zupełnie inaczej definiującymi sukces.

* Warsztatnix skupia się na ogólnie rozumianej _interakcji między uczestnikami_.
    * Zupełnie nie ma znaczenia faktyczna _liczba_ uczestników (poza fragmentem z luźną siecią).
    * Miary promują proaktywność, współdziałanie oraz branie spraw w swoje ręce.
* Spotkanie skupia się na szerokim zasięgu i wysokim poziomie merytorycznym
    * Proaktywność czy działanie uczestników nie ma znaczenia. Tu na piedestale są Firma i prelegenci.
    * Miary promują działanie masowe i przyciąganie dużej ilości "właściwych", "odpowiednich" dla Firmy osób.

Warsztatnix nie jest w stanie rywalizować ze Spotkaniem jako konferencja. Jednak Spotkanie nie jest w stanie rywalizować z Warsztatniksem jako warsztat, czy [antykonferencja](https://unconference.net/unconferencing-how-to-prepare-to-attend-an-unconference/).

![_Rysunek: Po lewej stronie jest Wielki Mistrz Programowania, który rozświetlony reflektorami opowiada setkom ludzi jak wygląda programowanie. Po prawej kilka osób próbuje wspólnie rozwiązać niemożliwe zadanie a w tle zaplątał się losowy kot._](/img/180127/konferencja_warsztat.jpg)

Dobrze więc, różnice w koncepcji przekładają się na różnicę Wizji...

## 6. ...z czego wynika odpowiednie podejście do Strategii.
### 6.1. Strategia

Dumne słowo "strategia" tak naprawdę oznacza jedynie "drogę dojścia do czegoś". Z uwagi na swoje usytuowanie w modelu SEF, strategia odpowiada nie na jedno a na kilka pytań:

* Jaki jest Twój kontekst? (pochodzi z Natury)
* Gdzie idziesz? (pochodzi z Wizji)
* Co musimy zbudować? (pochodzi z Uruchomienia)

Powyższe oznacza, że z naszego punktu widzenia Strategia to zbiór działań, który odpowiada na pytanie: "co musimy zrobić, by osiągnąć Wizję w taki sposób, by działać zgodnie z naszą Naturą i nadal osiągnąć pożądaną Koncepcję?"

W ramach tego artykułu zignorowaliśmy Naturę; jakkolwiek ubolewam nad tym (tracimy część informacji), ten artykuł jest już wystarczająco długi.

Wiedząc, że Strategia to zbiór działań prowadzących do osiągnięcia Wizji (dokładniej: celów strategicznych), możemy teraz zaproponować działania mające zbliżyć nas do owej Wizji.

1. Trzeba ustawić dla każdej Miary poziom Tolerancji ("niech będzie") i Doskonałości ("to byłoby super").
1. Proponujemy kilka Strategii - wiązek działań.
1. Próbujemy przewidzieć, czy wszystkie wybrane Strategie doprowadzą wszystkie Mierniki do poziomu Tolerancji. Te Strategie, które nie osiągną poziomu Tolerancji - odrzucamy.
1. Z pozostałych Strategii wybieramy tą, która rozwiązuje główny problem. Implementujemy ją i weryfikujemy, czy działa prawidłowo pod kątem Koncepcji i Wizji.
1. Jako, że cały czas  zmienia się Otoczenie, musi zmieniać się też Strategia. Najpewniej zmieni się Wizja. Raczej nie zmieni się Koncepcja.

...i w taki sposób (bardzo uproszczony) wygląda zarządzanie strategiczne. Oczywiście, nie powiedziałem ani słowa o priorytetyzacji, budżetach, zarządzaniu ryzykiem, wyborze Strategii... ten artykuł jest i tak za długi. Potraktujcie to jak wstęp.

### 6.2 Potencjalna strategia Warsztatnixu

Grupa działań mających osiągnąć cele strategiczne:

1. I - Model warsztatowy - współpraca ludzi między firmami buduje znajomości i "łamie pierwsze lody".
1. II - Promować przynoszenie i rozwiązywanie problemów. Pozyskiwać problemy interesujące i rozwojowe dla osób obecnych.
1. III - Umożliwienie różnym osobom prowadzenie, też na różnych poziomach umiejętności tak, by po każdym spotkaniu wszyscy wyszli wiedząc coś więcej
1. IV - Komunikacja celów oraz założeń uczestnikom. Kto chce osiągać te korzyści, przyjdzie. Kto nie, nie jest grupą docelową.
1. V - Budowanie dostępnej dla wszystkich bazy wiedzy. Silny kernel - luźna orbita. Graf, by zmniejszyć ilość stopni oddalenia.

mierzymy (w uproszczeniu):

1. A - ilość osób współpracujących i nie wycofanych
1. B - ilość różnych ludzi wykonujących działania aktywnie
1. C - ilość różnorodnych tematów i przyniesionych problemów
1. D - poziom komunikacji podczas Warsztatnixu i poza nim
1. E - poziom zaangażowania i zadowolenia

Tabelka pokrycia między działaniami a miarami (nie mamy jeszcze mierników; nie ustawiliśmy wartości Tolerancji i Doskonałości):

|     | A | B | C | D | E |
| I   | X | X |   | X | X |
| II  | X |   | X |   | X |
| III |   | X | X |   |   |
| IV  | X | X |   |   | X |
| V   |   |   |   | X |   |

Co z powyższego wynika?

Działaniami o największym wpływie na cele strategiczne są: Model Warsztatowy, Zdobycie Właściwych Problemów i Komunikacja Celów i Założeń:

* Model Warsztatowy sprawia, że będzie dochodziło do interakcji i wymiany energii.
* Właściwe Problemy sprawią, że różne osoby się zaangażują i będą zainteresowane.
* Komunikacja Celów sprawi, że komu nie pasuje... ten nie przyjdzie. Będzie też jasne, że Warsztatnix się różni od Spotkania.

Najbardziej zagrożonym celem strategicznym jest Budowa Luźnej Sieci oraz Różnorodność Tematów/ Problemów:

* Sieć jest wspierana pośrednio przez Model Warsztatowy, który promuje interakcję.
* Sieć jest wspierana przez niezbyt silne działanie Budowa Bazy Wiedzy. Coś, co wygląda na kosztowne i trudne.
* Różnorodność jest wspierana pośrednio przez Promowanie Przynoszenia Problemów, ale typ problemów jest pochodną uczestników.
* Różnorodność jest wspierana przez Promowanie Różnorodnych Prelegentów - ale zawsze znalezienie prelegentów było słabym punktem Warsztatnixa.

Mając powyższe, wiemy, na co należy teraz zwrócić szczególną uwagę.

Zaznaczam - nie jest to strategia _idealna_. Ale działamy w istniejącym środowisku - w Warsztatniksie. Nie projektujemy niczego od zera. Możemy wprowadzić trzy najsilniejsze działania i zacząć mierzyć ich efekty. Jeżeli wynik będzie zgodny z oczekiwaniem, szukamy co teraz jest największym problemem i jak możemy ten konkretny problem rozwiązać.

Jak refaktoryzacja w świecie programowania. Krok po kroku, do celu, mierząc to, co ważne.

### 6.3 Potencjalna strategia Spotkania

Ten artykuł jest już dość długi. Nie widzę dla Was korzyści z tego, że powtórzę powyższe kroki. Zaproponuję tylko przykładowe działania strategiczne:

Jeżeli mierzymy:

1. A - powracający uczestnicy
1. B - potencjalni pracownicy
1. C - potencjalni klienci
1. D - zasięg

To przykładowe działania mogłyby wyglądać tak:

1. I - Model konferencyjny ze światłem reflektorów na świetnych prelegentów - by pokazać jak dobrzy są nasi prelegenci i budować efekt "chcę tu pracować"
1. II - Ściągnąć jak najwięcej osób wpływowych i znanych - przyciągnąć influencerów, za którymi przyjdą potencjalni pracownicy
1. III - Zapewnić odpowiednio dużą salę i ciekawe tematy - idziemy w ilość ludzi; im więcej przyjdzie, tym większy zasięg
1. IV - Zrobić ciekawe konkursy, silnie udzielać się w mediach społecznościowych, wygrywać nagrody - by wzmocnić prestiż i dobre uczucia

### 6.4. Implikacje

Samo porównanie działań strategicznych pomiędzy Warsztatniksem a Spotkaniem pokazuje, że Warsztatnix zupełnie nie rywalizuje z innymi Spotkaniami. Co więcej, Spotkania mogą wykorzystać Warsztatnix do wzmocnienia zasięgu - a osoby, którym pasuje etos Warsztatnixa mogą zdecydować, że mogą chodzić zarówno na spotkania Warsztatnixa jak i na spotkania Spotkania.

### 6.5. Komentarz

#### 6.5.1. O strategiach

Strategia to nie jest to, co się mówi. To jest to, co się robi. Jeżeli firma "nie ma strategii", to ma strategię - bo coś jest robione. Po prostu nie jest to strategia świadomie zarządzana.

Z innej beczki, [pod tym linkiem](https://www.ere.net/its-about-what-you-do-not-what-you-say-strategies-for-reducing-turnover/) macie przykład strategii redukcji rotacji - niech ludzie nie odchodzą z naszej firmy. W skrócie:

* Umożliwiajcie ludziom zmieniać działy / projekty w organizacji
* Zapewniajcie darmowy rozwój i szkolenia
* Płaćcie tyle, co rynek, lub więcej
* Sprawdzajcie rotację u każdego managera z osobna
* Pamiętajcie, że jesteśmy na rynku pracownika

Mam też propozycję alternatywnej strategii:

* Podwójcie pensję każdemu pracownikowi.
* Dajcie każdemu pracownikowi możliwość pracy nad tym, nad czym ma ochotę
* Skróćcie dzień pracy do 6 godzin.

Powyższa strategia zadziała jeszcze lepiej z perspektywy rotacji. Widzicie, na czym polega problem?

Skupiliśmy się na jednym parametrze (rotacja, czyli ludzie odchodzący z firmy) i zapomnieliśmy o Koncepcji i tych wszystkich rzeczach wyżej - Celach Strategicznych.

Po raz kolejny: Strategia to **droga do celu**. Nie macie celu - idziecie "gdzieś". Nie macie sensownych mierników - nie wiecie, kiedy tam dotrzecie. A nawet nie wiecie, w którym kierunku idziecie. Macie błędne mierniki? Aktywnie idziecie w złym kierunku. To już lepiej iść na ślepo.

#### 6.5.2. O celach i miarach

Można tu zarzucić "Żółwiu, ale jak masz coś takiego: 'Promować przynoszenie i rozwiązywanie problemów. Pozyskiwać problemy interesujące i rozwojowe dla osób obecnych.'" to to nie jest szczególnie realizowalne. Nadal nie wiadomo jak to zrobić.

I z tym się w 100% zgodzę. Ale to także można rozbić na mniejsze elementy. Potraktować to "jak Koncepcję" i postawić nowe, mniejsze Cele mające prowadzić do osiągnięcia tego konkretnego efektu. Także ustawić Miary dla tych mniejszych Celów i zbudować mierniki.

Coś takiego fachowo nazywa się dezagregacją celów.

Przejdźmy na moment do świata firm, korporacji, startupów i innych takich.

Coś, co jest Celem Strategicznym dla Zarządu firmy jest Wizją dla managerów wyższego stopnia. Na bazie tej Wizji oni budują własne Cele Strategiczne, które "stają się Wizją" dla managerów średniego stopnia. I tak dalej. Jak długo łańcuch mierników działa poprawnie i każdy miernik niższego poziomu prawidłowo wzmacnia miernik wyższego poziomu, tak długo to wszystko działa.

Tyle, że jest koszmarnie trudno to wszystko prawidłowo złożyć. A wraz ze zmianą Otoczenia organizacji (Warsztatnixa, firmy, produktu...) elementy Wizji muszą ulec zmianie - i wraz z nimi zmianie powinny ulec wszystkie mierniki niższego stopnia. W organizacji podzielonej na kilka kontynentów i mającej około 10 tysięcy ludzi ten proces przebudowy potrafiłby trwać latami.

Między innymi stąd jest taka niechęć do zarządzania przez twarde liczby i twarde cele. To byłoby zbyt biurokratyczne i zbyt sztywne. "Walczymy we wczorajszej wojnie". 

Ten problem zwykle rozwiązywany jest przy użyciu właściwego sprzężenia Natury (Kultury i Struktury) (o których tu nie pisałem) oraz przez pozostawienie ludziom wystarczająco wysokiego poziomu autonomii. To jednak jest problematyczne z perspektywy jakichkolwiek systemów oceny pracowników... i znowu mamy problem.

Szczęśliwie, przy prostszych konstrukcjach prezentowane narzędzie działa dość dobrze i nie wpadamy w paskudne problemy.

## 7. Podsumowanie

Dotarliśmy wreszcie do końca tej podróży. Na początek, pozwólcie, że przedstawię oba zaprojektowane wydarzenia w całości:

* [Warsztatnix](/scraps/180126-warsztatnix-wydarzenie)
* [Spotkanie](/scraps/180126-spotkanie-wydarzenie)

Zwróćcie uwagę na następujące cechy powyższych konstrukcji:

1. Z poprzedniej części wynika część następna. Od Koncepcji do Strategii.
1. Mimo, że zaczęliśmy od dwóch "podobnych" wydarzeń, są one w rzeczywistości bardzo odmienne.
1. Każde działanie musi być zaczepione w "czymś wyżej" - fundamentalnie wszystko wraca do poziomu Koncepcji.

Czego **nie** omówiliśmy:

* Natura: Jaki jest kontekst naszych działań? Czym jesteśmy? Jak działamy?
* Uruchomienie: Co trzeba zbudować? (częściowo poruszyliśmy)
* Synteza: Jak to zbudujemy?
* Przejście/ Tranzycja: Jak będziemy faktycznie działać
* **Otoczenie**: ...i co się dzieje w tym czasie poza konstruowanym Warsztatnixem?

Jeżeli jesteście zainteresowani powyższym, spójrzcie na literaturę uzupełniającą. Lub wypijmy razem kawę ;-).

Co jest tu istotnego:

1. Podejście Top-Down umożliwia usunięcie działań zbędnych i zrobienie tylko tego, co prowadzi do celu.
1. Ustalanie Celów Strategicznych i Miar umożliwia nam faktyczne zdefiniowanie i osiąganie sukcesu.
1. **Jest to podejście dość uniwersalne**. Działa podczas robienia prezentacji, programowania, pisania dokumentu, budowy hipotetycznych organizacji...
1. Jeśli potraficie działać zgodnie z checklistą - teraz TEN krok, teraz TEN - to jesteście w stanie zrobić dokładnie to samo co ja.

I z tymi pozytywnymi myślami Was zostawiam. Gratuluję przeczytania tego artykułu, jest trudny.

## 8. Wykazanie korzyści

### 8.1. Przypomnienie korzyści

Czy zatem dostaliście to, co Wam obiecałem na początku? Dla przypomnienia, obiecałem Wam to:

* Chcesz zaprojektować COŚ (produkt, organizację, program komputerowy), co ma pełnić jakąkolwiek funkcję. Acz - musi być celowy.
* Chcesz umieć określić "czym jest sukces", lub "czy to co robię, pozwoli mi osiągnąć ten sukces".
* Uważasz, że celowość nie jest szczególnie istotna i chcesz przeczytać kontrprzykład.

### 8.2. Dowody pośrednie

Sprawdźmy.

* Podejście Top-Down pokazuje, jak przekształcić coś abstrakcyjnego w kolejne kroki do wykonania.
* Faza Koncepcji wymusza zastanowienie się przed rozpoczęciem działań. To jest to "nie pisz kodu, póki nie wiesz co robisz".
* Koncepcja w połączeniu z Wizją wymusza stworzenie idealnego obrazu wpływu tego co robisz na rzeczywistość.
    * W świecie programowania to odpowiednik "jeśli nie wiesz, jak to przetestować... to po co to robisz? Nie wiesz kiedy skończyć".
    * W Agile to odpowiednik "Definition of Done".
    * Zabezpiecza nas to przed robieniem rzeczy dla robienia rzeczy. Każdy nasz ruch jakoś odbija się w rzeczywistości.
* Cele strategiczne i mierniki dają nam informacje odnośnie tego, czy idziemy we właściwym kierunku i jak daleko jesteśmy.
    * To jest właśnie definicja 'sukcesu'
* Cele strategiczne stają się czymś, dzięki czemu możemy wybrać odpowiednią Strategię. Cele strategiczne i mierniki są w pewien sposób kryteriami akceptacyjnymi dobrej strategii. "Czy wszystko co ważne udało mi się pokryć?"

I wszystko powyższe możemy sprowadzić do:

* Po co to robię? Co chcę zmienić?
* Co w sumie robię?
* Po czym to poznam?
* Czy już tam jestem?

Jeżeli nadal uważacie, że celowość działań nie jest szczególnie potrzebna, to Was zawiodłem. Moja wina.

## 9. Literatura uzupełniająca

1. "Skuteczne Wdrażanie Strategii", Mark Morgan, Raymond E. Levitt, William Malek
1. [Aligning your organization to execute its strategy](http://www.esi-intl.se/pmo2015/pdfs/aligning-organization-strategy.pdf)

## 10. Metadane artykułu

* Czas poświęcony na budowę szkicu: 2 godziny
* Ilość osób korygujących szkic (poza autorem): 2
* Czas poświęcony na napisanie i korektę artykułu: 9 godzin
* Ilość osób korygujących artykuł (poza autorem): 1
* Czas poświęcony na przebudowę artykułu później: 1 godzina
* Ilość osób korygujących artykuł po publikacji (poza autorem): ?
* Ilość błędów / modyfikacji artykułu po wydaniu: ?

Szkic, dla chętnych zobaczenia jak "robi się kiełbasę", [tutaj](/scraps/180126-warsztatnix-szkielet)
