# What's New - Release Relevance Checker

Analysiert neue Claude Code Releases und zeigt dir, was für deinen Workflow relevant ist.

## Anleitung

Wenn der User `/whats-new` aufruft oder nach neuen Claude Code Features fragt:

### 1. Release Notes holen

Führe aus:
```bash
curl -s https://api.github.com/repos/anthropics/claude-code/releases/latest | jq -r '.tag_name, .body'
```

Oder für die letzten 5 Releases:
```bash
curl -s https://api.github.com/repos/anthropics/claude-code/releases | jq -r '.[:5][] | "## \(.tag_name)\n\(.body)\n"'
```

### 2. Projekt-Context laden

Lies die folgenden Dateien (falls vorhanden):
- `CLAUDE.md` - Projekt-Memory und Konventionen
- `.claude/settings.json` - Hooks, Permissions, Environment
- `.claude/commands/` - Custom Commands
- `.claude/skills/` - Installierte Skills
- `.mcp.json` - MCP Server Konfiguration

### 3. Analyse durchführen

Vergleiche die Release Notes mit dem Projekt-Setup und beantworte:

**Relevante neue Features:**
- Welche neuen Features passen zu den verwendeten Workflows?
- Welche lösen Probleme die in CLAUDE.md erwähnt werden?

**Konkrete Aktionen:**
- Was sollte der User sofort ausprobieren?
- Welche Config-Änderungen sind empfohlen?

**Veraltete Workarounds:**
- Gibt es Workarounds in CLAUDE.md oder Commands die jetzt nativ gelöst sind?
- Welche Custom Commands können durch neue Built-in Features ersetzt werden?

**Nicht relevant (kurz):**
- Features die für dieses Projekt nicht relevant sind (z.B. Sprachen die nicht verwendet werden)

### 4. Output Format

```
## 🚀 Claude Code [VERSION] - Was ist neu für dich?

### ✅ Sofort relevant
- **[Feature Name]**: [Warum relevant] → [Konkrete Aktion]

### 💡 Interessant für später
- **[Feature]**: [Kurze Erklärung]

### 🧹 Aufräumen
- [Workaround/Command] kann entfernt werden → jetzt nativ via [Feature]

### ⏭️ Nicht relevant für dieses Projekt
[Kurze Liste, keine Details nötig]

---
Aktuelle Version prüfen: `claude --version`
Update: `claude update`
```

## Beispiel-Aufruf

User: `/whats-new`

User: "Was gibt's Neues in Claude Code?"

User: "Welche neuen Features sollte ich mir anschauen?"

User: `/whats-new 5` (letzte 5 Releases)
