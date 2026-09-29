# Backend Deployment: From localhost to Production

## Teil 1: Deployment-Konzepte und Grundlagen

### 1. Was ist Deployment?

Backend-Deployment bedeutet, dass eine Anwendung von einem lokalen Computer auf einen Server bzw. eine Cloud-Plattform übertragen wird. Danach kann die Anwendung über das Internet erreichbar sein.

Während der Entwicklung läuft die Anwendung zum Beispiel auf `localhost`. Das ist nur auf dem eigenen Computer verfügbar. Beim Deployment wird die Anwendung auf einem Server gestartet, der rund um die Uhr Anfragen von Benutzerinnen und Benutzern bearbeiten kann.

Deployment ist für eine Anwendung in Production notwendig, weil echte Benutzer nicht auf meinen lokalen Computer zugreifen können.

---

### 2. Der Deployment-Prozess

Ein einfacher Deployment-Prozess sieht so aus:

1. Ich entwickle und teste meine Node.js/Express-Anwendung lokal.
2. Ich speichere den Code in einem Git-Repository.
3. Ich pushe den Code zu GitHub.
4. Eine Hosting-Plattform wie Render wird mit dem GitHub-Repository verbunden.
5. Nach einem Push erkennt Render die Änderung und startet automatisch ein neues Deployment.
6. Render lädt den Code herunter und installiert die benötigten Dependencies.
7. Die Anwendung wird mit dem angegebenen Start-Befehl gestartet.
8. Die Umgebungsvariablen werden aus der Hosting-Plattform geladen.
9. Die Anwendung verbindet sich mit der externen Datenbank.
10. Render stellt die Anwendung über eine öffentliche URL zur Verfügung.

Danach kann ein Benutzer eine HTTP-Anfrage an die API senden und die Express-Anwendung kann darauf antworten.

---

### 3. Die Einschränkungen von localhost

`localhost` bezeichnet den eigenen Computer. Eine Anwendung auf `localhost` ist normalerweise nur vom eigenen Computer erreichbar.

Das ist für echte Benutzer ungeeignet, weil:

- andere Personen nicht auf meinen Computer zugreifen können;
- mein Computer ständig eingeschaltet sein müsste;
- meine Internetverbindung funktionieren müsste;
- mein Computer ausreichend Leistung für alle Anfragen haben müsste;
- Sicherheitsprobleme entstehen könnten;
- es keine zuverlässige Produktionsumgebung ist.

Ein Hosting-Anbieter stellt dagegen Server und Infrastruktur für den Betrieb der Anwendung bereit.

---

### 4. Trennung von Zuständigkeiten

In Production werden Backend und Datenbank häufig auf getrennten verwalteten Diensten betrieben.

Zum Beispiel:

```text
User
  |
  v
Render
Node.js + Express API
  |
  v
MongoDB Atlas
Database
```

Die Trennung hat mehrere Vorteile:

- Die Datenbank ist nicht direkt Teil des Webservers.
- Datenbanken können unabhängig skaliert werden.
- Managed Database Services übernehmen viele Aufgaben wie Backups, Updates und Monitoring.
- Ein Problem mit dem Backend muss nicht automatisch die Datenbank betreffen.
- Sicherheitsregeln und Zugriffsrechte können getrennt eingerichtet werden.

Für kleine Lernprojekte kann man Backend und Datenbank auch auf einem Server betreiben. In professionellen Anwendungen ist die Trennung aber sehr üblich.

---

# Teil 2: Plattformüberblick und Optionen für Lernende

## 1. Plattformrecherche

Ich habe zwei Plattformen für Backend-Hosting und zwei Database-as-a-Service-Anbieter verglichen.

### Backend-Hosting

#### Render

Render bietet einen kostenlosen Free-Plan für Web Services an.

Für ein Node.js/Express-Projekt kann man zum Beispiel folgende Befehle verwenden:

```bash
npm install
npm start
```

Ein Free Web Service hat unter anderem:

