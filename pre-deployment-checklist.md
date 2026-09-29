# Pre-Deployment Checklist

Diese Checkliste hilft dabei, eine Node.js/Express-Anwendung vor dem Deployment auf Sicherheit, Datenbankverwaltung, Fehlerbehandlung und Production-Konfiguration zu überprüfen.

---

## 1. Sicherheit

### API-Schlüssel und Secrets

- [ ] API-Schlüssel niemals direkt in den Quellcode schreiben.
- [ ] Passwörter und Connection Strings niemals in GitHub speichern.
- [ ] Geheimnisse in einer `.env`-Datei lokal speichern.
- [ ] `.env` in `.gitignore` eintragen.
- [ ] Production-Secrets als Environment Variables auf der Hosting-Plattform speichern.
- [ ] Bereits veröffentlichte Secrets sofort ändern bzw. zurücksetzen.

Beispiel:

```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/database
JWT_SECRET=my-secret
API_KEY=my-api-key
```

Diese Datei darf nicht in das Git Repository gelangen.

---

### CORS

- [ ] CORS nur für die benötigten Origins erlauben.
- [ ] Nicht unnötig `*` als Origin verwenden, wenn die API nur von bestimmten Frontends verwendet wird.
- [ ] Erlaubte HTTP-Methoden und Headers überprüfen.

Beispiel:

```javascript
app.use(
  cors({
    origin: "https://my-frontend.example.com",
  }),
);
```

---

### Security Headers

- [ ] `helmet` installieren und konfigurieren.
- [ ] Security Headers für HTTP-Antworten aktivieren.

Beispiel:

```bash
npm install helmet
```

Danach:

```javascript
const helmet = require("helmet");

app.use(helmet());
```

Helmet hilft dabei, verschiedene HTTP-Sicherheits-Header automatisch zu setzen.

---

### Rate Limiting

- [ ] Rate Limiting für öffentliche API-Endpunkte verwenden.
- [ ] Besonders Login-, Register- und andere sensible Endpunkte schützen.
- [ ] Zu viele Requests von einem Client begrenzen.

Zum Beispiel kann `express-rate-limit` verwendet werden:

```bash
npm install express-rate-limit
```

---

## 2. Datenbankverwaltung

### Connection

- [ ] Die Datenbankverbindung über Environment Variables konfigurieren.
- [ ] Keine Datenbank-Passwörter im Code speichern.
- [ ] Connection Pool bzw. bestehende Datenbankverbindungen sinnvoll verwenden.
- [ ] Maximale Anzahl der Datenbankverbindungen beachten.

---

### Datenbankänderungen

Bei Prisma sollten Änderungen an der Datenbank in Production über Migrationen durchgeführt werden.

Für Production:

```bash
npx prisma migrate deploy
```

Nicht einfach:

```bash
npx prisma db push
```

`db push` ist vor allem für Entwicklung und Prototyping geeignet.

Bei Mongoose/MongoDB sollten Änderungen an der Datenstruktur ebenfalls kontrolliert und nachvollziehbar durchgeführt werden.

---

### Indizes

- [ ] Häufig gesuchte Felder identifizieren.
- [ ] Für wichtige Suchfelder passende Datenbank-Indizes erstellen.
- [ ] Keine unnötigen Indizes erstellen.
- [ ] Queries überprüfen und bei Bedarf optimieren.

Ein Index kann Datenbankabfragen schneller machen.

Zum Beispiel kann ein Index auf einem häufig gesuchten Feld wie `email` sinnvoll sein.

---

## 3. Fehlerbehandlung und Logs

### Fehlerbehandlung

- [ ] Eine zentrale Error-Handling-Middleware verwenden.
- [ ] Interne Fehlerdetails nicht an Benutzer senden.
- [ ] Keine Stack Traces in Production an Benutzer zurückgeben.
- [ ] Sinnvolle HTTP-Statuscodes verwenden.

Für Benutzer sollte beispielsweise nur eine allgemeine Fehlermeldung angezeigt werden:

```json
{
  "error": "Internal server error"
}
```

Nicht:

```text
Error: MongoServerError...
at Connection...
at processTicksAndRejections...
```

Der vollständige Fehler kann stattdessen in den Server-Logs gespeichert werden.

---

### Production Environment

- [ ] `NODE_ENV` auf `production` setzen, wenn die Anwendung dies verwendet.
- [ ] Keine Debug-Ausgaben mit sensiblen Informationen in Production verwenden.
- [ ] Keine Passwörter oder Tokens mit `console.log()` ausgeben.

---

### Live Logs

- [ ] Nach dem Deployment die Logs der Hosting-Plattform überprüfen.
- [ ] Bei Render können die Logs im Dashboard des Web Services geöffnet werden.
- [ ] Bei Fehlern zuerst Build Logs und Runtime Logs überprüfen.

Die Logs können beispielsweise zeigen:

```text
Application started
Connected to MongoDB
GET /api/users 200
POST /api/users 201
```

Bei Fehlern können sie bei der Fehlersuche helfen.

---

## 4. Umgebung einrichten

### Dependencies

- [ ] Prüfen, welche Pakete die Anwendung wirklich benötigt.
- [ ] Produktionsabhängigkeiten in `dependencies` speichern.
- [ ] Nur Entwicklungswerkzeuge in `devDependencies` speichern.
- [ ] Nicht benötigte Pakete entfernen.

Beispiel:

```json
{
  "dependencies": {
    "express": "...",
    "mongoose": "..."
  },
  "devDependencies": {
    "nodemon": "..."
  }
}
```

`nodemon` wird normalerweise nur während der Entwicklung benötigt.

---

### Lokale Testkonfiguration

- [ ] Lokale Testdatenbanken nicht in Production verwenden.
- [ ] Test-Secrets nicht als Production-Secrets verwenden.
- [ ] Testdateien und Testkonfigurationen nicht unnötig in den Production-Build aufnehmen.
- [ ] `.env` und andere lokale Konfigurationsdateien nicht committen.

---

## 5. Git und Repository

- [ ] Alle Änderungen committen.
- [ ] Commit-Messages verständlich formulieren.
- [ ] `.gitignore` überprüfen.
- [ ] Keine Secrets im Git Repository enthalten.
- [ ] `node_modules` nicht zu Git hinzufügen.
- [ ] Repository auf unnötige Dateien überprüfen.

Beispiel für `.gitignore`:

```gitignore
node_modules/
.env
.env.local
```

---

## 6. API testen

Vor dem Deployment:

- [ ] Alle wichtigen GET-Endpunkte testen.
- [ ] POST-Endpunkte testen.
- [ ] PUT/PATCH-Endpunkte testen.
- [ ] DELETE-Endpunkte testen.
- [ ] Fehlerfälle testen.
- [ ] Ungültige Daten testen.
- [ ] Nicht autorisierte Requests testen, wenn Authentication vorhanden ist.

Nach dem Deployment:

- [ ] Öffentliche URL testen.
- [ ] API-Endpunkte über die öffentliche URL testen.
- [ ] Daten aus der Remote-Datenbank lesen.
- [ ] Neue Daten in die Remote-Datenbank schreiben.
- [ ] MongoDB A
