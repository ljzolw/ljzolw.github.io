---
layout: post
title: "Jedna iteracja pracy nad systemem RPG"
date: 2017-05-08
---

## 1. Zakres artykułu

Rozpatruję tematy takie jak: 

* Gry RPG: a zwłaszcza mechanika, estetyka, świat i ich współdziałanie.
* Projektowanie produktu pod kątem określonych celów.
* Analiza informacji zwrotnej od użytkownika po zetknięciu użytkownika z produktem.
* Co to jest 'eksperyment' i jak go przeprowadzić

Wartość dodana dostarczona przeze mnie:

* Przeprowadzam pojedynczą iterację analizy informacji zwrotnej (feedbacku) i tłumaczę, jakiego typu decyzje muszą być podjęte.
* Pokazuję, w jaki sposób wykorzystać techniki ze świata inżynierii oprogramowania do pracy nad produktem innego typu.

## 2. Co to jest RPG?

Gry RPG są kontekstem tego wpisu, więc bez znajomości domeny niewiele się będzie dało zrozumieć. Jeśli wiecie co to są gry RPG, możecie przejść do następnego punktu. W innym wypadku... zapraszam ;-).

Nie zagłębiając się zbyt mocno w szczegóły (i upiornie upraszczając), gra RPG to gra w której **gracze** (ludzie siedzący przy stole) tworzą fikcję (wspólną opowieść) przez wykorzystywanie swoich **postaci** (awatary graczy, znajdujące się w fikcji) wykorzystując jako metodę rozwiązywania sporów **mechanikę** (zasady gry).

Poniższy obrazek dość dobrze ilustruje rozróżnienie między tymi konceptami:

![Po lewej stronie rysunku mamy dwie osoby siedzące przy stole. To jest to, co dzieje się w rzeczywistości. Po prawej mamy dwie postacie w fikcji. To jest to, co dzieje się w opowieści wspólnie tworzonej przez osoby siedzące przy stole. Lewy gracz mówi Prawemu graczowi, że chce poderwać pewną damę; w fikcji postać Lewego gracza próbuje to zrobić. Niestety, ta postać dostała kosza. W rzeczywistości Prawy gracz mówi Lewemu, że nie udało mu się poderwać tej damy.](/img/170508/rpg_whatis.png)

Na rysunku Lewy grający pyta Prawego, czy jego postać może poderwać pewną damę. Prawy proponuje sprawdzić, czy się udało stosując mechanikę. Niestety, wynik rzutu pokazuje, że postaci gracza Lewego nie udało się owej damy poderwać.

Po prawej stronie rysunku widzimy, jak się to odbyło w fikcji. Mimo, że Lewy gracz jedynie zadeklarował "to ja ją podrywam", wszyscy grający mogą założyć, że podrywająca postać się jednak trochę bardziej wysiliła w fikcji ;-). 

Innymi słowy, nie wszystko musi być dokładnie powiedziane i zasymulowane.

Jeden z grających (w naszym wypadku Prawy gracz) nie ma swojej postaci. Prawy gracz zarządza całym światem, ale nie kieruje historią - jego rolą jest to, by świat miał sens i odpowiednio reagował na działania innych graczy. To jest ściśle wydzielona rola która nosi nazwę Mistrza Gry (w skrócie: MG).

Każda postać ma jakieś umiejętności i możliwości działania. Takie informacje zapisywane są na karcie postaci, która stanowi część interfejsu komunikacyjnego pomiędzy graczem a grą.

## 3. Pojedynczy test produktu: gry RPG

Pod koniec kwietnia miałem przyjemność być na Pyrkonie (festiwalu fantastyki, kiedyś: konwencie RPG) w Poznaniu. Przy tej okazji przeprowadziliśmy eksperyment - przetestowaliśmy nasz system RPG na bardzo doświadczonych graczach, którzy jednak zazwyczaj grają w innym "stylu" niż ja z moją żoną. Dla wyjaśnienia: Poker i Tysiąc to gry karciane, acz gra się w nie zupełnie inaczej. Tak samo w różne gry RPG gra się zupełnie inaczej.

