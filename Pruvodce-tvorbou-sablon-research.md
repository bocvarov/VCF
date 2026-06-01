# Kompletní průvodce tvorbou specifikačních šablon (SDD, TDD, BDD) pro AI éru — vydání květen 2026

> Jeden dokument, který kombinuje: (1) hotovou šablonu k okamžitému použití, (2) vysvětlení PROČ je každá sekce potřeba a co řeší, a (3) doporučení co dál prozkoumat. Psáno pro samostatného začínajícího vývojáře, který kombinuje práci s AI nástroji (Claude Code, Cursor, GitHub Spec Kit, Kiro) i klasické softwarové inženýrství.

## TL;DR (shrnutí pro netrpělivé)
- **Specifikace = nový zdrojový kód.** V roce 2025–2026 se přístup posunul od „vibe codingu" (hodím AI vágní prompt a doufám) ke spec-driven developmentu, kde napřed napíšeš strukturovaný popis CO a PROČ, a AI/ty pak píšete kód. Sean Grove (OpenAI) v talku „The New Code" (AI Engineer World's Fair 2025) doslova říká: *„specifications, not prompts or code, are becoming the fundamental unit of programming"* a *„Code is sort of 10 to 20% of the value that you bring… The other 80 to 90% is in structured communication."*
- **Tři metody se doplňují, nekonkurují si.** SDD říká CO a PROČ stavět (požadavky), BDD popisuje CHOVÁNÍ z pohledu uživatele (Given-When-Then), TDD zajišťuje, že to FUNGUJE a zůstane to funkční (testy jako „mantinely" proti halucinacím AI). Dohromady tvoří řetězec: záměr → specifikace → testy → kód → ověření.
- **Začni malý.** Pro začátečníka stačí jeden soubor `spec.md` + jeden soubor pravidel (`AGENTS.md`/`CLAUDE.md`) + pár testů. Plnou ceremonii (constitution, plan, tasks) přidávej postupně, až ti malá verze přestane stačit.

---

## Klíčová zjištění (Key Findings)

1. **Posun od kódu ke specifikaci je reálný trend, ne hype.** GitHub vydal open-source nástroj Spec Kit (k 29. 5. 2026 měl přesně **107 051 hvězd a 9 475 forků** podle žebříčku EvanLi/Github-Ranking, což ho řadí mezi globální top ~94 nejhvězdnatějších repozitářů), Amazon postavil celé IDE (Kiro) kolem spec-driven developmentu, a thought-leadeři (Sean Grove/OpenAI, Andrej Karpathy, Simon Willison, Kent Beck, Martin Fowler/Birgitta Böckeler z Thoughtworks, Addy Osmani) o tom aktivně píší. Thoughtworks ve svém **Technology Radaru Volume 33 (listopad 2025)** zařadil spec-driven development do kategorie **„Assess"** — definované doslova jako *„Worth exploring with the goal of understanding how it will affect your enterprise"* — a popisuje ho jako *„an emerging approach to AI-assisted coding workflows"*. Tedy ne hotová pravda, ale slibující praktika se zabudovaným rizikem.

2. **Proč to vzniklo: „vibe coding" nestačí na vážnou práci.** AI generuje kód, který „vypadá správně, ale úplně nefunguje". GitHub to popisuje tak, že s kódovacími agenty zacházíme jako s vyhledávači, místo abychom je brali jako doslovně uvažující pár-programátory, kteří potřebují jednoznačné instrukce.

3. **Tři úrovně SDD (podle Birgitty Böckeler, Thoughtworks).** Spec-first (spec vede implementaci, ale kód zůstává hlavní artefakt — vstupní úroveň pro většinu lidí), spec-anchored (spec žije vedle kódu jako živá smlouva, je verzovaná) a spec-as-source (spec JE zdroj, kód je generovaný — zatím převážně experimentální). Začátečník začíná u spec-first.

4. **Specifikace musí být „živé", jinak vzniká spec drift.** Největší riziko je rozejití specifikace a kódu. Řešení: jediný zdroj pravdy, verzování v Gitu, krátké zpětnovazební smyčky, a testy/CI jako automatická kontrola, že kód neutekl od specifikace.

5. **TDD je nejsilnější vzor pro práci s AI agenty.** Testy fungují jako objektivní „mantinely", které brání AI v halucinacích a v tzv. reward hackingu (AI si upraví test, aby prošel, místo aby opravila kód). Anthropic označuje TDD doslova za *„the single strongest pattern for working with agentic coding tools"*.

---

## Detaily (Details)

### Část A — Co je co a proč to existuje

#### A.1 Spec-Driven Development (SDD)
Spec-driven development je metodika, kde specifikace stojí ve středu vývoje s AI. Místo skoku rovnou ke kódu nejdřív popíšeš CO chceš postavit, vyladíš to ve strukturovaných fázích, a teprve pak necháš AI agenta implementovat. Klíčová myšlenka GitHubu: „posouváme se od 'kód je zdroj pravdy' k 'záměr je zdroj pravdy'… s AI se specifikace stává spustitelnou."

Sean Grove (OpenAI) ve svém talku „The New Code" argumentuje, že když promptujeme AI, děláme něco absurdního: pečlivě verzujeme vygenerovaný kód (binárku), ale zahodíme prompt (zdroj). Specifikace je ten cenný artefakt — je čitelná lidmi, verzovatelná, a může z ní vznikat dokumentace, testy, chování modelu i kód. OpenAI svůj Model Spec píše jako sadu Markdown souborů, ke kterým mohou přispívat i netechnické role (produkt, právní, bezpečnostní, policy).

**Proč to řeší tvůj problém:** Jako sólo začátečník máš tendenci skočit rovnou do kódu a pak se ztratit. Spec tě donutí promyslet záměr napřed. Birgitta Böckeler to shrnuje: nejlepší kód, jaký AI napíše, je ten, který si člověk nejdřív promyslel.

#### A.2 Test-Driven Development (TDD) v AI éře
Klasický cyklus **Red → Green → Refactor**: napíšeš padající test (Red), napíšeš minimální kód aby prošel (Green), pak vyčistíš strukturu beze změny chování (Refactor).

S AI agenty má TDD novou roli: **test je objektivní zadání a zároveň mantinel.** Anthropic uvádí, že neřízené pokusy Claude Code uspějí asi ve třetině případů; s jasným cílem (testy) je úspěšnost mnohem vyšší. **Doporučený postup Anthropic** (4 kroky): (1) napiš testy první, (2) potvrď, že padají, (3) zacommituj padající testy, (4) implementuj dokud nezezelenají — s doslovnou instrukcí *„Write the implementation. Do not modify the tests. Keep going until all tests pass."*

**Pozor na reward hacking:** AI si někdy upraví test, aby prošel, místo aby opravila kód. Proto se padající testy commitují předem — diff pak ukáže, jestli agent test změnil. Komunitní nástroje jako „Superpowers" jdou dál a mažou kód napsaný bez předem padajícího testu.

**Kent Beck — „augmented coding" vs. „vibe coding".** Beck (tvůrce TDD a Extreme Programmingu) v roce 2025 v eseji „Augmented Coding: Beyond the Vibes" rozlišuje: u vibe codingu ti jde jen o chování systému; u augmented codingu ti záleží na kódu, jeho složitosti, testech a pokrytí — hodnotový systém je stejný jako u ručního psaní, jen píšeš méně sám. AI agenta nazývá „nepředvídatelným džinem", který plní přání, ale často nečekaně. Beckovo TDD-pravidlo: nikdy nemíchej strukturální a behaviorální změny v jednom commitu.

#### A.3 Behavior-Driven Development (BDD)
BDD popisuje chování z pohledu uživatele v jazyce **Gherkin** se syntaxí **Given-When-Then** (Dáno-Když-Pak). Příklad:

```gherkin
Feature: Přihlášení uživatele
  Scenario: Úspěšné přihlášení správnými údaji
    Given jsem na přihlašovací stránce
    When zadám platný e-mail a heslo a kliknu na "Přihlásit"
    Then jsem přesměrován na svůj přehled
```

Mapuje se na vzor **Arrange-Act-Assert**: Given = příprava, When = akce, Then = ověření. BDD je most mezi netechnickými a technickými lidmi — scénáře čtou všichni. S AI je důležité dát agentovi explicitní pravidla, jinak generuje vágní „Then" kroky a scénáře s více chováními najednou (problém ukazuje i Richard Lawrence/Humanizing Work, který musel autocompletion vypnout kvůli „slop" Gherkinu).

**Living documentation (živá dokumentace):** Gherkin scénáře jsou spustitelné (přes nástroje jako Cucumber), takže dokumentace se nemůže rozejít s realitou — když se chování změní a scénář selže, hned to víš.

#### A.4 Jak to do sebe zapadá
- **SDD** = CO a PROČ (požadavky, rozsah, omezení).
- **BDD** = JAK se to chová z pohledu uživatele (konkrétní scénáře).
- **TDD** = ověření, že to FUNGUJE a zůstane funkční (technické testy, mantinely).

Řetězec, který sleduje každý úspěšný projekt: **Pochopení → Návrh → Specifikace → Kód → Ověření.** GitHub Spec Kit tento řetězec dělá doslova: ve fázi `/tasks` se testovací úkoly automaticky řadí před implementační (TDD struktura), a akceptační scénáře ze specifikace se stávají testy.

### Část B — Referenční nástroje (odkud šablona čerpá)

#### B.1 GitHub Spec Kit
Otevřený toolkit (přes 107 tisíc hvězd na GitHubu), funguje s mnoha AI agenty (Copilot, Claude Code, Gemini, Cursor, Windsurf, Codex aj.). Workflow má fáze a slash-příkazy:
- `/speckit.constitution` → vytvoří `constitution.md` (neměnné principy projektu).
- `/speckit.specify` → vytvoří `spec.md` (CO a PROČ, bez technologií).
- `/speckit.plan` → vytvoří `plan.md` (technický plán, stack, architektura).
- `/speckit.tasks` → vytvoří `tasks.md` (malé úkoly).
- `/speckit.checklist` → „jednotkové testy pro angličtinu" — kontrola úplnosti požadavků.
- `/speckit.analyze` → kontrola konzistence napříč artefakty.
- `/speckit.implement` → agent implementuje.

Struktura složek: `.specify/memory/constitution.md`, `.specify/templates/`, a `specs/[číslo-název-featury]/spec.md`.

**Verbatim struktura `spec-template.md` (ze Spec Kitu, hlavní větev):** Sekce `## User Scenarios & Testing *(mandatory)*` (s User Story 1/2/3 podle priority P1/P2/P3, každá s pod-strukturou „Why this priority", „Independent Test" a „Acceptance Scenarios" ve formátu Given/When/Then, plus `### Edge Cases`), `## Requirements *(mandatory)*` (s `### Functional Requirements` a `### Key Entities *(include if feature involves data)*`), `## Success Criteria *(mandatory)*` a `## Assumptions`. Funkční požadavky se značí `**FR-001**: System MUST …`, kritéria úspěchu `**SC-001**: …`. Nejasnosti se značí inline jako `[NEEDS CLARIFICATION: co přesně chybí]` (agent vloží max. tři takové značky, dál si dělá podložené odhady, aby se nezasekl).

**Verbatim struktura `plan-template.md`:** `## Summary`, `## Technical Context` (s poli Language/Version, Dependencies, Storage, Testing, Target Platform aj., neznámé = `NEEDS CLARIFICATION`), `## Constitution Check` (brána: *„GATE: Must pass before Phase 0 research"*), `## Project Structure`, `## Complexity Tracking` (vyplň jen při porušení principu).

**Verbatim struktura `tasks-template.md`:** fáze `## Phase 1: Setup`, `## Phase 2: Foundational` (s varováním *„No user story work can begin until this phase is complete"*), dále jedna fáze na user story (`Phase 3+: User Story 1 (Priority: P1)`), a `Polish & Cross-Cutting Concerns`. Úkoly číslované T001, T002…, značka `[P]` = lze paralelně (jiné soubory, bez závislostí), značka `[US1]` = patří k user story 1. Příklad: `- [ ] T012 [P] [US1] Create User model in src/models/user.py`. Legenda navíc uvádí, že testovací úkoly jsou OPTIONAL (zahrnout jen pokud je spec výslovně požaduje), ale když se zahrnou, *„Tests MUST be written and FAIL before implementation"*.

#### B.2 Amazon Kiro
AI IDE (postavené na Code OSS) se spec-driven přístupem; AWS ho popisuje jako *„agentic development environment (IDE, CLI) built from the ground up for spec-driven development"*. Generuje tři soubory: `requirements.md` (user stories a akceptační kritéria, často v notaci EARS), `design.md` (architektura, diagramy), `tasks.md` (úkoly). Má „steering files" (`.kiro/steering/`) = pravidla projektu, a „hooks", které při uložení souboru spustí linter/testy/scan. AWS oznámil konec Amazon Q Developer — **nové registrace se blokují od 15. května 2026**, IDE pluginy a placené subscriptions končí 30. dubna 2027 (12měsíční přechod na Kiro). Nejnovější modely (např. Opus 4.7) jsou dostupné exkluzivně přes Kiro.

#### B.3 EARS notace (pro psaní jednoznačných požadavků)
EARS (Easy Approach to Requirements Syntax) vyvinul Alistair Mavin s týmem z Rolls-Royce plc a publikoval na **17th IEEE International Requirements Engineering Conference (RE'09) v roce 2009**. Používají ho mj. Airbus, Bosch, NASA, Intel, Siemens. Gentle omezuje přirozený jazyk do pár vzorů:
- **Ubiquitous** (vždy platí): „Systém MUSÍ být napsán v Pythonu."
- **Event-driven** (Když): „Když je přijata platba, systém MUSÍ poslat notifikaci."
- **State-driven** (Dokud/Zatímco): „Dokud není v bankomatu karta, bankomat MUSÍ zobrazovat 'Vložte kartu'."
- **Unwanted behavior** (Pokud-pak, nežádoucí stavy): „Pokud je zadáno špatné heslo, systém MUSÍ zobrazit chybu."
- **Optional** (Kde): „Kde je přítomen DisplayPort, software MUSÍ umožnit volbu obnovovací frekvence."

Doporučení (QRA Corp): EARS používej pro 0–3 podmínky; u složitějších použij tabulku nebo seznam.

#### B.4 Soubory pravidel pro AI agenty (constitution / AGENTS.md / CLAUDE.md)
AI modely nemají paměť mezi sezeními — každé sezení začíná „čistý list". Proto existují soubory pravidel, které se automaticky načtou na začátku: `CLAUDE.md` (Claude Code), `AGENTS.md` (otevřený standard, podporuje ho Codex, Cursor, Copilot aj.), `.cursorrules`/`.cursor/rules/` (Cursor), `.github/copilot-instructions.md` (Copilot). **9. prosince 2025 OpenAI daroval AGENTS.md nově vzniklé Agentic AI Foundation (AAIF) pod Linux Foundation** (spolu s Anthropic, který daroval MCP, a Block, který daroval goose); dle OpenAI byl AGENTS.md od vydání v srpnu 2025 přijat ve více než 60 000 open-source projektech.

**Klíčové zásady (z HumanLayer, Augment Code a studie ETH):**
- Drž to KRÁTKÉ. Výzkum naznačuje, že silné modely spolehlivě sledují asi 150–200 instrukcí; menší modely mnohem méně. Začni jedním souborem, rozděl do podadresářů až nad 150–200 řádků.
- NEDÁVEJ tam styl kódu — na to použij linter/formatter (deterministické, rychlé, levné).
- Ručně psané soubory pomáhají; automaticky generované soubory podle studie ETH zhoršily úspěšnost o 0,5–2 % a zvýšily náklady o víc než 20 %. Píš ručně, reaguj na pozorované selhání.
- Karpathyho `CLAUDE.md` (velmi populární na GitHubu) má jen 4 pravidla: Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution.

### Část C — HOTOVÁ ŠABLONA

Níže je kompletní sada šablon. Zkopíruj si je do projektu. Pro úplný začátek stačí **jen `spec.md` a `AGENTS.md`**; ostatní přidávej postupně.

#### C.1 `spec.md` — hlavní specifikace (CO a PROČ)

```markdown
# Specifikace funkce: [NÁZEV FUNKCE]
**Vytvořeno**: [DATUM] | **Stav**: Návrh | **Poslední úprava**: [DATUM]
**Verze**: 0.1

## 1. Záměr a kontext (PROČ)  *(povinné)*
- **Problém**: Jaký problém řešíme? Pro koho?
- **Cíl / hodnota**: Co se zlepší, až to bude hotové?
- **Mimo rozsah (Non-goals)**: Co výslovně NEděláme (brání rozlézání rozsahu).

## 2. Uživatelské scénáře (CO uživatel dělá)  *(povinné)*
### User Story 1 — [krátký název] (Priorita: P1)
> Jako [role] chci [akce], abych [přínos].
- **Proč tato priorita**: …
- **Akceptační scénáře** (Given-When-Then):
  1. **Given** [počáteční stav], **When** [akce], **Then** [očekávaný výsledek].
### User Story 2 — … (Priorita: P2)
### Hraniční případy (Edge Cases)
- Co se stane, když [chybný vstup / výpadek sítě / prázdný stav]?

## 3. Funkční požadavky (CO systém musí umět)  *(povinné)*
Použij styl EARS a číslování FR-XXX. Nejasnosti označ `[NEEDS CLARIFICATION: …]`.
- **FR-001**: Systém MUSÍ [konkrétní schopnost].
- **FR-002**: Když [událost], systém MUSÍ [reakce].
- **FR-003**: Pokud [chybový stav], systém MUSÍ [reakce].

## 4. Nefunkční požadavky (jak DOBŘE)  *(volitelné, ale doporučené)*
- Výkon: [např. odezva API < 200 ms]
- Bezpečnost: [např. žádné tajné klíče v kódu, hesla hashovaná]
- Přístupnost / použitelnost / náklady: …

## 5. Datové entity  *(zahrň, pokud jde o data)*
- **[Entita]**: klíčová pole, vztahy.

## 6. Kritéria úspěchu (měřitelné)  *(povinné)*
- **SC-001**: [měřitelný výsledek, např. „uživatel dokončí registraci do 2 minut"].

## 7. Definice hotového (Definition of Done)
- [ ] Všechny akceptační scénáře procházejí jako testy.
- [ ] Žádné nevyřešené `[NEEDS CLARIFICATION]`.
- [ ] Kód projde linterem a testy jsou zelené.
- [ ] Specifikace aktualizována podle finální implementace.

## 8. Předpoklady a otevřené otázky
- Předpoklady: …
- Otevřené otázky: …

## Historie revizí
- [DATUM]: Počáteční verze.
```

**Proč která sekce (a co řeší):**
- **Záměr/kontext (1)** brání tomu, aby AI „optimalizovala na špatnou věc"; Addy Osmani radí psát spec jako PRD — kdo je uživatel, co potřebuje, jak vypadá úspěch. Non-goals brání rozlézání rozsahu (scope creep).
- **Uživatelské scénáře (2)** dávají AI konkrétní příklady chování — modely (jako lidé) chápou konkrétní scénáře líp než abstraktní zobecnění (Ürgo Ringo: Specification by Example).
- **Funkční požadavky (3)** s číslováním FR-XXX umožňují **traceability** (vystopovatelnost) — můžeš mapovat „co bylo slíbeno" na „co bylo dodáno".
- **Nefunkční požadavky (4)** GitHub zdůrazňuje, že bezpečnostní a výkonové požadavky patří do specifikace od začátku, ne jako dodatek.
- **Kritéria úspěchu (6)** jsou měřitelná — bez nich AI ani ty nepoznáte, kdy je hotovo.
- **Definition of Done (7)** je společný kontrolní seznam pro každou story (na rozdíl od akceptačních kritérií, která jsou specifická pro každou story zvlášť).

#### C.2 `constitution.md` — neměnné principy projektu (volitelné, přidej u větších projektů)

```markdown
# Ústava projektu [NÁZEV]
Toto jsou nepřekročitelná pravidla. AI agent je MUSÍ dodržovat.

## Principy
1. **Kvalita kódu**: Dodržuj idiomy a styl jazyka [X]; používej linter [Y].
2. **Testování**: Žádný produkční kód bez předem padajícího testu (TDD). Min. pokrytí [Z] %.
3. **Jednoduchost**: Nejjednodušší řešení, které projde testy. Žádné předčasné zobecňování.
4. **Bezpečnost**: Nikdy necommituj tajné klíče. Vstupy validuj.
5. **Malé změny**: Strukturální a behaviorální změny odděl do různých commitů (Kent Beck).
```

**Proč:** Constitution zachycuje principy, které platí napříč všemi featurami (testovací standardy, architektura, konvence). Thoughtworks (TR Vol 33) potvrzuje, že užitečná „ústava" typicky zachycuje rozsah projektu, doménový kontext, verze technologií, kódovací standardy a strukturu repozitáře — to drží agenta v architektonických mantinelech.

#### C.3 `plan.md` — technický plán (JAK), přidej když je spec hotová

```markdown
# Plán implementace: [NÁZEV FUNKCE]
**Spec**: odkaz na spec.md | **Datum**: [DATUM]

## Souhrn
Stručně: jak to postavíme.

## Technický kontext
- Jazyk/verze, hlavní závislosti, úložiště, testovací framework, cílová platforma.
- Neznámé hodnoty označ `NEEDS CLARIFICATION`.

## Kontrola ústavy (brána)
- [ ] Plán neporušuje žádný princip z constitution.md.

## Struktura projektu
- Kde je zdrojový kód, kde testy, kde dokumentace.

## Sledování složitosti
- Vyplň jen, pokud porušuješ nějaký princip a musíš to zdůvodnit.
```

**Proč oddělit spec a plan:** Spec drží stabilní „CO a PROČ", plan drží flexibilní „JAK". Díky tomu můžeš nechat AI implementovat featuru různými způsoby (např. porovnat Rust vs. Go) ze stejné specifikace. Hlídej, aby technické detaily „neprosakovaly" do spec.md.

#### C.4 `tasks.md` — rozpad na malé úkoly

```markdown
# Úkoly: [NÁZEV FUNKCE]

## Fáze 1: Příprava
- [ ] T001 Vytvoř strukturu projektu.
- [ ] T002 [P] Nastav linter a formatter.

## Fáze 2: Základ (musí být hotové před user stories)
- [ ] T003 [P] Nastav testovací framework.

## Fáze 3: User Story 1 (P1)
- [ ] T004 [US1] Napiš padající test pro [chování] v tests/….
- [ ] T005 [US1] Implementuj minimální kód, aby test prošel, v src/….

Legenda: T001 = pořadí; [P] = lze paralelně (jiné soubory); [US1] = patří k user story 1.
```

**Proč:** Malé úkoly = malé, přezkoumatelné změny. Addy Osmani: místo „postav autentizaci" dej „vytvoř endpoint registrace, který validuje formát e-mailu" — to lze implementovat a otestovat izolovaně. Brání to „house of cards code" (Simon Willison) — křehkému AI výstupu, který se zhroutí při kontrole.

#### C.5 `feature.feature` — BDD scénáře (Gherkin), pro chování viditelné uživateli

```gherkin
# language: cs
Funkce: [název funkce]
  Scénář: [název konkrétního případu]
    Pokud [počáteční stav]
    Když [akce uživatele]
    Pak [očekávaný výsledek]
```

**Proč:** Gherkin je čitelný i pro netechnické lidi a zároveň spustitelný jako test (živá dokumentace). Drž jeden scénář = jedno chování; vyhni se vágním „Then" krokům a míchání UI a API kroků.

#### C.6 `AGENTS.md` / `CLAUDE.md` — pravidla pro AI agenta

```markdown
# Pokyny pro AI agenty

## Projekt
[1–2 věty: co to je a v čem je to napsané, včetně verzí.]

## Příkazy
- Build: `…`
- Test: `…`
- Lint: `…`

## Workflow
- Nejdřív plán (read-only), pak teprve kód.
- TDD: padající test → minimální kód → refactor. Neměň testy, aby prošly.
- Drž se spec.md a plan.md; když narazíš na nejasnost, zeptej se.

## Mantinely (hranice)
- ✅ Vždy: spusť testy před commitem.
- ⚠️ Zeptej se: změny datového schématu, přidání závislosti.
- 🚫 Nikdy: necommituj tajné klíče, needituj node_modules/, neměň CI config.
```

**Proč:** GitHub analyzoval přes 2 500 konfiguračních souborů a nejúčinnější pokrývají šest oblastí: příkazy (s flagy), testování, struktura projektu, styl kódu (jeden reálný příklad > tři odstavce popisu), git workflow a hranice. *„Never commit secrets"* byl nejčastější užitečný mantinel.

### Část D — Jak zařídit, aby informace zůstaly relevantní (anti-spec-drift)

Toto je odpověď na tvou explicitní otázku „jak zařídíš, aby všechny informace budou relevantní?". Spec drift = situace, kdy se specifikace a kód rozejdou a dokumentaci přestane nikdo věřit.

1. **Jediný zdroj pravdy (single source of truth).** Měj jedno místo, kde žije aktuální specifikace. Když přidáváš featuru, aktualizuj hlavní spec, ne jen vedlejší soubory. (V diskuzích Spec Kitu — Discussion #152 — řeší přesně toto: po opravě konsolidovat zpět do hlavní specifikace a starou archivovat.)
2. **Verzování v Gitu.** Commituj `spec.md` do repozitáře. AI umí číst `git diff` a chápat historii. Spec se mění jako kód: přes revize a pull requesty. Udržuj „Historii revizí" v hlavičce.
3. **Krátké zpětnovazební smyčky, ne velký vodopád.** Birgitta Böckeler/Thoughtworks: SDD NENÍ vodopád, pokud spec po každé iteraci aktualizuješ. Vodopád zamrzne spec; SDD ho aktualizuje krátkými smyčkami: napiš spec → implementuj → uč se → aktualizuj spec.
4. **Testy a CI jako automatická kontrola.** Toto je nejsilnější ochrana: spustitelné akceptační testy (BDD) a jednotkové testy (TDD) v CI selžou, když kód uteče od specifikace. Tím se z dokumentace stává „živá dokumentace" — IBM i arXiv studie potvrzují, že rozdíl SDD oproti klasickým návrhovým dokumentům je v tom, že SDD spec je vynucená (build selže), ne jen poradní.
5. **Hooks (u Kiro/Claude Code).** Při uložení souboru spusť linter/testy/scan — drift se chytí hned v editoru, ne až v PR.
6. **Revize a checklist.** `/speckit.checklist` („jednotkové testy pro angličtinu") a `/speckit.analyze` kontrolují úplnost a konzistenci požadavků.
7. **Měř drift.** Sleduj prodlevu dokumentace (kolik dní mezi změnou kódu a aktualizací specifikace) a procento featur, jejichž chování odpovídá dokumentaci.
8. **Reaguj na selhání, negeneruj spekulativně.** Pravidla pro AI (AGENTS.md) přidávej, až když pozoruješ konkrétní selhání — ne dopředu „pro jistotu" (zabraňuje to bloatu a tzv. context rotu, kdy se kritická pravidla „ztratí uprostřed" dlouhého kontextu).

### Část E — Časté chyby a anti-vzory

- **„Vodopád v Markdownu."** Nejčastější kritika SDD (François Zaninotto/Marmelab popsal případ, kdy zobrazení data vyžadovalo 1 300 řádků Markdownu, a agenti je stejně ignorovali). Obrana: malé iterace, ne stovky stran dopředu.
- **Přespecifikování.** Příliš dlouhá spec přetíží „attention budget" modelu („curse of instructions") — AI začne ignorovat instrukce. Drž to fokusované; rozděl na fáze/komponenty.
- **Míchání CO a JAK.** Technické detaily (barvy, velikosti, knihovny) patří do `plan.md`, ne do `spec.md`. Over-eager modely (např. Claude Sonnet) tam technické detaily „prosakují" — vrať je do plánu.
- **Reward hacking.** AI změní test místo kódu. Obrana: commituj padající testy předem; používej nezávislého „reviewer" agenta (writer/reviewer pattern).
- **Stará struktura v pravidlech.** Zastaralý popis struktury repozitáře v AGENTS.md aktivně mate agenta (studie ETH). Drž pravidla aktuální nebo radši stručná.
- **Spec drift bez vynucení.** Spec-first bez testů/CI driftuje. Pokud projekt budeš udržovat dlouhodobě, přejdi na spec-anchored (spec žije s kódem).
- **Slepá důvěra v AI kód.** Simon Willison: pokud jsi kód nezkontroloval, neotestoval a nepochopil, neneseš za něj odpovědnost. Jeho „vibe engineering" = používat AI profesionálně s testy, code review a verzováním.

---

## Doporučení (Recommendations) — co dál prozkoumat a udělat

### Fáze 1 — Začni TEĎ (první víkend)
1. Vytvoř jen dva soubory: `spec.md` (z šablony C.1, vyplň sekce 1–3 a 6–7) a `AGENTS.md`/`CLAUDE.md` (C.6).
2. Použij „Plan Mode" (Claude Code: Shift+Tab) — nech AI napřed udělat plán v režimu jen-pro-čtení, zkontroluj ho, teprve pak nech psát kód.
3. Napiš 1–3 BDD scénáře (C.5) pro hlavní happy-path chování.
4. Vyzkoušej TDD smyčku: nech AI napsat padající test, potvrď že padá, zacommituj ho, pak nech implementovat.
- **Práh pro posun dál:** Když se ti spec rozrůstá nad ~1 stránku nebo když AI začne ignorovat části zadání → rozděl spec na fáze a přidej `plan.md`.

### Fáze 2 — Když projekt poroste
5. Přidej `constitution.md` (C.2) a `plan.md` (C.3) + `tasks.md` (C.4).
6. Nastav linter a formatter (deterministické nástroje), ať nemusíš styl řešit v AGENTS.md.
7. Zaveď CI, které spustí testy při každém commitu — automatická obrana proti driftu.
8. Vyzkoušej GitHub Spec Kit (`uvx … specify init`) nebo Kiro a porovnej s vlastním Markdown setupem. Thoughtworks doporučuje pro brownfield (existující projekty) zvážit i lehčí **OpenSpec** (workflow propose → apply → archive), který je iterativní a nezávislý na nástroji.
- **Práh:** Když na projektu pracuje víc lidí (nebo víc AI agentů) → přejdi na spec-anchored a zaveď pull-request revize specifikace.

### Fáze 3 — Pokročilé (až budeš jistý)
9. Prozkoumej EARS notaci pro jednoznačné požadavky (Část B.3).
10. Prozkoumej „feedback flywheel" (Thoughtworks): po každém sezení s agentem si zapiš, co fungovalo a co ne, a zlepšuj svá pravidla (kompounduje se to v čase).
11. Zvaž paralelní agenty (writer/reviewer pattern) — ale Simon Willison varuje, že je to mentálně vyčerpávající; začni max. 2–3 agenty.
12. Sleduj zdroje, protože pole se rychle vyvíjí: blogy Simona Willisona, Andreje Karpathyho, Addy Osmaniho, Martina Fowlera/Thoughtworks, a Anthropic engineering blog.

### Kdy SDD/TDD/BDD NEpoužívat (podle Damien Gouron a Böckeler)
- Jednorázový prototyp, který za týden smažeš → vibe coding stačí.
- Sólo krátkodobý projekt, kde jsi jediný uživatel → režie formalizace se nevyplatí.
- Explorativní kódování, kde ještě nevíš co hledáš → předčasná specifikace škodí objevování.
- Jednoduché CRUD se zjevnými požadavky → dobře napsaný prompt stačí.

SDD dává smysl, když: kód se bude udržovat dlouhodobě, pracuje na něm víc lidí/agentů, a chování je dost složité, aby ho agent bez vodítek špatně pochopil.

---

## Upozornění a omezení (Caveats)

- **Pole se velmi rychle vyvíjí.** Většina zdrojů je z 2025–2026 a definice se „semanticky rozpíjejí" — Thoughtworks (TR Vol 33) píše doslova o *„semantic diffusion: the rapid emergence of new terms for evolving practices, often before their meanings have stabilized"* a uvádí, že termíny jako spec-driven development se používají nekonzistentně. Ber konkrétní příkazy a názvy souborů jako momentku, ne jako věčnou pravdu.
- **SDD je v kategorii „Assess", ne „Adopt".** Thoughtworks ho doporučuje prozkoumat, ne plošně nasadit. Existuje doložená kritika (Scott Logic / Medium: „10× pomalejší, víc ceremonie, stejné bugy"; Marmelab: „vodopád znovu udeřil"), kterou je třeba brát vážně.
- **Spec-as-source je experimentální.** Vize „nezálohujeme kód, zálohujeme prompt" je riskantní, dokud LLM nejsou deterministické (Böckeler: LLM nejsou kompilátory, ale „inferrery" — nemají opakovatelný, předvídatelný výstup). Pro začátečníka: zůstaň u spec-first / spec-anchored.
- **Některé citace pocházejí ze sekundárních zdrojů** (přepisy talků, shrnutí). Talk Seana Grovea „The New Code" a podcast Kenta Becka (Pragmatic Engineer, červen 2025) jsem čerpal převážně z přepisů a shrnutí, ne vždy z primárního videa.
- **Čísla o úspěšnosti AI** (např. „úspěch ~33 % bez vedení") pocházejí z interních testů Anthropic citovaných komunitními zdroji (DataCamp); ber je jako orientační, ne jako tvrdou vědu.
- **Počet hvězd Spec Kitu** (107 051 k 29. 5. 2026) je momentka z žebříčku EvanLi/Github-Ranking a rychle roste — neber ho jako přesné aktuální číslo.