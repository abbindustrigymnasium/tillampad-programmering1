# Kapitel 2: Cybersäkerhet: grunder, risk och etik

> **I det här kapitlet lär du dig**
> - vad informationssäkerhet går ut på och hur du beskriver den med tre begrepp
> - hur du bedömer och rangordnar risker
> - varför lösenordets längd betyder så mycket, med siffror
> - var gränsen går mellan lagligt och olagligt, och hur du agerar om du hittar en brist

> **Regeln som gäller i hela kursen.** Du testar bara system som du själv äger eller som du har uttryckligt tillstånd att testa, helst skriftligt. Att något är tekniskt möjligt betyder inte att det är tillåtet. Att vara nyfiken är ingen ursäkt i lagens mening.

Nästan allt du gör digitalt bygger på att någon annan har skyddat systemen bakom. Skolans betygsdata, din bank och sjukvårdens journaler behöver försvaras varje dag. Den som vill arbeta med programmering behöver förstå hur det går till, och det ansvar som följer med kunskapen.

## Tre mål: konfidentialitet, riktighet och tillgänglighet

Säkerhet handlar om att skydda information. Tre mål brukar användas för att beskriva vad det betyder.

| Mål | Betyder att | Exempel på när det brister |
|---|---|---|
| **Konfidentialitet** | bara rätt personer kan läsa informationen | En obehörig läser dina meddelanden |
| **Riktighet** | informationen inte ändras obemärkt | Någon ändrar ett belopp i en faktura |
| **Tillgänglighet** | informationen går att nå när den behövs | En tjänst ligger nere under en tentamen |

Målen kan krocka. Om du krypterar allt och tappar nyckeln är konfidentialiteten perfekt, men tillgängligheten borta. Säkerhetsarbete är därför en fråga om avvägningar.

## Hot, sårbarhet och risk

Tre ord blandas ofta ihop.

- Ett **hot** är något som kan orsaka skada, till exempel en person som vill stjäla inloggningsuppgifter eller en brand i serverrummet.
- En **sårbarhet** är en svaghet som gör skadan möjlig, till exempel en programvara som inte har uppdaterats.
- En **risk** är kombinationen av hur troligt något är och hur illa det blir om det händer.

Ett hot utan sårbarhet ger ingen skada. Därför lönar det sig att täppa till svagheter även när du inte vet vem som skulle kunna utnyttja dem.

## Vanliga hot och försvar

| Hot | Så går det till | Försvar |
|---|---|---|
| **Nätfiske** | Ett falskt meddelande lurar dig att lämna uppgifter | Kontrollera avsändaren, gå till tjänsten själv i stället för via länken |
| **Återanvända lösenord** | Ett läckt lösenord prövas på andra tjänster | Lösenordshanterare, unika lösenord |
| **Skadlig kod** | Program som skadar eller tar över datorn | Uppdateringar, försiktighet med filer |
| **Utpressningsvirus** | Filer krypteras och lösen krävs | Säkerhetskopior som ligger skilda från datorn |
| **Överbelastning** | En tjänst översvämmas av trafik | Kapacitet, filtrering, leverantörens skydd |

Tvåfaktorsautentisering, ofta kallat **MFA**, betyder att inloggningen kräver något mer än lösenordet, till exempel en kod från en app. Även om lösenordet läcker räcker det då inte för att komma in.

## Genomräknat exempel: en riskmatris

En enkel **riskmatris** ger varje risk två betyg från 1 till 5: sannolikhet och konsekvens. Multiplicerar du dem får du ett riskvärde som går att jämföra. Tabellen gäller en påhittad webbtjänst.

| Risk | Sannolikhet | Konsekvens | Riskvärde | Åtgärd |
|---|---|---|---|---|
| Ett återanvänt lösenord läcker | 4 | 4 | 16 | Inför MFA |
| Nätfiske mot en administratör | 4 | 3 | 12 | Utbildning, MFA |
| Servern kraschar utan kopia | 2 | 5 | 10 | Automatiska säkerhetskopior |
| En dator tappas bort | 2 | 3 | 6 | Kryptera disken |

Siffrorna är bedömningar, inte mätningar. Styrkan i metoden är att den tvingar dig att säga vad du tror och varför, så att andra kan säga emot. Den rangordnar också arbetet: börja uppifrån.

## Genomräknat exempel: hur långt är långt nog?

Antalet möjliga lösenord beror på hur många tecken eller ord som finns att välja mellan och hur långt lösenordet är. Tabellen utgår från lösenord som är **slumpmässigt valda**, och från en påhittad angripare som hinner pröva en miljard gissningar per sekund.

| Lösenord | Möjliga kombinationer | Tid att pröva alla |
|---|---|---|
| 8 små bokstäver a–z | 26⁸ ≈ 2,1 · 10¹¹ | cirka 3,5 minuter |
| 4 slumpvalda ord ur en lista på 7 776 | 7776⁴ ≈ 3,7 · 10¹⁵ | cirka 42 dygn |
| 5 slumpvalda ord ur samma lista | 7776⁵ ≈ 2,8 · 10¹⁹ | cirka 900 år |
| 12 tecken av a–z, A–Z och 0–9 | 62¹² ≈ 3,2 · 10²¹ | cirka 100 000 år |

Räkningen är enkel. För fyra ord: 7776⁴ ≈ 3 656 158 440 062 976 kombinationer. Delat med en miljard per sekund blir det omkring 3,66 miljoner sekunder, och delat med 86 400 sekunder per dygn blir det ungefär 42 dygn.