Ten konkretny eksperyment był interesujący, bo gracze uczestniczący w tej rozgrywce nie należeli do kategorii 'idealnego użytkownika produktu' oraz nie mieliśmy typowych 4 godzin na rozgrywkę. Mieliśmy niecałe 3 godziny na wszystko, łącznie ze zbudowaniem od zera postaci i powiedzeniem paru słów o świecie.

Ale - żeby można było coś nazwać "eksperymentem", musimy chcieć się czegoś dowiedzieć i musimy oczekiwać jakiegoś wyniku. W ramach tej rozgrywki ustaliłem więc następujące rzeczy do sprawdzenia:

| **Co sprawdzamy** | **Sukces** | **Porażka** |
| da się zrobić płynną rozgrywkę poniżej 3 godzin nawet, jeśli nie wiemy dość o świecie | nie będzie momentów 'nie wiem co robić' i rozgrywka się skończy (z zakończeniem zamykającym wątek) poniżej 3 godzin | każda inna sytuacja - w szczególności "zacięcia się" |
| gracze zawsze wiedzą jak grać postaciami i nie mają konfliktu 'gracz czy postać' | optymalna (najskuteczniejsza z perspektywy wpływu na grę) gra postacią jest zgodna z charakterem i wizją postaci | postać zachowuje się nielogicznie z perspektywy świata gry i/lub wizji owej postaci |
| uproszczona karta postaci jest szybka w budowie i jednocześnie w poprawny sposób pomaga graczowi zachować wizję postaci | kartę da się zbudować poniżej 10 minut, plus punkt o logiczności postaci | każda inna sytuacja |
| karty postaci i sama mechanika są łatwe w użyciu, czytelne i zrozumiałe | gracze sami zbudują karty postaci bez większego tłumaczenia i będą umieli audytować / stosować mechanikę na własną rękę | będą sytuacje, gdzie będę musiał pomóc w zrozumieniu karty i/lub mechaniki; w szczególności, gdy usłyszę "to nie ma sensu" |
| gra jest ogólnie "grywalna", nawet, jeśli nie w ich stylu | powiedzą mi to po tym, gdy zapytam | zaobserwuję coś przeciwnego - wycofanie, pasywność... albo niechęć do narzędzi oferowanych przez grę |

Ci z Was, którzy pochodzą ze świata inżynierii oprogramowania z pewnością rozpoznają powyższą formułę - to jest forma konstruowania kryteriów akceptacyjnych i weryfikacja produktu pod kątem warunków jego użycia. Innymi słowy, to jest tablica mająca zweryfikować, czy założenia odnośnie działania produktu w kontekście jego użycia są prawdziwe.

![Rysunek przedstawiający diagram: 'co chcemy osiągnąć' prowadzi do 'czym chcemy to zrealizować'. Po co - co - po czym poznamy sukces. Uwzględnione zostają priorytety. Następnie: budowanie produktu równolegle z weryfikacją produktu. Weryfikacja jest w kolorze pomarańczowym.](/img/170508/cykl_produktu.png)

A dokładniej, mówimy o fragmencie gdzie weryfikujemy produkt pod kątem oczekiwań (bloczek pomarańczowy).

Jako, że powyższy model może brzmieć dość abstrakcyjnie, spróbuję przetłumaczyć go na przypadek tego systemu RPG nad którym pracujemy (ewentualny brak czytelności wynika z braku miejsca i żargonu; skupcie się na tym jak produkt się dodefiniowuje w trakcie pracy nad nim):

![Ten sam diagram co powyżej, acz tym razem w bloczkach pojawia się tekst pokazujący założenia tego konkretnego systemu RPG. Najważniejszy bloczek - 'po co' - głosi 'chcę zrobić wspólną historię tak, by wszyscy tworzyli opowieść, bez wyjątku'. 'Co' - głosi 'Chcę to osiągnąć używając odpowiednio skonstruowanego systemu RPG gdzie gramy postaciami magów'](/img/170508/cykl_produktu_przyklad.png)

