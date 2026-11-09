# Kapitel 10: Konsekvenser och reflektion

> **I det här kapitlet lär du dig**
> - hur du reflekterar över och värderar dina val, metoder och resultat
> - hur programmering påverkar individ, samhälle och teknisk utveckling
> - vad hållbar utveckling, etik och genus har med kod att göra
> - hur du utvecklar ett resonemang från enkelt till välgrundat

Kod är aldrig neutral. Någon har bestämt vad programmet ska göra, vilka data det ska använda och vilka det är till för. De besluten får följder för människor som aldrig läser koden. Det här kapitlet handlar om att se de följderna, och om att granska sitt eget arbete ärligt.

## Reflektion över det egna arbetet

**Reflektion** är att tänka igenom vad som hände och vad det betyder. **Värdering** betyder att ta ställning till hur bra något var, och varför. En användbar arbetsgång har fyra steg.

| Steg | Fråga | Exempel |
|---|---|---|
| Beskriv | Vad gjorde jag? | Jag valde SQLite för att spara data. |
| Analysera | Varför blev det så? | Det krävde ingen server, men två användare kunde inte skriva samtidigt. |
| Värdera | Var det bra, och mätt med vad? | Bra för kraven vi hade, sämre om antalet användare skulle växa. |
| Förbättra | Vad gör jag annorlunda? | Jag skulle ha testat flera användare tidigare. |

Använd din projektlogg, dina testresultat och din tidsplan som underlag. Ett resonemang utan fakta blir lätt tyckande. Prata också om det som gick dåligt. Det är ofta där lärdomen finns.

## Att analysera konsekvenser

En enkel matris hjälper dig att se bredden. Fyll i den för ditt projekt.

| | Hållbar utveckling | Etik | Genus |
|---|---|---|---|
| **Individ** | Kan användaren behålla sin telefon längre? | Vilken data samlas om användaren? | Är användaren tänkt att vara en viss sorts person? |
| **Samhälle** | Vad går åt av energi och material? | Vem får nytta, och vem får skada? | Förstärks gamla mönster? |
| **Teknisk utveckling** | Kan systemet underhållas länge? | Vilka vanor skapas i branschen? | Vem är med och utvecklar? |

Hållbarhet har en ekologisk, en social och en ekonomisk sida. Den ekologiska kan handla om el och material. Den sociala handlar om vem som får vara med, och den ekonomiska handlar om vem som ska betala för att systemet fortsätter fungera.

### Hållbarhet: ett räkneexempel

Anta att en server drar 200 watt, dygnet runt, hela året. Det är ett påhittat exempel.

200 W · 24 h · 365 dygn = 1 752 000 Wh = 1 752 kWh per år.

```python
print(200 * 24 * 365 / 1000)
```

