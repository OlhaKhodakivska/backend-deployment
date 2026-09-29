# Backend Deployment: From localhost to Production

## Teil 1: Deployment-Konzepte und Grundlagen

### 1. Was ist Deployment?

Backend-Deployment bedeutet, dass eine Anwendung von einem lokalen Computer auf einen Server bzw. eine Cloud-Plattform übertragen und dort gestartet wird.

Während der Entwicklung läuft eine Anwendung zum Beispiel auf `localhost`. Sie ist dann normalerweise nur auf dem eigenen Computer erreichbar.

Beim Deployment wird die Anwendung auf einem Server ausgeführt, der über das Internet erreichbar ist. Dadurch können andere Benutzerinnen und Benutzer auf die API zugreifen.

Deployment ist für eine Anwendung in Production notwendig, weil echte Benutzer nicht auf meinen lokalen Computer zugreifen können.

---

### 2. Der Deployment-Prozess

Ein einfacher Deployment-Prozess sieht so aus:

1. Ich entwickle und teste meine Node.js/Express-Anwendung lokal.
2. Ich speichere den Code in einem Git-Repository.
3. Ich pushe den Code zu GitHub.
4. Eine Hosting-Plattform wie Render wird mit dem GitHub-Repository verbunden.
5. Nach einem Push erkennt Render die Änderung und startet automatisch ein neues Deployment.
6. Die Plattform lädt den Code herunter.
7. Die benötigten Dependencies werden installiert.
8. Die Environment Variables werden geladen.
9. Die Anwendung wird mit dem Start-Befehl gestartet.
10. Die Anwendung verbindet sich mit der externen Datenbank.
11. Die Plattform stellt die Anwendung über eine öffentliche URL zur Verfügung.

Danach kann ein Benutzer eine HTTP-Anfrage an die API senden und die Express-Anwendung kann darauf antworten.

---

### 3. Die Einschränkungen von localhost

`localhost` bezeichnet den eigenen Computer. Eine Anwendung auf `localhost` ist normalerweise nur vom eigenen Computer erreichbar.

Das ist für echte Benutzer ungeeignet, weil:

- andere Personen nicht auf meinen Computer zugreifen können;
- mein Computer ständig eingeschaltet sein müsste;
- meine Internetverbindung ständig funktionieren müsste;
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
- Datenbank und Backend können unabhängig skaliert werden.
- Managed Database Services übernehmen viele Aufgaben wie Wartung und Verwaltung.
- Ein Problem mit dem Backend muss nicht automatisch die Datenbank betreffen.
- Sicherheitsregeln und Zugriffsrechte können getrennt eingerichtet werden.

Für kleine Lernprojekte kann man Backend und Datenbank auch auf einem Server betreiben. In professionellen Anwendungen ist die Trennung jedoch sehr üblich.

---

# Teil 2: Plattformüberblick und Optionen für Lernende

## 1. Plattformrecherche

Ich habe zwei Plattformen für Backend-Hosting und zwei Database-as-a-Service-Anbieter verglichen.

### Backend-Hosting

## Render

Render bietet einen kostenlosen Free-Plan für Web Services an.

Für ein Node.js/Express-Projekt kann man zum Beispiel folgende Befehle verwenden:

```bash
npm install
npm start
```

Der kostenlose Web Service bietet unter anderem:

- 512 MB RAM;
- weniger als 1 CPU;
- 750 kostenlose Instance-Stunden pro Workspace und Kalendermonat;
- automatisches Herunterfahren nach 15 Minuten ohne eingehenden Traffic.

Wenn ein kostenloser Web Service nach 15 Minuten keine eingehenden Anfragen erhält, wird er heruntergefahren. Bei der nächsten Anfrage startet er wieder. Dieser Start kann ungefähr eine Minute dauern.

Wenn alle 750 kostenlosen Instance-Stunden verbraucht wurden, werden die kostenlosen Web Services bis zum Beginn des nächsten Monats pausiert. Die Stunden werden am Monatsanfang auf 750 zurückgesetzt und nicht übertragen.

Render weist ausdrücklich darauf hin, dass kostenlose Instanzen für Lernen, Hobbyprojekte und Tests gedacht sind und nicht für Production-Anwendungen.

Die aktuellen Compute-Preise beginnen bei:

| Render Web Service                     |     Preis |
| -------------------------------------- | --------: |
| Free                                   |  $0/Monat |
| Starter: 512 MB RAM, weniger als 1 CPU |  $7/Monat |
| Standard: 2 GB RAM, 1 CPU              | $25/Monat |
| 2 CPU, 4 GB RAM                        | $85/Monat |

Die Preise können sich je nach Konfiguration und Region ändern.

---

## Railway

Railway bietet ebenfalls Hosting für Node.js-Anwendungen.

