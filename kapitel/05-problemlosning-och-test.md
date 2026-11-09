# Kapitel 5: Problemlösning, test och felsökning

> **I det här kapitlet lär du dig**
> - ett arbetssätt för att lösa programmeringsproblem steg för steg
> - hur du testar din kod automatiskt
> - hur du felsöker systematiskt
> - vilka gränser det finns för vad ett program kan göra

Skillnaden mellan en van och en ovan programmerare är sällan att den vana gör färre fel. Den vana hittar felen snabbare, och det beror på arbetssätt mer än på talang. Här får du ett arbetssätt du kan använda på alla problem i kursen.

## Fem steg

1. **Förstå problemet.** Kan du säga med egna ord vad som ska hända? Skriv upp ett par exempel med indata och förväntat svar.
2. **Dela upp det.** Vilka delar kan du lösa var för sig?
3. **Skriv pseudokod.** **Pseudokod** är en beskrivning på vanlig svenska, men med programmets struktur.
4. **Koda en del i taget.** Kör ofta, så att du vet var ett nytt fel kommer ifrån.
5. **Testa och granska.** Stämmer svaret för de exempel du skrev upp? Vad händer i extremfallen?

## Genomgånget exempel: hitta en ledig tid

Problemet: en lokal har bokningar under en dag. Du vill hitta den första lediga tiden som räcker för ett möte. Tiderna anges i minuter från midnatt, så 09:00 blir 540.

Exempel på väntat svar: om lokalen är bokad 540–600 och 630–660, och dagen börjar 540, så är första lediga tiden för ett 30 minuters möte 600.

Pseudokod:

```text
sätt tid till dagens början
gå igenom bokningarna i tidsordning
    om det finns tillräckligt med plats före bokningen: svara tid
    annars flytta tid till bokningens slut, om den är senare
efter sista bokningen: finns plats före dagens slut? svara tid
annars finns ingen ledig tid
```

```python
def forsta_lediga(bokningar, langd, dag_start, dag_slut):
    """Returnerar första lediga starttid, eller None om inget ryms."""
    tid = dag_start
    for start, slut in sorted(bokningar):
        if start - tid >= langd:
            return tid
        tid = max(tid, slut)
    if dag_slut - tid >= langd:
        return tid
    return None

assert forsta_lediga([(540, 600), (630, 660)], 30, 540, 720) == 600
assert forsta_lediga([], 60, 540, 720) == 540
assert forsta_lediga([(540, 720)], 30, 540, 720) is None
assert forsta_lediga([(600, 700), (620, 650)], 30, 540, 720) == 540
assert forsta_lediga([(540, 700), (600, 650)], 30, 540, 720) is None
print("Alla tester gick igenom")
```

## Tester

Raderna med `assert` är **enhetstester**, små kontroller av en enskild funktion. `assert` stoppar programmet om villkoret inte stämmer.

Lägg märke till vilka fall som testas. Vanligt fall. Tom lista. Fullbokat. Sista testet är ett **extremfall** med en överlappning, där en bokning ligger inuti en annan. Det är därför raden `tid = max(tid, slut)` behövs: utan `max` skulle tiden flyttas bakåt till en bokning som slutar tidigare. Testa att ta bort `max` och kör koden, så ser du testet fallera.

Bra tester ska alltid täcka vanliga fall, tomma fall och gränsfall. När du hittar en bugg, skriv ett test som visar den innan du rättar den. Då kommer den inte tillbaka.

## Felsökning

När något är fel, börja inte ändra på måfå. Det gör att felet ofta förvandlas till två fel. Gör så här:

1. **Återskapa felet** så att du kan framkalla det när du vill.
2. **Läs felmeddelandet** noga, särskilt vilken rad det gäller.
3. **Formulera en gissning**: "Jag tror att listan är tom här."
4. **Testa gissningen** med en utskrift eller en debugger. En **debugger** låter dig köra programmet en rad i taget och se värdena.
5. **Ändra en sak i taget** och kontrollera resultatet.

## Versionshantering