- 750 kostenlose Instance-Stunden pro Monat;
- 512 MB RAM;
- begrenzte CPU-Ressourcen;
- automatisches Herunterfahren nach 15 Minuten ohne Aktivität;
- automatische Bereitstellung nach einem Push zu GitHub.

Wenn die 750 Stunden vollständig verbraucht sind, werden die kostenlosen Web Services bis zum nächsten Monat pausiert.

Render gibt an, dass der Free-Plan vor allem für Lernen, Testprojekte und Hobbyprojekte gedacht ist.

---

#### Railway

Railway bietet ebenfalls eine Möglichkeit, Node.js-Anwendungen zu hosten.

Der aktuelle Free Trial beinhaltet:

- einmalig 5 USD Guthaben;
- Nutzung des Guthabens für bis zu 30 Tage;
- bis zu 1 vCPU;
- bis zu 0,5 GB RAM pro Service.

Nach Ablauf des Trials bietet Railway einen Free-Plan mit 1 USD Guthaben pro Monat. Das Guthaben wird nicht in den nächsten Monat übertragen.

Railway verwendet ein nutzungsbasiertes Preismodell. Bei kostenpflichtigen Plänen können zusätzliche Ressourcen nach Verbrauch berechnet werden.

---

### Database-as-a-Service

#### MongoDB Atlas

MongoDB Atlas ist ein Managed Database Service für MongoDB.

Der kostenlose Free Cluster bietet:

- 512 MB Speicher;
- Shared RAM;
- Shared vCPU;
- kostenlos auf Dauer.

Der Free Cluster ist ausdrücklich für Lernen und kleinere Projekte gedacht.

Wenn mehr Ressourcen benötigt werden, gibt es kostenpflichtige Optionen. Zum Beispiel beginnt ein M2 Cluster bei ungefähr 9 USD pro Monat und ein M5 Cluster bei ungefähr 25 USD pro Monat.

---

#### Neon

Neon ist ein Managed-PostgreSQL-Service und kann deshalb mit PostgreSQL und Prisma verwendet werden.

Der Free Plan bietet unter anderem:

- bis zu 100 Projekte;
- 100 Compute-Stunden pro Projekt und Monat;
- 0,5 GB Speicher pro Projekt;
- insgesamt bis zu 5 GB über 10 Projekte.

Neon verwendet ein nutzungsbasiertes Modell. Bei kostenpflichtiger Nutzung bezahlt man für die tatsächlich verwendeten Ressourcen.

---

## 2. Kostenanalyse

| Plattform     | Typ             | Kostenloses Angebot                   | Kostenmodell                   |
| ------------- | --------------- | ------------------------------------- | ------------------------------ |
| Render        | Backend Hosting | 750 Stunden/Monat, Free Web Service   | Kostenpflichtige Compute-Pläne |
| Railway       | Backend Hosting | $5 Trial, danach $1 Free Credit/Monat | Usage-based                    |
| MongoDB Atlas | MongoDB         | 512 MB kostenlos                      | M2 ab ca. $9/Monat             |
| Neon          | PostgreSQL      | 0,5 GB pro Projekt                    | Usage-based                    |

Für Lernende sind kostenlose Angebote mit klaren Limits besonders interessant.

Render ist für ein einfaches Lernprojekt praktisch, weil der Free Web Service ohne Kreditkarte genutzt werden kann und ein monatliches Stundenlimit besitzt.

MongoDB Atlas ist ebenfalls interessant, weil der kostenlose Free Cluster dauerhaft kostenlos angeboten wird.

Bei nutzungsbasierten Angeboten muss man dagegen besonders auf die Abrechnung achten.

---

# Teil 3: Limits kostenloser Tarife einfach erklärt

## 1. RAM-/Speicherlimits

RAM ist der Arbeitsspeicher des Servers.

Wenn ein Server zum Beispiel 512 MB RAM hat, kann die Anwendung nur eine begrenzte Menge an Daten gleichzeitig im Arbeitsspeicher halten.

