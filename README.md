Protokoll: DOM XSS mit innerHTML und location.search
1. Ziel des Labs
In diesem Lab geht es darum, eine DOM-basierte XSS-Schwachstelle auszunutzen, um eigenen JavaScript-Code auszuführen.
Als Beweis soll ein einfaches alert(1) erscheinen.
2. Technischer Hintergrund
Was ist DOM XSS?
DOM XSS passiert direkt im Browser und nicht auf dem Server.
Das Problem entsteht, wenn JavaScript Daten aus unsicheren Quellen nimmt und sie ohne Prüfung in die Webseite einfügt.
Wichtige Teile im Lab:
    • Source: location.search
→ Das ist alles, was in der URL nach dem ? steht 
    • Sink: innerHTML
→ Damit wird Inhalt direkt als HTML in die Seite eingefügt 
 Problem:
Wenn ungeprüfte Daten in innerHTML landen, kann ein Angreifer eigenen Code einschleusen.
3. Analyse der Schwachstelle
Die Anwendung macht im Prinzip sowas:
var query = location.search;
document.getElementById("output").innerHTML = query;
 Das bedeutet:
Alles aus der URL wird einfach als HTML auf der Seite angezeigt – ohne Kontrolle.
4. Angriff (Exploit)
Payload:
<img src=1 onerror=alert(1)>
Warum funktioniert das?
    • src=1 → ungültige Bildquelle 
    • Das Bild lädt nicht → Fehler entsteht 
    • onerror wird ausgelöst 
    • → alert(1) wird ausgeführt 
5. Durchführung
    1. Payload ins Suchfeld eingeben 
    2. Auf „Search“ klicken 
    3. Die Eingabe landet in der URL (location.search) 
    4. JavaScript übernimmt sie in die Seite (innerHTML) 
    5. Der Browser führt den Code aus 
6. Ergebnis
    • Ein Popup (alert(1)) erscheint 
    • Das zeigt: 
        ◦ Eigener JavaScript-Code wurde ausgeführt 
        ◦ Die Seite ist anfällig für DOM XSS 
7. Sicherheitsbewertung
Das ist kritisch, weil ein Angreifer z. B.:
    • Cookies stehlen kann 
    • Accounts übernehmen kann 
    • Fake-Login-Seiten einbauen kann 
    • Tastatureingaben ausspionieren kann 
8. Sichere Lösung 
Unsicher:
element.innerHTML = userInput;
Sicher:
element.textContent = userInput;
Oder zusätzlich:
    • Eingaben bereinigen (sanitizen) 
    • Libraries nutzen (z. B. DOMPurify) 
9. Fazit
Das Problem entsteht durch die Kombination von:
    • location.search (unsichere Datenquelle) 
    • innerHTML (unsichere Verarbeitung) 
Wichtig:
Niemals ungeprüfte Nutzereingaben direkt in innerHTML einfügen.

