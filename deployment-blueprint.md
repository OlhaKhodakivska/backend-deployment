# Deployment Blueprint: Node.js + Express + MongoDB Atlas + Render

## 1. Ziel

In diesem Beispiel wird eine einfache Node.js- und Express-Anwendung mit MongoDB und Mongoose auf Render bereitgestellt.

Der verwendete Stack:

```text
GitHub
   |
   v
Render
   |
   v
Node.js + Express
   |
   v
MongoDB Atlas
```

Die Anwendung wird auf Render gehostet und die Datenbank wird bei MongoDB Atlas betrieben.

---

## 2. Voraussetzungen

Vor dem Deployment brauche ich:

- ein GitHub-Konto;
- ein GitHub-Repository mit dem Backend;
- eine Node.js-Anwendung;
- Express;
- Mongoose;
- ein MongoDB-Atlas-Konto;
- ein Render-Konto.

Die Anwendung sollte lokal funktionieren, bevor sie deployed wird.

Zum Beispiel sollte `package.json` einen Start-Befehl enthalten:

```json
{
  "scripts": {
    "start": "node server.js"
  }
}
```

Der genaue Dateiname kann je nach Projekt unterschiedlich sein.

---

# 3. MongoDB Atlas einrichten

## Schritt 1: MongoDB Atlas öffnen

Ich erstelle ein Konto bei MongoDB Atlas und öffne das Atlas Dashboard.

Danach erstelle ich ein neues Projekt.

## Schritt 2: Einen kostenlosen Cluster erstellen

Ich erstelle einen kostenlosen MongoDB-Cluster.

Für ein Lernprojekt reicht der kostenlose Cluster aus.

Der Free Cluster bietet 512 MB Speicher.

## Schritt 3: Database User erstellen

Für die Anwendung brauche ich einen eigenen MongoDB Database User.

Ich erstelle zum Beispiel:

```text
Username: backend-user
Password: ********
```

Das Passwort muss sicher sein und darf nicht im GitHub Repository gespeichert werden.

MongoDB Atlas benötigt einen Database User, damit eine Anwendung auf die Datenbank zugreifen kann.

## Schritt 4: Netzwerkzugriff konfigurieren

MongoDB Atlas erlaubt nur Verbindungen von IP-Adressen, die in der IP Access List eingetragen sind.

Für das Deployment muss deshalb der Zugriff für die Umgebung konfiguriert werden, aus der Render auf MongoDB Atlas zugreift.

Die IP Access List befindet sich in MongoDB Atlas unter:

```text
Security
→ Network Access
```

Für ein Lernprojekt kann man dort die notwendige IP bzw. den notwendigen CIDR-Bereich eintragen.

In einer echten Production-Umgebung sollte der Netzwerkzugriff möglichst stark eingeschränkt werden.

## Schritt 5: Connection String kopieren

Im Atlas Dashboard gehe ich zu:

```text
Database
→ Connect
→ Drivers
→ Node.js
```

MongoDB stellt dort eine Connection URI zur Verfügung.

Sie sieht ungefähr so aus:

```text
mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<database>
```

Ich ersetze `<username>`, `<password>` und `<database>` mit meinen Daten.

Die Connection URI ist ein Secret und darf nicht auf GitHub veröffentlicht werden.

MongoDB dokumentiert die Verbindung über den Node.js Driver und die Verwendung einer `mongodb+srv://` URI.

---

# 4. Umgebungsvariablen konfigurieren

Lokal kann die Anwendung zum Beispiel eine `.env`-Datei verwenden:

```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/mydatabase
PORT=3000
```

Die `.env`-Datei darf nicht in GitHub gespeichert werden.

Deshalb muss sie in `.gitignore` stehen:

```gitignore
.env
node_modules/
```

Im Code kann die Variable zum Beispiel so verwendet werden:

```javascript
mongoose.connect(process.env.MONGODB_URI);
```

Für Production wird die echte Connection URI direkt in Render als Environment Variable gespeichert.

Die Daten werden also nicht in den Quellcode geschrieben.

---

# 5. GitHub Repository vorbereiten

Das Backend muss in einem GitHub Repository liegen.

Zum Beispiel:

```text
backend-project/
├── server.js
├── package.json
├── package-lock.json
├── .gitignore
└── ...
```

Vor dem Deployment teste ich die Anwendung lokal:

```bash
npm install
npm start
```

Danach teste ich die API beispielsweise mit:

```text
http://localhost:3000
```

Wenn alles funktioniert, pushe ich den Code zu GitHub.

---

# 6. Render mit GitHub verbinden

Ich öffne das Render Dashboard.

Dann wähle ich:

```text
New
→ Web Service
```

Render kann anschließend mit meinem GitHub-Konto verbunden werden.

Ich wähle das Repository aus, das mein Backend enthält.

