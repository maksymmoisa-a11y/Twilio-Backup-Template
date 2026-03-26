# n8n Backup

Automatisches Backup aller n8n Workflows und Credentials als JSON-Dateien in diesem GitHub Repository.

## Funktionsweise

- **Regelmäßiger Abruf**: Eine GitHub Action ruft alle 4 Stunden die n8n API ab
- **Einzeldateien**: Jeder Workflow und jedes Credential wird als eigene JSON-Datei gespeichert
- **Nur Änderungen**: Ein Commit wird nur erstellt, wenn sich tatsächlich etwas geändert hat
- **Orphan-Erkennung**: Gelöschte Workflows/Credentials werden automatisch aus dem Backup entfernt
- **Pagination**: Unterstützt mehr als 100 Workflows/Credentials

## Dateistruktur

```
├── workflows/
│   ├── {id}_{name}.json
│   └── ...
└── credentials/
    ├── {id}_{name}.json
    └── ...
```

## Einrichtung

### 1. n8n API Key

1. Öffne deine n8n-Instanz unter **Settings** → **API**
2. Erstelle einen neuen API Key

### 2. GitHub Environment anlegen

1. Gehe zu diesem Repository → **Settings** → **Environments**
2. Erstelle ein Environment namens `production`

### 3. Secret und Variable anlegen

1. Im Environment `production` → **Add secret**
2. Name: `N8N_API_KEY`, Value: Dein n8n API Key
3. Unter **Add variable** → Name: `N8N_URL`, Value: Die URL deiner n8n-Instanz (z.B. `https://n8n.example.com`)

### 4. Fertig!

Der Workflow läuft ab sofort automatisch alle 4 Stunden.

## Manuell auslösen

1. Gehe zum Tab **Actions**
2. Wähle **Backup n8n** in der linken Seitenleiste
3. Klicke **Run workflow** → **Run workflow**

## Technische Details

| Eigenschaft | Wert |
|---|---|
| Zeitplan | Alle 4 Stunden (`0 */4 * * *`) |
| API-Endpunkte | `GET /api/v1/workflows`, `GET /api/v1/credentials` |
| JSON-Format | Sortierte Keys, Pretty Print |
| Authentifizierung | `GITHUB_TOKEN` (automatisch), n8n API Key |
| Commit-Author | `github-actions[bot]` |