Na razie brzmi dobrze. Testy systemu RPG zakończyły się pozytywnie; dostaliśmy sukcesy we wszystkich badanych obszarach - zarówno w tych, w których grający byli obserwowani jak i w tych, gdzie grający udzielali informacji zwrotnej.

Pod sam koniec jednak pojawiły się bardzo istotne uwagi, która skutecznie pogorszyły mi humor:

* W tym systemie RPG magia wygląda jak oszukiwanie, nie jak magia. Wszystko dało się przy użyciu magii zrobić, nie było kosztu.
* Nie ma tego wrażenia, że jest się magiem. Gdy gram magiem, chcę czuć się jak mag.

I to jest faktyczny problem.

## 4. Problem magii i poczucia, że się jest magiem.

### 4.1. Dlaczego nikt tego nie zauważył wcześniej?

Praprzyczyna tego, że nikt tego nie zauważył leży w lewej części powyższego diagramu:

![Fragment powyższego diagramu, z zaznaczonym 'po co' gdzie nie ma ani słowa o magii i magach oraz z zaznaczonym 'co' gdzie jest wyraźny tekst 'gramy postaciami magów'](/img/170508/cykl_produktu_przyklad_problem.png)

Pierwotnym założeniem tego systemu RPG było to, żeby wszyscy gracze mogli wspólnie tworzyć historię - głównym warunkiem porażki jest sytuacja, w której dowolny gracz może być pozbawiony wpływu na fikcję (czy to z przyczyn mechanicznych czy społecznych, np. jest jedyną osobą niedoświadczoną w gronie weteranów). 

Dopiero na te założenia nałożyliśmy 'setting', czyli 'fikcyjny świat' - w naszym wypadku jest to "świat rzeczywisty w okolicach 2015 roku plus magia". Ale ta magia była tylko dodatkiem, przykładem settingu - ten setting może być dowolny. To może być fantasy, cyberpunk... u nas padło na 'magię'. Ale nie jest to w kategorii _co chcemy osiągnąć_ a w kategorii _w jaki sposób chcemy to osiągnąć_. To jest środek, a nie cel.

To powoduje, że w mechanice tego systemu RPG magia nie jest centralnym elementem rzeczywistości; jest jednym z modułów który można dodać bądź wyłączyć. W świecie - w fikcji - magia jest centralnym elementem fizyki i logiki świata. Jednak ta sytuacja nie występuje zupełnie w mechanice, przez co nie będzie "poczucia" bycia magiem.

Logiczną konsekwencją tego wyboru było to, że magia jest jedynie odpowiednikiem "bardzo zaawansowanej technologii", innymi słowy, umożliwia zrobienie pewnych rzeczy bez magii niemożliwych, ale jest rzeczą "zwyczajną" a nie "cudowną". Odpowiednik "poszerzonej fizyki" a nie "zaklęć".

Literaturowym odpowiednikiem takiej magii są wiedźmy w "Jednym Zaklęciem" Lawrence'a Watta-Evansa czy magia w genialnej tetralogii "Taniec Bogów" Mayera Alana Brennera. Ewentualnie, większość steampunka. Magia jako dziedzina nauki, magia jako narzędzia, coś "pospolitego". Tzw. "magia niska". W odróżnieniu od magii w "Harrym Potterze" czy w jakiekolwiek książce w cyklu D&D. Tam występuje magia jako cuda, magia, jako coś niezwykłego. Tzw. "magia wysoka".

Magia, która _mnie_ interesuje to zdecydowanie jest magia niska. Jednak tu pojawia się pytanie ze strony graczy: "czemu mam zatem grać magiem, jeśli nie ma się wrażenia, że gra się magiem"? Z rozmów na Pyrkonie wynikało jednoznacznie, że z przyczyn kulturowych (m.in. popularność "Harry'ego Pottera") od słowa "magia" oczekuje się przede wszystkim magii wysokiej.

**Zaznaczam - gdybym testował tą grę RPG jedynie na 'idealnym użytkowniku', ten problem by nie został odkryty.** Idealny użytkownik nie jest zainteresowany estetyką magii a jedynie wspólnym tworzeniem historii z grupą przyjaciół w settingu wybranym przez siebie.