Render dokumentiert diesen Prozess für Node.js/Express-Anwendungen offiziell.

---

# 7. Build Command und Start Command konfigurieren

Für eine normale Node.js-Anwendung kann ich folgende Einstellungen verwenden:

```text
Build Command:
npm install
```

und:

```text
Start Command:
npm start
```

Wenn das Projekt einen anderen Start-Befehl benötigt, muss dieser entsprechend angepasst werden.

Zum Beispiel kann der Start-Befehl auch direkt sein:

```bash
node server.js
```

Render führt beim Deployment den Build Command aus und startet danach die Anwendung mit dem Start Command.

---

# 8. Environment Variables auf Render einrichten

Im Render Dashboard öffne ich die Einstellungen des Web Services.

Dort gehe ich zu:

```text
Environment
```

Ich füge die benötigten Variablen hinzu:

```text
MONGODB_URI = mongodb+srv://username:password@cluster.mongodb.net/mydatabase
```

Falls die Anwendung weitere Secrets verwendet, werden diese ebenfalls als Environment Variables gespeichert.

Zum Beispiel:

```text
JWT_SECRET = ********
API_KEY = ********
```

Diese Werte werden nicht in das Git Repository geschrieben.

---

# 9. Deployment starten

Nachdem Repository, Build Command, Start Command und Environment Variables konfiguriert wurden, starte ich das Deployment.

Render lädt den Code aus GitHub herunter.

Danach passiert ungefähr Folgendes:

```text
GitHub
   ↓
Code herunterladen
   ↓
npm install
   ↓
Dependencies installieren
   ↓
Environment Variables laden
   ↓
npm start
   ↓
Express Server startet
   ↓
MongoDB Atlas Verbindung
   ↓
Öffentliche URL
```

Wenn der Build erfolgreich ist, stellt Render die Anwendung unter einer öffentlichen URL zur Verfügung.

Zum Beispiel:

```text
https://my-backend.onrender.com
```

Render stellt nach erfolgreichem Deployment eine `onrender.com`-URL bereit.

---

# 10. Automatisches Deployment bei GitHub Push

Render kann mit einer GitHub-Branch verbunden werden.

Wenn ich danach eine Änderung mache:

```bash
git add .
git commit -m "Update API"
git push
```

erkennt Render den neuen Push und startet automatisch einen neuen Build und ein neues Deployment.

Dadurch muss ich das Deployment nicht jedes Mal manuell starten.

---

# 11. Deployment testen

Nach dem Deployment teste ich zuerst die öffentliche URL.

Zum Beispiel:

```text
https://my-backend.onrender.com
```

Danach teste ich die API-Endpunkte.

Zum Beispiel:

```text
GET /api/users
```

Ich kann dafür den Browser, Postman oder Thunder Client verwenden.

Wenn der Endpoint Daten aus MongoDB Atlas zurückgibt, funktioniert die Verbindung zur Datenbank.

Zum Beispiel:

```json
[
  {
    "name": "Olha",
    "email": "test@example.com"
  }
]
```

Danach teste ich einen POST-Request:

```text
POST /api/users
```

mit zum Beispiel:

```json
{
  "name": "Test User",
  "email": "test@example.com"
}
```

Wenn der neue Datensatz erfolgreich gespeichert wird, kann ich in MongoDB Atlas überprüfen, ob der Datensatz tatsächlich in der Datenbank vorhanden ist.

Damit habe ich getestet:

```text
Client
  ↓
Public API
  ↓
Express
  ↓
Mongoose
  ↓
MongoDB Atlas
```

---

# 12. Fehlerbehebung

Wenn das Deployment nicht funktioniert, überprüfe ich zuerst die Logs auf Render.

Typische Probleme sind:

- falscher Start Command;
- fehlende Environment Variables;
- falsche MongoDB Connection URI;
- falscher Database User oder falsches Passwort;
- MongoDB Atlas blockiert die Verbindung wegen der IP Access List;
- Fehler im Node.js-Code;
- fehlende Dependencies in `package.json`.

Bei einer fehlgeschlagenen MongoDB-Verbindung sollte insbesondere die IP Access List und der Database User überprüft werden. MongoDB Atlas dokumentiert beide Einstellungen als Voraussetzungen für eine Verbindung.

---

# 13. Ergebnis

Nach erfolgreichem Deployment ist die Anwendung nicht mehr nur über `localhost` erreichbar.

Die Architektur sieht dann so aus:

```text
                Internet
                    |
                    v
             Public Render URL
                    |
                    v
             Node.js + Express
                    |
                    v
                 Mongoose
                    |
                    v
             MongoDB Atlas
```

Der Code befindet sich in GitHub, die Anwendung läuft auf Render und die Daten werden in MongoDB Atlas gespeichert.

Damit ist das Backend für externe Benutzer über das Internet erreichbar.
