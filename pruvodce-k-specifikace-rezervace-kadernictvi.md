# 📖 Průvodce ke specifikaci rezervačního systému kadeřnictví

> **K čemu je tento dokument**
> Toto je doprovodná příručka k souboru `specifikace-rezervace-kadernictvi.html` — vyplněné specifikaci rezervačního webu kadeřnictví. U **každé části** specifikace vysvětluje čtyři věci:
> - **🎯 Proč je důležitá** — jaký problém řeší a co se stane, když ji vynecháš.
> - **📌 Co je jejím cílem** — k čemu přesně slouží.
> - **✅ Čeho dosáhne** — jaký konkrétní výsledek přinese.
> - **⚠️ Příklad chyby** — jak vypadá situace, kdy část chybí nebo je špatně vyplněná (na příkladech z tohoto kadeřnického projektu).
>
> Příručka kopíruje pořadí sekcí ve specifikaci, takže oba soubory můžeš číst vedle sebe. Nemusíš ji číst celou najednou — když narazíš na sekci, které nerozumíš, najdi si ji tady.

---

## Než začneme: tři pojmy, které tě v textu potkají

- **SDD (Spec-Driven Development)** — „nejdřív popiš CO a PROČ, pak teprve piš kód". Specifikace je výchozí bod, ne dodatek.
- **TDD (Test-Driven Development)** — „nejdřív napiš test, který padá, pak kód, který ho rozsvítí na zeleno". Test je zadání i pojistka.
- **BDD (Behavior-Driven Development)** — „popiš chování slovy, kterým rozumí i netechnický člověk" (formát *Dáno–Když–Pak*). Tyto popisy jdou rovnou spustit jako testy.

Tato specifikace všechny tři přístupy spojuje do jednoho dokumentu. Příručka vysvětluje, proč každá jednotlivá část za to spojení stojí — konkrétně na příkladu kadeřnictví, kde si zákazník sám rezervuje termín.

---

# 🧭 Hlavička dokumentu

**🎯 Proč je důležitá**
Hlavička je „štítek na krabici". Bez ní za měsíc nepoznáš, jestli se díváš na aktuální verzi, nebo na poznámku, kterou jsi dávno přepsal. U dokumentu, který se vyvíjí společně s kódem, je informace „kdy naposledy upraveno" a „jaký je stav" stejně důležitá jako samotný obsah.

**📌 Co je jejím cílem**
Dát komukoliv (včetně tebe za půl roku a včetně AI asistenta) okamžitou orientaci: o co jde, v jaké je to fázi, kdo na tom dělá a jakými nástroji. Pole „Stav" navíc říká, jestli se podle dokumentu už smí stavět, nebo se ještě domlouvá.

**✅ Čeho dosáhne**
Zabrání práci podle zastaralé verze. V této specifikaci stojí `Stav: Schváleno` a `Verze: 1.0` — to znamená „tohle platí, kadeřnický web se podle toho už smí stavět". Kdyby tam bylo `Návrh`, věděl bys, že obsah ještě není závazný.

**⚠️ Příklad chyby**
Necháš v projektu tři textové soubory s nápady na rezervační web a po týdnu nevíš, který je ten platný. Začneš programovat podle starého — třeba se starým pravidlem zrušení „48 h předem" místo aktuálních „24 h" — a půl dne předěláváš něco, co už dávno neplatí.

---

# 1. SPECIFIKACE — *CO a PROČ stavíme*

Toto je srdce celého dokumentu. Než půjdeme po jednotlivých pod-sekcích, jedna zastřešující myšlenka: specifikace popisuje **záměr a chování**, nikdy ne technologii. „Zákazník vybere volný slot, potvrdí a uvidí potvrzení rezervace" patří sem. „Použijeme Next.js a Prisma" patří až do části 3 (Plán). Tohle oddělení je nejčastější věc, kterou začátečníci pletou — proto se k němu budu vracet.

## 1.1 Záměr a kontext (PROČ)

**🎯 Proč je důležitá**
AI (a často i člověk) udělá přesně to, co řekneš — ne to, co jsi myslel. Když nevysvětlíš *proč* něco stavíš, asistent „optimalizuje na špatnou věc": technicky splní zadání, ale mine podstatu. Sekce „Mimo rozsah" (non-goals) je tu proto, že nejčastější způsob, jak projekt nabobtná a nikdy se nedokončí, je tiché přidávání „a ještě by to mohlo…". Když si předem napíšeš, co *ne*děláš, máš se o co opřít, až ta pokušení přijdou.

