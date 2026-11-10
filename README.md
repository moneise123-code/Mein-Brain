# Mein Brain – J.A.R.V.I.S.

Persönlicher, sprachgesteuerter Assistent als **eine einzige HTML-Datei** (`index.html`) – ohne Installation.
`jarvis.html` ist die Konzeptübersicht des Projekts.

## Bedienung

- `index.html` per Doppelklick öffnen – am besten in **Microsoft Edge** oder **Google Chrome**.
- **Mikrofon-Knopf** (oder Leertaste): sprechen. Nochmal drücken = fertig gesprochen.
  Während JARVIS spricht, unterbricht der Mikrofon-Knopf die Antwort und hört sofort wieder zu.
- **Stopp-Knopf** (oder Esc): bricht Zuhören, Verarbeitung und Sprachausgabe ab.
- **Tastatur-Knopf**: öffnet das Chatfenster für die Texteingabe.
- **Leuchtender Punkt links**: Menü mit Overview, Business, Trading, Research, Aufgaben,
  Gedächtnis, Einstellungen und Systemstatus.

## Was tatsächlich funktioniert

| Funktion | Status |
|---|---|
| Roter KI-Kreis mit den Zuständen Idle / Listening / Thinking / Speaking / Error | ✅ an den echten Systemzustand gekoppelt |
| Spracheingabe (Web Speech API, Deutsch) | ✅ Chrome, Edge, Safari – **nicht Firefox** |
| Reaktion des Kreises auf das Mikrofon (Web Audio, echter Pegel) | ✅ abschaltbar (Android) |
| Weibliche deutsche Stimme (SpeechSynthesis), automatische Auswahl + Auswahl in den Einstellungen | ✅ abhängig von den installierten Stimmen |
| Sprachausgabe der echten Antworten, unterbrechbar | ✅ |
| Chat mit Verlauf, Status- und Werkzeugmeldungen, Auto-Scroll, einklappbar | ✅ |
| Aufgaben, Ideen, Trading-Journal, Erinnerungen (bearbeiten/löschen), Export/Import | ✅ lokal im Browser |
| Lokaler Modus ohne KI (Befehle: „Neue Aufgabe: …“, „Merk dir …“, „Idee: …“, „Prioritäten“) | ✅ |
| Claude-Anbindung (freie Gespräche, Aufgaben/Ideen/Erinnerungen per Werkzeug speichern) | ✅ mit eigenem API-Schlüssel |
| Internetrecherche (Claude-Websuche mit Quellenangabe im Chat) | ✅ mit API-Schlüssel, optional |

Die Daten liegen unter demselben Speicherschlüssel wie in der vorherigen Version (`jarvis_local_v1`).
Bereits gespeicherte Aufgaben, Ideen, Trading-Notizen und Chats bleiben deshalb erhalten,
solange die Datei im selben Browser geöffnet wird.

## Ehrliche Grenzen

- **Kreis beim Sprechen:** Die Browser-Sprachausgabe lässt sich technisch nicht per Web Audio messen.
  Der Kreis reagiert auf Wortgrenzen-Ereignisse der Stimme; liefert eine Stimme diese nicht,
  wird ein Sprechrhythmus angenähert. Für eine echte Pegelkopplung braucht es eine externe
  TTS (z. B. ElevenLabs, OpenAI TTS, Azure), deren Audio über Web Audio abgespielt wird – die
  Schnittstelle dafür ist `Voice.speak()` / `Voice.level()` in `index.html`.
- **Stimmqualität:** hängt vom Gerät ab. Die natürlichsten kostenlosen deutschen Frauenstimmen
  bietet derzeit Microsoft Edge („Katja“, „Amala“, „Seraphina“ – Natural). Fehlt eine deutsche
  weibliche Stimme, zeigt JARVIS das in Einstellungen und Systemstatus an.
- **Spracherkennung** nutzt in Chrome/Edge einen Online-Dienst des Browserherstellers.
- **API-Schlüssel** wird im localStorage des Browsers gespeichert und direkt an `api.anthropic.com`
  gesendet. Das ist für ein privates Gerät vertretbar, aber kein sicherer Schlüsselspeicher.
  Am besten einen eigenen Schlüssel mit Ausgabenlimit verwenden. Er wird nie exportiert.
- **Keine Hintergrundprozesse:** JARVIS arbeitet nur, solange die Seite geöffnet ist. Geplante
  Recherchen, Morgenberichte ohne Aufforderung, Marktdaten, Backtesting, Paper Trading und
  Broker-Anbindung brauchen ein eigenes Backend (geplant: Python/FastAPI).
- **Echtgeldhandel** ist nicht vorhanden. JARVIS führt keine externen Aktionen aus.

## Aufbau (`index.html`)

`Store` (Daten) · `Orb` (Canvas-Visualisierung) · `AudioIn` (Mikrofonpegel) · `Voice` (Sprachausgabe) ·
`Listener` (Spracherkennung) · `Brain` (lokaler Provider / Claude-Provider) · `Chat` · `Controller`
(Zustandsmaschine) · `UI` (Menü und Bereiche). Neue KI-Anbieter oder TTS-Dienste lassen sich als
weiterer Provider in `Brain` bzw. `Voice` ergänzen.