### 4.2. Dlaczego to jest problem?

Problem leży w ogólnie rozumianej rozbieżności oczekiwań pomiędzy systemem RPG a graczami grającymi w ten system. Każda gra RPG próbuje modelować jakąś rzeczywistość - próbuje w sposób uproszczony oddać pewien świat i jego mechanizmy. Innymi słowy, jeśli _gra RPG niewłaściwie modeluje rzeczywistość_, ta gra nie działa. Wyobraźcie sobie Conana Barbarzyńcę, który nie potrafi pokonać pojedynczego goblina. To nie działa - to może być 'fantasy', ale na pewno nie 'fantasy o Conanie Barbarzyńcy'.

Za słowem "magia" kryje się pewna estetyka i pewne oczekiwania ze strony osób grających w grę RPG. Nieważne, czy te oczekiwania są prawidłowe, czy nie - jeżeli gracze oczekują magii wysokiej a otrzymają magię niską, będą czuli się oszukani. 

Wyobraźcie sobie, że idziecie na film o wampirach do kina. Widzieliście "Wywiad z wampirem" i Wam się podobał. Widzieliście "Zmierzch" i, cóż, przeżyjecie wampiry błyszczące w świetle.

Film jest niezły, zaczyna się scenami godnymi pierwszego "Obcego" ("Alien"). Aż do momentu pojawienia się wampira, który wyłonił się zza skały...

![Rysunek wampira. Ale ten wampir ma oczy jak komar, sześć odnóży jak komar, ssawkę jak komar... ogólnie, jest wzorowany na komarze a nie nietoperzu](/img/170508/wampir_owad.jpg)

Ten wampir jest "inny" - pije krew, ale nie jest wzorowany na nietoperzach jak większość swoich pobratymców. Nie, ten wampir jest wzorowany na komarach.

Na szczęście główny bohater miał przy sobie spray na owady i w efektownej walce udało mu się pokonać straszliwego adwersarza.

Po wyjściu z kina, podejrzewam, można by usłyszeć zdanie w stylu "całkiem niezły film, ale powinni inaczej nazwać tego potwora niż 'wampir'".

To właśnie jest powód, dla którego fakt, że z "magią" kojarzona jest tylko magia wysoka jest bardzo poważnym problemem naszego systemu.

**Nie ma znaczenia, czy ja mam rację, czy nie. Z perspektywy odbiorcy systemu RPG - percepcja odbiorcy jest jedyną, która ma znaczenie.** Odbiorca nie będzie starał się zrozumieć różnicy między magią wysoką a niską. Odbiorcy obiecano "grę, w której będzie grał magiem" - i odbiorca nie dostał tego, _na co liczył_.

### 4.3. Jak można rozwiązać ten problem?

#### 4.3.1. Zmienić nazwę z 'magii' na 'psionikę'

Jeżeli "magia" jest problematyczna, zawsze można zmienić nazwę na coś innego, coś co nie niesie za sobą takiego ładunku kulturowego. Zawsze można nazwać to "psioniką", "EP (energią paranormalną)" lub nawet "gulguthorem".

Zalety tego rozwiązania są takie, że mamy możliwość całkowitego zdefiniowania tego terminu, łącznie z budowaniem oczekiwań. Wadą jest to, że znacząco zwiększamy koszt kognitywny użytkownika gry - mniej więcej można oczekiwać, co "magia" robi, ale nikt nie ma pojęcia co robi "gulguthor".

Dodatkowo, nie rozwiązuje to problemu nie powiązania estetyki systemu (a dokładniej, mocy paranormalnych) z mechaniką systemu.

#### 4.3.2. Wydać system RPG bez mocy nadnaturalnych

Test udowodnił, że mechanika działa dla rozgrywek, w których nie występują moce paranormalne. Oczywistą sugestią jest stworzenie nowego settingu (świata), który nie zawiera żadnych mocy paranormalnych i skupieniu się na tej części gry.

