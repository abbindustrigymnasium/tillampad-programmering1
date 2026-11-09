# Kapitel 3: Säker kod

> **I det här kapitlet lär du dig**
> - varför du aldrig kan lita på indata från användaren
> - hur du skriver databasfrågor som inte går sönder av konstiga tecken
> - hur lösenord ska lagras av den som bygger en tjänst
> - hur du håller hemligheter borta från koden

> **Det här kapitlet handlar om att bygga skydd.** Kör exemplen på din egen dator mot din egen data. Regeln från [kapitel 2](02-cybersakerhet-grunder.md) gäller fortfarande: testa bara det du äger eller har tillstånd att testa.

De flesta säkerhetsbrister är inte smarta. Det är vanliga programmeringsfel: en kontroll som saknas, ett lösenord som ligger kvar i koden, en fråga som byggs ihop på fel sätt. Det är goda nyheter, för det betyder att du kan undvika dem med några enkla vanor.

## Tre principer

**Lita inte på indata.** Allt som kommer utifrån kan innehålla något annat än du väntar dig: formulärfält, filer, nätverksmeddelanden. Kontrollera det på servern, eftersom kontroller i webbläsaren går att kringgå.

**Minsta möjliga behörighet.** Ett program eller konto ska bara kunna göra det det behöver. Den som bara ska läsa data ska inte kunna radera den.

**Försvar i flera lager.** Utgå från att ett skydd kan brista. Då ska nästa skydd fortfarande fungera, ungefär som att ha både lås och larm.

Organisationen OWASP sammanställer en lista över de vanligaste säkerhetsriskerna i webbapplikationer. Den är en bra utgångspunkt när du bygger något på riktigt.

## Exempel 1: kontrollera indata

Det säkraste sättet är att beskriva vad som är **tillåtet** och avvisa allt annat. Det kallas en tillåtelselista. Motsatsen, att försöka räkna upp allt farligt, misslyckas nästan alltid eftersom något glöms bort.

```python
import re

ANVANDARNAMN = re.compile(r"[a-z0-9_]{3,20}")

def giltigt_anvandarnamn(text):
    """Tillåter bara små bokstäver, siffror och understreck, 3-20 tecken."""
    return ANVANDARNAMN.fullmatch(text) is not None

print(giltigt_anvandarnamn("anvandare_12"))
print(giltigt_anvandarnamn("ab"))
print(giltigt_anvandarnamn("anvandare 12"))
```

Körningen skriver ut `True`, `False` och `False`. Det andra namnet är för kort, och det tredje innehåller ett mellanslag.

## Exempel 2: parametrar i databasfrågor

En **databas** lagrar information i tabeller, och du ställer frågor till den med språket **SQL**. Felet många gör är att bygga ihop frågan med vanlig textsammanfogning. Då blandas instruktionen och informationen ihop, och databasen kan inte längre skilja på dem.

Du behöver ingen angripare för att se problemet. Efternamnet O'Brien innehåller en apostrof, och apostrofen avslutar texten i SQL. Med sammanfogning går frågan sönder. Samma fel kan i värsta fall göra att text från en användare tolkas som kommandon till databasen, och det kallas **SQL-injektion**.

Lösningen är **parametrar**: platshållaren `?` står i frågan och värdet skickas separat. Databasen behandlar då värdet som data, oavsett vilka tecken det innehåller.

```python
import sqlite3

db = sqlite3.connect(":memory:")
db.execute("CREATE TABLE elever (namn TEXT, poang INTEGER)")
db.execute("INSERT INTO elever VALUES (?, ?)", ("O'Brien", 42))

namn = "O'Brien"
rad = db.execute("SELECT poang FROM elever WHERE namn = ?", (namn,)).fetchone()
print(rad[0])
```

Körningen skriver ut `42`, och apostrofen är inget problem. Tumregeln är kort: stoppa aldrig in ett värde i en SQL-fråga med f-strängar eller `+`.

## Exempel 3: så lagras lösenord

En tjänst ska aldrig kunna läsa användarnas lösenord. I stället sparas ett **hashvärde**. En **hashfunktion** gör om text till ett värde på ett sätt som inte går att räkna baklänges. När du loggar in hashas det du skriver, och värdena jämförs.

Två saker till behövs. Ett **salt** är ett slumptal som är unikt för varje användare, så att två likadana lösenord får olika hashvärden. Hashfunktionen ska också vara **långsam med flit**, så att varje gissning kostar tid och minne. Vanliga snabba hashfunktioner passar därför inte, och du ska inte heller hitta på en egen.

