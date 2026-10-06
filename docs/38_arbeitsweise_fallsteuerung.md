# 38 Arbeitsweise und Fallsteuerung

**Stand:** 2026-10-04 (Satz für Satz an Primärquellen geprüft; Prüfprotokoll: Claude outputs/2026-10-04_Wiki_Pruefprotokoll.md) · Grundlage: Bericht `Claude outputs/2026-10-03_Best_Practices_Fachkraeftevermittlung.md` (Methoden) und Prüfbericht `Claude outputs/2026-10-03_Rechtspruefung_Wiki_Visum_Vermittlung.md` (Recht)
**Nächste Prüfung:** nach drei Monaten Wochenrunde (Januar 2027)

> **Ziel:** Wie AATRIUM Visa-Fälle steuert: wo welche Daten leben, wie die Ampel funktioniert, wie die Wochenrunde läuft und welche Regeln aus anderen Branchen wir übernehmen.

!!! note "Hinweis zu den Belegen"
    Die Methoden stammen aus Medizin, Luftfahrt, Bau und Software. Dass sie bei Visumfällen genauso wirken, ist unsere Ableitung und nirgends direkt getestet. Zahlen aus Studien stehen mit Quelle dabei.

---

## 38.1 Welche Daten leben wo

Eine Fall-ID für alles: die **AA-Nummer**. Sie wird kopiert oder ausgewählt, nie abgetippt. Abgleich nie über Namen – usbekische Namen haben mehrere lateinische und kyrillische Schreibweisen. Maßgeblich ist die Schreibweise in der maschinenlesbaren Passzeile.

| Daten | Master-System | Regel |
|---|---|---|
| Leads | Supabase | Nur bis zur Umwandlung in einen Fall |
| Stammdaten wie im Pass, formaler Prozess, fertige Dokumente | Ankaadia | Nur hier wird geändert |
| Rohscans und Arbeitsdateien | Google Drive | Fall-ID, Nummernplan, Datum, Version |
| Steuerung: Station, wartet auf, nächster Schritt, Frist | Board „Fallsteuerung Visa“ (Claude) | Keine Dokumente, keine Passdaten |
| Nachrichten | E-Mail, Telegram, WhatsApp | Nur Hinweise, keine Dokumente oder Passscans; Entscheidungen am selben Tag im Master-System notieren (Datenschutz: Artikel 40) |

Ältere Boards (Visa-Board, Prozessboard) und das Sheet „Fallsteuerung Visa“ sind für Visa-Fälle nur noch Archiv.

---

## 38.2 Stationen und Ampel

| Station | Weiter, wenn … | Ampel grün bis | gelb bis |
|---|---|---|---|
| 1 Zusage | Akte im Drive angelegt (Pass, Diplom mit Anlage, Sprachstand) | 7 Tage | 14 Tage |
| 2 Sprache | telc-Zertifikat B1 oder höher ausgestellt und als 6.2 abgelegt | 180 Tage | 270 Tage |
| 3 Akte | Alle Pflichtdokumente laut Nummernplan geprüft im Ordner | 30 Tage | 60 Tage |
| 4 Vertrag & EzB | Unterschriebener Vertrag und EzB im Ordner (3.1, 8.1) | 14 Tage | 28 Tage |
| 5 Antrag & Termin | Termin mit Datum bestätigt und Unterlagen vollständig | 30 Tage | 60 Tage |
| 6 Bei der Botschaft | Visum erteilt oder abgelehnt | 21 Tage | 42 Tage |
| 7 Visum erteilt | Kandidat ist eingereist | 21 Tage | 42 Tage |
| 8 Eingereist | – | – | – |

