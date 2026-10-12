---
layout: default
lang: cs
title: Pro učitele
permalink: /cs/pro-ucitele/
description: Jednorázová ukázka do výuky, při které lidé, skriptovaní boti a LLM agent řízený harnessem hrají v jednom světě najednou, aby studenti rozdíl viděli naživo. Cílová skupina, vzdělávací cíle, průběh hodiny a podmínky nasazení.
translations:
  en: /educators/
---

Tato stránka popisuje pedagogickou stranu projektu Cell agents jako
jednorázovou školní ukázku. Cell agents je otevřený vzdělávací
projekt, ve kterém spolu v multiplayerové hře hrají lidé,
skriptovaní boti a LLM agenti. Celý projekt je volně upravitelný
a rozšiřitelný podle konkrétních výukových potřeb.

Nejde o to, aby studenti během hodiny AI programovali, ale aby **ve
stejném světě naráz viděli tři druhy hráčů** a dokázali je od sebe
odlišit podle toho, jak se pohybují.

## Cílová skupina a formát

Školní ukázka pro studenty ve věku 12 až 15 let. Prezentační slot
trvá 15 až 20 minut, počítáno s cca 25 účastníky, každý na svém
notebooku nebo mobilu s připojením k internetu. Studenti se do hry
připojují jako lidští hráči. Učitel pouští na projektor skriptované
boty a LLM agenta. Projekt zůstává k dispozici jako veřejný
repozitář, takže si studenti mohou hrát dál doma, upravovat kód
a prohlubovat porozumění. Nasazené herní prostředí dál běží na
cellagents.dev.

## Hlavní myšlenka

Projekt forkuje open source hru `agar.io-clone`, klon populární
prohlížečové hry `agar.io`, ve které se buňka pohybuje po mapě,
sbírá jídlo, dělí se a požírá menší hráče. Vyhrává ten, kdo přežije
a vyroste nejvíc.

Hra je zvolená tak, aby byla veřejně známá, zvládla ukázku pro
velkou skupinu a aby existovala otevřená verze, s níž mohou
studenti dál experimentovat bez licenčních starostí. Známost hry je
záměrná: studenti nemusejí přemýšlet, jak hra funguje, a mohou se
soustředit jenom na to, **jak se v témže světě chovají různé druhy
hráčů**, přičemž původní hra je kdykoli po ruce pro srovnání.

### Co hodina záměrně *neučí*

Je dobré říct nahlas, co je **mimo rozsah**, aby očekávání seděla.

- Hodina neučí, jak trénovat modely. Používáme hotové modely třetích
  stran.
- Neučí žádný konkrétní programovací jazyk. Repozitáře jsou v
  TypeScriptu, ale celá pointa na syntaxi nestojí ani nepadá.
- Studenti během této hodiny nepíší prompty. LLM agent běží na
  předem připravené konfiguraci, aby se ukázka vešla do slotu.
  Psaní promptů je přirozené pokračování doma.
- Není to kurz bezpečnosti, byť se dotkne jednoho jejího principu
  (důvěra na rozhraní klient-server).

## Co si studenti odnesou

### Základní mentální model

**Skoro cokoli, co existuje ve světě softwaru, se dá napojit na
umělou inteligenci a nechat model, aby to řídil.** `agar.io` o AI
nic neví. Nebylo pro AI napsáno. Přesto ho můžeme ovládat jazykovým
modelem, aniž bychom jeho kód výrazně měnili, a stejně snadno na něj
můžeme napojit skriptovaného ne-AI bota, nebo si prostě sednout a
hrát sami. Serveru je to jedno; vidí jen hráče. Totéž platí pro
emailové klienty, kalendáře, editory, platební systémy, roboty:
pro cokoli, co má rozhraní.

Tenhle vhled přežije déle než jakákoli konkrétní technologie, co si
student zapamatuje.

### Dílčí koncepty, kterých se studenti dotknou