Zalety tego rozwiązania są takie, że mechanika _działa_ w tego typu settingach (co zostało potwierdzone przez grupę testową; to było rozwiązanie przez tych graczy zaproponowane). Wady tego rozwiązania to dwa zasadnicze problemy:

Pierwszy problem - trzeba zaprojektować i zbudować spójny świat. To nigdy nie jest prosta sprawa. Co gorsza, nie będziemy mogli skorzystać z wyników ostatnich 10 lat pracy nad tym, co nosi chwilowo nazwę "Magic:Invasion".

Drugi problem - czasami na sesjach RPG niektóre rzeczy są fizycznie niemożliwe. Wprowadzenie magii lub mocy paranormalnych powoduje, że gracze _mogą_ osiągnąć rzeczy niemożliwe, jeśli są skłonni zapłacić odpowiednią cenę. Jest to ciekawy wehikuł budujący historię. Konwencja silnie realistyczna sprawia, że gracze mogą nie być w stanie osiągnąć większości swoich celów. Najczęściej dlatego, bo Mistrz Gry się pomylił i źle dopasował przeciwnika do rozgrywki (co się zdarza). Wtedy albo naruszona zostaje logika świata (bardzo źle), albo nikt z grających przy stole nie jest w stanie w żaden sposób rozwiązać sytuacji w ciekawy sposób, bez użycia jakiejś formy Deus Ex Machina (fatalnie).

Innymi słowy, znowu zrzucamy odpowiedzialność na Mistrza Gry. To nie jest dla mnie akceptowalne rozwiązanie.

#### 4.3.3. Wydać mechanikę bez settingu, nie system RPG

