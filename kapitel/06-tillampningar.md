# Kapitel 6: Tillämpningar och val av område

> **I det här kapitlet lär du dig**
> - hur programmering används inom sex olika områden
> - vad som är gemensamt och vad som skiljer dem åt
> - hur du väljer område och verktyg för ett eget projekt
> - hur du räknar på tid och data i spel och bilder

Samma variabler och loopar finns i ett spel och i styrningen av en industrirobot. Ändå ser programmen helt olika ut, eftersom kraven är olika. Spelet får hacka lite, men en maskin får inte missa en nödstopp. När du förstår de här skillnaderna kan du också välja område och verktyg på ett medvetet sätt.

## Sex områden

| Område | Vad som är särskilt viktigt | Vanliga verktyg |
|---|---|---|
| **Industriell programmering** | Tillförlitlighet, säkerhet, exakt tid | PLC, robotspråk |
| **Spelprogrammering** | Känsla, bildfrekvens, många objekt samtidigt | Spelmotorer som Godot och Unity |
| **Administrativa system** | Korrekta data, behörigheter, spårbarhet | Databaser, webbramverk |
| **Pedagogiska program** | Att användaren lär sig, återkoppling | Webb, appar |
| **Applikationsutveckling** | Användarupplevelse, att fungera på många enheter | Webb, mobilramverk |
| **Bild, ljud och film** | Stora datamängder, snabb beräkning | Bibliotek för signalbehandling |

### Industriell programmering

Styrningen av maskiner görs ofta med en **PLC**, en industridator byggd för att klara tuff miljö. En PLC arbetar i en **scan-cykel**: läs alla ingångar, kör programmet, skriv alla utgångar, börja om. Cykeln upprepas hela tiden. Det gör systemet förutsägbart, och förutsägbarhet är ett krav när en felaktig utgång kan skada någon. Säkerhetsfunktioner som nödstopp hanteras därför med särskild hårdvara och särskilda regler, inte med vanlig programkod.

### Spelprogrammering

Ett spel kör en **spelloop**: läs vad spelaren gör, uppdatera världen, rita bilden, och börja om, många gånger per sekund. Se exemplet längre ned.

### Administrativa system

Bokningar, lager och register bygger på att skapa, läsa, uppdatera och ta bort poster i en databas. Arbetet kallas ofta **CRUD**, efter de engelska orden för de fyra operationerna. Det svåra är sällan koden, utan att datan alltid ska vara rätt och att bara rätt personer får se den.

### Pedagogiska program

Här är målet att någon ska lära sig något. Ett program som bara rättar svaret ger liten effekt jämfört med ett som förklarar varför svaret blev fel. Det är ett bra exempel på att området styr vad som är en bra lösning.

### Applikationsutveckling

En app består ofta av ett gränssnitt och en server som kommunicerar genom ett **API**, ett bestämt sätt för program att prata med varandra. Delarna kan bytas ut var för sig. Tillgänglighet, alltså att appen fungerar för personer med olika behov, hör också hit.

### Bild, ljud och film

Media är stora mängder tal. En bild är ett rutnät av **pixlar**, och varje pixel består av tal för rött, grönt och blått. Ljud är en lång följd av mätvärden.

## Genomräknat exempel: spelets fart

Ett spelobjekt ska röra sig 200 pixlar per sekund. En dator ritar 60 bilder per sekund, en annan bara 30. Lägger du till ett fast antal pixlar per bild kommer spelet att gå dubbelt så fort på den snabbare datorn.

Lösningen är att multiplicera med **delta time**, tiden sedan förra bilden:

```python
def ny_position(position, fart, delta_time):
    return position + fart * delta_time

position_60 = 0.0
for _ in range(60):
    position_60 = ny_position(position_60, 200, 1 / 60)

position_30 = 0.0
for _ in range(30):
    position_30 = ny_position(position_30, 200, 1 / 30)

print(round(position_60), round(position_30))
```

Per bild flyttas objektet 200 · 1/60 ≈ 3,33 pixlar på den snabba datorn och 200 · 1/30 ≈ 6,67 pixlar på den långsamma. Efter en sekund har båda kommit 200 pixlar, och programmet skriver ut `200 200`.

## Genomräknat exempel: hur stor är en bild?

Hur mycket minne tar en okomprimerad bild i upplösningen 1920 × 1080, med en byte per färgkanal?

1920 · 1080 · 3 = 6 220 800 byte, cirka 6,2 MB.

Som film med 30 bilder per sekund blir det 6 220 800 · 30 ≈ 187 MB per sekund. Därför komprimeras film nästan alltid, och därför måste den som arbetar med media tänka på prestanda.

Så omvandlas en pixel till gråskala. Ögat upplever grönt som ljusare än blått, så färgerna väger olika mycket:

```python
def gra(r, g, b):
    return round(0.299 * r + 0.587 * g + 0.114 * b)

print(gra(200, 100, 50))
```

Svaret blir 0,299 · 200 + 0,587 · 100 + 0,114 · 50 = 124,2, som avrundas till `124`. Vikterna kommer från en vanlig standard för videosignaler.

## Så väljer du område och verktyg

När du ska välja till ett eget projekt, ställ de här frågorna:

1. **Vad vill du lära dig?** Projektet ska vara svårt på rätt sätt.
2. **Vem är användaren?** En enkel webbsida kan vara rätt när ingen behöver en app.
3. **Vilka är kraven?** Måste det fungera utan nät? Är det känsliga data?
4. **Vad finns redan?** Att återanvända ett bibliotek är ofta klokare än att skriva allt själv.
5. **Hur mycket tid har du?** En ny motor och ett nytt språk samtidigt är mycket att lära sig.

Ett sätt att jämföra alternativ är en enkel tabell där varje alternativ får poäng för varje fråga. Det viktiga är inte siffrorna utan att du kan motivera ditt val i efterhand, och förklara vad du valde bort.

## Begrepp

- **PLC:** en industridator som styr maskiner.
- **Scan-cykel:** en PLC:s upprepade arbetsgång: läs in, kör, skriv ut.
- **Spelloop:** spelets upprepade cykel av indata, uppdatering och ritning.
- **Delta time:** tiden sedan förra bilden, som gör att rörelsen blir oberoende av bildfrekvensen.
- **CRUD:** att skapa, läsa, uppdatera och ta bort poster.
- **API:** ett bestämt sätt för program att kommunicera med varandra.
- **Pixel:** en punkt i en bild, oftast beskriven med tre färgvärden.

## Testa dig själv

1. Varför är förutsägbarhet så viktig i industriell programmering?
2. Varför ska nödstopp inte hanteras med vanlig programkod?
3. Vad händer i ett spel om du inte använder delta time?
4. Varför är CRUD enkelt att koda men svårt att göra rätt?
5. Varför måste film komprimeras?
6. Vad är skillnaden mellan ett pedagogiskt program som rättar och ett som förklarar?
7. Vilka frågor hjälper dig att välja verktyg?

## Koppling till projektet

I [Önskeprojekt](../projekt/onskeprojekt/README.md) väljer du område och verktyg själv. Frågorna ovan hjälper dig att göra och motivera det valet.

## Läs mer

- [Godots dokumentation](https://docs.godotengine.org/en/stable/)
- [MDN: webbteknik](https://developer.mozilla.org/en-US/docs/Web)
- [W3C: introduktion till tillgänglighet](https://www.w3.org/WAI/fundamentals/accessibility-intro/)

[← Föregående](05-problemlosning-och-test.md) | [Innehåll](../README.md) | [Nästa →](07-forstudien.md)
