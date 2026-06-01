# 📋 Univerzální specifikační šablona (SDD + TDD + BDD)

> **Jak tuto šablonu používat**
> - Text v `[HRANATÝCH ZÁVORKÁCH]` jsou **zástupné texty** — nahraď je svým obsahem a závorky smaž.
> - Řádky začínající `> 💡` jsou **nápověda** (proč sekce existuje / jak ji vyplnit). Můžeš je po vyplnění smazat, nebo nechat jako vodítko.
> - Sekce označené `*(povinné)*` vyplň vždy. Sekce `*(volitelné)*` přidávej, až je budeš potřebovat.
> - **Pro úplný začátek stačí vyplnit část 1 (Specifikace) + část 6 (AGENTS.md).** Zbytek přidávej postupně, jak projekt poroste.
> - Tento jeden soubor zastupuje celou sadu (`spec.md`, `constitution.md`, `plan.md`, `tasks.md`, `*.feature`, `AGENTS.md`). U větších projektů je můžeš rozdělit zpět do samostatných souborů.

---

## 🧭 Hlavička dokumentu

| Pole | Hodnota |
|---|---|
| **Název projektu / funkce** | `[NÁZEV]` |
| **Verze** | `0.1` |
| **Stav** | `Návrh` · `Schváleno` · `Implementováno` · `Hotovo` |
| **Autor** | `[JMÉNO]` |
| **Vytvořeno** | `[DATUM]` |
| **Poslední úprava** | `[DATUM]` |
| **Použité AI nástroje** | `[Claude Code · Cursor · Spec Kit · …]` |

---

# 1. SPECIFIKACE — *CO a PROČ stavíme* *(povinné)*

> 💡 Tahle část je srdce dokumentu. Popisuje **záměr a chování**, ne technologie. Nech ji čitelnou i pro netechnického člověka. Drž zde „CO a PROČ" — „JAK" patří do části 3 (Plán).

## 1.1 Záměr a kontext (PROČ) *(povinné)*

- **Problém:** `[Jaký problém řešíme? Pro koho?]`
- **Cíl / hodnota:** `[Co se zlepší, až to bude hotové? Jak poznáme úspěch?]`
- **Mimo rozsah (Non-goals):** `[Co výslovně NEděláme — brání rozlézání rozsahu (scope creep).]`

> 💡 *Příklad non-goal:* „V této verzi neřešíme přihlášení přes sociální sítě."

## 1.2 Uživatelské scénáře — *co uživatel dělá* *(povinné)*

> 💡 Modely (i lidé) chápou konkrétní příklady líp než abstraktní popisy. Seřaď stories podle priority (P1 = nezbytné, P2 = důležité, P3 = volitelné). Každá P1 story by měla jít otestovat samostatně.

### User Story 1 — `[krátký název]` (Priorita: **P1**)

> Jako `[role/typ uživatele]` chci `[akce]`, abych `[přínos]`.

- **Proč tato priorita:** `[1 věta]`
- **Akceptační scénáře** (formát Given–When–Then):
  1. **Given** `[počáteční stav]` · **When** `[akce]` · **Then** `[očekávaný výsledek]`.
  2. **Given** `[…]` · **When** `[…]` · **Then** `[…]`.

### User Story 2 — `[krátký název]` (Priorita: **P2**)

> Jako `[role]` chci `[akce]`, abych `[přínos]`.

- **Akceptační scénáře:**
  1. **Given** `[…]` · **When** `[…]` · **Then** `[…]`.

### 1.2.x Hraniční případy (Edge Cases) *(povinné)*

> 💡 Tady AI nejčastěji chybuje. Vyjmenuj, co se stane při chybách a okrajových stavech.

- Co se stane, když `[chybný / prázdný vstup]`?
- Co se stane, když `[výpadek sítě / nedostupná služba]`?
- Co se stane, když `[uživatel nemá oprávnění]`?
- Co se stane při `[souběžném přístupu / duplicitě]`?

## 1.3 Funkční požadavky — *co systém musí umět* *(povinné)*