Der aktuelle Free Plan kostet:

```text
$0 / Monat
```

und beinhaltet:

```text
$1 kostenloses Nutzungsguthaben pro Monat
```

Der Free Plan erlaubt pro Service maximal:

- 0,5 GB RAM;
- 1 vCPU;
- 1 Replica;
- 1 GB temporären Speicher;
- 0,5 GB Volume Storage.

Neue Benutzer erhalten zusätzlich einmalig $5 kostenloses Guthaben für den Trial. Das Trial-Guthaben ist einmalig und kann nicht erneut monatlich verwendet werden.

Die aktuellen Pläne sind:

| Railway Plan |                              Preis |
| ------------ | ---------------------------------: |
| Free         | $0/Monat + $1 kostenloses Guthaben |
| Hobby        |                           $5/Monat |
| Pro          |                          $20/Monat |
| Enterprise   |                        individuell |

Railway verwendet ein nutzungsabhängiges Preismodell. Ressourcen wie CPU, RAM und Netzwerk werden entsprechend der Nutzung berechnet.

Railway weist außerdem darauf hin, dass für kostenpflichtige Nutzung eine Zahlungsmethode erforderlich ist. Wenn ein Guthabenmodell verwendet wird und das Guthaben aufgebraucht ist, werden die Workloads gestoppt, bis wieder ausreichend Guthaben vorhanden ist.

---

# Database-as-a-Service

## MongoDB Atlas

MongoDB Atlas ist ein Managed Database Service für MongoDB.

Der kostenlose Free Cluster kostet:

```text
$0
```

und ist dauerhaft kostenlos.

Er bietet:

- 512 MB Speicher;
- Shared RAM;
- Shared vCPU;
- bis zu 100 Operationen pro Sekunde.

Der Free Cluster ist besonders für Lernen und kleine Projekte geeignet.

Für größere Projekte bietet MongoDB Atlas unter anderem Flex und Dedicated an.

Aktuelle Preise:

| MongoDB Atlas |                                Preis |
| ------------- | -----------------------------------: |
| Free          |                             $0/Monat |
| Flex          | ab $0.011/Stunde, bis etwa $30/Monat |
| Dedicated     | ab $0.08/Stunde bzw. ab $56.94/Monat |

Der Flex-Tarif ist nutzungsabhängig. Bei kontinuierlicher Nutzung liegt der Preis je nach Operations-Level zwischen etwa $8 und $30 pro Monat. Dedicated Cluster beginnen bei etwa $56.94 pro Monat.

---

## Neon

Neon ist ein Managed-PostgreSQL-Service. Deshalb kann Neon beispielsweise mit PostgreSQL und Prisma verwendet werden.

Der Free Plan kostet:

```text
$0
```

Der aktuelle Free Plan bietet unter anderem:

- bis zu 100 Projekte;
- 100 Compute-Stunden (CU-hours) pro Projekt und Monat;
- 0,5 GB Speicher pro Projekt;
- insgesamt bis zu 5 GB Speicher über bis zu 10 Projekte.

Eine Compute Unit entspricht bei Neon 1 vCPU und 4 GB RAM.

Neon verwendet ein nutzungsabhängiges Preismodell. Die kostenpflichtigen Pläne berechnen Compute und Storage nach tatsächlicher Nutzung.

Neon unterstützt außerdem Scale-to-Zero. Dadurch kann eine Datenbank bei fehlender Aktivität automatisch in den Idle-Zustand wechseln und später wieder gestartet werden.

---

## Vergleich der kostenlosen Angebote

| Plattform     | Typ             | Kostenloses Angebot                        | Nach Überschreitung                                                                                               |
| ------------- | --------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| Render        | Backend Hosting | 512 MB RAM, 750 Stunden/Monat              | Free Services werden bei Erreichen des Stundenlimits pausiert; zusätzliche Bandbreite kann kostenpflichtig werden |
| Railway       | Backend Hosting | $1 Guthaben/Monat                          | Usage-based; kostenpflichtiger Hobby Plan ab $5/Monat                                                             |
| MongoDB Atlas | MongoDB         | 512 MB, dauerhaft kostenlos                | Flex oder Dedicated, nutzungsabhängig                                                                             |
| Neon          | PostgreSQL      | 100 CU-hours/Projekt/Monat, 0,5 GB/Projekt | Usage-based                                                                                                       |

Die Angaben beziehen sich auf die zum Zeitpunkt der Recherche verfügbaren offiziellen Preise und Limits.

---

## 2. Kostenanalyse

Die Plattformen unterscheiden sich deutlich beim Preismodell.

### Render

Render hat einen echten kostenlosen Web-Service-Tarif. Das ist für Lernprojekte praktisch, weil die Nutzung durch Limits begrenzt ist.