Enskilda program kan alltså dra mycket el om de körs ständigt. Beräkningar som tränar stora AI-modeller är särskilt energikrävande. Hur stor elförbrukningen i datacenter är totalt förändras snabbt, så läs aktuella uppgifter från [Internationella energirådet](https://www.iea.org/reports/energy-and-ai) om du behöver siffror. Programmerarens bidrag kan vara att effektivisera kod, undvika onödiga beräkningar och se till att gammal hårdvara fungerar längre.

### Etik

**Integritet** är rätten att ha kontroll över sina egna uppgifter. Enligt GDPR ska dataskydd byggas in från början, och den som bygger något bör inte samla mer än vad som behövs. EU har dessutom regler för AI som bland annat handlar om system med hög risk. Lagar ändras, så gå till källorna när du behöver veta exakt vad som gäller.

Fråga dig själv: vem har nytta av det här? Vem kan skadas? Kan det missbrukas? Skulle du vara bekväm med att berätta för användarna hur det fungerar? Kunskap om säkerhet från [kapitel 2](02-cybersakerhet-grunder.md) är ett bra exempel på teknik som kan användas både till att skydda och till att skada, och det är ditt val som avgör vilket.

### Genus, normer och vem som får vara med

**Genus** handlar om hur föreställningar om kön formas i samhället. **Normer** är oskrivna regler om vad som anses normalt. I teknik syns de bland annat i vilka som bygger systemen, vilka som testar dem och vems behov som räknas som självklara.

Ett känt exempel är forskningsprojektet Gender Shades. Forskarna Joy Buolamwini och Timnit Gebru granskade kommersiella system som klassificerar kön från ansiktsbilder, och fann att felen var betydligt vanligare för kvinnor med mörk hud än för män med ljus hud. Systemen var alltså sämst för dem som var minst representerade i träningsdata och testdata.

## Genomräknat exempel: när ett genomsnitt döljer problem

Ett påhittat ansiktsigenkänningssystem testas på 90 personer.

| Grupp | Antal testade | Fel | Felandel |
|---|---|---|---|
| A | 50 | 1 | 1 / 50 = 2,0 % |
| B | 40 | 12 | 12 / 40 = 30,0 % |
| Totalt | 90 | 13 | 13 / 90 ≈ 14,4 % |

Totalsiffran 14,4 procent säger inte hela sanningen. Den som bara redovisar genomsnittet missar att systemet är femton gånger sämre för grupp B (30 / 2 = 15). Tänk nu på vad det skulle innebära om systemet användes för att ge människor tillträde till något.

```python
grupper = {"A": (50, 1), "B": (40, 12)}
for namn, (testade, fel) in grupper.items():
    print(namn, round(fel / testade * 100, 1))
print("Totalt", round(13 / 90 * 100, 1))
```

Slutsatsen är att du bör dela upp dina testresultat på grupper när du testar något som ska användas av olika människor, och fundera på vilka grupper som saknas i dina data.

## Från enkelt till välutvecklat resonemang

Kursens kriterier beskriver att du ska analysera konsekvenser med allt mer utvecklade omdömen. Exakt hur din lärare bedömer det kan du fråga om. Så här kan ett resonemang växa, med loggning av användardata som exempel.

- **Enkelt:** Appen sparar vad användarna gör, och det kan vara dåligt för integriteten.
- **Utvecklat:** Appen sparar vad användarna gör. Det hjälper oss att hitta fel, men risken är att uppgifterna läcker eller används till annat än de samlades in för. Därför behöver vi begränsa vad som sparas.
- **Välutvecklat:** Vi behöver loggar för att felsöka, men vi behöver inte veta vem som gjorde vad. Genom att spara händelser utan koppling till användaren och radera dem efter en kort tid behåller vi det mesta av nyttan med mindre risk. Nackdelen är att vi inte kan hjälpa en enskild användare med ett problem i efterhand, och det får vi lösa på annat sätt. Valet gynnar den enskildes integritet, men kostar oss något i support.

Skillnaden ligger i att du går från att konstatera till att väga för- och nackdelar, se vem som påverkas och föreslå en lösning med dess kostnader.

## Begrepp

- **Reflektion:** att tänka igenom vad som hände och vad det betyder.
- **Värdering:** att ta ställning till hur bra något var, och utifrån vilka grunder.
- **Hållbar utveckling:** utveckling med hänsyn till ekologi, människor och ekonomi på lång sikt.
- **Integritet:** rätten till kontroll över sina egna uppgifter.
- **Genus:** föreställningar om kön som formas i samhället.
- **Norm:** oskriven regel om vad som anses normalt.
- **Träningsdata:** data som en AI-modell lär sig av.

## Koppling till projektet

Reflektion och konsekvensanalys hör till alla projekten: [Cyberintro](../projekt/cyberintro/README.md), [Önskeprojekt](../projekt/onskeprojekt/README.md), [Förstudie + projekt + marknad](../projekt/forstudie-projekt-marknad/README.md) och [Robotik: Sköldpaddan](../projekt/skoldpaddan/README.md). Använd matrisen när du planerar och skriver om dina resultat.

## Läs mer

- [Gender Shades, MIT Media Lab](https://www.media.mit.edu/projects/gender-shades/overview/)
- [Forskningsartikeln bakom Gender Shades](https://proceedings.mlr.press/v81/buolamwini18a.html)
- [IEA: Energy and AI](https://www.iea.org/reports/energy-and-ai)
- [Globala målen för hållbar utveckling](https://www.globalamalen.se/)
- [EU:s regler för AI](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)
- [IMY: inbyggt dataskydd](https://www.imy.se/verksamhet/dataskydd/det-har-galler-enligt-gdpr/inbyggt-dataskydd-och-dataskydd-som-standard/)

[← Föregående](09-robotik-ros2.md) | [Innehåll](../README.md)