**Git** är ett **versionshanteringssystem**, ett verktyg som sparar kodens historia så att du kan gå tillbaka. Varje sparad version kallas en **commit**. Spara ofta, med ett meddelande som säger vad du gjorde. Då kan du tryggt testa en idé, och om den inte fungerar går du tillbaka.

## Genomräknat exempel: spelar metoden roll?

Säg att du vill hitta ett värde i en sorterad lista med en miljon tal. Du kan gå igenom listan från början. Det är **linjär sökning**, och i värsta fall krävs en miljon jämförelser. Du kan också titta i mitten, kasta bort den halva som inte kan innehålla talet och upprepa. Det är **binär sökning**.

Varje jämförelse halverar antalet kvarvarande tal, så antalet steg blir ungefär log₂(n).

| Antal tal | Linjär, värsta fall | Binär, värsta fall |
|---|---|---|
| 1 000 | 1 000 | 10 |
| 1 000 000 | 1 000 000 | 20 |
| 1 000 000 000 | 1 000 000 000 | 30 |

Det som är värt att se: om listan växer tusen gånger behöver binär sökning bara tio steg till. Men metoden kräver att listan är sorterad, så det finns ett pris. Att jämföra strategier på det här sättet är just den värdering av metoder som kursen tar upp.

## Programmeringens begränsningar

Program kan räkna snabbt, men de har tydliga gränser.

**Datorn räknar inte exakt med decimaltal.** Prova själv:

```python
print(0.1 + 0.2)
print(0.1 + 0.2 == 0.3)
```

Du får `0.30000000000000004` och `False`, eftersom talen lagras binärt och 0,1 inte går att skriva exakt på det sättet. Jämför därför aldrig decimaltal med `==`, och använd heltal, till exempel ören, när pengar ska räknas.

**Vissa problem blir för stora.** Ska du testa alla ordningar för att besöka 30 städer är antalet möjligheter så stort att ingen dator hinner. Då får du nöja dig med en lösning som är tillräckligt bra.

**Vissa frågor går inte att avgöra.** Det är bevisat att inget program i allmänhet kan avgöra om ett godtyckligt annat program någonsin kommer att stanna. Det kallas **stopproblemet**.

**Programmet vet bara det du berättat.** Otydliga krav, dåliga data eller oförutsedda situationer går inte att lösa med mer kod.

## Begrepp

- **Pseudokod:** en beskrivning av en algoritm med vanliga ord.
- **Enhetstest:** en liten automatisk kontroll av en enskild del av koden.
- **Extremfall:** ett ovanligt eller yttersta fall som lätt missas.
- **Debugger:** ett verktyg för att köra programmet steg för steg.
- **Git och commit:** ett verktyg för versionshantering och en sparad version.
- **Linjär och binär sökning:** att gå igenom allt, respektive att halvera sökområdet.
- **Stopproblemet:** det bevisat olösbara problemet att avgöra om vilket program som helst stannar.

## Testa dig själv

1. Varför skriver du exempel med väntat svar innan du börjar koda?
2. Vad kontrollerar det sista testet i exemplet, och varför är det viktigt?
3. Varför är det dåligt att ändra flera saker samtidigt när du felsöker?
4. Vad krävs för att binär sökning ska fungera?
5. Hur många steg behöver binär sökning ungefär för en miljon tal?
6. Varför ska du inte använda `==` på decimaltal?
7. Vad betyder det att stopproblemet är olösbart?

## Koppling till projektet

Du använder de här arbetssätten direkt i [Önskeprojekt](../projekt/onskeprojekt/README.md). Dessutom är dina testresultat och dina val av strategi ett bra underlag när du ska reflektera över och värdera ditt arbete.

## Läs mer

- [Python: unittest](https://docs.python.org/3/library/unittest.html)
- [Python: flyttalsaritmetik](https://docs.python.org/3/tutorial/floatingpoint.html)
- [Pro Git, boken om Git](https://git-scm.com/book/en/v2)

[← Föregående](04-fran-ide-till-plan.md) | [Innehåll](../README.md) | [Nästa →](06-tillampningar.md)