Nach dem kostenlosen Compute-Tarif beginnen bezahlte Web Services aktuell bei $7 pro Monat.

Ein wichtiger Punkt ist jedoch, dass zusätzliche Nutzung für Bandbreite und Build-Pipeline kostenpflichtig werden kann, wenn die entsprechenden Limits überschritten werden und eine Zahlungsmethode hinterlegt ist. Ohne Zahlungsmethode werden bestimmte Free Services stattdessen pausiert.

### Railway

Railway verwendet ein stärker nutzungsabhängiges Modell.

Der Free Plan enthält $1 kostenloses Guthaben pro Monat. Der Hobby Plan kostet $5 pro Monat und enthält ein Nutzungsguthaben von $5. Zusätzliche Nutzung kann berechnet werden.

Deshalb sollte man bei Railway die Nutzung und die Billing-Einstellungen regelmäßig kontrollieren.

### MongoDB Atlas

Der MongoDB Atlas Free Cluster ist dauerhaft kostenlos und hat ein festes Limit von 512 MB.

Wenn mehr Ressourcen benötigt werden, kann man auf Flex oder Dedicated wechseln. Flex ist nutzungsabhängig und kostet bis zu etwa $30 pro Monat. Dedicated beginnt bei etwa $56.94 pro Monat.

### Neon

Neon bietet einen kostenlosen Free Plan mit begrenzten Compute- und Storage-Ressourcen.

Die kostenpflichtige Nutzung ist ebenfalls usage-based. Man bezahlt entsprechend der tatsächlich verwendeten Compute- und Storage-Ressourcen.

### Optionen für Lernende

Für ein kleines Lernprojekt sind Angebote mit klaren kostenlosen Limits besonders praktisch.

Eine mögliche Kombination ist:

```text
Backend:
Render Free

Database:
MongoDB Atlas Free

Code:
GitHub
```

Diese Kombination eignet sich gut für ein kleines Node.js/Express/Mongoose-Lernprojekt.

Bei allen Plattformen sollte man trotzdem die aktuellen Usage- und Billing-Einstellungen kontrollieren, bevor man eine Zahlungsmethode hinterlegt.

---

# Teil 3: Limits kostenloser Tarife einfach erklärt

## 1. RAM-/Speicherlimits

RAM ist der Arbeitsspeicher des Servers.

Wenn ein Server zum Beispiel 512 MB RAM hat, kann die Anwendung nur eine begrenzte Menge an Daten gleichzeitig im Arbeitsspeicher halten.

Bei einer Node.js-Anwendung kann zu hoher RAM-Verbrauch dazu führen, dass:

- die Anwendung langsamer wird;
- Prozesse beendet werden;
- die Anwendung neu gestartet wird;
- Anfragen fehlschlagen.

Deshalb sollte eine Anwendung nicht unnötig viele Daten gleichzeitig im Speicher halten.

Bei Render verfügt ein Free Web Service über 512 MB RAM.

---

## 2. Cold Starts / Ruhephasen / Inaktivitäts-Timeouts

Bei kostenlosen Hosting-Angeboten kann ein Server nach einer bestimmten Zeit ohne Anfragen heruntergefahren werden.

Bei Render wird ein kostenloser Web Service nach 15 Minuten ohne eingehenden Traffic heruntergefahren.

Wenn anschließend eine neue Anfrage kommt, wird der Service wieder gestartet. Dieser Start kann ungefähr eine Minute dauern.

Für Benutzer bedeutet das, dass die erste Anfrage nach einer längeren Pause deutlich länger dauern kann.

Danach läuft der Server wieder normal.

---

## 3. Rechenstunden und CPU-Kontingente

CPU-Zeit beschreibt, wie lange die Rechenressourcen einer Anwendung verwendet werden.

Render bietet für Free Web Services 750 Instance-Stunden pro Workspace und Kalendermonat.

Ein Free Web Service verbraucht diese Stunden nur während er läuft. Während eines Idle-Sleep-Zustands werden keine Free Instance Hours verbraucht.

Wenn alle 750 Stunden verbraucht wurden, werden die kostenlosen Web Services bis zum Beginn des nächsten Monats pausiert.

Das bedeutet nicht, dass die Anwendung nach 750 einzelnen Anfragen stoppt. Es geht um die Zeit, in der die Serverinstanz aktiv ist.

---

## 4. Datenbankspeicher und Limits für aktive Verbindungen

Der Datenbankspeicher bestimmt, wie viele Daten in einer Datenbank gespeichert werden können.

MongoDB Atlas Free bietet beispielsweise 512 MB Speicher. Wenn mehr Daten gespeichert werden müssen, reicht der Free Cluster nicht mehr aus.

Auch die Anzahl der Datenbankverbindungen ist wichtig.

Mongoose verwendet einen Connection Pool. Dadurch können mehrere Datenbankanfra
