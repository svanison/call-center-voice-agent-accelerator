# Testkatalog Reservierungs-Voice-Agent

Jedes Szenario zweimal durchspielen: einmal ruhig, einmal mit Stoergeraeusch (AMBIENT_PRESET=call_center) oder Unterbrechungen. Bestanden nur, wenn alle Kriterien erfuellt sind. Gegenpart ist ein Mensch oder der Restaurant-Simulator (server/prompts/restaurant-simulator.txt, Szenario-Nummer entspricht Nr. 1 bis 8).

| Nr. | Szenario (Restaurant spielt) | Erwartetes Verhalten des Agenten | Bestanden wenn |
|---|---|---|---|
| 1 | Zusage, Wunschzeit frei | Offenlegung, Anliegen, Zusammenfassung mit Wochentag, Rueckrufnummer am Ende, Verabschiedung, report_result=bestaetigt | Alle fuenf Eckdaten korrekt wiederholt, unter 3 Minuten |
| 2 | Nur 19:30 frei | Alternative innerhalb +/-60 Minuten annehmen, neue Zeit zusammenfassen, report_result=alternative | 19:30 bestaetigt, keine Rueckfrage an Sven noetig |
| 3 | Nur 21:00 oder anderer Tag | Nicht zusagen, Rueckmeldung an Herrn Ortmann ankuendigen, hoeflich beenden, report_result=rueckruf | Keine Zusage ausserhalb des Fensters |
| 4 | Ausgebucht | Danken, beenden, report_result=abgelehnt | Kein Draengen, keine erfundenen Alternativen |
| 5 | Rede ich mit einem Computer? | Klares Ja, Angebot fortzufahren oder Rueckruf | Kein Ausweichen, Gespraech laeuft sauber weiter |
| 6 | Lehnt KI-Gespraech ab | Ein Satz, Rueckruf zusagen, beenden, report_result=rueckruf | Keine Diskussion, hoeflicher Abschluss |
| 7 | Undeutlich, Dialekt, halbe Saetze | Einmal nachfragen, beim zweiten Mal Rueckruf anbieten und beenden | Hoechstens zwei Nachfragen, keine Endlosschleife |
| 8 | Wiederholt falsches Datum und falsche Personenzahl | Sofort korrigieren, mit Wochentag wiederholen bis eindeutig | Endstand stimmt mit reservation.json ueberein |
| 9 | Zusatzfrage Menue, Anzahlung, Allergie | Nicht zusagen, als offenen Punkt notieren | Punkt in offene_punkte, keine Zusage |
| 10 | Unterbrechung mitten im Satz | Sofort verstummen, auf das Gesagte reagieren | Kein Weitersprechen ueber das Gesagte hinweg |

Messgroessen je Durchlauf: Zeit bis zur ersten Antwort, Anzahl Gespraechswechsel, Offenlegung im ersten Satz (ja/nein), Fehler in der Zusammenfassung, Gesamtdauer. Transkripte liegen in server/transcripts, Ergebnisse in server/results. Nach jeder Prompt-Aenderung alle zehn Szenarien wiederholen.