> 💡 Piš ve stylu **EARS** (jednoznačné věty) a čísluj `FR-XXX`. Číslování umožňuje **vystopovatelnost** (traceability) — namapovat „co bylo slíbeno" na „co bylo dodáno". Nejasnosti označ přímo v textu jako `[NEEDS CLARIFICATION: co přesně chybí]`.
>
> **EARS vzory:**
> - *Vždy:* „Systém MUSÍ `[schopnost]`."
> - *Když (událost):* „Když `[událost]`, systém MUSÍ `[reakce]`."
> - *Dokud/Zatímco (stav):* „Dokud `[stav]`, systém MUSÍ `[chování]`."
> - *Pokud (nežádoucí stav):* „Pokud `[chyba]`, systém MUSÍ `[reakce]`."
> - *Kde (volitelná funkce):* „Kde `[je přítomna funkce]`, systém MUSÍ `[chování]`."

- **FR-001:** Systém MUSÍ `[konkrétní schopnost]`.
- **FR-002:** Když `[událost]`, systém MUSÍ `[reakce]`.
- **FR-003:** Pokud `[chybový stav]`, systém MUSÍ `[reakce]`.
- **FR-004:** `[…]`

## 1.4 Nefunkční požadavky — *jak DOBŘE* *(volitelné, ale doporučené)*

> 💡 Bezpečnost a výkon patří do specifikace od začátku, ne jako dodatek. Vyplň jen relevantní řádky.

- **Výkon:** `[např. odezva API < 200 ms při běžné zátěži]`
- **Bezpečnost:** `[např. žádné tajné klíče v kódu; hesla hashovaná; validace všech vstupů]`
- **Použitelnost / přístupnost:** `[…]`
- **Spolehlivost / dostupnost:** `[…]`
- **Náklady / limity:** `[…]`

## 1.5 Datové entity *(zahrň, pokud jde o data)*

> 💡 Popiš hlavní „věci", se kterými systém pracuje, a jejich vztahy. Nepiš konkrétní DB schéma (to je „JAK", patří do plánu).

- **`[Entita A]`:** klíčová pole `[…]`; vztah k `[Entita B]` `[1:N / N:M / …]`.
- **`[Entita B]`:** klíčová pole `[…]`.

## 1.6 Kritéria úspěchu — *měřitelná* *(povinné)*

> 💡 Bez měřitelných kritérií nikdo (ani AI) nepozná, kdy je hotovo. Vyhni se nejasnostem typu „rychle" → uveď číslo.

- **SC-001:** `[měřitelný výsledek, např. „uživatel dokončí registraci do 2 minut"]`
- **SC-002:** `[…]`

## 1.7 Předpoklady a otevřené otázky *(volitelné)*

- **Předpoklady:** `[co bereme jako dané — např. „uživatel má účet"]`
- **Otevřené otázky:** `[na co se musíme ještě doptat / rozhodnout]`

---

# 2. ÚSTAVA PROJEKTU — *neměnné principy* *(volitelné — přidej u větších projektů)*

> 💡 „Ústava" zachycuje pravidla, která platí **napříč všemi funkcemi** (testovací standardy, architektura, konvence). AI agent je MUSÍ dodržovat při každé úloze. Drž ji krátkou a konkrétní.

## Principy

1. **Kvalita kódu:** Dodržuj idiomy a styl jazyka `[X]`. Používej linter `[Y]` a formatter `[Z]`.
2. **Testování:** Žádný produkční kód bez předem padajícího testu (TDD). Minimální pokrytí `[N]` %.
3. **Jednoduchost:** Nejjednodušší řešení, které projde testy. Žádné předčasné zobecňování.
4. **Bezpečnost:** Nikdy necommituj tajné klíče. Validuj všechny vstupy.
5. **Malé změny:** Strukturální (refactor) a behaviorální (nová funkce) změny odděl do různých commitů.
6. **Struktura repozitáře:** `[stručný popis, kde co je]`.

---

# 3. PLÁN IMPLEMENTACE — *JAK to postavíme* *(volitelné — vyplň, až je specifikace hotová)*

