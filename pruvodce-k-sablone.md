# 📖 Průvodce k univerzální specifikační šabloně

> **K čemu je tento dokument**
> Toto je doprovodná příručka k souboru `univerzalni-specifikacni-sablona.md`. U **každé části** šablony vysvětluje čtyři věci:
> - **🎯 Proč je důležitá** — jaký problém řeší a co se stane, když ji vynecháš.
> - **📌 Co je jejím cílem** — k čemu přesně slouží.
> - **✅ Čeho dosáhne** — jaký konkrétní výsledek přinese.
> - **⚠️ Příklad chyby** — jak vypadá situace, kdy část chybí nebo je špatně vyplněná.
>
> Příručka kopíruje pořadí sekcí ve šabloně, takže oba soubory můžeš číst vedle sebe. Nemusíš ji číst celou najednou — když narazíš na sekci, které nerozumíš, najdi si ji tady.

---

## Než začneme: tři pojmy, které tě v textu potkají

- **SDD (Spec-Driven Development)** — „nejdřív popiš CO a PROČ, pak teprve piš kód". Specifikace je výchozí bod, ne dodatek.
- **TDD (Test-Driven Development)** — „nejdřív napiš test, který padá, pak kód, který ho rozsvítí na zeleno". Test je zadání i pojistka.
- **BDD (Behavior-Driven Development)** — „popiš chování slovy, kterým rozumí i netechnický člověk" (formát *Dáno–Když–Pak*). Tyto popisy jdou rovnou spustit jako testy.

Šablona tyto tři přístupy spojuje do jednoho dokumentu. Tato příručka vysvětluje, proč každá jednotlivá část za to spojení stojí.

---

# 🧭 Hlavička dokumentu

**🎯 Proč je důležitá**
Hlavička je „štítek na krabici". Bez ní za měsíc nepoznáš, jestli se díváš na aktuální verzi, nebo na poznámku, kterou jsi dávno přepsal. U dokumentu, který se vyvíjí společně s kódem, je informace „kdy naposledy upraveno" a „jaký je stav" stejně důležitá jako samotný obsah.

**📌 Co je jejím cílem**
Dát komukoliv (včetně tebe za půl roku a včetně AI asistenta) okamžitou orientaci: o co jde, v jaké je to fázi, kdo na tom dělá a jakými nástroji. Pole „Stav" navíc říká, jestli se podle dokumentu už smí stavět, nebo se ještě domlouvá.

**✅ Čeho dosáhne**
Zabrání práci podle zastaralé verze. Jakmile uvidíš `Stav: Návrh`, víš, že obsah ještě není závazný; `Stav: Schváleno` znamená „tohle platí, můžeš podle toho implementovat".

**⚠️ Příklad chyby**
Necháš v projektu tři textové soubory s nápady a po týdnu nevíš, který je ten platný. Začneš programovat podle starého a půl dne předěláváš něco, co už dávno neplatí.

---

# 1. SPECIFIKACE — *CO a PROČ stavíme*

Toto je srdce celého dokumentu. Než půjdeme po jednotlivých pod-sekcích, jedna zastřešující myšlenka: specifikace popisuje **záměr a chování**, nikdy ne technologii. „Tlačítko odešle formulář a uživatel uvidí potvrzení" patří sem. „Použijeme React a fetch()" patří až do části 3 (Plán). Tohle oddělení je nejčastější věc, kterou začátečníci pletou — proto se k němu budu vracet.

## 1.1 Záměr a kontext (PROČ)

**🎯 Proč je důležitá**
AI (a často i člověk) udělá přesně to, co řekneš — ne to, co jsi myslel. Když nevysvětlíš *proč* něco stavíš, asistent „optimalizuje na špatnou věc": technicky splní zadání, ale mine podstatu. Sekce „Mimo rozsah" (non-goals) je tu proto, že nejčastější způsob, jak projekt nabobtná a nikdy se nedokončí, je tiché přidávání „a ještě by to mohlo…". Když si předem napíšeš, co *ne*děláš, máš se o co opřít, až ta pokušení přijdou.

**📌 Co je jejím cílem**
Zachytit problém, hodnotu a hranice dřív, než se napíše jediný řádek kódu. Odpovídá na tři otázky: Jaký problém řeším? Pro koho? A co naopak vědomě nechávám stranou?