```python
import hashlib
import hmac
import secrets

def hasha(losenord, salt=None):
    salt = salt or secrets.token_bytes(16)
    varde = hashlib.scrypt(
        losenord.encode(), salt=salt, n=2**17, r=8, p=1, maxmem=2**28
    )
    return salt, varde

def stammer(losenord, salt, sparat):
    _, varde = hasha(losenord, salt)
    return hmac.compare_digest(varde, sparat)

salt, sparat = hasha("en lång lösenordsfras")
print(stammer("en lång lösenordsfras", salt, sparat))
print(stammer("fel gissning", salt, sparat))
```

Körningen skriver ut `True` och `False`. Tabellen från kapitel 2 räknade med en miljard gissningar i sekunden. Med en långsam hashfunktion som den här kan angriparen bara pröva en bråkdel av det, för varje konto för sig. Exakt vilka inställningar som rekommenderas ändras över tid, så kontrollera OWASP:s aktuella råd när du bygger något som ska användas.

## Exempel 4: hemligheter hör inte hemma i koden

En **hemlighet** är allt som ger åtkomst, till exempel lösenord och API-nycklar. En **API-nyckel** är en kod som ger ett program tillgång till en tjänst. Läggs den i koden och koden hamnar på GitHub är den i praktiken offentlig, och den finns kvar i historiken även om du raderar den senare.

Läs i stället hemligheten från miljön och se till att filen där den ligger aldrig följer med till Git.

```python
import os

nyckel = os.environ.get("MIN_TJANST_NYCKEL")
if nyckel is None:
    print("Nyckeln saknas. Sätt miljövariabeln MIN_TJANST_NYCKEL.")
```

Lägg sedan filen `.env` i `.gitignore`. Har en hemlighet ändå läckt är det enda säkra att byta ut den, eftersom du inte kan veta vem som redan har sett den.

## Loggning och att märka när något händer

Ingen hinner stoppa allt. Därför ska ett system kunna upptäcka misstänkta mönster och visa vad som har hänt, som loggen i kapitel 1. Men logga aldrig lösenord eller känsliga uppgifter, för då skapar du en ny sårbarhet.

## Begrepp

- **Indata:** det som kommer in i programmet utifrån.
- **Tillåtelselista:** en beskrivning av vad som är tillåtet, där allt annat avvisas.
- **SQL:** språket för att ställa frågor till en databas.
- **SQL-injektion:** ett angrepp där indata tolkas som del av en SQL-fråga.
- **Parameter:** ett värde som skickas separat från SQL-frågan.
- **Hashfunktion:** en funktion som gör om data till ett värde som inte går att räkna baklänges.
- **Salt:** slumpdata som gör varje lösenords hashvärde unikt.
- **API-nyckel:** en kod som ger ett program tillgång till en tjänst.

## Testa dig själv

1. Varför räcker det inte att kontrollera indata i webbläsaren?
2. Varför är en tillåtelselista bättre än att lista det som är farligt?
3. Vad gör en platshållare i en SQL-fråga?
4. Varför sparas lösenord som hashvärden och inte i klartext?
5. Vad är saltets uppgift?
6. Varför ska en lösenordshash vara långsam?
7. Vad ska du göra om en API-nyckel råkar hamna på GitHub?

## Koppling till projektet

Kapitlet ger dig försvarssidan av [Cyberintro](../projekt/cyberintro/README.md). Vanorna är också värda att ta med till alla program du bygger i [Önskeprojekt](../projekt/onskeprojekt/README.md) och i [Förstudie + projekt + marknad](../projekt/forstudie-projekt-marknad/README.md).

## Läs mer

- [OWASP Top Ten](https://owasp.org/www-project-top-ten/)
- [OWASP: lagring av lösenord](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP: parametriserade frågor](https://cheatsheetseries.owasp.org/cheatsheets/Query_Parameterization_Cheat_Sheet.html)
- [Python: hashlib](https://docs.python.org/3/library/hashlib.html)
- [Python: secrets](https://docs.python.org/3/library/secrets.html)
- [Python: sqlite3](https://docs.python.org/3/library/sqlite3.html)

[← Föregående](02-cybersakerhet-grunder.md) | [Innehåll](../README.md) | [Nästa →](04-fran-ide-till-plan.md)