**📌 Co je jejím cílem**
Zachytit problém, hodnotu a hranice dřív, než se napíše jediný řádek kódu. Odpovídá na tři otázky: Jaký problém řeším? (telefon ruší kadeřnici při stříhání, vznikají dvojité rezervace) Pro koho? (malé kadeřnictví a jeho zákazníci) A co naopak vědomě nechávám stranou?

**✅ Čeho dosáhne**
Dá všem dalším rozhodnutím měřítko. Kdykoliv se později ptáš „mám přidat tuhle funkci?", porovnáš ji se záměrem a hned víš. Drží projekt malý a dokončitelný.

**⚠️ Příklad chyby**
Zadáš AI „udělej mi rezervační web pro kadeřnictví". Dostaneš systém s online platbami, věrnostními body, e-shopem s produkty a nativní iOS aplikací — a ani jedno z toho jsi nechtěl, protože ti šlo jen o to, aby si zákazník sám zarezervoval termín. Právě proto specifikace v 1.1 vyjmenovává non-goaly („žádné online platby, žádný e-shop, žádná nativní appka, pouze jedna pobočka") — ušetří ti to hodiny zbytečné práce a kódu, který bys teď musel udržovat.

## 1.2 Uživatelské scénáře — *co uživatel dělá*

**🎯 Proč je důležitá**
Lidé i jazykové modely chápou **konkrétní příklady** mnohem líp než abstraktní popisy. „Systém spravuje rezervace" je mlhavé; „když zákazník vybere slot 10:00, který už je obsazený, uvidí nejbližší volné časy" je jednoznačné. Priority (P1/P2/P3) jsou důležité proto, že bez nich AI i ty utopíte čas v okrajových vychytávkách, zatímco hlavní funkce ještě nestojí.

**📌 Co je jejím cílem**
Popsat chování očima uživatele přes konkrétní scénáře ve formátu *Given–When–Then* (Dáno–Když–Pak) a seřadit je podle toho, co je nezbytné a co počká. V této specifikaci je vlastní rezervace a její zrušení **P1** (bez nich produkt nemá smysl), správa kalendáře personálem **P2** a automatické připomínky **P3**. Každý scénář je zároveň polotovar budoucího testu.

**✅ Čeho dosáhne**
Vznikne přesné, otestovatelné zadání. Z každého „Then" se dá přímo udělat kontrola, jestli to funguje. Priority navíc určí pořadí práce, takže nejdřív stojí samotná rezervace, až potom připomínky.

**⚠️ Příklad chyby**
Napíšeš jen „zákazník si rezervuje termín". AI vyrobí rezervaci, která funguje, když je slot volný, ale při obsazeném slotu spadne nebo vytvoří dvojitou rezervaci — protože jsi scénář pro tu situaci nikdy nezmínil. Konkrétní scénář US1 *„Given zákazník nevyplnil e-mail, When odešle, Then chyba u pole"* a edge case *„slot mezitím obsazen"* tyhle případy ošetří.

### 1.2.x Hraniční případy (Edge Cases)

**🎯 Proč je důležitá**
Hraniční případy jsou přesně tam, kde AI nejvíc chybuje a kde reálné aplikace nejčastěji padají. „Šťastnou cestu" (zákazník v klidu rezervuje volný slot) zvládne každý; rozdíl mezi hračkou a použitelným rezervačním systémem je v tom, co se stane při prázdném formuláři, výpadku odeslání e-mailu nebo dvojím kliknutí na „Rezervovat".

**📌 Co je jejím cílem**
Donutit tě vyjmenovat nepříjemné situace předem — chybné vstupy, zavřené dny, rezervaci v minulosti, souběžné akce. Je to seznam „co se může pokazit", který tato specifikace shrnuje v sekci 1.2.x.

**✅ Čeho dosáhne**
Web, který se chová předvídatelně i ve chvíli, kdy něco selže. Místo pádu uvidí zákazník srozumitelnou hlášku, a ty nemusíš tytéž chyby pracně hledat až v ostrém provozu o víkendu.