**✅ Čeho dosáhne**
Dá všem dalším rozhodnutím měřítko. Kdykoliv se později ptáš „mám přidat tuhle funkci?", porovnáš ji se záměrem a hned víš. Drží projekt malý a dokončitelný.

**⚠️ Příklad chyby**
Zadáš AI „udělej mi appku na poznámky". Dostaneš poznámkovou appku se sdílením, tagy, exportem do PDF a tmavým režimem — a ani jedno z toho jsi nechtěl, protože ti šlo jen o rychlé zapisování nápadů. Kdybys napsal non-goal „žádné sdílení ani export", ušetříš si hodiny zbytečné práce a kódu, který teď musíš udržovat.

## 1.2 Uživatelské scénáře — *co uživatel dělá*

**🎯 Proč je důležitá**
Lidé i jazykové modely chápou **konkrétní příklady** mnohem líp než abstraktní popisy. „Systém spravuje uživatele" je mlhavé; „když Anna zadá e-mail, který už existuje, uvidí hlášku 'účet už existuje'" je jednoznačné. Priority (P1/P2/P3) jsou důležité proto, že bez nich AI i ty utopíte čas v okrajových vychytávkách, zatímco hlavní funkce ještě nestojí.

**📌 Co je jejím cílem**
Popsat chování očima uživatele přes konkrétní scénáře ve formátu *Given–When–Then* (Dáno–Když–Pak) a seřadit je podle toho, co je nezbytné (P1) a co počká (P3). Každý scénář je zároveň polotovar budoucího testu.

**✅ Čeho dosáhne**
Vznikne přesné, otestovatelné zadání. Z každého „Then" se dá přímo udělat kontrola, jestli to funguje. Priority navíc určí pořadí práce, takže nejdřív stojí to nejdůležitější.

**⚠️ Příklad chyby**
Napíšeš jen „uživatel se přihlásí". AI vyrobí přihlášení, které funguje pro správné heslo, ale při špatném hesle spadne s nicneříkající chybou — protože jsi scénář pro špatné heslo nikdy nezmínil. Konkrétní scénář *„Given špatné heslo, When přihlášení, Then hláška a možnost zkusit znovu"* by to ošetřil.

### 1.2.x Hraniční případy (Edge Cases)

**🎯 Proč je důležitá**
Hraniční případy jsou přesně tam, kde AI nejvíc chybuje a kde reálné aplikace nejčastěji padají. „Šťastnou cestu" (vše proběhne správně) zvládne každý; rozdíl mezi hračkou a použitelným programem je v tom, co se stane při prázdném vstupu, výpadku sítě nebo dvojím kliknutí.

**📌 Co je jejím cílem**
Donutit tě vyjmenovat nepříjemné situace předem — chybné vstupy, chybějící oprávnění, nedostupné služby, souběžné akce. Je to seznam „co se může pokazit".

**✅ Čeho dosáhne**
Aplikace, která se chová předvídatelně i ve chvíli, kdy něco selže. Místo pádu uvidí uživatel srozumitelnou hlášku, a ty nemusíš tytéž chyby pracně hledat až v provozu.

