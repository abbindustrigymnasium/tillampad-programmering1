> **Under planering.** Läraren fastställer upplägg och krav för det här projektet.

# Robotik: Sköldpaddan

## Syfte

Projektet handlar om robotik med simulatorn turtlesim i ROS2, där du styr en sköldpadda på skärmen med program. Det delas med kursen Teknik Specialisering Nivå 1 (Smarta System), och den här sidan utgår från kursen Tillämpad programmering. Exakt upplägg är inte fastställt.

## Koppling till kursen

**Kriterier i vanliga ord.** Skolpunkten anger ännu inte vilka av kursens kriterier projektet ska bedömas mot. Kursens fyra kriterier handlar om att du

- kan begrepp, modeller, teorier och arbetssätt inom programmering
- känner till utvecklingsprocesser och kan resonera om hur delarna hänger ihop
- kan använda programmering på det valda området
- kan analysera vad programmering får för konsekvenser för individ, samhälle och teknisk utveckling

**Centralt innehåll.** Projektet kan, utifrån sitt ämne, beröra grundläggande begrepp och modeller, problemlösning och tillämpningar inom robotik. Se [täckningsmatrisen](../../kursplan.md#täckningsmatris).

**Kapitel som förbereder.**

- [Kapitel 9: Robotik med ROS2 och turtlesim](../../kapitel/09-robotik-ros2.md)
- [Kapitel 1: Programmering som verktyg](../../kapitel/01-programmering-som-verktyg.md), om modeller och tillstånd
- [Kapitel 5: Problemlösning, test och felsökning](../../kapitel/05-problemlosning-och-test.md), om tester

## Vad du ska göra

Uppgiften är inte fastställd. Läraren berättar vad som ska göras och vilka labbar som ingår.

Det material som finns är labbserien i [ROS2-repot](https://github.com/abbjetmus/ROS2). Labbarna i simulering går från att starta turtlesim till att bygga egna noder:

| Labb | Innehåll |
|---|---|
| [Labb 1](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-1-turtlesim.md) | Starta turtlesim, styra med tangentbordet och förstå noder |
| [Labb 2](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-2-topics.md) | Topics, meddelanden, `cmd_vel` och `pose` |
| [Labb 3](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-3-publicera.md) | Skicka kommandon från terminalen och använda services |
| [Labb 4](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-4-workspace.md) | Workspace, package och att bygga med `colcon` |
| [Labb 5](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-5-publisher.md) | Skriva en publisher-nod i Python |
| [Labb 6](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-6-subscriber.md) | Skriva en subscriber-nod i Python |
| [Labb 7](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-7-vagg-undvikare.md) | Kombinera noder till ett autonomt beteende |
| [Labb 8](https://github.com/abbjetmus/ROS2/blob/main/labs/labb-8-robotmopp-simulering.md) | Mäta täckning och städa arenan i simulering |

Labb 9 och 10 i samma repo handlar om en fysisk robotbil från ett annat projekt.

## Krav och leverabler

Krav och leverabler för projektet är inte fastställda. Labbarna i repot har egna inlämningar, och läraren anger vad som gäller.

## Arbetsgång

Planeringen omfattar fyra lektioner. Innehållet är inte fastställt.

| Lektion | Innehåll |
|---|---|
| Lektion 1 | Ej fastställt (80 minuter) |
| Lektion 2 | Ej fastställt (80 minuter) |
| Lektion 3 | Ej fastställt (95 minuter) |
| Lektion 4 | Ej fastställt (80 minuter) |

## Bedömning

Bedömningen fastställs av läraren. Kursens betygskriterier finns i [kursplanen](../../kursplan.md).

## Material

- [ROS2-repot med labbar](https://github.com/abbjetmus/ROS2), där README-filen beskriver installation och miljö. Labbarna är skrivna för Windows med WSL och Ubuntu samt ROS2 Jazzy eller Humble.
- [10 enkla Python-övningar](https://github.com/abbjetmus/ROS2/blob/main/10%20enkla%20Python-programmerings%C3%B6vningar.md) som uppvärmning
- [ROS2-dokumentationen](https://docs.ros.org/en/jazzy/)
- [Kapitel 9](../../kapitel/09-robotik-ros2.md) i den här boken

## Tips

- Starta om och kontrollera att du kört `source` i rätt terminal om ett kommando inte hittas.
- Håll logiken i egna funktioner som du kan testa utan simulatorn.
- Ändra en sak i taget när du felsöker, och kontrollera vilka noder och topics som körs med `ros2 node list` och `ros2 topic list`.

[Innehåll](../../README.md)