**⚠️ Příklad chyby**
Web funguje krásně, dokud dva zákazníci nekliknou na stejný slot v 10:00 ve stejnou vteřinu — a vzniknou dvě rezervace na jeden termín. Kadeřnice pak má v deset dvě dámy najednou. Edge case *„souběžná rezervace stejného slotu"* (1.2.x) na tuhle možnost upozorní a vynutí atomickou kontrolu na úrovni databáze, takže se potvrdí jen první zákazník.

## 1.3 Funkční požadavky — *co systém musí umět*

**🎯 Proč je důležitá**
Scénáře (1.2) ukazují příběhy; funkční požadavky je převádějí na přesná, číslovaná pravidla. Číslování (FR-001, FR-002…) není byrokracie — umožňuje **vystopovatelnost**: ke každému slíbenému požadavku dohledáš odpovídající kód a test. Styl EARS (krátké věty „Systém MUSÍ…", „Když… systém MUSÍ…") existuje proto, že běžná čeština je dvojznačná a AI z dvojznačnosti vyrobí dvojznačný kód.

**📌 Co je jejím cílem**
Zformulovat jednoznačná, ověřitelná pravidla chování, každé s vlastním číslem. Značka `[NEEDS CLARIFICATION: …]` slouží k tomu, abys nejasnost nezametl pod koberec, ale viditelně označil k dořešení — v této specifikaci ji najdeš u FR-015 (retence osobních údajů podle GDPR).

**✅ Čeho dosáhne**
Vznikne kontrolní seznam, podle kterého na konci ověříš „je hotové vše, co bylo slíbeno?". Díky číslům a jednoznačným větám AI implementuje přesně to, co tam stojí — ne svoji interpretaci.

**⚠️ Příklad chyby**
Napíšeš požadavek „rezervace by měla být spolehlivá". To je nicneříkající — AI si „spolehlivou" vyloží po svém. Oproti tomu *„FR-010: Pokud dva zákazníci současně rezervují stejný slot, systém MUSÍ potvrdit pouze prvního a druhému zobrazit, že slot už není volný"* je konkrétní, otestovatelné a nedá se vyložit dvěma způsoby.

## 1.4 Nefunkční požadavky — *jak DOBŘE*

**🎯 Proč je důležitá**
Funkční požadavky říkají, *co* systém dělá; nefunkční říkají, *jak dobře* to dělá — jak rychle, jak bezpečně, jak dostupně. Tyhle vlastnosti se strašně těžko dolepují zpětně. Bezpečnost a výkon vestavěné od začátku jsou levné; přidávané do hotového systému jsou drahé a křehké.

**📌 Co je jejím cílem**
Zachytit měřitelné nároky na kvalitu — rychlost odezvy, bezpečnostní pravidla, dostupnost, náklady. Klíčové slovo je *měřitelné*: ne „rychlé", ale „API pro dostupné sloty odpoví do 300 ms".

**✅ Čeho dosáhne**
Systém, který nejen funguje, ale je i použitelný v reálných podmínkách. Zároveň dáš AI jasné mantinely — bez řádku o bezpečnosti by ti klidně nechala admin kalendář s rezervacemi (a kontakty zákazníků) veřejně přístupný bez přihlášení.

**⚠️ Příklad chyby**
Web funguje skvěle s deseti rezervacemi, ale stránka s výběrem termínu se na mobilu načítá osm sekund, protože nikdy nebyl stanoven výkonový cíl — a většina zákazníků rezervuje právě z telefonu. Řádek *„stránka s výběrem termínu se načte do 2 s na mobilu"* (1.4) by vývoj nasměroval na efektivní řešení od začátku.

## 1.5 Datové entity

**🎯 Proč je důležitá**
Většina aplikací je hlavně o tom, jak spolu souvisí data. Pokud předem nepojmenuješ „věci" v systému a jejich vztahy, AI si je domyslí — a domyslí si je nekonzistentně (na jednom místě „kadeřnice", na druhém „stylista", na třetím „zaměstnanec", a najednou nikdo neví, jestli je to totéž).

