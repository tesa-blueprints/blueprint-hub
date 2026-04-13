# tesa-blueprints

Zentrale Wissensbasis und Standards-Organisation fuer saubere, sichere Softwareentwicklung.

## Blueprints

| Repository | Beschreibung | Inhalt |
|------------|-------------|--------|
| [blueprint-project-management](https://github.com/tesa-blueprints/blueprint-project-management) | Projektmanagement Standards | Dokumentation, GitHub Projects, Branching, Code Reviews, Releases |
| [blueprint-security-advisory](https://github.com/tesa-blueprints/blueprint-security-advisory) | Sicherheitsstandards | OWASP Top 10, Clean Code, Dependencies, Secrets, Code Scanning |
| [blueprint-terraform-guide](https://github.com/tesa-blueprints/blueprint-terraform-guide) | Terraform + Azure Guide | Projektstruktur, Module, State, Azure Best Practices, CI/CD Pipelines |
| [blueprint-application-coding-guide](https://github.com/tesa-blueprints/blueprint-application-coding-guide) | Application Coding Guide | TypeScript, React/Next.js, Node.js, API Design, Testing, Auth |

## Struktur

Jedes Blueprint-Repo folgt der gleichen Struktur:

```
docs/               Menschenlesbare Guides (nummeriert)
templates/          Kopierbare Vorlagen (Configs, Workflows, etc.)
claude-config/      Claude-Konfigurationsdateien zum Kopieren in Projekte
CLAUDE.md           Claude-Regeln fuer die Arbeit am Blueprint selbst
README.md           Uebersicht und Inhaltsverzeichnis
```

## Verwendung

### Fuer Menschen

1. Das passende Blueprint-Repo oeffnen
2. Guides in `docs/` lesen
3. Templates aus `templates/` ins eigene Projekt kopieren

### Fuer Claude (AI-Assistent)

1. `claude-config/CLAUDE.md` aus dem passenden Blueprint kopieren
2. In das Root-Verzeichnis des eigenen Projekts legen
3. Claude haelt sich automatisch an die definierten Standards

```bash
# Beispiel: Security-Regeln und Coding-Guide in ein Projekt kopieren
cp blueprint-security-advisory/claude-config/CLAUDE.md mein-projekt/CLAUDE-security.md
cp blueprint-application-coding-guide/claude-config/CLAUDE.md mein-projekt/CLAUDE.md
```

## Dieses Repo

`placeholder-codespace` dient als Arbeitsumgebung (GitHub Codespace) fuer die Verwaltung der Blueprints. Es enthaelt keinen produktiven Code.