> 💡 Odděluj „CO/PROČ" (část 1) od „JAK" (tady). Díky tomu lze stejnou specifikaci implementovat různými způsoby. Hlídej, aby technické detaily „neprosakovaly" zpět do specifikace.

## 3.1 Souhrn

`[2–3 věty: jakým způsobem to postavíme.]`

## 3.2 Technický kontext

> 💡 Neznámé hodnoty označ `NEEDS CLARIFICATION`, ať se na ně nezapomene.

- **Jazyk / verze:** `[např. Python 3.12]`
- **Hlavní závislosti / frameworky:** `[…]`
- **Úložiště / databáze:** `[…]`
- **Testovací framework:** `[…]`
- **Cílová platforma:** `[web / CLI / mobil / …]`

## 3.3 Kontrola ústavy (brána) ✅

> 💡 Než začneš implementovat, ověř, že plán neporušuje principy z části 2.

- [ ] Plán neporušuje žádný princip z Ústavy projektu.
- [ ] Případná porušení jsou zdůvodněna níže (3.5).

## 3.4 Struktura projektu

```
[projekt]/
├── src/          # zdrojový kód
├── tests/        # testy
├── docs/         # dokumentace
└── [...]
```

## 3.5 Sledování složitosti *(vyplň jen při porušení principu)*

| Co porušujeme | Proč je to nutné | Jednodušší varianta, kterou jsme zamítli |
|---|---|---|
| `[…]` | `[…]` | `[…]` |

---

# 4. ÚKOLY — *rozpad na malé kroky* *(volitelné — vyplň před implementací)*

> 💡 Malé úkoly = malé, přezkoumatelné změny. Místo „postav autentizaci" piš „vytvoř endpoint registrace, který validuje formát e-mailu". Brání to křehkému AI kódu, který se zhroutí při kontrole.
>
> **Legenda:** `T001` = pořadí · `[P]` = lze dělat paralelně (jiné soubory, bez závislostí) · `[US1]` = patří k User Story 1.

## Fáze 1: Příprava

- [ ] **T001** Vytvoř strukturu projektu.
- [ ] **T002** `[P]` Nastav linter a formatter.

## Fáze 2: Základ *(musí být hotové před prací na user stories)*

- [ ] **T003** `[P]` Nastav testovací framework.
- [ ] **T004** `[P]` Vytvoř základní datové modely.

## Fáze 3: User Story 1 (P1)

> 💡 Pořadí v rámci TDD: nejdřív padající test, pak minimální implementace.

- [ ] **T005** `[US1]` Napiš padající test pro `[chování]` v `tests/[...]`.
- [ ] **T006** `[US1]` Implementuj minimální kód, aby test prošel, v `src/[...]`.
- [ ] **T007** `[US1]` Refactor (vyčisti kód beze změny chování).

## Fáze 4: User Story 2 (P2)

- [ ] **T008** `[US2]` `[…]`

## Fáze 5: Dokončení (Polish)

- [ ] **T0xx** Doplň dokumentaci, ošetři hraniční případy, zkontroluj pokrytí testy.

---

# 5. BDD SCÉNÁŘE — *chování viditelné uživateli* *(volitelné, ale silné)*

> 💡 Gherkin je čitelný i pro netechnické lidi a zároveň **spustitelný jako test** (živá dokumentace přes nástroje typu Cucumber/Behave). Pravidla: **jeden scénář = jedno chování**; konkrétní „Then" (žádné vágní „funguje to"); nemíchej UI a API kroky v jednom scénáři. V praxi tahle část žije v samostatném souboru `*.feature`.

```gherkin
# language: cs
Funkce: [název funkce]
  Jako [role]
  Chci [akce]
  Abych [přínos]

  Scénář: [název konkrétního případu — happy path]
    Pokud [počáteční stav]
    Když [akce uživatele]
    Pak [očekávaný výsledek]

  Scénář: [název konkrétního případu — chyba / edge case]
    Pokud [počáteční stav]
    Když [chybná akce nebo okrajový vstup]
    Pak [očekávané ošetření chyby]
```

---

# 6. AGENTS.md — *pravidla pro AI agenta* *(povinné, pokud používáš AI nástroje)*

