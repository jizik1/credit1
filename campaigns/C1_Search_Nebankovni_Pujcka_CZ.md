# Návrh kampaně: C1_Search_Nebankovni_Pujcka_CZ

> **Stav:** návrh ke schválení • **status pro spuštění:** PAUSED • **provedení:** validate_only=True nejprve

---

## A. Shrnutí první kampaně

První vyhledávací kampaň pro účet **Credit 1** (customer_id `5726766523`, MCC `2373464403`), web `https://www.credit1.cz`. Účel: získávat **registrace / odeslané žádosti** o online půjčku (SIGNUP), nikoli prokliky.

| Položka | Hodnota |
|---|---|
| Název | `C1_Search_Nebankovni_Pujcka_CZ` |
| Typ | Search / vyhledávací |
| Lokalita | Česká republika |
| Jazyk | čeština |
| Status (pro vytvoření) | **PAUSED** |
| Bidding | **Maximize conversions s target CPA = 195 Kč** |
| Denní rozpočet | **5 000 Kč/den** (= 5 000 000 000 micros) |
| Konverzní cíl | účet-level default (SIGNUP → `credit1_step2`) |
| Customer acquisition | stejná cena nové i stávající (NEnastavovat „new only") |
| `contains_eu_political_advertising` | `DOES_NOT_CONTAIN_EU_POLITICAL_ADVERTISING` |
| Reklamní sestavy | 9 (viz sekce D) |

---

## B. Compliance kontrola

### B.1 Compliance rámec — co reklama smí / nesmí

| ✅ Smí říkat | ❌ Nesmí říkat |
|---|---|
| Online žádost, vyřízení online | Schválíme každému / pro každého |
| Půjčka 1 000–100 000 Kč (rozsah z webu) | Bez registru / bez kontroly registru |
| Vyplňte žádost / posoudíme žádost | Bez doložení příjmu / bez bonity |
| Žádost z pohodlí domova, bez návštěvy pobočky | Pro dlužníky / v exekuci / v insolvenci |
| Předčasné splacení zdarma (z webu) | Garantované / jisté schválení |
| Nezávazná žádost | Peníze každému / bez ověření |

### B.2 Specifické zákazy formulace (uživatelské pravidlo)

V **žádném** asset textu se NESMÍ objevit:
- „zprostředkujeme", „zprostředkovatel", „zprostředkování"
- „my půjčujeme", „půjčíme vám", „půjčujeme peníze"
- Cokoli, co naznačuje, že **inzerent je věřitel**.

Místo toho: neutrální / akční formulace („vyřídíme", „půjčte si online", „online žádost"). Návštěvník při kliknutí nemá ihned poznat, že jde o zprostředkování.

### B.3 Rizikové ad groups — zvláštní opatrnost

| Sestava | Riziko | Mitigace |
|---|---|---|
| Půjčka do výplaty | Asociace s „payday loan" — obecně agresivní segment | Pouze PHRASE match, žádné EXACT na „půjčka do výplaty bez registru" apod.; texty bez slov „okamžitě", „rychle" v kombinaci s částkou |
| Půjčka bez poplatku | „Bez poplatku" může klamavě naznačovat půjčku zcela zdarma. Z webu víme: **zdarma je odeslání žádosti, ne půjčka.** | Texty MUSÍ být přesné: „Odeslání žádosti zdarma", nikdy ne „Půjčka zdarma" |
| Půjčka na OP | Asociace s „stačí občanka" → klamavé | Texty obecné: „Online žádost o půjčku", ne „Půjčka jen na OP" |

### B.4 Web compliance (z analýzy credit1.cz)

- Web je provozován **LIFO-CB, s.r.o.** jako samostatný zprostředkovatel spotřebitelských úvěrů (§ 17 zák. 257/2016). Reklama tedy musí být v souladu se zprostředkovatelskou rolí — viz B.2.
- Web sám použivá formulace „100% online", „bez zbytečných papírů". V reklamních textech tyto NEpřebírám doslovně, místo toho neutrální „online žádost", „vyřízení online".

---

## C. Nastavení kampaně (technické parametry)

```
campaign:
  name: "C1_Search_Nebankovni_Pujcka_CZ"
  status: PAUSED
  advertising_channel_type: SEARCH
  contains_eu_political_advertising: DOES_NOT_CONTAIN_EU_POLITICAL_ADVERTISING
  campaign_budget: <viz níže>
  bidding_strategy_type: MAXIMIZE_CONVERSIONS
  maximize_conversions:
    target_cpa_micros: 195000000        # 195 Kč
  network_settings:
    target_google_search: true
    target_search_network: true          # včetně partner sites
    target_content_network: false
    target_partner_search_network: false
  geo_target_type_setting:
    positive_geo_target_type: PRESENCE_OR_INTEREST
    negative_geo_target_type: PRESENCE
  customer_acquisition_setting:
    optimization_mode: OPTIMIZE_ACQUISITION   # rovnocenné nové i stávající, NE "BID_HIGHER_FOR_NEW"

campaign_budget:
  name: "C1_Search_Nebankovni_Pujcka_CZ — denni 5000"
  amount_micros: 5000000000             # 5 000 Kč/den
  delivery_method: STANDARD
  explicitly_shared: false              # dedikovaný budget pro tuto kampaň

campaign_criterion (location):
  location: "geoTargetConstants/2203"   # Czech Republic

campaign_criterion (language):
  language: "languageConstants/1021"    # Czech
```

---

## D. Reklamní sestavy (přehled)

| # | Ad group | Záměr | Match strategie | Doporučení |
|---|---|---|---|---|
| 1 | `AG_Online_Pujcka` | online půjčka obecně | EXACT + PHRASE | spustit |
| 2 | `AG_Rychla_Pujcka` | rychlá půjčka | EXACT + PHRASE | spustit |
| 3 | `AG_Nebankovni_Pujcka` | nebankovní půjčka | EXACT + PHRASE | spustit |
| 4 | `AG_Pujcka_Ihned` | půjčka ihned / dnes | EXACT + PHRASE | spustit, sledovat search terms |
| 5 | `AG_Pujcka_Na_Ucet` | půjčka na účet | EXACT + PHRASE | spustit |
| 6 | `AG_Pujcka_Na_OP` | půjčka na OP | PHRASE only | spustit opatrně |
| 7 | `AG_Zadost_O_Pujcku` | žádost o půjčku | EXACT + PHRASE | spustit |
| 8 | `AG_Pujcka_Do_Vyplaty` | půjčka do výplaty | PHRASE only | spustit opatrně, hlídat search terms |
| 9 | `AG_Pujcka_Bez_Poplatku` | bez poplatku (myšleno žádost) | PHRASE only | spustit opatrně, texty přesné |

Default CPC bid (kvůli systému, MAXIMIZE_CONVERSIONS si řídí sám) — nepotřebujeme, kampaň nemá manuální CPC.

---

## E. Klíčová slova podle sestav

> Notace: `[exact]`, `"phrase"`. U každé sestavy je 3–5 hlavních KW označeno **★** jako priority pro start.

### E.1 AG_Online_Pujcka
- ★ `[online půjčka]`
- ★ `[půjčka online]`
- ★ `"online půjčka"`
- ★ `"půjčka online"`
- `[půjčka přes internet]`
- `"půjčka přes internet"`
- `[internetová půjčka]`
- `"online půjčka ihned"`
- `"online půjčka na účet"`
- `"sjednání půjčky online"`

### E.2 AG_Rychla_Pujcka
- ★ `[rychlá půjčka]`
- ★ `"rychlá půjčka"`
- ★ `[rychlá online půjčka]`
- ★ `"rychlá online půjčka"`
- `"půjčka rychle"`
- `[rychlá nebankovní půjčka]`
- `"rychlá půjčka online"`
- `"rychlé vyřízení půjčky"`
- `"rychlá půjčka na účet"`
- `"půjčka rychle online"`

### E.3 AG_Nebankovni_Pujcka
- ★ `[nebankovní půjčka]`
- ★ `"nebankovní půjčka"`
- ★ `[nebankovní půjčka online]`
- ★ `"nebankovní půjčka online"`
- `"nebankovní půjčka ihned"`
- `"nebankovní půjčka na účet"`
- `[nebankovní online půjčka]`
- `"sjednání nebankovní půjčky"`
- `"online nebankovní půjčka"`
- `"nebankovní půjčka přes internet"`

### E.4 AG_Pujcka_Ihned
- ★ `[půjčka ihned]`
- ★ `"půjčka ihned"`
- ★ `[půjčka ještě dnes]`
- ★ `"půjčka ještě dnes"`
- `"půjčka ihned na účet"`
- `"půjčka hned"`
- `"online půjčka ihned"`
- `"rychlá půjčka ihned"`
- `"půjčka dnes"`
- `"půjčka ihned online"`

### E.5 AG_Pujcka_Na_Ucet
- ★ `[půjčka na účet]`
- ★ `"půjčka na účet"`
- ★ `[půjčka na bankovní účet]`
- ★ `"půjčka na účet online"`
- `"půjčka na účet ihned"`
- `"online půjčka na účet"`
- `"půjčka rovnou na účet"`
- `"půjčka na účet bez papírování"` *(bez papírování = bez návštěvy pobočky, neutrální)*
- `"nebankovní půjčka na účet"`
- `"půjčka na účet do hodiny"`

### E.6 AG_Pujcka_Na_OP (PHRASE only, opatrnost)
- ★ `"půjčka na OP"`
- ★ `"půjčka na občanský průkaz"`
- ★ `"půjčka na občanku"`
- `"online půjčka na OP"`
- `"půjčka na OP ihned"`
- `"rychlá půjčka na OP"`
- `"nebankovní půjčka na OP"`
- `"půjčka na OP online"`
- `"půjčka na občanský průkaz online"`
- `"půjčka na občanku ihned"`

### E.7 AG_Zadost_O_Pujcku
- ★ `[žádost o půjčku]`
- ★ `"žádost o půjčku"`
- ★ `[žádost o půjčku online]`
- ★ `"žádost o půjčku online"`
- `"online žádost o půjčku"`
- `"vyplnit žádost o půjčku"`
- `"podat žádost o půjčku"`
- `"online formulář půjčka"`
- `"online žádost půjčka"`
- `"sjednání žádosti o půjčku"`

### E.8 AG_Pujcka_Do_Vyplaty (PHRASE only, opatrnost)
- ★ `"půjčka do výplaty"`
- ★ `"krátkodobá půjčka"`
- ★ `"půjčka do výplaty online"`
- `"půjčka před výplatou"`
- `"krátkodobá půjčka online"`
- `"půjčka do výplaty ihned"`
- `"půjčka před výplatou online"`
- `"krátkodobá online půjčka"`
- `"půjčka do výplaty na účet"`
- `"online krátkodobá půjčka"`

### E.9 AG_Pujcka_Bez_Poplatku (PHRASE only, opatrnost — texty přesné!)
- ★ `"půjčka bez poplatku"`
- ★ `"žádost o půjčku zdarma"`
- ★ `"online žádost o půjčku zdarma"`
- `"půjčka bez poplatku online"`
- `"odeslání žádosti zdarma"`
- `"žádost o půjčku bez poplatku"`
- `"půjčka zdarma žádost"`
- `"online půjčka bez poplatku"`
- `"půjčka bez poplatku za žádost"`
- `"zdarma žádost o online půjčku"`

> **Compliance note pro E.9:** Texty RSA pro tuto ad group nesmí říkat „půjčka zdarma" / „bez poplatku" v sense, že je úvěr bezplatný. Vždy upřesnit „odeslání žádosti zdarma".

---

## F. Negativní klíčová slova (campaign level)

Negativní KW jsou aplikovaná na úrovni celé kampaně. Match type: EXACT (`[neg]`) nebo PHRASE (`"neg"`), broad negative pouze tam, kde je to bezpečné.

### F.1 Compliance riziko / klamavé tvrzení
- `"bez registru"`, `"bez kontroly registru"`, `"bez nahlížení do registru"`
- `"bez doložení příjmu"`, `"bez příjmu"`, `"bez ověření příjmu"`
- `"bez bonity"`, `"bez kontroly"`, `"bez posouzení"`, `"bez ověření"`
- `"půjčka bez bonity"`, `"půjčka bez ověření"`, `"půjčka bez kontroly"`
- `"půjčka pro každého"`, `"půjčka každému"`, `"schválíme každému"`
- `"půjčka 100"`, `"100% schválení"`, `"garantovaná půjčka"`

### F.2 Exekuce / insolvence / dlužníci
- `"exekuce"`, `"v exekuci"`, `"půjčka v exekuci"`, `"půjčka pro exekuci"`
- `"insolvence"`, `"v insolvenci"`, `"půjčka v insolvenci"`, `"insolvenční půjčka"`
- `"oddlužení"`, `"oddlužení půjčka"`
- `"dlužník"`, `"pro dlužníky"`, `"půjčka pro dlužníky"`
- `"se záznamem v registru"`, `"záznam v registru"`

### F.3 Nezaměstnanost / bez zaměstnání
- `"bez zaměstnání"`, `"nezaměstnaný"`, `"pro nezaměstnané"`
- `"půjčka pro nezaměstnané"`, `"půjčka bez zaměstnání"`
- `"půjčka na mateřské"` *(zranitelná skupina, vyloučeno preventivně)*
- `"půjčka na rodičovské"`

### F.4 Informační / vzdělávací dotazy
- `[co je nebankovní půjčka]`, `[jak funguje půjčka]`, `[jak získat půjčku]`
- `"kalkulačka"`, `"srovnání"`, `"recenze"`, `"diskuze"`, `"forum"`
- `"wikipedia"`, `"definice"`, `"vysvětlení"`
- `"jak se počítá rpsn"`, `"co je rpsn"`

### F.5 Kariéra / práce
- `"práce"`, `"zaměstnání"`, `"kariéra"`, `"brigáda"`
- `"plat"`, `"mzda"` (může chytit „půjčka před výplatou", ale chceme)
- `"nábor"`, `"hiring"`, `"job"`

### F.6 Konkurence (volitelné, doporučené pro start)
- `"zaplo"`, `"creditportal"`, `"home credit"`, `"cetelem"`, `"cofidis"`, `"essox"`
- `"raiffeisen"`, `"airbank"`, `"česká spořitelna"`, `"komerční banka"`, `"moneta"`, `"unicredit"`
- `"hello bank"`, `"mbank"`, `"creamfinance"`, `"ferratum"`, `"vivus"`, `"kamali"`
- `"creditea"`, `"vista"`, `"smart půjčka"`

### F.7 Nevhodné finanční typy
- `"hypotéka"`, `"hypoteční úvěr"`, `"refinancování hypotéky"`
- `"půjčka na auto"`, `"leasing"`, `"autoleasing"`
- `"podnikatelská půjčka"`, `"firemní úvěr"`, `"půjčka pro firmy"`, `"půjčka pro OSVČ"`
- `"konsolidace"`, `"refinancování"`
- `"půjčka na bydlení"`, `"americká hypotéka"`
- `"půjčka v hotovosti"` (web jen na účet)
- `"půjčka na směnku"`, `"lichva"`, `"lichvář"`

### F.8 Geografická / cizí jazyky
- `"po polsku"`, `"po slovensky"`, `"in english"`, `"online loan"`
- `"slovensko"`, `"polsko"`, `"rakousko"`

---

## G. RSA reklamy podle sestav

> Každá RSA má 15 nadpisů (≤30 znaků) a 4 popisy (≤90 znaků). Final URL: `https://www.credit1.cz`. Cesta: `path1 = /zadost-online`, `path2 = /online` (per ad group).
>
> **Doporučené pinningy:** žádné defaultní pinningy — Google Ads RSA se nejlépe učí s volnou rotací. Pokud bude později problém s asset comply, zvážíme pinning H1[1] na brand-safe „Online žádost o půjčku".

### G.1 AG_Online_Pujcka

**Nadpisy (15)**
1. Online půjčka 1 000–100 000 Kč
2. Půjčte si online a v klidu
3. Online žádost o půjčku
4. Půjčka online na účet
5. Vyřízení žádosti online
6. Vyplňte žádost online
7. Půjčka 1 000–100 000 Kč
8. Online formulář žádosti
9. Nezávazná online žádost
10. Vyřídíte vše online
11. Žádost z pohodlí domova
12. Bez návštěvy pobočky
13. Online půjčka přehledně
14. Posouzení žádosti online
15. Online půjčka i o víkendu

**Popisy (4)**
1. Vyplňte online žádost o půjčku a vyřiďte vše z pohodlí domova. Posouzení online.
2. Online půjčka 1 000–100 000 Kč. Žádost online, bez návštěvy pobočky. Nezávazně.
3. Vyplnění žádosti během pár minut. Vyřízení online i o víkendu. Předčasné splacení zdarma.
4. Pošlete online žádost a posoudíme ji online. Půjčte si jednoduše a přehledně.

**Compliance:** texty obecné, žádné garantované schválení, žádné „bez registru". OK.

---

### G.2 AG_Rychla_Pujcka

**Nadpisy (15)**
1. Rychlá online žádost o půjčku
2. Rychlé vyřízení online
3. Půjčka online rychle
4. Žádost online za pár minut
5. Vyplnění žádosti za 2 minuty
6. Rychlá půjčka 1 000–100 000 Kč
7. Online žádost, rychle a snadno
8. Vyřízení žádosti i o víkendu
9. Pošlete žádost online rychle
10. Online půjčka, rychlá odpověď
11. Vyplňte žádost online
12. Půjčte si rychle online
13. Žádost z pohodlí domova
14. Rychlá nebankovní půjčka online
15. Online formulář, rychlé vyřízení

**Popisy (4)**
1. Rychlé online vyřízení žádosti o půjčku. Vyplnění do 2 minut. Odpověď obvykle do několika minut.
2. Půjčte si online a rychle. Žádost 1 000–100 000 Kč. Bez návštěvy pobočky.
3. Online formulář pro rychlé odeslání žádosti. Posouzení online i o víkendu.
4. Rychlá nebankovní půjčka online. Vyřízení z pohodlí domova. Předčasné splacení zdarma.

**Compliance:** rychlost odkazuje na vyřízení žádosti (faktické z webu), ne na garantované schválení.

---

### G.3 AG_Nebankovni_Pujcka

**Nadpisy (15)**
1. Nebankovní online půjčka
2. Nebankovní půjčka 1 000–100 000
3. Online žádost o půjčku
4. Vyřízení žádosti online
5. Nebankovní půjčka na účet
6. Půjčte si online
7. Vyplňte online žádost
8. Nebankovní půjčka přehledně
9. Online formulář žádosti
10. Půjčka na účet online
11. Žádost z pohodlí domova
12. Nezávazná online žádost
13. Vyřídíte vše online
14. Posouzení žádosti online
15. Bez návštěvy pobočky

**Popisy (4)**
1. Vyplňte online žádost o nebankovní půjčku. Vyřízení online, bez návštěvy pobočky.
2. Nebankovní půjčka 1 000–100 000 Kč. Online žádost, posouzení online. Nezávazně.
3. Půjčte si online a přehledně. Žádost během pár minut, vyřízení i o víkendu.
4. Online žádost o nebankovní půjčku. Předčasné splacení zdarma. Vyřízení z domova.

---

### G.4 AG_Pujcka_Ihned

**Nadpisy (15)**
1. Online půjčka, žádost ihned
2. Vyplňte online žádost ihned
3. Online vyřízení žádosti
4. Půjčte si online ještě dnes
5. Online žádost, rychlé posouzení
6. Půjčka 1 000–100 000 Kč online
7. Vyplnění žádosti za pár minut
8. Online žádost o půjčku
9. Půjčka online ještě dnes
10. Pošlete žádost ihned online
11. Online vyřízení i o víkendu
12. Žádost online, bez papírování
13. Online formulář žádosti
14. Půjčte si jednoduše online
15. Posouzení žádosti online

**Popisy (4)**
1. Vyplňte online žádost o půjčku ještě dnes. Online vyřízení i o víkendu.
2. Online půjčka 1 000–100 000 Kč. Žádost vyplníte do 2 minut, posouzení online.
3. Pošlete žádost online a získáte odpověď obvykle během několika minut.
4. Půjčte si online z pohodlí domova. Bez návštěvy pobočky, předčasné splacení zdarma.

**Compliance:** „ještě dnes" odkazuje na podání žádosti, ne na garanci výplaty peněz.

---

### G.5 AG_Pujcka_Na_Ucet

**Nadpisy (15)**
1. Online půjčka na účet
2. Půjčka na bankovní účet
3. Online žádost, půjčka na účet
4. Vyplňte žádost online
5. Online vyřízení žádosti
6. Půjčte si online na účet
7. Půjčka 1 000–100 000 Kč
8. Online formulář žádosti
9. Půjčka na účet bez pobočky
10. Žádost z pohodlí domova
11. Online půjčka přehledně
12. Vyřízení žádosti i o víkendu
13. Posouzení žádosti online
14. Online půjčka, nezávazně
15. Vyplnění žádosti za 2 minuty

**Popisy (4)**
1. Vyplňte online žádost o půjčku. Vyřízení online, bez návštěvy pobočky.
2. Online půjčka 1 000–100 000 Kč na bankovní účet. Žádost přehledně online.
3. Posouzení žádosti online. Odpověď obvykle do několika minut. Předčasné splacení zdarma.
4. Půjčte si online z pohodlí domova. Online formulář, jednoduché vyplnění.

---

### G.6 AG_Pujcka_Na_OP (opatrné texty — žádné „stačí OP")

**Nadpisy (15)**
1. Online žádost o půjčku
2. Online půjčka 1 000–100 000 Kč
3. Vyplňte online žádost
4. Online vyřízení žádosti
5. Půjčte si online
6. Vyplnění žádosti online
7. Online formulář žádosti
8. Nezávazná online žádost
9. Online půjčka na účet
10. Vyřízení žádosti i o víkendu
11. Žádost online z domova
12. Půjčka online přehledně
13. Posouzení žádosti online
14. Online půjčka jednoduše
15. Bez návštěvy pobočky

**Popisy (4)**
1. Vyplňte online žádost o půjčku z pohodlí domova. Vyřízení online, bez papírování.
2. Online půjčka 1 000–100 000 Kč. Posouzení žádosti online, nezávazně.
3. Online formulář pro odeslání žádosti. Vyřízení i o víkendu.
4. Půjčte si online jednoduše a přehledně. Předčasné splacení zdarma.

**Compliance:** vědomě **bez** zmínky o občanském průkazu v textech — aby reklama nepůsobila „stačí OP, půjčíme každému". KW „půjčka na OP" pouze v ad group keywords.

---

### G.7 AG_Zadost_O_Pujcku

**Nadpisy (15)**
1. Online žádost o půjčku
2. Vyplňte online žádost
3. Online formulář žádosti
4. Žádost o půjčku 1 000–100 000
5. Online žádost přehledně
6. Vyřízení žádosti online
7. Pošlete žádost online
8. Žádost online za 2 minuty
9. Online žádost nezávazně
10. Online vyřízení i o víkendu
11. Žádost z pohodlí domova
12. Posouzení žádosti online
13. Vyplnění žádosti jednoduše
14. Online půjčka, online žádost
15. Bez návštěvy pobočky

**Popisy (4)**
1. Vyplňte online žádost o půjčku během pár minut. Posouzení online, nezávazně.
2. Online formulář žádosti o půjčku 1 000–100 000 Kč. Vyřízení i o víkendu.
3. Pošlete žádost online z pohodlí domova. Bez návštěvy pobočky, jednoduché vyplnění.
4. Online žádost o půjčku přehledně. Předčasné splacení zdarma.

---

### G.8 AG_Pujcka_Do_Vyplaty (PHRASE only — opatrné!)

**Nadpisy (15)**
1. Krátkodobá online půjčka
2. Online žádost o půjčku
3. Vyplňte online žádost
4. Půjčka online 1 000–100 000 Kč
5. Online vyřízení žádosti
6. Krátkodobá půjčka online
7. Online formulář žádosti
8. Půjčte si online
9. Žádost online přehledně
10. Vyřízení i o víkendu
11. Online půjčka nezávazně
12. Žádost z pohodlí domova
13. Posouzení žádosti online
14. Online půjčka jednoduše
15. Bez návštěvy pobočky

**Popisy (4)**
1. Vyplňte online žádost o krátkodobou půjčku. Vyřízení online, nezávazně.
2. Online půjčka 1 000–100 000 Kč. Online formulář, posouzení online.
3. Krátkodobá online půjčka přehledně. Žádost z pohodlí domova, online vyřízení.
4. Pošlete žádost online a získejte odpověď obvykle během několika minut.

**Compliance:** texty se vyhýbají slovu „výplata" v hlavičkách — méně dramatický tón. Žádné „peníze do výplaty" / „rychle do výplaty".

---

### G.9 AG_Pujcka_Bez_Poplatku (texty přesné — „odeslání žádosti zdarma")

**Nadpisy (15)**
1. Odeslání žádosti zdarma
2. Online žádost zdarma
3. Online žádost o půjčku
4. Vyplňte online žádost
5. Online formulář žádosti
6. Půjčka 1 000–100 000 Kč
7. Žádost online bez poplatku
8. Online vyřízení žádosti
9. Půjčte si online
10. Žádost z pohodlí domova
11. Online půjčka přehledně
12. Vyřízení i o víkendu
13. Posouzení žádosti online
14. Nezávazná online žádost
15. Bez návštěvy pobočky

**Popisy (4)**
1. Odeslání online žádosti o půjčku je zdarma. Posouzení online, nezávazně.
2. Vyplňte online žádost o půjčku 1 000–100 000 Kč. Žádost online bez poplatku.
3. Online formulář, vyřízení online, předčasné splacení zdarma.
4. Online žádost přehledně a jednoduše. Vyřízení i o víkendu, bez návštěvy pobočky.

**Compliance:** **kritické** — texty říkají „odeslání žádosti zdarma", nikdy ne „půjčka zdarma".

---

## H. Sitelinky, callouty a structured snippets (asset-level, kampaň-level)

### H.1 Sitelinky (4)

| # | Title (≤25) | Desc 1 (≤35) | Desc 2 (≤35) | URL | Compliance |
|---|---|---|---|---|---|
| 1 | Online žádost | Vyplnění do 2 minut | Z pohodlí domova | `https://www.credit1.cz/#zadost` | OK |
| 2 | Časté dotazy | Co je třeba vědět | Přehledné odpovědi | `https://www.credit1.cz/#faq` | OK |
| 3 | Reference klientů | Hodnocení zákazníků | Reálné zkušenosti | `https://www.credit1.cz/review/` | OK |
| 4 | Poradna a blog | Tipy a články | Informace o půjčkách | `https://www.credit1.cz/blog/default` | OK |

### H.2 Callouty (8)

1. Online žádost zdarma
2. Vyplnění za 2 minuty
3. Vyřízení i o víkendu
4. Předčasné splacení zdarma
5. Půjčka 1 000–100 000 Kč
6. Bez návštěvy pobočky
7. Žádost z pohodlí domova
8. Posouzení žádosti online

Všechny ≤25 znaků, kompliantně neutrální. Žádné „garantujeme", „schválíme každému".

### H.3 Structured snippets

Typ **„Service catalog"** s hodnotami:
- Online žádost
- Vyplnění online
- Vyřízení online
- Posouzení žádosti
- Bez návštěvy pobočky
- Předčasné splacení zdarma

---

## I. Bidding a rozpočet

| Položka | Hodnota |
|---|---|
| Strategie | `MAXIMIZE_CONVERSIONS` s `target_cpa_micros` |
| Target CPA | **195 Kč** (195 000 000 micros) |
| Denní rozpočet | **5 000 Kč** (5 000 000 000 micros) |
| Delivery | STANDARD |
| Sdílený budget | NE (dedikovaný) |
| Customer acquisition | `OPTIMIZE_ACQUISITION` (rovnocenné nové/stávající) |

**Poznámka k tCPA:** target CPA 195 Kč je výchozí dle promptu. Realistický **dosažitelný CPA** záleží na kvalitě konverze (krok 2 registrace) a poměru kliknutí → konverze. Pokud po 14 dnech bude **vykázané CPA výrazně nad target**, doporučím zvážit:
- vypnout PHRASE keywords s nízkým konverzním poměrem,
- zúžit ad groups (E.6, E.8, E.9 = rizikové) na pouhý priority subset,
- až teprve potom navrhnout úpravu target CPA (vždy s tvým schválením).

---

## J. Kontrola konverzí

| Kontrola | Stav | Poznámka |
|---|---|---|
| Existuje primární konverze pro registraci/lead? | ✅ | `credit1_step2` (ID 7601130703), category SIGNUP |
| Je primární a započítává se do conversions metric? | ✅ | `primary_for_goal=True`, `include_in_conversions_metric=True` |
| Je customer-level goal pro SIGNUP biddable? | ✅ | `customer_conversion_goal: SIGNUP / WEBSITE / biddable=True` |
| Je vhodné pro automated bidding? | ✅ | ano (SIGNUP + biddable + primary) |
| Měření aktivní na doméně credit1.cz? | ⚠️ | **Nelze přes API ověřit** — vyžaduje kontrolu Tag Assistant / GTM. **Doporučuju ověřit ručně** před spuštěním. |

**Otevřená otázka:** konverze se jmenuje `credit1_step2` — to typicky znamená *„krok 2 registračního funnelu"*. **Je to ten správný hluboký lead** (odeslaný formulář), nebo jen mezikrok? Pokud je to mezikrok a nikoli „submit", bidding bude optimalizovat na měkčí signál a kvalita leadů může klesat. **Vyjasni prosím** před spuštěním.

---

## K. API validateOnly plán

### K.1 Logická struktura objektů (pořadí vytváření)

```
1. CampaignBudget       ───────► budget_resource_name
                                  │
2. Campaign             ◄─────────┘ (campaign_budget = budget_resource_name)
   └─► campaign_resource_name
        │
3. CampaignCriterion[]  ◄─── lokalita CZ (geoTargetConstants/2203)
        │            ◄─── jazyk CZ (languageConstants/1021)
        │            ◄─── negative keywords (campaign-level), F.1-F.8
        │
4. AdGroup[9]           ◄─── jeden per sestava (D)
   └─► ad_group_resource_name[1..9]
        │
5. AdGroupCriterion[]   ◄─── keywords per ad group (E.1-E.9), match types
        │
6. AdGroupAd[9]         ◄─── RSA per ad group (G.1-G.9)
        │
7. Assets (kampaň-level)
   ├─► SitelinkAsset[4] (H.1)
   ├─► CalloutAsset[8] (H.2)
   └─► StructuredSnippetAsset[1] (H.3)
8. CustomerAssetSet / CampaignAsset (link)
```

### K.2 Dočasná resource_names (pro validate batch)

V SDK se používají negativní integer IDs jako `customers/{cid}/campaigns/-1`, `customers/{cid}/campaignBudgets/-1`, atd. Tím lze v jednom `mutate` request odkazovat objekty, které ještě nemají reálné ID.

### K.3 Co lze validovat v jednom requestu

Google Ads API podporuje **batch mutate** přes `GoogleAdsService.Mutate` (jeden customer, více operations). Pro tento návrh logicky:

- **Batch 1 (struktura kampaně)**: CampaignBudget + Campaign + CampaignCriterion (location, language)
- **Batch 2 (ad groups)**: AdGroup × 9
- **Batch 3 (criteria + ads)**: AdGroupCriterion (positives + negatives) + AdGroupAd × 9
- **Batch 4 (assety)**: Asset + AssetLink

Důvod více batchů: některé operace mají odkazy mezi sebou. Validate-only batch validuje konzistenci jen v rámci batche.

### K.4 Rizika z validate_only

- **POLICY_VIOLATION** na konkrétních RSA assetech — pokud Google vyhodnotí text jako problematický (typicky finanční segment).
- **DUPLICATE_KEYWORD** napříč ad groups, pokud se KW překryjí.
- **TOO_LONG** na headline/description, pokud podcení čítání diakritiky.
- **MISSING_BUDGET** pokud campaign odkazuje na budget, který v batch ještě neexistuje (špatné pořadí operations).

### K.5 Implementační poznámka

Vytvoření samotného kódu (`scripts/create_c1_search_campaign.py`) je **dalším krokem**, který udělám až po tvém schválení tohoto návrhu. Skript:
- Použije `lib/guard.assert_allowed_customer(5726766523)` a `assert_allowed_login(2373464403)`.
- Postavá všechny operations.
- Spustí MUTATE s `validate_only=True`.
- Zapíše do `logs/audit.jsonl` jak request, tak response_status.
- Vrátí strukturovaný výpis: které operations validovaly, které selhaly, s rozumnou chybovou zprávou.
- **Nikdy** v jediném běhu neudělá ostrou změnu — pro `validate_only=False` musí být skript spuštěn s explicitním flagem `--apply` *a* po kontextovém potvrzení.

---

## L. Co musíš potvrdit před ostrým vytvořením

Než cokoli pošlu do API (i jako validate-only), potřebuju tvoje OK na:

1. **Schválení návrhu jako celku** (struktura, klíčová slova, texty, assety).
2. **Konverzní akce** `credit1_step2`: je to skutečný „submit registration", nebo mezikrok? Pokud mezikrok, je vhodné, aby se na něj optimalizovalo bidding? (Klíčová otázka pro kvalitu leadů.)
3. **Ad groups E.6, E.8, E.9** (Na OP / Do výplaty / Bez poplatku) — chceš je rovnou ve startovní sadě, nebo je odložit jako fázi 2 po stabilizaci hlavních AG (E.1–E.5, E.7)?
4. **Konkurence v F.6** (negativní KW) — chceš tam brand konkurence jako negativa, nebo budeš chtít později brand-bidding kampaň proti nim?
5. **Pinning RSA assetů** — nyní bez pinningů (volná rotace). OK?

Po odsouhlasení napíšu `scripts/create_c1_search_campaign.py`, pustím **validate_only=True** a teprve po tvém potvrzení **z výsledku validace** dáme ostré vytvoření kampaně.

---

## M. Finální doporučení

**Spustit jako PAUSED** v této pevné posloupnosti:

1. ✅ Schválit tento návrh (komentáře / úpravy).
2. ⏳ Vyjasnit `credit1_step2` (sekce J).
3. ⏳ Napsat skript `create_c1_search_campaign.py`.
4. ⏳ Spustit validate_only — analyzovat výsledek (policy issues, conflicts).
5. ⏳ Tvoje potvrzení k ostrému vytvoření.
6. ⏳ Spustit ostré vytvoření → kampaň v účtu jako **PAUSED**.
7. ⏳ Před ENABLE: vizuální kontrola v Google Ads UI (preview RSA, sitelinky, callouty, struktura).
8. ⏳ Tvoje potvrzení k ENABLE → nasdílíme aktivaci.

> Veškeré API změny zalogují `logs/audit.jsonl` (i validate_only běh). Žádné mazání. Rozpočet 5 000 Kč/den a tCPA 195 Kč se nezvyšují bez tvého souhlasu.