- Die Ampel zählt die **Tage in der aktuellen Station**. Rot auch, wenn eine Frist („Fällig bis“) überschritten ist.
- **Rot heißt:** binnen 2 Werktagen eine benannte Aktion mit Datum.
- Die Grenzen sind **Schätzwerte vom 03.10.2026**. Sobald es je Station etwa zehn abgeschlossene Fälle gibt, ersetzen echte Werte sie: grün bis zum Median, gelb bis zum 85. Perzentil ([ProKanban, SLE](https://prokanban.org/blog/the-kanban-pocket-guide-chapter-2-the-service-level-expectation)). Die Grenzen lassen sich im Board ändern.
- Farben kommen aus Regeln, nicht aus Gefühl: Projektleiter schreiben in 60 % der Fälle verzerrte Statusberichte, meist zu optimistisch ([Keil u. a. 2014](https://www.academia.edu/106287821/The_Pitfalls_of_Project_Status_Reporting)).
- „Gesamt“ zählt ab der Zusage des Arbeitgebers. So bleiben lange Wartezeiten sichtbar, auch wenn ein Fall die Station wechselt.

### Wer ist am Zug: Warten ist nicht blockiert

Warten ist bei AATRIUM der Normalzustand. Jede Karte zeigt deshalb, auf wen sie wartet, und ein zugesagtes Datum:

| Kürzel | Wer |
|---|---|
| AG | Arbeitgeber |
| UZ1, UZ2 | Partner in Usbekistan |
| BEH | Behörde (ZSEF, BA, Botschaft, ZAB) |
| KAN | Kandidat |
| AAT | AATRIUM intern |

- Eine Karte wird **blockiert** markiert, wenn das zugesagte Datum vorbei ist oder ihr Alter über der gelben Grenze liegt. Sie **bleibt in ihrer Spalte**. Eine eigene Spalte „Blockiert“ gibt es nicht, weil Fälle dort aus dem Blick geraten ([ProKanban](https://prokanban.org/blog/whats-wrong-with-having-a-blocked-column)).
- Jede Blockade bekommt Grund und Dauer. Einmal im Monat nach Ursache gruppieren („Blocker Clustering“): Welche Gruppe kostet die meisten Tage? ([DJAA, Risk Review](https://djaa.com/project-management-kanban-part-4-risk-review/))

### Dringlichkeit und Obergrenze

- **Vier Klassen:** „Eilt“ (schon zu spät, überholt alles), „fester Termin“, „Standard“ (älteste zuerst) und „unbestimmt“ ([Kanban University, Glossar](https://kanban.university/glossary/)). Höchstens **ein** Fall ist gleichzeitig „Eilt“, etwa ein Botschaftstermin in zehn Tagen mit Lücke in der Akte.
- **Feste Termine** (Prüfung, Botschaftstermin, Arbeitsbeginn, Ablauf von Pass, Sprachzertifikat oder Vorabzustimmung) stehen auf der Karte. Spätester sicherer Start = Termin minus Restdauer an der gelben Grenze.
- **Obergrenze offener Fälle:** Durchlaufzeit ≈ offene Fälle ÷ Ankünfte pro Monat (Little's Law). Beispiel: Bei 2,5 Ankünften im Monat bedeuten 35 offene Fälle rund 14 Monate, 20 Fälle rund 8 Monate (**Beispielzahlen**). Faustregel: Zielzahl offener Fälle = gewünschte Durchlaufzeit × Ankünfte pro Monat. Für Prognosen taugt die Formel nicht, nur als Regel ([Vacanti 2018](https://scrumorg-website-prod.s3.amazonaws.com/drupal/2018-05/Little%E2%80%99s%20Law%20for%20Professional%20Scrum%20with%20Kanban.pdf)).

### Drei Fallen

1. **Durchschnitte lügen:** Sie zeigen nur abgeschlossene Fälle; die ganz alten fehlen. Die Altersliste ist ehrlicher.
2. **Alte Daten verfallen:** Nach einem Wechsel des Verfahrens (anderer Terminweg, § 81a) beginnt die Datenreihe neu.
3. **Pausen nur aus festen Gründen,** mit Datum. Das Alter einmal mit und einmal ohne Pausen führen.

Ein Protokoll genügt: je Fall und Stationswechsel eine Zeile mit Fall-ID, Station (1–8 oder P für pausiert), Datum und bei Abbruch dem Grund. Abgebrochene Fälle werden nie gelöscht, sondern mit Grund beendet.

---

## 38.3 Wochenrunde

**Termin:** jeden Dienstag 10:00–11:00 (Vorschlag, im Board änderbar). Gleicher Tag, gleiche Uhrzeit, gleiche Tagesordnung.

| Minuten | Punkt |
|---|---|
| 10 | Zusagen der letzten Woche: erledigt? Wenn nicht, warum (fehlende Info, keine Zeit, andere Priorität, Fehler)? |
| 20 | Fallliste nach Alter, rote Fälle zuerst: Was hält den Fall auf? Nächster Schritt, wer, bis wann |
| 10 | Hindernisse der nächsten 8 bis 12 Wochen: Was muss vorher geklärt sein, wer hat ein Datum zugesagt? |
| 10 | Acht Kennzahlen, je Zahl nur „im Plan“ oder „nicht im Plan“ |
| 10 | Zwei bis drei Probleme lösen, Zusagen für diese Woche festhalten |

Vorbilder: der NHS steuert Krebs-Behandlungspfade mit einer wöchentlichen Liste, längster Wartender oben ([NHS England](https://www.england.nhs.uk/wp-content/uploads/2015/03/delivering-cancer-wait-times.pdf)); EOS fordert für Wochentreffen gleichen Tag, gleiche Uhrzeit, gleiche Tagesordnung ([EOS](https://www.eosworldwide.com/level-10-meeting)).

### Vier Takte

Die Zeiten sind Vorschläge.

| Takt | Dauer | Inhalt |
|---|---|---|
| Board-Runde, Mo/Mi/Fr | 10–15 Min. | Von Station 8 nach Station 1, rote und blockierte Fälle zuerst. Je Fall: Was hält ihn auf? Nächster Schritt, Person, Datum? Keine Berichte über gestern ([Stray u. a. 2020](https://arxiv.org/pdf/1808.07650)) |
| Wochenrunde, Di 10:00 | 60 Min. | wie oben |
| Monatsrückblick, erste Woche | 90 Min. | Durchlaufzeiten und Altersbild, Blockaden nach Partei, Zusagenquoten von Arbeitgeber und Partnern, Kohorten, Fehlerprotokoll, Kasse, Prognose, Regeländerungen |
| Quartal | halber Tag | Drei bis fünf Prioritäten mit je einer verantwortlichen Person, Quartalsgespräch mit dem Arbeitgeber, Partner-Reviews ([EOS, Rocks](https://www.eosworldwide.com/rocks)) |

### Acht Kennzahlen

| # | Kennzahl | Art |
|---|---|---|
| 1 | Neue Leads je Quelle | Eingang |
| 2 | Anteil neuer Leads mit echtem Gespräch binnen 2 Werktagen | Eingang |
| 3 | Kursstarts (erste Stunde besucht) | Eingang |
| 4 | Lernende: Anwesenheit, gefährdete Lernende, bestandene Prüfungen | Frühindikator |
| 5 | Akten ohne Nacharbeit (Anteil) | Qualität |
| 6 | Rote Fälle ohne Aktion mit Datum (rechnet das Board) | Fluss |
| 7 | Antwortzeit von Arbeitgeber und Partnern (Median, Tage) | Fluss |
| 8 | Ankünfte in den letzten 13 Wochen (rechnet das Board) | Ergebnis |

Jede Zahl bekommt eine verantwortliche Person und ein Ziel. Ziele für Eingangsgrößen so setzen, dass etwa drei Viertel erreicht werden ([Commoncog zu Amazons Weekly Business Review](https://commoncog.com/the-amazon-weekly-business-review/)). Zeigt eine Zahl nichts Besonderes, heißt es „nichts zu sehen“, und es geht weiter. „Wissen wir noch nicht, prüfen wir“ ist erlaubt, Spekulation nicht.

### Trichter rückwärts rechnen, Kohorten vergleichen

- **Rückwärts:** Benötigte Eintritte je Stufe = Zielankünfte ÷ Produkt der Quoten danach. Beispiel mit erfundenen Quoten: Vier Ankünfte im Monat bräuchten etwa 190 neue Leads und 14 Kursstarts pro Monat (**Beispielzahlen**). Steigt die B1-Quote von 50 auf 65 %, sinken die nötigen Kursstarts um 23 %.
- Änderungen am Anfang wirken erst nach über einem Jahr bei den Ankünften. Darum steuert die Wochenrunde über Eingangsgrößen.
- **Kohorten statt Wochen:** Eine Tabelle je Kursstart-Monat zeigt die echten Quoten. Dafür braucht Supabase eine Tabelle mit jedem Stufenwechsel: Fall-ID, von, nach, Zeitpunkt, Grund, wer.
- **Ehrliches Rot wird belohnt.** Wer früh Rot meldet, bekommt Hilfe, keinen Ärger.

---

## 38.4 Zehn Regeln aus anderen Branchen

1. **Nach dem Alter der Fälle steuern.** Älteste Arbeit zuerst, Altern verhindern, Blockaden lösen ([ProKanban, Kanban Guide](https://prokanban.org/the-kanban-guide/)).
2. **Warten ist nicht blockiert.** Fälle, die auf Externe warten, bleiben in ihrer Spalte sichtbar; läuft die Zusage ab, wird der Fall als blockiert markiert und aktiv verfolgt ([DJAA](https://djaa.com/kanban-evergreen-should-we-include-waiting-or-blocked-items-in-wip-limits/)).
3. **Den Engpass voll nutzen.** Kein Botschaftstermin ohne vollständige Akte („Full Kit“); neue Fälle nur so schnell aufnehmen, wie der Engpass sie abarbeitet ([Goldratt, 5 Focusing Steps](https://northriverpress.com/wp-content/uploads/2018/01/Free-download-5FS.pdf)).
4. **Zusagen messen.** Anteil eingehaltener Zusagen je Partei (Arbeitgeber, Partner, Behörden, wir selbst). Ohne Last Planner wird etwa die Hälfte der Wochenaufgaben wie geplant fertig, mit Last Planner meist 65–70 % ([Ballard 2000](https://lean-construction-gcs.storage.googleapis.com/wp-content/uploads/2022/09/08152942/the-last-planner-system-of-production-control-ballard2000-dissertation.pdf)).
5. **Kurze Prüflisten an festen Toren**, weniger als zehn Punkte, Werte statt Haken (siehe 38.5). Die WHO-Checkliste senkte in acht Kliniken die Sterblichkeit von 1,5 auf 0,8 % ([Haynes 2009](https://pubmed.ncbi.nlm.nih.gov/19144931/)); in Ontario wirkte sie ohne sorgfältige Einführung nicht ([Urbach 2014](https://pubmed.ncbi.nlm.nih.gov/24620866/)). Antworten mit dem tatsächlichen Wert ([Degani & Wiener 1990](https://skybrary.aero/sites/default/files/bookshelf/1568.pdf)).
6. **Unabhängige Prüfung nur für Killer-Punkte.** 92,5 % vorgeschriebener Doppelkontrollen waren nicht unabhängig und brachten keinen Sicherheitsgewinn ([Westbrook 2020](https://discovery-pp.ucl.ac.uk/10107890/1/bmjqs-2020-011473.full.pdf)).
7. **IDs automatisch abgleichen,** nicht mit dem Auge: Doppelte Eingabe oder automatischer Abgleich hinterlässt 20- bis 30-mal weniger Fehler ([Barchard-Labor](https://img.faculty.unlv.edu/lab/newsletter/issue3/data-checking/)).
8. **Übersetzungen mit Begriffsblatt:** Name laut Pass, Wortlaut von Abschluss und Fachrichtung im Original, anabin-Eintrag. Revision durch eine zweite Person (Prinzip der Norm ISO 17100).
9. **Prognosen als Wahrscheinlichkeit,** etwa „mit 85 % mindestens vier Ankünfte bis 31. März“, nicht als einzelnes Datum ([55 Degrees, Monte Carlo](https://www.55degrees.se/blog/post/monte-carlo-simulations-and-forecasting)).
10. **Aus jedem Fehler lernen:** Fehlerprotokoll ohne Schuldfrage; 15 Minuten Nachbesprechung nach jeder Botschaftsentscheidung. Strukturierte Nachbesprechungen verbessern die Leistung um 20–25 % ([Tannenbaum & Cerasoli 2013](https://journals.sagepub.com/doi/10.1177/0018720812448394)).

---

## 38.5 Prüfliste vor jeder Einreichung (Tor T3)

Vor dem Upload bei ZSEF oder Ausländerbehörde und vor dem Botschaftstermin. Jede Antwort ist ein **Wert**, kein Haken. Ruhige Zeit, Benachrichtigungen aus; nach einer Unterbrechung von vorn.

| # | Prüfpunkt | Antwort als Wert |
|---|---|---|
| 1 | Diplom mit Anlage vollständig | „Original 2 + 3 Seiten = 5; PDF = 5 Seiten“ |
| 2 | Fachrichtung stimmt überall | „Original = Übersetzung = anabin-Ausdruck: [Wortlaut]“ |
| 3 | Fall-ID identisch | „Ankaadia = Supabase = Drive = Board: [ID]“ |
| 4 | Dokumenttyp stimmt mit Inhalt | „Datei geöffnet, Inhalt = Etikett“ |
| 5 | Name wie im Pass auf allen Papieren | „Schreibweise laut Passzeile: [Name]; Abweichungen notiert“ |
| 6 | Sprachzertifikat | „Ausgestellt am …; am Termin jünger als 1 Jahr“ |
| 7 | Pass | „Gültig bis …; mindestens 1 Jahr ab Antrag“ (Artikel 20) |
| 8 | Krankenversicherung | „GKV-Nachweis + Incoming aus der EU, 90 Tage, ≥ 30.000 €, inkl. COVID-19“ (Artikel 22) |
| 9 | anabin-Ausdrucke | „Abschluss + Hochschule, ausgedruckt am …“ |
| 10 | EzB passt zum Vertrag | „Betriebsnummer der Betriebsstätte, Tätigkeit, Tarif = Vertrag“ |

Die Liste an zwei alten Akten testen, datieren und nur nach einer Nachbesprechung ändern.

---

## 38.6 Fristen im beschleunigten Verfahren (für die Planung)

| Schritt | Frist | Hinweis |
|---|---|---|
| BA-Zustimmung | gilt nach **1 Woche** ohne Rückmeldung als erteilt | sonst 2 Wochen |
| Anerkennung / ZAB | „soll“ binnen **2 Monaten** | erst ab vollständigen Unterlagen |
| Vorabzustimmung | „unverzüglich“ | keine Gesamtfrist; BMWK schätzt insgesamt ca. 4 Monate |
| Botschaftstermin | binnen **3 Wochen** | § 31a Abs. 1 AufenthV |
| Visumentscheidung | „in der Regel“ binnen **3 Wochen** | § 31a Abs. 2 AufenthV |

Station 5 im Board hat damit zwei Uhren: Behörde bis zur Vorabzustimmung und Botschaft bis zum Termin. Details: Artikel 36.

---

## 38.7 Prüf-Tore und Lernen aus Fehlern

| Tor | Wann | Was |
|---|---|---|
| T1 | in Station 3 | Rohdokumente vollständig |
| T2 | Ende Station 3 | Übersetzungen und anabin/ZAB fertig |
| T3 | vor jeder Einreichung (ZSEF-Upload, Botschaftstermin) | Prüfliste 38.5, von einer zweiten Person |
| T4 | Übergang 5 → 6 | Termin bestätigt, Mappe gepackt |
| T5 | nach der Entscheidung | 15 Minuten Nachbesprechung, wenn möglich mit dem Partner |

**Fehler weggestalten, bevor man prüft** (Poka-Yoke, [Grout 2007](https://www.govinfo.gov/content/pkg/GOVPUB-HE20_6500-PURL-gpo3107/pdf/GOVPUB-HE20_6500-PURL-gpo3107.pdf)):

- Dokumenttyp nur aus einer Auswahlliste, Dateiname automatisch aus Fall-ID und Typ.
- Fall-ID wird ausgewählt oder kopiert, nie abgetippt.
- Beim Hochladen Pflichtfeld „Seiten im Original“ neben „Seiten im PDF“.

**Unabhängig prüfen, aber nur die Killer-Punkte.** Die prüfende Person beginnt beim Original und notiert die erwarteten Werte, bevor sie die Akte ansieht. „Kannst du kurz drüberschauen?“ gilt nicht als Prüfung.

**Quote ohne Nacharbeit je Übergabe messen** (Uploads der Kandidaten, Übersetzungen, Arbeitgeberunterlagen, Einreichungen). Die Quoten multiplizieren sich: 0,70 × 0,85 × 0,90 × 0,95 ≈ 0,51 (**Beispielzahl**). Dann ginge nur jede zweite Akte ohne Nacharbeit durch.

**Feste Lernschleifen:**

- **Fehlerprotokoll** für Fehler und Beinahe-Fehler, ohne Schuldfrage. Feste Auslöser: jede Nachforderung einer Behörde, jede ID-Abweichung, jede Korrektur einer Übersetzung.
- **Nachbesprechung** mit vier Fragen: Was sollte passieren? Was ist passiert? Warum? Was behalten oder ändern wir? ([US-Armee, TC 25-20](https://nick.groenen.me/attachments/public/gitignored/TC%2025-20%20A%20Leader's%20Guide%20to%20After-Action%20Reviews.pdf))
- **Pre-Mortem** vor jeder neuen Gruppe und jedem neuen Partner: Wir stellen uns vor, es ist schiefgegangen. Warum? ([Klein 2007](https://hbr.org/2007/09/performing-a-project-premortem))
- Jede Erkenntnis ändert etwas Konkretes: einen Prüfpunkt, eine Vorlage oder eine Arbeitsanweisung.

---

## 38.8 Rückwärtsplan und Engpass-Liste

Das Verfahren stammt aus dem Bau (Last Planner System, [Ballard & Tommelein 2016](https://p2sl.berkeley.edu/wp-content/uploads/2016/10/Ballard_Tommelein-2016-Current-Process-Benchmark-for-the-Last-Planner-System.pdf)).

1. **Zieltermin je Fall:** der Arbeitsbeginn, abgestimmt mit dem Arbeitgeber.
2. **Rückwärts die spätesten Termine rechnen:** Einreise, Visum, Botschaftstermin, Vorabzustimmung, Antrag bei der ZSEF, Vertrag und EzB, vollständige Akte, Sprachzertifikat. Fristen aus 38.6.
3. **Vorschau 8 bis 12 Wochen.** Die längste bekannte Vorlaufzeit ist die ZAB-Bewertung (2 Monate im § 81a, sonst 3 Monate).
4. **Engpass-Liste:** jedes Hindernis mit Fall, verantwortlicher Partei, zugesagtem Datum und Status, z. B. „Anlage zum Diplom fehlt – UZ1 – Fr 09.10.“. Ziel: Hindernis weg, bevor der Fall die Station erreicht.
5. **In die Woche kommen nur freie Aufgaben,** also ohne offenes Hindernis.
6. **Zusagen messen** (PPC, Anteil erledigter Wochenzusagen): wöchentlich fürs Team, monatlich je Partei (Arbeitgeber, UZ1, UZ2, Behörden, Kandidaten, AATRIUM). Jeder Fehlschlag bekommt einen Grund: Information fehlte, keine Kapazität, andere Priorität, Fehler. Start bei 50 % ist normal, 70–80 % ist ein gutes Ziel ([Ballard 2000](https://lean-construction-gcs.storage.googleapis.com/wp-content/uploads/2022/09/08152942/the-last-planner-system-of-production-control-ballard2000-dissertation.pdf)).

**Den Engpass finden:** die Station mit den meisten wartenden Fällen, dem höchsten Alter und dem geringsten Abfluss. Einmal im Monat einen Satz aufschreiben: „Unser Engpass ist Station X, weil …“. Bei jeder bremsenden Regel fragen: Kommt sie vom Gesetz, von der Botschaft, vom Arbeitgeber oder aus Gewohnheit? Beispiel: „Erst B1, dann Akte und Vertrag“ ist für § 18b keine gesetzliche Pflicht.

**Papiere altern:** Akten nicht Monate zu früh fertig machen. Sprachzertifikate über ein Jahr kann die Botschaft nachprüfen, der Pass muss noch ein Jahr gelten, und wer den Pass mehr als drei Monate nach der Benachrichtigung abgibt, wird in der Regel neu geprüft ([Botschaft, Nationales Visum](https://taschkent.diplo.de/uz-de/service/05-visaeinreise/1604022-1604022)). **Register der Ablaufdaten** je Fall: Pass, Sprachzertifikat, Vorabzustimmung (9 Monate), Botschaftstermin, Visum, Datum der anabin-Ausdrucke.

---

## 38.9 Partner und Arbeitgeber steuern

**Partnervereinbarung (1–2 Seiten):** Umfang und Lieferungen, Bearbeitungszeiten, Qualitätsregeln (Name wie im Pass, Abschluss und Fachrichtung wörtlich mit Originalbegriff in Klammern, alle Seiten mit Anlagen und Stempeln, Revision durch eine zweite Person), Korrekturen auf Kosten des Partners, **null Gebühren für Kandidaten**, Datenschutz mit Standardvertragsklauseln, Prüf- und Kündigungsrecht, Zahlung nach Abnahme. Auch was AATRIUM schuldet, z. B. Rückmeldung binnen zwei Werktagen. Welche Aufgaben die Partner übernehmen dürfen, hängt vom usbekischen Lizenzrecht ab (Artikel 40.4).

**Monatliche Scorecard je Partner:** pünktliche Lieferungen (%), Annahme ohne Korrektur (%), Zahl der Nachbesserungen, Antwortzeit, Regeltreue (Schwelle: null Verstöße). Drei Tage vor einem 30-Minuten-Gespräch verschicken. Der bessere Partner bekommt mehr Volumen.

**Gegenseitige Zusagen mit dem Arbeitgeber** (alle Werte **Beispielzahlen**):

| AATRIUM sagt zu | Arbeitgeber sagt zu |
|---|---|
| geprüfte, vollständige Akten | Vertrag und EzB binnen 5–10 Werktagen |
| wöchentlicher Ausnahmebericht: nur blockierte oder verspätete Fälle mit Grund, wer handeln muss, bis wann | Unterschriften für § 81a binnen 5 Werktagen |
| Plan für die Zeit bis zur Einreise | eine benannte Ansprechperson mit Vertretung |
| | rollierende Bedarfsprognose für vier Quartale |

Weg dorthin: erst messen (jede Anfrage und Antwort mit Datum), dann im Quartalsgespräch zeigen, dann ein Pilotquartal vorschlagen, in dem AATRIUM zuerst liefert. Ein fester „Vertragstag“ alle zwei Wochen bündelt die Anfragen.

**Zeit bis zur Einreise gestalten:** alle ein bis zwei Wochen Kontakt, ein Videogespräch mit dem künftigen Bauleiter, Infos zur Unterkunft, ein Pate unter den schon Eingereisten. Die gefährlichste Phase ist „bereit, aber wartend“ (eigene Ableitung).

**Mehrere Kontakte beim Arbeitgeber:** wer entscheidet, wer Verträge ausstellt, wer § 81a betreut, Bauleiter vor Ort. Frühwarnzeichen: Einstellungsstopp, Wechsel der Ansprechperson, Qualitätsvorfall.

**Risiko streuen, in dieser Reihenfolge:** zweiter Arbeitgeber in derselben Branche, dann zweites Herkunftsland, dann angrenzende Berufe.

**Geld nach Meilensteinen:** ein Teil bei Vertrag, ein Teil beim Visum, der Rest bei Arbeitsbeginn (Beispiel). Ersatz statt Erstattung bei frühem Abgang. Liquidität über 12–18 Monate planen. Kosten je Ankunft mit Schwund rechnen: Kosten je Kursstarter ÷ Wahrscheinlichkeit, dass er ankommt (1.000 € ÷ 0,29 ≈ 3.450 €, **Beispielzahl**). Gebühren vom Kandidaten: höchstens 2.000 €, besser keine (Artikel 40).

---

## 38.10 Kommunikation mit Partnern und Arbeitgeber

**Nachrichtenvorlage:**

- Betreff mit Fall-ID.
- Erste Zeile: Kennzeichen und Kern, z. B. „[AKTION bis Fr 09.10.2026, 12:00 Taschkent / 09:00 Berlin]“ oder „[INFO]“.
- Zweite Zeile: **eine** Bitte an **eine** benannte Person.
- Höchstens drei Zeilen Kontext.
- Letzte Zeile: „Bitte bestätigt mit …“ oder „Nächster Schritt: wer macht was bis wann“.

Kurze Mails werden öfter beantwortet: 49 Wörter brachten 4,8 % Antworten, 127 Wörter nur 2,7 % ([Rogers & Lasky-Fink 2023](https://behavioralscientist.org/when-writing-for-busy-readers-less-is-more/)).

**Kanal-Charta** (Antwortzeiten sind unsere Konvention):

| Kanal | Wofür | Antwort |
|---|---|---|
| Telegram, WhatsApp | Kurze Fragen und Bestätigungen; **keine Dokumente oder Passscans** (Artikel 40.3) | am selben Arbeitstag |
| E-Mail | Dokumente, Verbindliches, Arbeitgeber | binnen zwei Arbeitstagen |
| Anruf, Video | Dringendes, Streit, Unklares, jeder Faden nach drei Runden ohne Einigung | sofort |
| Zusammenfassung nach jedem Anruf | Was vereinbart wurde | am selben Tag |

**Regeln:**

- **Rückmeldung mit Werten:** Namen wie im Pass, Daten, Dokumentlisten und Termine wiederholt der Partner mit den Werten. „Ok“ oder Daumen hoch zählt nicht ([AHRQ, Check-back](https://www.ahrq.gov/teamstepps-program/curriculum/communication/tools/checkback.html)).
- **Nachfassen mit System:** Jede Bitte kommt mit Sende- und Nachfassdatum auf eine Warte-auf-Liste (zwei Arbeitstage, Dringendes am selben Tag). Dann Erinnerung mit Zitat der ersten Nachricht, danach Anruf.
- **Entscheidungsprotokoll:** nummeriert ab E-001 mit Datum, Entscheidung, Grund, wer, Status. An Partner mit Etikett „FINAL“ oder „VORSCHLAG“.
- **Aufträge mit Absicht:** Zweck, Endzustand, höchstens drei Kernaufgaben, Grenzen, Frist mit Zeitzone. Antwort beginnt mit „Ich beabsichtige …“.
- **Ein Nein leicht machen:** „Was könnte X bis Freitag verhindern?“ statt „Schafft ihr X bis Freitag?“.
- **Maschinelle Übersetzung mit Rückprüfung:** Ausgangstext in einfachem Deutsch oder Englisch, kritische Nachrichten zurückübersetzen, kleines Glossar Deutsch–Russisch–Usbekisch. Für amtliche Dokumente bleibt die beglaubigte Übersetzung Pflicht.
- **Zeitzonen immer doppelt angeben:** Taschkent ist UTC+5 ohne Sommerzeit. Bis 25.10.2026 ist es dort 3 Stunden später als in Berlin, danach 4.

**Persönliche Arbeitsweise:** eine Liste, viele Eingänge (einmal am Tag landet alles auf dem Board); jede Karte mit Verb, nächstem Schritt und „Wann“; zehn Minuten Feierabend-Plan (wo, wann, wie es morgen weitergeht; [Masicampo & Baumeister 2011](https://doi.org/10.1037/a0024192)); zwei geschützte Konzentrationsblöcke pro Tag; vor jedem Wechsel eine Parknotiz auf der Karte; freitags 30–45 Minuten Wochenrückblick.

---

## Quellen

Alle Links in den Abschnitten, abgerufen am 03.10.2026 (Fristen und Botschaftsangaben erneut am 04.10.2026). Die Methoden stammen aus anderen Branchen; ihre Wirkung bei Visumfällen ist nicht getestet und wird mit den eigenen Kennzahlen gemessen. Rechtliche Fristen: BA Fachliche Weisungen AufenthG/BeschV (Stand 12/2024), BT-Drs. 19/8285, Visumhandbuch (Stand 21.08.2026), BMWK-FAQ beschleunigtes Fachkräfteverfahren (Stand 08/2024).