> 💡 AI nemá paměť mezi sezeními — každé sezení začíná „čistý list". Tenhle blok se načte automaticky a říká agentovi, jak se v projektu chovat. **Drž ho krátký** (ideálně do ~150 řádků). Styl kódu sem NEDÁVEJ — od toho je linter. Pravidla přidávej, až když pozoruješ konkrétní selhání, ne dopředu „pro jistotu". V praxi tahle část žije v souboru `AGENTS.md` nebo `CLAUDE.md` v kořeni repozitáře.

```markdown
# Pokyny pro AI agenty

## Projekt
[1–2 věty: co to je a v čem je to napsané, včetně verzí.]

## Příkazy
- Build:  `[příkaz]`
- Test:   `[příkaz]`
- Lint:   `[příkaz]`
- Spuštění: `[příkaz]`

## Workflow
- Nejdřív plán (read-only / Plan Mode), pak teprve kód.
- TDD: padající test → minimální kód → refactor.
- NEMĚŇ testy jen proto, aby prošly. Oprav kód.
- Drž se Specifikace (část 1) a Plánu (část 3). Při nejasnosti se zeptej.

## Mantinely (hranice)
- ✅ Vždy:   spusť testy před commitem; commituj malé, logické celky.
- ⚠️ Zeptej se: změny datového schématu; přidání nové závislosti; změna veřejného API.
- 🚫 Nikdy:  necommituj tajné klíče; needituj `node_modules/` ani generované soubory; neměň CI konfiguraci bez svolení.

## Příklad správného stylu (1 reálný útržek kódu řekne víc než odstavec popisu)
[krátký příklad funkce / commit message / pojmenování dle konvence projektu]
```

---

# ✅ 7. Definice hotového (Definition of Done) *(povinné)*

> 💡 Na rozdíl od akceptačních kritérií (specifická pro každou story) je tohle **společný kontrolní seznam pro každou dodávku**.

- [ ] Všechny akceptační scénáře (1.2) procházejí jako automatické testy.
- [ ] Pokryté hraniční případy (1.2.x).
- [ ] Žádné nevyřešené `[NEEDS CLARIFICATION]` ve specifikaci.
- [ ] Kód prochází linterem; všechny testy jsou zelené.
- [ ] Splněna měřitelná kritéria úspěchu (1.6).
- [ ] Specifikace aktualizována podle finální implementace (proti spec driftu).
- [ ] Kód jsem přečetl/a a rozumím mu (neslepá důvěra v AI).

---

# 🔄 8. Údržba a relevance — *aby se specifikace nerozešla s kódem* *(důležité u dlouhodobých projektů)*

> 💡 Největší riziko je **spec drift** — specifikace a kód se rozejdou a dokumentaci přestane nikdo věřit. Pár návyků, které tomu brání:

- [ ] **Jediný zdroj pravdy:** každou změnu promítni do tohoto dokumentu, ne jen do kódu.
- [ ] **Verzuj v Gitu:** tento soubor commituj spolu s kódem; měň ho přes stejný proces (revize / pull request).
- [ ] **Krátké smyčky:** spec → implementuj → uč se → **aktualizuj spec**. (Tím se z toho nestane vodopád.)
- [ ] **Testy v CI:** spustitelné akceptační a jednotkové testy automaticky odhalí, když kód uteče od specifikace.
- [ ] **Udržuj historii revizí** (níže).

## Historie revizí

| Datum | Verze | Co se změnilo | Autor |
|---|---|---|---|
| `[DATUM]` | `0.1` | Počáteční verze. | `[JMÉNO]` |

---

<sub>Tato univerzální šablona slučuje šest běžných artefaktů spec-driven workflow (specifikace, ústava, plán, úkoly, BDD scénáře, pravidla pro AI agenty) do jednoho souboru. Vychází z aktuálních praktik AI engineering komunity (GitHub Spec Kit, Amazon Kiro, EARS, AGENTS.md, TDD/BDD). Pole se rychle vyvíjí — ber konkrétní formáty jako výchozí bod, ne jako dogma.</sub>