Två slutsatser. Ett extra ord gör enorm skillnad, eftersom antalet multipliceras med 7 776 varje gång. Och lösenord som människor själva hittar på är långt svagare än tabellen visar, eftersom människor väljer förutsägbart. Aktuella riktlinjer från den amerikanska standardmyndigheten NIST betonar längd och kontroll mot listor med kända lösenord snarare än krav på specialtecken. Hur lösenord ska lagras av den som bygger en tjänst tar vi upp i nästa kapitel.

## Lag och etik

I brottsbalken, 4 kap. 9 c §, finns brottet **dataintrång**. Den som olovligen skaffar sig tillgång till uppgifter som är avsedda för automatiserad behandling, eller olovligen ändrar eller förstör dem, kan dömas till böter eller fängelse. Ordet **olovligen** är nyckeln. Lagar ändras, så kontrollera den aktuella texten via länken nedan.

Det som skiljer ett säkerhetstest från ett brott är **tillståndet**. Därför gäller detta:

- Testa bara det du äger eller har uttryckligt tillstånd att testa, helst skriftligt och med tydlig avgränsning.
- Skolans nätverk, kompisars konton och webbsidor på internet ingår inte om du inte har fått uttryckligt tillstånd av den som ansvarar för dem, även om du bara vill se om det går.
- Vill du öva, använd miljöer som är gjorda för det. [picoCTF](https://picoctf.org/) och [OverTheWire](https://overthewire.org/wargames/bandit/) är två exempel. Läs deras regler först.
- Om du av en slump upptäcker en säkerhetsbrist: gå inte vidare, hämta inga uppgifter och prata med en vuxen du litar på eller med den som ansvarar för systemet. Många organisationer anger hur de vill ta emot sådana rapporter. Standarden för det kallas `security.txt`.

Personuppgifter skyddas dessutom av dataskyddsförordningen **GDPR**. En **personuppgift** är all information som kan kopplas till en levande person. Det gäller också om du bygger något litet och till synes oskyldigt.

Etik är inte bara regler. Den som kan säkerhet vet ofta hur skada görs, och väljer ändå att bygga skydd. Yrkesrollen bygger på att andra kan lita på det valet.

## Begrepp

- **Konfidentialitet, riktighet, tillgänglighet:** informationssäkerhetens tre mål.
- **Hot:** något som kan orsaka skada.
- **Sårbarhet:** en svaghet som gör skadan möjlig.
- **Risk:** sannolikheten för en skada vägd mot hur allvarlig den är.
- **Nätfiske:** falska meddelanden som lurar någon att lämna uppgifter.
- **MFA:** inloggning som kräver mer än ett lösenord.
- **Dataintrång:** brott där någon olovligen tar del av eller ändrar uppgifter i ett datasystem.
- **Personuppgift:** information som kan kopplas till en levande person.

## Testa dig själv

1. Ge ett eget exempel på ett brott mot riktigheten.
2. Förklara skillnaden mellan hot och sårbarhet.
3. Varför hjälper MFA även om lösenordet har läckt?
4. Varför är riskvärdet i en riskmatris en bedömning och inte en mätning?
5. Varför multipliceras antalet kombinationer med 7 776 för varje extra ord?
6. Varför gör tillstånd skillnad, om det du gör tekniskt sett är samma sak?
7. Vad gör du om du råkar upptäcka en säkerhetsbrist på en webbplats?

## Övningar

**Grund.** Gör en riskmatris för ditt eget digitala liv med minst fyra risker. Vilken åtgärd ger mest nytta?

**Grund.** Räkna ut hur många lösenord det finns med 6 siffror, och hur lång tid det tar att pröva alla med en miljard försök per sekund.

<details><summary>Svar</summary>

10⁶ = 1 000 000 kombinationer. 1 000 000 / 1 000 000 000 = 0,001 sekunder, alltså en millisekund.

</details>

**Vidare.** Räkna ut hur många gånger starkare ett lösenord på 10 små bokstäver är jämfört med ett på 8.

<details><summary>Svar</summary>

26¹⁰ / 26⁸ = 26² = 676 gånger fler möjliga lösenord.

</details>

**Utmaning.** Skriv ett kort regelverk på en halv sida för hur en klubb eller organisation ska kunna ge tillstånd till ett säkerhetstest. Vad måste stå i tillståndet för att alla ska veta vad som gäller?

## Koppling till projektet

Det här är bakgrunden till [Cyberintro](../projekt/cyberintro/README.md). Oavsett hur läraren lägger upp projektet gäller regeln om tillstånd, och riskanalysen återkommer i [Förstudie + projekt + marknad](../projekt/forstudie-projekt-marknad/README.md).

## Läs mer

- [Brottsbalken hos Sveriges riksdag](https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/brottsbalk-1962700_sfs-1962-700/)
- [IMY: det här gäller enligt GDPR](https://www.imy.se/verksamhet/dataskydd/det-har-galler-enligt-gdpr/)
- [CERT-SE, Sveriges nationella funktion för IT-incidenter](https://www.cert.se)
- [NIST: riktlinjer för autentisering](https://pages.nist.gov/800-63-4/sp800-63b.html)
- [RFC 9116 om security.txt](https://www.rfc-editor.org/rfc/rfc9116)

[← Föregående](01-programmering-som-verktyg.md) | [Innehåll](../README.md) | [Nästa →](03-saker-kod.md)