Bei einer Node.js-Anwendung kann zu viel RAM-Verbrauch dazu führen, dass:

- die Anwendung langsamer wird;
- Prozesse beendet werden;
- die Anwendung neu gestartet wird;
- Anfragen fehlschlagen.

Deshalb sollte eine Anwendung nicht unnötig viele Daten gleichzeitig im Speicher halten.

---

## 2. Cold Starts / Ruhephasen / Inaktivitäts-Timeouts

Bei kostenlosen Hosting-Angeboten kann ein Server nach einer bestimmten Zeit ohne Anfragen heruntergefahren werden.

Bei Render wird ein kostenloser Web Service nach 15 Minuten ohne Aktivität heruntergefahren. Bei der nächsten Anfrage muss der Service wieder gestartet werden. Dieser Vorgang wird als Cold Start bezeichnet.

Für Benutzer bedeutet das, dass die erste Anfrage nach einer längeren Pause deutlich länger dauern kann.

Danach läuft der Server wieder normal.

---

## 3. Rechenstunden und CPU-Kontingente

CPU-Zeit beschreibt, wie lange die Rechenressourcen einer Anwendung verwendet werden.

Render bietet für Free Web Services 750 Instance-Stunden pro Workspace und Monat. Ein laufender Free Service verbraucht diese Stunden. Wenn alle 750 Stunden verbraucht wurden, werden die kostenlosen Web Services bis zum Beginn des nächsten Monats pausiert.

Das bedeutet nicht, dass eine Anwendung nach genau 750 einzelnen Anfragen stoppt. Es geht um die Zeit, in der die Instanz läuft.

---

## 4. Datenbankspeicher und aktive Verbindungen

Der Datenbankspeicher bestimmt, wie viele Daten in der Datenbank gespeichert werden können.

MongoDB Atlas Free Cluster bietet beispielsweise 512 MB Speicher. Dieser Speicher ist begrenzt. Wenn der Speicher voll ist, können weitere Schreiboperationen fehlschlagen.

Auch die Anzahl der Datenbankverbindungen ist wichtig.

Mongoose verwendet einen Connection Pool. Dadurch können mehrere Datenbankanfragen über bestehende Verbindungen verarbeitet werden.

Prisma verwendet ebenfalls einen Connection Pool für Datenbankverbindungen.

Wenn eine Anwendung zu viele Verbindungen gleichzeitig öffnet, kann das Datenbanklimit erreicht werden. Dann können neue Anfragen Fehler bekommen.

Deshalb sollte eine Backend-Anwendung nicht für jede Anfrage eine komplett neue Datenbankverbindung erstellen. Stattdessen sollte die Anwendung eine Verbindung bzw. einen Connection Pool wiederverwenden.

---

## 5. Ausgehender Datentransfer / Bandbreite

Bandbreite beschreibt die Menge an Daten, die ein Server an Benutzer oder andere Systeme sendet.

Beispiel:

Wenn eine Anwendung jeden Monat 5 GB Daten an Benutzer sendet und das kostenlose Limit 5 GB beträgt, ist das Limit erreicht.

Hoher Datentransfer kann zum Beispiel durch folgende Dinge entstehen:

- große Dateien;
- Bilder;
- Videos;
- große API-Antworten;
- viele Benutzer.

Wenn ein Limit erreicht wird, kann der Anbieter den Service einschränken, pausieren oder zusätzliche Kosten berechnen. Deshalb sollte man die Limits des jeweiligen Anbieters kontrollieren.

---

## Zusammenfassung

Für dieses Projekt würde ich folgenden Stack verwenden:

```text
Backend:
Node.js + Express

Database:
MongoDB Atlas

ODM:
Mongoose

Hosting:
Render

Code Repository:
GitHub
```

Dieser Stack ist für ein Lernprojekt einfach zu verstehen und ermöglicht einen klaren Deployment-Prozess.
