# APINet 1.0

Eine kleine Web-App, die Daten aus beliebigen APIs abruft und auf ein Bluetooth-Display (Unifi Notifier / Thumper V2) schickt.

## Funktionen

- Beliebig viele **Datenquellen** (GET oder POST, eigene Header und Body, Feldpfad in der Antwort)
- **Automatischer Abruf** in einstellbaren Intervallen (ab 5 Sekunden)
- **Bei Änderung**: neuer Wert wird sofort angezeigt. **Rotation**: Werte werden nacheinander gezeigt
- **Vorlagen** mit Platzhaltern `{name}`, `{value}`, `{time}`, zwei Zeilen pro Anzeige
- **Vorschau** eines simulierten Displays, auch ohne Gerät
- **Protokoll** aller Abrufe, Änderungen und gesendeten Bytes
- Installierbar als App auf dem Handy (PWA)

## Dateien

| Datei | Zweck |
| --- | --- |
| `index.html` | Gesamte App (Oberfläche, Logik, Protokoll) |
| `manifest.webmanifest` | Installations-Information für Handy und Desktop |
| `sw.js` | Service Worker für die Offline-Nutzung der Oberfläche |
| `icon.svg` | App-Symbol |
| `.nojekyll` | Verhindert, dass GitHub Pages Dateien verarbeitet |

## Starten

**Lokal:** `index.html` im Browser öffnen. Die Vorschau funktioniert sofort.

**Auf dem Handy (GitHub Pages):**
1. Repository auf GitHub anlegen und alle Dateien hochladen.
2. Settings → Pages → Branch `main`, Ordner `/ (root)`.
3. Die angezeigte HTTPS-Adresse auf dem Handy öffnen.
4. Android in Chrome: Menü → „Zum Startbildschirm hinzufügen“.

Web Bluetooth braucht HTTPS. Auf `localhost` funktioniert es am Computer ebenfalls.

## Bluetooth-Verbindung

| Gerät | Browser |
| --- | --- |
| Android | Chrome oder Edge |
| iPhone | **Bluefy** (Safari unterstützt Web Bluetooth nicht) |
| Desktop | Chrome oder Edge |

- Während der Verbindung darf die offizielle Unifi-App nicht mit dem Gerät verbunden sein.
- Findet der Browser das Gerät nicht, im Bereich Einstellungen „Alle Geräte anzeigen“ aktivieren.

## Datenquellen einrichten

- **URL:** z. B. `https://api.example.com/preis`. Die API muss CORS erlauben, sonst meldet die App „Netzwerk- oder CORS-Fehler“.
- **Feldpfad:** Pfad zum gewünschten Wert, z. B. `data.0.price`. Leer lassen, um die ganze Antwort zu verwenden.
- **Testquelle:** `demo:random` liefert Zufallswerte zum Ausprobieren.

## Protokoll-Grundlage

Basis: „Unifi Thumper/Notifier BLE Protocol“ (V3.6, 25. Juli 2024).

- Send Message (`C4B0AB41-…`): ASCII-Text, max. 160 Zeichen, zwei Zeilen getrennt durch CR (0x0D), in Paketen von 20 Byte
- Queue Message (`C4B0AB45-…`): 3 Byte (Warteschlange, Anzeigeverhalten, Dauer in Sekunden)
- Set Text Attributes (`C4B0AB40-…`): 8 Byte (Farben, Größe, Laufgeschwindigkeit), optional

**Offen, bitte am Gerät prüfen:**
- Die Reihenfolge „Text senden, Attribute, Queue“ ist eine Annahme. Das Dokument legt sie nicht ausdrücklich fest.
- Die Farbcodes stehen im Anhang „Constant Definitions“ und sind in der App nur als Zahlen eingetragen.
- Ob Send und Queue auf dem Thumper V2 genauso funktionieren, ist nicht geprüft.

## Lizenz und Hinweis

Keine Lizenz festgelegt. Das Protokoll stammt nicht von diesem Projekt und ist als vertraulich gekennzeichnet. Das Projekt ist nicht mit Unifi oder Illuminati Magic verbunden.

## Changelog

- **1.0**: Erste Version. Datenquellen, Rotation und Änderungserkennung, Vorlagen, Vorschau, Bluetooth-Anbindung, PWA.