1. **Tři druhy hráčů, jedna hra.** Lidé, deterministicky
   skriptovaní boti a LLM agent sdílejí tutéž mapu ve stejnou chvíli.
   Herní server je všechny vidí jako běžné klienty se jménem. Vedle
   sebe na projektoru je styl každého z nich rozeznatelný: lidé
   váhají, boti jsou předvídatelní, LLM reaguje na nové situace, ale
   viditelně jiným tempem.
2. **LLM je agent, ne chatbot.** Model neodpovídá, nýbrž ve smyčce
   rozhoduje a jedná. Vybere, co udělá, udělá to a sleduje, co se
   stalo.
3. **Skriptovaní boti jsou laťka.** Bez AI naši boti stále hrají,
   podle ručně vyladěných pravidel. Vidět je v akci dělá jasnějším,
   co vlastně LLM přidává a kde naopak klopýtá.
4. **Nástroje jako jazyk mezi AI a světem.** Model nemá ruce. Má
   seznam funkcí, které může zavolat. To, co může ovlivnit, je přesně
   to, co mu nabídneme.
5. **MCP (Model Context Protocol) jako průmyslový standard.**
   Studenti vidí, že existuje otevřený protokol, kterým se modely
   napojují na nástroje, a že stejný protokol používají reálné
   produkty jako Claude Desktop, Cursor, Zed. To, co tu stavíme,
   není hračka ani imitace. Je to tentýž mechanismus.
6. **Harness jako prostředí, ve kterém model žije.** Model sám o sobě
   je jen funkce. Teprve zabalený do smyčky, paměti a nástrojů vzniká
   něco, co se chová jako agent. Ve třídě studenti vidí jeden
   konkrétní harness na projektoru; doma si mohou pustit svůj.
7. **Latence není abstraktní.** V běžném chatu si čekání
   nevšimnete. Když ale přežití buňky závisí na odpovědi, čas reakce
   se stává hmatatelným strategickým rozhodnutím, obzvlášť vedle
   skriptovaného bota, který reaguje každý tick.
8. **Hranice důvěry mezi klientem a serverem.** Některé hodnoty si
   klient počítá sám a serveru je jen hlásí. Server se musí
   rozhodnout, jestli klientovi věří. Studenti si uvědomí, že
   nepoctivý klient může serveru lhát a že je to obecný problém, ne
   zvláštnost naší hry.

### Trvalá hodnota

1. **Odvaha napojit AI na existující systémy.** Pochopení, že je to
   překvapivě přímočará věc, otevírá dveře k vlastním pokusům.
2. **Slovník.** Pojmy jako nástroj, agent, harness, MCP, bot,
   hranice důvěry přestávají být cizí. Student je potkává v médiích,
   dokumentaci, tutoriálech a ví, kam je zařadit.
3. **Cit pro to, co AI přidává a co ne.** Když viděli skriptovaného
   bota a LLM sdílet jednu mapu, mají studenti konkrétní intuici,
   kde si model zaslouží svůj chléb a kde je prostý kód rychlejší,
   levnější a spolehlivější.
4. **Zážitek, že programovat AI je dostupné.** Celý projekt má jasné
   README v každém repozitáři, komentáře v jednoduché angličtině
   a návody na spuštění i úklid doma. Student s minimální zkušeností
   má šanci většině porozumět.

## Průběh hodiny

### Před hodinou

Učitel se už dřív zmínil o tom, co je jazykový model, nebo mají
studenti sami zkušenost s ChatGPT a podobnými nástroji.

### Prvních 5 minut: úvod

Učitel ukáže hru na projektoru a pojmenuje tři hráče, kteří si
tohle kolo rozdělí:

- **Lidé** (studenti, za chvíli).
- **Skriptovaní boti**, ručně vyladění, bez AI uvnitř.
- **LLM agent**, který běží přes harness a volá hru přes MCP.

Zdůrazní dva body:

- Hra sama o sobě o AI nic neví. AI je **napojená zvenčí**, přes
  tentýž standardní protokol, který používají skutečné produkty.