**⚠️ Příklad chyby**
Appka funguje krásně, dokud do pole pro věk někdo nenapíše „dvacet" místo „20". Protože tato možnost nebyla nikde zmíněná, program spadne. Jeden řádek edge case („Co se stane při nečíselném vstupu?") by upozornil, že to musíš ošetřit.

## 1.3 Funkční požadavky — *co systém musí umět*

**🎯 Proč je důležitá**
Scénáře (1.2) ukazují příběhy; funkční požadavky je převádějí na přesná, číslovaná pravidla. Číslování (FR-001, FR-002…) není byrokracie — umožňuje **vystopovatelnost**: ke každému slíbenému požadavku dohledáš odpovídající kód a test. Styl EARS (krátké věty „Systém MUSÍ…", „Když… systém MUSÍ…") existuje proto, že běžná čeština je dvojznačná a AI z dvojznačnosti vyrobí dvojznačný kód.

**📌 Co je jejím cílem**
Zformulovat jednoznačná, ověřitelná pravidla chování, každé s vlastním číslem. Značka `[NEEDS CLARIFICATION: …]` slouží k tomu, abys nejasnost nezametl pod koberec, ale viditelně označil k dořešení.

**✅ Čeho dosáhne**
Vznikne kontrolní seznam, podle kterého na konci ověříš „je hotové vše, co bylo slíbeno?". Díky číslům a jednoznačným větám AI implementuje přesně to, co tam stojí — ne svoji interpretaci.

**⚠️ Příklad chyby**
Napíšeš požadavek „přihlášení by mělo být bezpečné". To je nicneříkající — AI si „bezpečné" vyloží po svém. Oproti tomu *„FR-005: Pokud uživatel zadá špatné heslo 5× za sebou, systém MUSÍ účet na 15 minut zamknout"* je konkrétní, otestovatelné a nedá se vyložit dvěma způsoby.

## 1.4 Nefunkční požadavky — *jak DOBŘE*

**🎯 Proč je důležitá**
Funkční požadavky říkají, *co* systém dělá; nefunkční říkají, *jak dobře* to dělá — jak rychle, jak bezpečně, jak dostupně. Tyhle vlastnosti se strašně těžko dolepují zpětně. Bezpečnost a výkon vestavěné od začátku jsou levné; přidávané do hotového systému jsou drahé a křehké.

**📌 Co je jejím cílem**
Zachytit měřitelné nároky na kvalitu — rychlost odezvy, bezpečnostní pravidla, dostupnost, náklady. Klíčové slovo je *měřitelné*: ne „rychlé", ale „pod 200 ms".

**✅ Čeho dosáhne**
Systém, který nejen funguje, ale je i použitelný v reálných podmínkách. Zároveň dáš AI jasné mantinely — bez řádku o bezpečnosti ti klidně uloží hesla v čitelné podobě.

**⚠️ Příklad chyby**
Appka funguje skvěle s deseti záznamy, ale při tisíci se načítá deset sekund, protože nikdy nebyl stanoven výkonový cíl. Řádek *„Seznam se MUSÍ načíst do 1 s při 10 000 záznamech"* by vývoj nasměroval na efektivní řešení od začátku.

## 1.5 Datové entity

**🎯 Proč je důležitá**
Většina aplikací je hlavně o tom, jak spolu souvisí data. Pokud předem nepojmenuješ „věci" v systému a jejich vztahy, AI si je domyslí — a domyslí si je nekonzistentně (na jednom místě „zákazník", na druhém „uživatel", na třetím „klient", a najednou nikdo neví, jestli je to totéž).