Jeżeli mamy sprawną mechanikę i nie mamy działającego do niej settingu, zawsze istnieje możliwość wydania samej mechaniki. Takie rzeczy są przyjęte w świecie RPG; bardzo popularną i sprawną mechaniką wydaną jako mechanika jest [FATE](http://www.faterpg.com/). Fakt, że większość Zespołów RPG i tak tworzy własne realia i rzeczywistość powoduje, że może można sobie darować tworzenie kompatybilnego settingu...

Zalety tego rozwiązania są takie, że wystarczy opisać istniejącą mechanikę i można ją od razu wydać. Jak zauważyłem wcześniej, świetnie działa.

Wady tego rozwiązania są bardziej złożone. Jak widać, ta mechanika nie do końca wspiera _każdy_ setting - w chwili obecnej nie do końca działa z siłami paranormalnymi z perspektywy symulacyjno-imersyjnej. To powoduje, że czysto przypadkowo użytkownik może stworzyć sobie setting do tej mechaniki który nie do końca będzie działał dla tego użytkownika i jego Zespołu.

Druga wada tego rozwiązania kryje się w poniższym zbiorze:

![Dwa zbiory - większy 'gracze kupujący grę RPG dla świata', mniejszy 'gracze kupujący grę RPG dla mechaniki'. W części wspólnej tych zbiorów jest jeszcze jeden, mniejszy zbiór głoszący 'nowi gracze'.](/img/170508/zbiory graczy.png)

Sprawdziliśmy, że nasz istniejący system RPG najlepiej sprawdza się dla dwóch kategorii graczy:

* Mistrzów Gry, którzy nie chcieli nigdy być Mistrzami Gry, ale "ktoś musiał" (i występują tu jako gracze)
* graczy całkowicie nowych, niedoświadczonych w grach RPG

Nowi i niedoświadczeni gracze RPG są tymi, którzy najczęściej kupią pełny system z nadzieją, że dostaną podręcznik, który powie im "jak grać". To powoduje, że wydanie tego systemu jako czystej mechaniki sprawi, że najpewniej nowi gracze nie będą zainteresowani zapoznaniem się z tą grą.

Co byłoby dość problematyczne, skoro ten system działa najlepiej dla tej kategorii graczy.

#### 4.3.4. Opracować mechanikę magii i ją włączyć do rdzenia systemu

Tutaj wpływamy już na kategorię 'po co to robię', czyli 'co chcę osiągnąć':

![Ten sam diagram co poprzednio. Zmienione jest 'po co' na 'chcę zrobić wspólną historię tak, by wszyscy tworzyli opowieść, bez wyjątku. Gracze mają mieć poczucie grania magami'. Wszystkie inne bloczki są zamazane czerwienią - trzeba je przebudować.](/img/170508/cykl_produktu_przyklad_rozwiazanie.png)

Zmiana wysokopoziomowej praprzyczyny powstania produktu wymaga ingerencji w całość produktu - zmieni się świat i mechanika a sama magia stanie się jednym z rdzennych komponentów całej mechaniki i rzeczywistości. Wzrośnie spójność pomiędzy mechaniką i światem oraz będzie dało się "poczuć", że gra się magiem. Co ważniejsze, estetyka świata będzie wspierana przez mechanikę.

Przy okazji, tutaj trzeba też podjąć decyzję - magia wysoka czy magia niska. Magia niska bardziej do mnie przemawia, jednak potencjalna grupa docelowa niestety jest bardziej zainteresowana magią wysoką...

Zalety tego rozwiązania są dość oczywiste - system będzie spójny i będzie ogólnie działał lepiej dla gry magiem. 

Wady - może być trudniej lub mniej przyjemnie grać "nie-magiem" niż w chwili obecnej (choć np. Ars Magica rozwiązała ten problem w dość elegancki sposób). Dodatkowo, wymagać to będzie bardzo poważną przebudowę założeń mechaniki i przez to samej mechaniki. Z uwagi na dodanie nowych priorytetów aktualna mechanika może ulec spowolnieniu lub utrudnieniu. Innymi słowy, to będzie bardzo dużo pracy.

Usunięcie "czystości" tego systemu przez dodanie komponentów paranormalnych ma tą wadę, że system może gorzej spełniać swoje podstawowe funkcje. Ma tą zaletę, że może dostarczać przyjemności graczom nie tylko ze wspólnego tworzenia fabuły, ale i z poczucia grania magiem w spójnym i logicznym świecie, gdzie mechanika silnie wspiera setting.

Ciekawostka - mieliśmy kiedyś sprawny i elegancki system magii wysokiej. Musiałem go skasować. Gracze zbyt dużo czasu skupiali się na kartach postaci i tym, jak złożyć ciekawe zaklęcie a zbyt mało czasu skupiali na podejmowaniu decyzji odnośnie tego, co ma się dziać w fikcji. Innymi słowy, zaczęli traktować grę RPG bardziej jak _grę logiczną_ (wchodząc w interakcję z przeszkodami stworzonymi przez Mistrza Gry traktując je jak zagadki) niż jak _grę fabularną_ (wchodząc w interakcję z innymi ludźmi - graczami i Mistrzem Gry - i próbując przesunąć fabułę do przodu).

Innymi słowy, nie do końca to, co chcemy od tego systemu. 

#### 4.3.5. Ostateczna decyzja?

Nie wiem :-).

Najprawdopodobniej spróbujemy opracować mechanikę magii wysokiej i retroaktywnie zmodyfikować świat w taki sposób, by nie stracić logiki tej rzeczywistości. Jest to możliwe, acz... **czego nie rozumiem, tego nie potrafię zamodelować**. Jako, że nie do końca rozumiem czym jest "poczucie, że gra się magiem", będzie wymagało to dużo więcej testów na różnych graczach.

To nie tak, że istnieje jedno dobre rozwiązanie. Wszystkie powyższe rozwiązania mogą zadziałać. Szczęśliwie, nie mamy limitu czasowego i mogę pozwolić sobie na jeszcze kilka eksperymentów.

Jeżeli wybrane rozwiązanie się nie powiedzie, zawsze mogę się cofnąć do poprzedniej wersji mechaniki i wybrać inną opcję.

Cóż, żyjemy w ciekawych czasach.

### 5. Obserwacje i wnioski