- Z pohledu hry všichni tři vypadají stejně. Je to závod člověka,
  skriptu a modelu.

### Prostředních 10 minut: zápas

Studenti otevřou odkaz, zadají jméno a připojí se. Skriptovaní boti
už ve hře jsou. LLM agent je puštěn na projektoru tak, aby byl
viditelný jeho log volání nástrojů.

Během zápasu učitel komentuje tři vrstvy toho, co se děje:

- **Jak hrají lidé.** Váhání, riziko, kamarádi, co se spolčují.
- **Jak hrají skriptovaní boti.** Hodinkově přesné honby, předvídatelné
  útěky, žádná překvapení.
- **Jak hraje LLM.** Log na projektoru ukazuje každý cyklus
  `pozoruj → rozhodni → jednej`. Komentář typu: *"Všimněte si, že
  model se tady zastavil na celou sekundu. Vedlejší bot se v té
  mezeře stihl pohnout třikrát. Model udělal lepší dlouhodobé
  rozhodnutí, ale zaplatil za ně latencí."* Abstraktní princip tak
  dostává konkrétní obraz na obrazovce.

Rané fáze jsou volné: po smrti hráč respawnuje menší. Pak nastává
**sudden death**, kde další smrt je definitivní. Z plné místnosti
nakonec zbyde jediný přeživší, nebo zápas ukončí časový limit. Kdo
vyhraje (člověk, bot nebo LLM) je méně důležité než to, že se každý
z nich ukázal v akci.

### Posledních 5 minut: debrief

Učitel zvýrazní klíčové body:

- Celý systém je lepidlo mezi pár nezávislými komponentami, které
  spolu mluví standardními protokoly.
- MCP protokol, který jsme použili, je tentýž, který třeba velké AI
  firmy používají k integraci modelů s nástroji ve světě.
- Skriptovaní boti a LLM nejsou protiklady; jsou to různé nástroje
  pro různé úkoly. Studenti teď viděli oboje.
- To, že jsme hráli "férově", bylo naše rozhodnutí. Server
  nekontroluje, co klient hlásí. **To je obecný princip všech API
  na světě.**
- Doma si kdokoli může stáhnout Claude Desktop, napojit ho na náš
  herní server stejným protokolem a hrát naplno. Repozitář je
  veřejný, návod bude v README.

## Praktické podmínky

- Každý účastník potřebuje zařízení s moderním prohlížečem
  a připojením k internetu.
- Herní server, skriptované boty i LLM harness pouští učitel nebo
  organizátor hodiny. Studenti nic neinstalují.
- Spotřebu modelu během hodiny platí provozovatel.
- Po hodině server dál běží, aby si studenti mohli zahrát doma.
  Repozitář a návody na samostatné spuštění stacku zůstávají
  dostupné.

## Rozšíření, pokud zbude čas nebo jako domácí úkol

- **Napojení Claude Desktopu nebo jiného klienta.** Student si
  nainstaluje hotového AI klienta, přidá do konfigurace náš MCP
  server a hraje přímo z chatu.
- **Psaní promptu pro LLM agenta.** Student si pustí harness
  a iteruje nad strategií, kterou model používá.
- **Ladění skriptovaných botů.** Boti mají pár osobnostních
  regulátorů (agresivita, ochota dělit se, zbabělost). Student
  upraví konfigurační soubor a sleduje, jak se zápas mění.
- **Experimentování s podváděním.** Student zkoumá, kde server
  klientovi věří a jak toho lze zneužít. Je to bezpečný prostor;
  hra vydrží cokoli.
- **Změna nástrojů.** Pokročilý student přidá do MCP serveru vlastní
  nástroj, například pro komunikaci mezi agenty, a sleduje, co se
  stane.
- **Nové aplikace.** Nejpokročilejší student zkusí znalost využít
  tak, že si postaví vlastní MCP server, kterým ovládne jinou hru,
  program, web nebo něco, co sahá do fyzického světa.