**📌 Co je jejím cílem**
Vyjmenovat hlavní datové objekty, jejich klíčová pole a vztahy mezi nimi (jeden zákazník má více objednávek apod.). Pozor — popisuješ *co* data jsou, ne *jak* je uložíš (konkrétní tabulky DB jsou „JAK", patří do plánu).

**✅ Čeho dosáhne**
Konzistentní pojmenování a strukturu napříč celým projektem. AI i ty mluvíte o stejných věcech stejnými slovy, což zásadně snižuje zmatky a chyby.

**⚠️ Příklad chyby**
V jedné části kódu je „objednávka" napojená na „uživatele", v jiné na „zákazníka". Protože nikde nebylo řečeno, že je to tentýž objekt, vzniknou dvě nekompatibilní struktury a propojení dat se rozsype.

## 1.6 Kritéria úspěchu — *měřitelná*

**🎯 Proč je důležitá**
Bez měřitelného cíle nikdy nevíš, kdy je hotovo — a „skoro hotovo" se umí táhnout donekonečna. Měřitelnost zároveň brání sebeklamu: „je to docela rychlé" je pocit, „načte se do 2 s" je fakt, který se dá ověřit.

**📌 Co je jejím cílem**
Definovat konkrétní, číselně ověřitelné výsledky, podle kterých poznáš úspěch. Liší se od funkčních požadavků tím, že měří *dopad* („uživatel dokončí registraci do 2 minut"), ne jen schopnost.

**✅ Čeho dosáhne**
Jasnou cílovou čáru. Ty i AI víte, kam směřujete, a po dokončení objektivně ověříte, jestli jste tam došli.

**⚠️ Příklad chyby**
Cíl zní „registrace má být jednoduchá". Postavíš osmikrokový formulář, který *technicky* funguje, ale uživatelé ho v půlce opouštějí. Měřitelné kritérium *„90 % uživatelů dokončí registraci do 2 minut"* by hned ukázalo, že „jednoduchá" znamená výrazně méně kroků.

## 1.7 Předpoklady a otevřené otázky

**🎯 Proč je důležitá**
Každý projekt stojí na tichých předpokladech („uživatel má internet", „má účet", „platí v korunách"). Když jsou *nevyřčené*, naráží se na ně až v půlce práce, kdy už je drahé je měnit. Otevřené otázky jsou poctivé přiznání „tohle ještě nevím" — lepší než si tiše domyslet špatnou odpověď.

**📌 Co je jejím cílem**
Vytáhnout skryté předpoklady na světlo a sepsat nezodpovězené otázky na jedno viditelné místo, aby se na ně nezapomnělo.

**✅ Čeho dosáhne**
Méně nepříjemných překvapení v půlce vývoje. Otevřené otázky se stanou viditelným seznamem věcí k dořešení, místo aby zůstaly skrytými minami.

**⚠️ Příklad chyby**
Tiše předpokládáš, že appku budou používat jen češtináři, a celé to napíšeš napevno česky. O dva týdny později přijde požadavek na angličtinu a přepisuješ texty roztroušené po celém kódu. Předpoklad *„zatím pouze čeština"* zapsaný předem by aspoň upozornil, že je to vědomé rozhodnutí k případnému přehodnocení.

---

# 2. ÚSTAVA PROJEKTU — *neměnné principy*

**🎯 Proč je důležitá**
Specifikace (část 1) řeší jednu konkrétní funkci. Ústava řeší pravidla, která platí **napříč úplně vším** — styl, testování, bezpečnost, jak velké dělat změny. AI nemá paměť mezi sezeními, takže bez takového „pevného bodu" by ti v každém novém sezení mohla míchat styly a porušovat zásady, na kterých ti záleží. Ústava je sada pravidel, která si agent přečte pokaždé.

**📌 Co je jejím cílem**
Zachytit pár nepřekročitelných principů projektu na jednom místě. Důraz je na slově *pár* — ústava má být krátká a konkrétní, ne stostránkový manuál (dlouhý seznam pravidel AI stejně přestane sledovat).

**✅ Čeho dosáhne**
Konzistenci napříč celým projektem a všemi sezeními s AI. Každá nová funkce ctí stejné standardy, takže projekt nezdegeneruje do směsice nesourodých přístupů.

**⚠️ Příklad chyby**
V pondělí AI napíše kód s testy, v úterý (nové sezení) je vynechá, ve středu použije jinou konvenci pojmenování. Po týdnu máš projekt, který vypadá, jako by ho psali tři různí lidé. Princip *„Žádný kód bez testu; pojmenování v camelCase"* v ústavě by tomu zabránil.

---

# 3. PLÁN IMPLEMENTACE — *JAK to postavíme*

**🎯 Proč je důležitá**
Tady se konečně řeší technologie — a je oddělená od specifikace záměrně. Když máš „CO" (spec) a „JAK" (plán) zvlášť, můžeš stejné zadání postavit dvěma způsoby a porovnat je, aniž bys přepisoval celou specifikaci. Oddělení také brání tomu, aby ti technické detaily zaplevelily čitelnou specifikaci, kterou má rozumět i netechnický člověk.

**📌 Co je jejím cílem**
Rozhodnout a zapsat, jakými prostředky specifikaci naplníš: jazyk, knihovny, úložiště, struktura projektu. „Brána kontroly ústavy" je tu jako pojistka — než začneš stavět, ověříš, že tvůj plán neporušuje principy z části 2.

**✅ Čeho dosáhne**
Promyšlený technický směr dřív, než se píše kód. AI (i ty) máte mapu, takže neimprovizujete za pochodu a nevzniká chaotická architektura, kterou by bylo nutné později bourat.

**⚠️ Příklad chyby**
Necháš technické detaily prosáknout do specifikace („uložíme to do PostgreSQL tabulky users se sloupcem email VARCHAR(255)"). Netechnický spolupracovník přestane specifikaci rozumět a zároveň ses zbytečně zavázal k jednomu řešení dřív, než jsi zvážil alternativy. Tyhle detaily patří sem, do plánu.

---

# 4. ÚKOLY — *rozpad na malé kroky*

**🎯 Proč je důležitá**
Velké zadání („postav přihlašování") je pro AI past — vygeneruje hromadu kódu najednou, ty ji nestihneš pořádně zkontrolovat, a stačí jedna skrytá chyba dole, aby se celá stavba sesypala. Malé úkoly produkují malé změny, které *jdou* přečíst a ověřit. Je to rozdíl mezi „zkontroluj těchto 500 řádků" a „zkontroluj tuto jednu patnáctiřádkovou funkci".

**📌 Co je jejím cílem**
Rozsekat práci na nejmenší smysluplné kroky, seřadit je do fází a vyznačit závislosti (co musí být první) a co lze dělat souběžně (značka `[P]`). Pořadí uvnitř fáze respektuje TDD: nejdřív padající test, pak kód.

**✅ Čeho dosáhne**
Práce po zvládnutelných soustech, kde po každém kroku máš funkční, ověřený mezistav. Když se něco pokazí, víš přesně u kterého malého úkolu — ne že hledáš jehlu v kupce vygenerovaného kódu.

**⚠️ Příklad chyby**
Zadáš AI „naimplementuj celý e-shop košík" jedním příkazem. Dostaneš 800 řádků, mezi nimi tichou chybu ve výpočtu DPH. Protože všechno přišlo najednou, chybu objevíš až když ti sedí špatné částky, a dohledáváš ji hodiny. Rozpad na úkoly „přidat položku → spočítat mezisoučet → přidat DPH → …", každý s vlastním testem, by chybu odhalil hned u příslušného kroku.

---

# 5. BDD SCÉNÁŘE — *chování viditelné uživateli*

**🎯 Proč je důležitá**
Gherkin (formát *Pokud–Když–Pak*) má dvojí kouzlo: čte se skoro jako běžná věta, takže mu rozumí i netechnický člověk, a zároveň jde **spustit jako automatický test**. Tím vzniká „živá dokumentace" — popis chování, který se nemůže rozejít s realitou, protože když se chování změní, příslušný scénář prostě spadne a ty to hned víš.

**📌 Co je jejím cílem**
Zapsat konkrétní příklady chování tak, aby současně sloužily jako srozumitelná dokumentace i jako vykonatelné testy. Pravidlo „jeden scénář = jedno chování" udržuje scénáře čisté a snadno pochopitelné.

**✅ Čeho dosáhne**
Dokumentace a testy v jednom, které nikdy nezastarají bez varování. Když někdo (i AI) omylem rozbije chování, test selže a upozorní na to dřív, než se chyba dostane k uživatelům.

**⚠️ Příklad chyby**
Napíšeš vágní scénář „Pak to funguje". To se nedá automaticky ověřit ani z toho člověk nepozná, co se má stát. Konkrétní *„Pak uvidím hlášku 'Uloženo' a počet poznámek se zvýší o jednu"* lze spustit jako test i přečíst jako srozumitelný popis.

---

# 6. AGENTS.md — *pravidla pro AI agenta*

**🎯 Proč je důležitá**
AI asistent začíná každé sezení jako čistý list — nepamatuje si, co bylo včera, ani jak je tvůj projekt postavený. `AGENTS.md` (nebo `CLAUDE.md`) je první věc, kterou si přečte, a říká mu, jak se v projektu chovat: jak spustit testy, kde jsou hranice, co nikdy nedělat. Bez něj agent hádá a hádá špatně. Klíčové je držet ho *krátký* — když ho přeplníš, AI začne pravidla ignorovat (zahltí se).

**📌 Co je jejím cílem**
Předat agentovi nezbytný provozní kontext a mantinely na jednom místě: příkazy, postup práce (nejdřív plán, pak kód; TDD), a tři kategorie hranic — co dělat vždy (✅), na co se ptát (⚠️) a co nikdy (🚫). Styl kódu sem nepatří — od toho je linter, který to vynutí automaticky a zdarma.

**✅ Čeho dosáhne**
AI, která pracuje v souladu s tvými pravidly i bez toho, abys je v každém promptu opakoval. Mantinel „🚫 Nikdy necommituj tajné klíče" třeba zabrání tomu, aby ti agent nahrál do veřejného repozitáře hesla.

**⚠️ Příklad chyby**
Nemáš žádná pravidla a v jednom sezení AI „opraví" padající test tím, že ho přepíše, aby procházel — místo aby spravila kód. Test je teď nazelenalý, ale chyba v kódu zůstala. Pravidlo *„NEMĚŇ testy jen proto, aby prošly. Oprav kód."* přesně tohle blokuje.

---

# ✅ 7. Definice hotového (Definition of Done)

**🎯 Proč je důležitá**
„Hotovo" znamená pro každého něco jiného — pro jednoho „kód jsem napsal", pro druhého „je to otestované, zdokumentované a nasazené". Bez společné definice se stává, že odškrtneš úkol jako splněný, ale chybí testy, dokumentace nebo ošetření chyb, a vrací se to jako bumerang. Tahle sekce je společný kontrolní seznam *pro každou* dodávku.

**📌 Co je jejím cílem**
Stanovit jednotnou laťku „kdy je práce skutečně dokončená" — testy zelené, edge cases ošetřené, žádné nevyřešené nejasnosti, specifikace zaktualizovaná, a (důležité u AI) kód, kterému *ty* rozumíš.

**✅ Čeho dosáhne**
Konzistentní kvalitu napříč vším, co dodáš. Nic se neprohlásí za hotové, dokud opravdu hotové není, takže se nehromadí skrytý nedodělaný dluh.

**⚠️ Příklad chyby**
Odškrtneš funkci jako „hotovo", protože běží na tvém počítači. Jenže nemá testy, takže ji o měsíc později nechtěně rozbiješ a nikdo si toho nevšimne. Položka v DoD *„Všechny scénáře procházejí jako automatické testy"* by funkci nepustila dál bez té pojistky.

---

# 🔄 8. Údržba a relevance — *aby se specifikace nerozešla s kódem*

**🎯 Proč je důležitá**
Tohle je nejčastější způsob, jak hezká specifikace umře: kód se vyvíjí, dokument se neaktualizuje, a po pár týdnech popisuje něco, co už neplatí. Tomu se říká **spec drift**. Jakmile k němu dojde, lidé (i AI) přestanou dokumentu věřit a je z něj jen mrtvý text. Sekce 8 je sada návyků, které tomu brání.

**📌 Co je jejím cílem**
Udržet dokument a kód v souladu pomocí jednoduchých pravidel: jediný zdroj pravdy, verzování v Gitu, krátké smyčky (po každé změně aktualizuj spec), a hlavně automatické testy v CI, které samy odhalí, když se kód a popis rozejdou. „Historie revizí" drží přehled o tom, co a kdy se měnilo.

**✅ Čeho dosáhne**
Živý dokument, kterému lze i za rok věřit, protože se mění *spolu* s kódem, ne pozadu za ním. Testy navíc fungují jako automatický hlídač — když kód uteče od specifikace, build spadne dřív, než si toho někdo všimne v provozu.

**⚠️ Příklad chyby**
Půl roku po startu se nový spolupracovník (nebo AI) zeptá specifikace, jak se chová platba. Dokument tvrdí jednu věc, kód dělá druhou, protože se to mezitím změnilo a nikdo to nezapsal. Strávíš odpoledne dohadováním, co vlastně platí. Návyk „každou změnu promítni i do specifikace + testy v CI" by dokument udržel pravdivý.

---

## 🧩 Jak to celé do sebe zapadá (rychlé shrnutí)

Když se na šablonu podíváš z výšky, je v ní jeden logický řetězec:

1. **PROČ** to děláme (1.1) → vede k tomu, **CO** uživatel dělá (1.2) → z čehož plynou **přesná pravidla** (1.3) a **kvalita** (1.4).
2. Tato pravidla a chování se převedou na **testy** (BDD scénáře v části 5 + TDD v úkolech části 4), které slouží jako **zadání i pojistka**.
3. **Plán** (3) a **úkoly** (4) říkají, **jak** to postavit, krok za krokem.
4. **Ústava** (2) a **AGENTS.md** (6) drží **AI i tebe** v mantinelech po celou dobu.
5. **Definice hotového** (7) je **cílová čára** a **údržba** (8) zajišťuje, že to **zůstane pravdivé** i potom.

Začátečníkovi stačí na začátku rozumět hlavně části **1** (specifikace) a **6** (pravidla pro AI). Zbytek dává smysl postupně, jak projekt poroste — a tato příručka tu bude, až na něj dojde řada.

---

<sub>Tento průvodce vysvětluje sekce souboru `univerzalni-specifikacni-sablona.md`. Čti je vedle sebe — pořadí kapitol je shodné.</sub>