Mieliście okazję zobaczyć pojedynczą iterację analizy produktu pod kątem informacji zwrotnej od użytkownika. Jak to mówią, "żaden plan nie wytrzymał nigdy kontaktu z użytkownikiem" - a system RPG nie jest wyjątkiem. Szczęśliwie, problem został wykryty dość szybko; byłoby dużo gorzej, gdyby taka informacja zwrotna pojawiła się przy pierwszej papierowej książce...

Warto konfrontować produkt z użytkownikiem nie będącym 'użytkownikiem idealnym'. Warto też wiedzieć, kiedy informacja od takiego użytkownika może zostać zignorowana. Gdyby, na przykład, mój zespół testowy powiedział mi "za mało wczuwaliśmy się w nasze postacie", to ta informacja zostałaby przeze mnie odrzucona - ten system nie jest zaprojektowany do promowania immersji.

Z drugiej strony użytkownik nie będący 'użytkownikiem idealnym' jest w stanie zauważyć coś, czego 'użytkownik idealny' nie będzie w stanie zauważyć. Ta pojedyncza rozgrywka dostarczyła mi dużo danych do analizy i _znowu_ zmusiła mnie do zastanowienia się nad starym problemem - czy da się zrobić mechanikę dość uniwersalną, by wspierała pewne cele, czy jednak każda mechanika będzie musiała też wspierać estetykę settingu? Przed tamtą rozgrywką uważałem, że da się zrobić mechanikę dość uniwersalną. Teraz... nie jestem już pewny. 

Świat Warhammera bardzo różni się od świata D&D. Ta mechanika zadziałałaby w obu tych światach, ale różnica w estetyce tych światów nie zostałaby uwzględniona. Świat Warhammera to świat, który umiera; gdzie Chaos wygrywa. Bohaterowie próbują żyć w tym świecie, ale zwycięstwo Chaosu jest wpisane w tą rzeczywistość; to tylko kwestia czasu. Świat D&D to - dla odmiany - heroiczny świat, gdzie bohaterowie powstrzymują zło i są często ostatnią czy jedyną deską ratunku. Jeżeli to nie będzie uwzględnione w mechanice, cała estetyka staje się odpowiedzialnością Mistrza Gry... a zrzucenie nadmiaru odpowiedzialności na jedną osobę jedynie sprawia, że mało kto chce się w to bawić i mało kto chce pełnić tą rolę.

Mieliście też okazję zaobserwować, jak wykorzystać techniki ze świata wytwarzania oprogramowania do pracy nad czymś, co nie jest oprogramowaniem. Weryfikacja pod kątem określonych kryteriów to przecież praca QA. Podejmowanie decyzji odnośnie wariantu implementacji produktu pod kątem określonych celów do osiągnięcia to przecież praca programisty. I tak dalej...

Przy okazji mogliście też zobaczyć, że gdy pracuje się nad produktem to dużo większe znaczenie ma percepcja tego produktu ze strony odbiorców niż sam produkt. Wyobraźcie sobie, że zaprojektuję _idealny system oparty na magii niskiej_. Co z tego, jeśli odbiorcy nie są zainteresowani magią niską i będą rozczarowani, że "ta magia jest lipna"?

No i, chyba, najważniejsze. Pracujemy nad tym systemem od dawna. Ponad 10 lat. To nie oznacza, że zawsze mamy rację. Bardzo łatwo byłoby się obrazić na zespół testowy i powiedzieć "ale ta gra nie wspiera Waszego stylu". Byłoby to też bardzo szkodliwe - w ten sposób moglibyśmy nie zauważyć niektórych błędów i wypaczeń. Do każdej informacji warto podejść z otwartym umysłem - jeśli komuś zależy na Was na tyle, by udzielić Wam negatywnej informacji - warto zastanowić się nad tym, co się usłyszy.

(Udzielenie pozytywnej informacji zwrotnej nic nie kosztuje. Udzielenie negatywnej informacji zwrotnej wiąże się z ryzykiem konfrontacji lub urażenia kogoś. To powoduje, że negatywna, konstruktywna informacja zwrotna jest czymś kosztownym dla osoby udzielającej tej informacji)
