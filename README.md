# Challenge Mode — Skill pro kritické myšlení

Skill, který z Claudea udělá partnera pro kritické myšlení místo souhlasícího
asistenta. Když ho zapneš, Claude přestane potvrzovat tvé nápady a začne je
zátěžově testovat — hledá skryté předpoklady, staví nejsilnější protiargument
a nutí tě svou pozici obhájit.

---

## Proč ho mít

Jazykové modely mají vrozený sklon k **patolízalství** (sycophancy): souhlasit,
chválit a přizpůsobovat se tomu, co podle nich chceš slyšet. To je příjemné, ale
zákeřné — když potřebuješ tlak, dostaneš potlesk. Nejvíc to bolí přesně ve
chvílích, kdy jsi do nápadu zainvestovaný a chyba tě stojí nejvíc.

Tento skill ten sklon obrací. Hodí se, když:

- stojíš před **rozhodnutím**, u kterého je cena omylu vysoká (kariéra, peníze,
  směr produktu),
- máš **nápad nebo plán**, který zní dobře, ale ještě ho nikdo nezkoušel rozbít,
- chceš **zpětnou vazbu na práci** a zajímají tě slabiny, ne pochvala,
- potřebuješ **myslet nahlas s odporem**, ne s ozvěnou.

Není to nástroj na běžnou konverzaci ani na fakta — je to režim, který si
vědomě zapínáš, když chceš oponenta.

---

## Co umí (a co schválně nedělá)

**Umí:**

- Pojmenovat neověřený předpoklad pod tvým tvrzením — často cennější než
  samotný nesouhlas.
- Postavit nejsilnější protiargument, jaký by vznesl chytrý oponent, bez
  změkčování.
- Neustoupit jen proto, že se ohradíš — ustoupí, až přidáš nový důkaz, novou
  úvahu nebo dříve nezmíněné omezení.
- U revize práce začít tím nejslabším místem, ne nejsilnějším.
- **Přiznat, když nápad obstojí** — místo vymýšlení umělé chyby řekne přímo, že
  slabinu nenašel, a pojmenuje jedinou podmínku, která by nápad rozbila.
- Zakončit otázkou k zamyšlení, ne shrnutím.

**Schválně nedělá:**

- Nebrzdí u faktických dotazů, definic, debugování ani provozních úkolů — ty
  zodpoví rovnou, i když je režim zapnutý.
- Nevymýšlí si námitky, aby vypadal důkladně.
- Netvrdí nic o tvém emočním stavu jako fakt (na to se nanejvýš zeptá).
- Nezačíná chválou, nehedguje („možná se mýlím, ale…"), nezavírá ujištěním
  („máš dobrý instinkt").

---

## Instalace

1. Stáhni soubor `challenge-mode.skill` (nebo rozbal `challenge-mode.zip`).
2. V Claude.ai jdi do **Settings → Capabilities → Skills**.
3. Nahraj `.skill` soubor.

Skill se pak aktivuje automaticky, jakmile v konverzaci použiješ některý ze
spouštěčů níže.

---

## Jak ho používat

### Zapnutí

Režim je **vypnutý ve výchozím stavu** — chrání tě to před nevyžádanou kritikou,
když chceš jen přemýšlet nahlas. Zapneš ho jedním z těchto způsobů:

| Spouštěč | Příklad |
|---|---|
| `/chm` na začátku zprávy | `/chm Chci přepsat appku do Svelte.` |
| Fráze | „challenge this", „poke holes", „devil's advocate", „argue against me", „stress-test this" |

Jakmile je zapnutý, **zůstává aktivní po celé vlákno** až do vypnutí.

### Vypnutí

Napiš `/chm off` nebo dej jakkoli jinak najevo, že už nechceš být rozporován.

### Když zapomeneš zapnout

Pokud sdílíš rozhodnutí nebo práci bez spouštěče, Claude se **jednou zeptá**:
„Chceš to rozporovat, nebo jen zkontrolovat?" — takže o ochranu nepřijdeš úplně,
i když na `/chm` zapomeneš.

---

## Sedm pravidel v kostce

1. **Odhal předpoklad** — pojmenuj jednu neověřenou věc, na které tvrzení stojí.
2. **Nejsilnější protiargument první** — bez změkčování.
3. **Neustup kvůli tlaku** — jen kvůli nové informaci.
4. **Slabiny napřed** — silné stránky si najdeš sám.
5. **Kalibruj** — když nápad obstojí, řekni to; nevymýšlej chyby.
6. **Emoce jen jako otázka** — nikdy jako diagnóza, a jen jednou.
7. **Zakonči otázkou** — ne shrnutím.

---

## Příklad

> **Ty:** `/chm Než budu psát kód, ověřím poptávku landing page a waitlistem.`
>
> **Claude:** Hledal jsem slabinu a skutečnou nenacházím — tohle je standardní
> „nejlevnější test první" a tady je správný. Jediná věc, která by to rozbila,
> kdyby se ukázala jako nepravdivá, je předpoklad, že zápis do waitlistu
> předpovídá platícího zákazníka; často ne, protože říct „ano, zajímá mě to"
> nestojí nic. Otázka k zamyšlení: jaký signál kromě kódu by tě přesvědčil, že
> tihle lidé opravdu zaplatí?

Všimni si, že Claude **nevymyslel** falešnou chybu — to je hlavní rozdíl oproti
naivnímu „buď protivný".

---

## Omezení, o kterých je dobré vědět

- **Spouštěč musíš zapnout sám.** Ochrana funguje jen tam, kde si na `/chm`
  vzpomeneš — tedy paradoxně tam, kde už nějakou obezřetnost máš. Pojistka
  „zeptám se jednou" to tlumí, ale neřeší úplně.
- **Spolehlivost aktivace přes frázi není zaručená** u úplně jednoduchých zpráv;
  `/chm` je nejjistější spouštěč.
- **Není to nestranný rozhodčí.** Režim je *navržen* oponovat — bere to jako
  roli. U pravdivě dobrých nápadů to vyrovnává pravidlo 5, ale počítej s tím, že
  výchozí postoj je skeptický.
- **Nečte ti myšlenky.** Pokud označí, že jsi „zainvestovaný", je to odhad z
  textu, ne fakt — ber to jako otázku, ne verdikt.

---

## Přizpůsobení

Chceš jiné chování? Edituj `SKILL.md`:

- **Jiný spouštěč než `/chm`** — přepiš ho v sekci *Activation and scope* a v
  obou příkladech.
- **Vždy zapnuto** místo na vyžádání — uprav sekci *Activation* tak, aby režim
  běžel defaultně (pozor: riskuješ únavu a „obrácené patolízalství", tedy
  reflexivní rozporování).
- **Měkčí nebo ostřejší tón** — uprav sekci *Tone*.

Po úpravě skill znovu zabal (např. přes `skill-creator`) a nahraj.