**📌 Co je jejím cílem**
Vyjmenovat hlavní datové objekty, jejich klíčová pole a vztahy mezi nimi. Tato specifikace pojmenovává šest entit — Služba, Kadeřnice, Směna, Rezervace, Zákazník a Zavřeno — a říká například, že jeden zákazník má více rezervací. Pozor — popisuješ *co* data jsou, ne *jak* je uložíš (konkrétní tabulky DB jsou „JAK", patří do plánu).

**✅ Čeho dosáhne**
Konzistentní pojmenování a strukturu napříč celým projektem. AI i ty mluvíte o stejných věcech stejnými slovy, což zásadně snižuje zmatky a chyby.

**⚠️ Příklad chyby**
V jedné části kódu je „rezervace" napojená na „zákazníka", v jiné na „uživatele". Protože nikde nebylo řečeno, že je to tentýž objekt, vzniknou dvě nekompatibilní struktury a propojení rezervací s kontakty se rozsype. Vyjmenované entity v 1.5 tomu předcházejí.

## 1.6 Kritéria úspěchu — *měřitelná*

**🎯 Proč je důležitá**
Bez měřitelného cíle nikdy nevíš, kdy je hotovo — a „skoro hotovo" se umí táhnout donekonečna. Měřitelnost zároveň brání sebeklamu: „rezervace je docela rychlá" je pocit, „zákazník ji dokončí do 90 sekund" je fakt, který se dá ověřit.

**📌 Co je jejím cílem**
Definovat konkrétní, číselně ověřitelné výsledky, podle kterých poznáš úspěch. Liší se od funkčních požadavků tím, že měří *dopad* (např. „60 % rezervací proběhne online" nebo „no-show klesne o 20 %"), ne jen schopnost.

**✅ Čeho dosáhne**
Jasnou cílovou čáru. Ty i AI víte, kam směřujete, a po dokončení objektivně ověříte, jestli jste tam došli.

**⚠️ Příklad chyby**
Cíl zní „rezervace má být jednoduchá". Postavíš osmikrokový formulář, který *technicky* funguje, ale zákazníci ho v půlce opouštějí. Měřitelné kritérium *„SC-002: zákazník dokončí rezervaci do 90 sekund"* by hned ukázalo, že „jednoduchá" znamená výrazně méně kroků.

## 1.7 Předpoklady a otevřené otázky

**🎯 Proč je důležitá**
Každý projekt stojí na tichých předpokladech („zákazník má e-mail", „salon má jednu pobočku", „nepotřebuje účet"). Když jsou *nevyřčené*, naráží se na ně až v půlce práce, kdy už je drahé je měnit. Otevřené otázky jsou poctivé přiznání „tohle ještě nevím" — lepší než si tiše domyslet špatnou odpověď.

**📌 Co je jejím cílem**
Vytáhnout skryté předpoklady na světlo (jeden salon, max. 4 kadeřnice, e-mail jako hlavní kanál) a sepsat nezodpovězené otázky na jedno viditelné místo, aby se na ně nezapomnělo.

**✅ Čeho dosáhne**
Méně nepříjemných překvapení v půlce vývoje. Otevřené otázky se stanou viditelným seznamem věcí k dořešení, místo aby zůstaly skrytými minami.

**⚠️ Příklad chyby**
Tiše předpokládáš, že web budou používat jen češtináři, a celé to napíšeš napevno česky. O dva týdny později chce majitelka i anglickou verzi pro zahraniční klienty a ty přepisuješ texty roztroušené po celém kódu. Otevřená otázka *„Bude web vícejazyčný (CZ/EN)?"* v 1.7 drží tohle rozhodnutí viditelné jako vědomé, ne náhodné.

---

# 2. ÚSTAVA PROJEKTU — *neměnné principy*

**🎯 Proč je důležitá**
Specifikace (část 1) řeší jednu konkrétní funkci. Ústava řeší pravidla, která platí **napříč úplně vším** — styl, testování, bezpečnost, jak velké dělat změny. AI nemá paměť mezi sezeními, takže bez takového „pevného bodu" by ti v každém novém sezení mohla míchat styly a porušovat zásady, na kterých ti záleží. Ústava je sada pravidel, která si agent přečte pokaždé.

**📌 Co je jejím cílem**
Zachytit pár nepřekročitelných principů projektu na jednom místě — v této specifikaci například „žádný produkční kód bez padajícího testu (TDD)", „pokrytí kritické logiky 90 %" nebo „nejjednodušší řešení, žádné předčasné zobecňování (zatím neřeš multi-salon)". Důraz je na slově *pár* — ústava má být krátká a konkrétní, ne stostránkový manuál.

**✅ Čeho dosáhne**
Konzistenci napříč celým projektem a všemi sezeními s AI. Každá nová funkce ctí stejné standardy, takže rezervační web nezdegeneruje do směsice nesourodých přístupů.

**⚠️ Příklad chyby**
V pondělí AI napíše logiku rezervací s testy, v úterý (nové sezení) je u zrušení vynechá, ve středu použije jinou konvenci pojmenování. Po týdnu máš projekt, který vypadá, jako by ho psali tři různí lidé. Princip *„Žádný produkční kód bez předem padajícího testu; pokrytí kritické logiky 90 %"* z ústavy by tomu zabránil.

---

# 3. PLÁN IMPLEMENTACE — *JAK to postavíme*

**🎯 Proč je důležitá**
Tady se konečně řeší technologie — a je oddělená od specifikace záměrně. Když máš „CO" (spec) a „JAK" (plán) zvlášť, můžeš stejné zadání postavit dvěma způsoby a porovnat je, aniž bys přepisoval celou specifikaci. Oddělení také brání tomu, aby ti technické detaily zaplevelily čitelnou specifikaci, kterou má rozumět i netechnická majitelka salonu.

**📌 Co je jejím cílem**
Rozhodnout a zapsat, jakými prostředky specifikaci naplníš: jazyk, knihovny, úložiště, struktura projektu. V tomto projektu plán volí Next.js + TypeScript + Prisma + PostgreSQL — ale je to jen *jedna* z možných realizací. „Brána kontroly ústavy" (3.3) je tu jako pojistka — než začneš stavět, ověříš, že tvůj plán neporušuje principy z části 2.

**✅ Čeho dosáhne**
Promyšlený technický směr dřív, než se píše kód. AI (i ty) máte mapu, takže neimprovizujete za pochodu a nevzniká chaotická architektura, kterou by bylo nutné později bourat.

**⚠️ Příklad chyby**
Necháš technické detaily prosáknout do specifikace („uložíme rezervace do PostgreSQL tabulky `bookings` se sloupcem `email VARCHAR(255)`"). Netechnická majitelka přestane specifikaci rozumět a zároveň ses zbytečně zavázal k jednomu řešení dřív, než jsi zvážil alternativy. Tyhle detaily patří do plánu (sekce 3.2 této specifikace), kde jsou správně — Next.js, Prisma, PostgreSQL.

---

# 4. ÚKOLY — *rozpad na malé kroky*

**🎯 Proč je důležitá**
Velké zadání („postav rezervační systém") je pro AI past — vygeneruje hromadu kódu najednou, ty ji nestihneš pořádně zkontrolovat, a stačí jedna skrytá chyba dole, aby se celá stavba sesypala. Malé úkoly produkují malé změny, které *jdou* přečíst a ověřit. Je to rozdíl mezi „zkontroluj těchto 500 řádků" a „zkontroluj tuto jednu patnáctiřádkovou funkci na výpočet volných slotů".

**📌 Co je jejím cílem**
Rozsekat práci na nejmenší smysluplné kroky, seřadit je do fází a vyznačit závislosti (co musí být první) a co lze dělat souběžně (značka `[P]`). Pořadí uvnitř fáze respektuje TDD: nejdřív padající test, pak kód — proto je v úkolu T007 „napiš padající test pro výpočet volných slotů" a teprve T008 „implementuj logiku".

**✅ Čeho dosáhne**
Práce po zvládnutelných soustech, kde po každém kroku máš funkční, ověřený mezistav. Když se něco pokazí, víš přesně u kterého malého úkolu — ne že hledáš jehlu v kupce vygenerovaného kódu.

**⚠️ Příklad chyby**
Zadáš AI „naimplementuj celý rezervační systém" jedním příkazem. Dostaneš stovky řádků, mezi nimi tichou chybu ve výpočtu dostupnosti (např. nezohlední délku služby, takže nabídne barvení 15 minut před zavírací dobou). Protože všechno přišlo najednou, chybu objevíš až když si zákazník stěžuje. Rozpad na úkoly T007–T013 (nejdřív test dostupnosti, pak kód, pak test kolize, pak atomické vytvoření rezervace…), každý s vlastním testem, by chybu odhalil hned u příslušného kroku.

---

# 5. BDD SCÉNÁŘE — *chování viditelné uživateli*

**🎯 Proč je důležitá**
Gherkin (formát *Pokud–Když–Pak*) má dvojí kouzlo: čte se skoro jako běžná věta, takže mu rozumí i majitelka salonu, a zároveň jde **spustit jako automatický test**. Tím vzniká „živá dokumentace" — popis chování, který se nemůže rozejít s realitou, protože když se chování změní, příslušný scénář prostě spadne a ty to hned víš.

**📌 Co je jejím cílem**
Zapsat konkrétní příklady chování tak, aby současně sloužily jako srozumitelná dokumentace i jako vykonatelné testy. Pravidlo „jeden scénář = jedno chování" udržuje scénáře čisté — proto má specifikace zvlášť scénář pro úspěšnou rezervaci, zvlášť pro obsazený slot a zvlášť pro pozdní zrušení.

**✅ Čeho dosáhne**
Dokumentace a testy v jednom, které nikdy nezastarají bez varování. Když někdo (i AI) omylem rozbije chování rezervace, test selže a upozorní na to dřív, než se chyba dostane k zákazníkům.

**⚠️ Příklad chyby**
Napíšeš vágní scénář „Pak rezervace funguje". To se nedá automaticky ověřit ani z toho člověk nepozná, co se má stát. Konkrétní *„Pak je rezervace vytvořena a slot 10:00 je označen jako obsazený a obdržím potvrzovací e-mail"* (jako v happy-path scénáři této specifikace) lze spustit jako test i přečíst jako srozumitelný popis.

---

# 6. AGENTS.md — *pravidla pro AI agenta*

**🎯 Proč je důležitá**
AI asistent začíná každé sezení jako čistý list — nepamatuje si, co bylo včera, ani jak je tvůj rezervační web postavený. `AGENTS.md` (nebo `CLAUDE.md`) je první věc, kterou si přečte, a říká mu, jak se v projektu chovat: jak spustit testy, kde jsou hranice, co nikdy nedělat. Bez něj agent hádá a hádá špatně. Klíčové je držet ho *krátký* — když ho přeplníš, AI začne pravidla ignorovat.

**📌 Co je jejím cílem**
Předat agentovi nezbytný provozní kontext a mantinely na jednom místě: příkazy (`npm test`, `npm run dev`), postup práce (nejdřív plán, pak kód; TDD), a tři kategorie hranic — co dělat vždy (✅ kolize řeš atomicky), na co se ptát (⚠️ změny schématu v `prisma/`) a co nikdy (🚫 necommituj `.env`). Styl kódu sem nepatří — od toho je linter, který to vynutí automaticky a zdarma.

**✅ Čeho dosáhne**
AI, která pracuje v souladu s tvými pravidly i bez toho, abys je v každém promptu opakoval. Mantinel „🚫 Nikdy necommituj tajné klíče ani `.env`" třeba zabrání tomu, aby ti agent nahrál do veřejného repozitáře přístup k databázi s kontakty zákazníků.

**⚠️ Příklad chyby**
Nemáš žádná pravidla a v jednom sezení AI „opraví" padající test na kolizi rezervací tím, že ho přepíše, aby procházel — místo aby spravila kód. Test je teď nazelenalý, ale dvojité rezervace se pořád vytvářejí. Pravidlo *„NEMĚŇ testy jen proto, aby prošly. Oprav kód."* z `AGENTS.md` přesně tohle blokuje.

---

# ✅ 7. Definice hotového (Definition of Done)

**🎯 Proč je důležitá**
„Hotovo" znamená pro každého něco jiného — pro jednoho „kód jsem napsal", pro druhého „je to otestované, zdokumentované a nasazené". Bez společné definice se stává, že odškrtneš úkol jako splněný, ale chybí testy, ošetření chyb nebo aktuální specifikace, a vrací se to jako bumerang. Tahle sekce je společný kontrolní seznam *pro každou* dodávku.

**📌 Co je jejím cílem**
Stanovit jednotnou laťku „kdy je práce skutečně dokončená" — všechny akceptační scénáře procházejí jako testy, edge cases (zvlášť kolize slotů a pozdní zrušení) ošetřené, žádné nevyřešené `[NEEDS CLARIFICATION]` (vč. GDPR retence u FR-015), specifikace zaktualizovaná, a (důležité u AI) kód, kterému *ty* rozumíš.

**✅ Čeho dosáhne**
Konzistentní kvalitu napříč vším, co dodáš. Nic se neprohlásí za hotové, dokud opravdu hotové není, takže se nehromadí skrytý nedodělaný dluh.

**⚠️ Příklad chyby**
Odškrtneš rezervační formulář jako „hotovo", protože běží na tvém počítači. Jenže nemá testy, takže o měsíc později při úpravě připomínek nechtěně rozbiješ kontrolu kolizí a nikdo si toho nevšimne, dokud nepřijdou dvě dámy na jeden termín. Položka v DoD *„Všechny akceptační scénáře procházejí jako automatické testy"* by funkci nepustila dál bez té pojistky.

---

# 🔄 8. Údržba a relevance — *aby se specifikace nerozešla s kódem*

**🎯 Proč je důležitá**
Tohle je nejčastější způsob, jak hezká specifikace umře: kód se vyvíjí, dokument se neaktualizuje, a po pár týdnech popisuje něco, co už neplatí. Tomu se říká **spec drift**. Jakmile k němu dojde, lidé (i AI) přestanou dokumentu věřit a je z něj jen mrtvý text. Sekce 8 je sada návyků, které tomu brání.

**📌 Co je jejím cílem**
Udržet dokument a kód v souladu pomocí jednoduchých pravidel: jediný zdroj pravdy, verzování v Gitu, krátké smyčky (po každé změně aktualizuj spec), a hlavně automatické testy v CI, které samy odhalí, když se kód a popis rozejdou. „Historie revizí" drží přehled o tom, co a kdy se měnilo (zde 0.1 → 1.0).

**✅ Čeho dosáhne**
Živý dokument, kterému lze i za rok věřit, protože se mění *spolu* s kódem, ne pozadu za ním. Testy navíc fungují jako automatický hlídač — když kód uteče od specifikace, build spadne dřív, než si toho někdo všimne v provozu.

**⚠️ Příklad chyby**
Půl roku po startu se nový spolupracovník (nebo AI) zeptá specifikace, jak funguje zrušení termínu. Dokument tvrdí „24 h předem" (FR-008), ale kód mezitím někdo změnil na 48 h a nikdo to nezapsal. Strávíš odpoledne dohadováním, co vlastně platí. Návyk „každou změnu promítni i do specifikace + testy v CI" by dokument udržel pravdivý.

---

## 🧩 Jak to celé do sebe zapadá (rychlé shrnutí)

Když se na specifikaci podíváš z výšky, je v ní jeden logický řetězec:

1. **PROČ** to děláme (1.1 — telefon ruší kadeřnici, vznikají dvojité rezervace) → vede k tomu, **CO** zákazník dělá (1.2 — sám si rezervuje termín) → z čehož plynou **přesná pravidla** (1.3 — FR-001 až FR-015) a **kvalita** (1.4 — odezva, bezpečnost, přístupnost).
2. Tato pravidla a chování se převedou na **testy** (BDD scénáře v části 5 + TDD v úkolech části 4), které slouží jako **zadání i pojistka**.
3. **Plán** (3) a **úkoly** (4) říkají, **jak** to postavit, krok za krokem (Next.js, Prisma, T001 → T020).
4. **Ústava** (2) a **AGENTS.md** (6) drží **AI i tebe** v mantinelech po celou dobu.
5. **Definice hotového** (7) je **cílová čára** a **údržba** (8) zajišťuje, že to **zůstane pravdivé** i potom.

Začátečníkovi stačí na začátku rozumět hlavně části **1** (specifikace) a **6** (pravidla pro AI). Zbytek dává smysl postupně, jak projekt poroste — a tato příručka tu bude, až na něj dojde řada.

---

<sub>Tento průvodce vysvětluje sekce souboru `specifikace-rezervace-kadernictvi.html`. Čti je vedle sebe — pořadí kapitol je shodné.</sub>
