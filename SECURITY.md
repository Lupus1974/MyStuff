# Verbindliche Sicherheitsregel: Code in GitHub, Secrets ausschließlich lokal

**Gültig ab 17.11.2026.** Diese Regel gilt für dieses und alle weiteren gemeinsamen Softwareprojekte – unabhängig davon, ob ein GitHub-Repository öffentlich oder privat ist.

## Grundsatz

**GitHub enthält ausschließlich bereinigten Quellcode, Dokumentation und Beispielkonfigurationen. Geheime Zugangsdaten verbleiben ausschließlich in der lokalen Umgebung des Betreibers zu Hause.**

**Niemals in Git/GitHub speichern, auch nicht vorübergehend, in privaten Repositories oder in früheren Commits:**

- Passwörter, Zugangsdaten, API-Schlüssel, Access-/Refresh-Tokens und OAuth-Client-Secrets
- private SSH-/TLS-Schlüssel, Zertifikate mit privatem Schlüssel und Recovery-Codes
- Zugangsdaten zu Home Assistant, MQTT, WLAN, Fronius, BYD, Tuya und anderen Geräten/Diensten
- Session-Cookies, Verbindungsstrings mit Passwort, Webhooks mit Geheimwerten
- Konfigurations-Backups, Logs, Screenshots oder Testdaten, die solche Werte enthalten

## Technische Umsetzung

1. Programme lesen benötigte Geheimwerte **zur Laufzeit** aus lokalen Umgebungsvariablen oder einem lokalen, zugriffsgeschützten Secrets-Speicher, z. B. Home Assistants `secrets.yaml` oder dem Credential Manager des Betriebssystems.
2. `.env`, `secrets.yaml`, private Schlüssel, produktive Geräte-Backups und ähnliche Dateien werden **nicht eingecheckt**. Nur `.env.example` und sonstige Templates mit **fiktiven Platzhaltern** dürfen nach Prüfung nach GitHub.
3. Vor **jedem Commit und Push** die tatsächlich vorgemerkten Dateien und Änderungen kontrollieren und lokal auf versehentlich enthaltene Secrets prüfen, möglichst mit einem Secret-Scanner (z. B. Gitleaks) als Pre-Commit-/Pre-Push-Hook. Ein Scanner reduziert das Risiko, ersetzt die manuelle Prüfung aber nicht.
4. `.gitignore` ist nur eine zusätzliche Sperre: Es verhindert weder `git add -f` noch schützt es bereits versionierte Dateien oder die Git-Historie.
5. GitHub Actions und andere Cloud-Workflows dürfen keine lokalen Secrets voraussetzen oder in GitHub speichern. Wenn eine Funktion geheime Werte benötigt, läuft sie in der entsprechend geschützten lokalen Umgebung.
6. Vor Veröffentlichung oder Synchronisation: Allowlist der zulässigen Dateien, Code-Review und Prüfung auf personenbezogene Informationen sowie Infrastrukturdetails. Standard für neue Projekt-Repositories ist **privat**, jedoch auch dort ohne Secrets.

## Falls ein Secret versehentlich im Repository landet

1. **Sofort den zugehörigen Schlüssel widerrufen bzw. das Passwort ändern** und abhängige Dienste prüfen.
2. Nutzung und mögliche Kompromittierung untersuchen.
3. Betroffene Dateien bereinigen und gegebenenfalls die Git-Historie kontrolliert säubern; nur das Löschen im neuesten Commit reicht nicht aus.
4. Den Vorfall dokumentieren. Kompromittierte Werte bleiben ungültig, auch nach Bereinigung der Historie.

**Verantwortungsregel:** Vor einem Upload gilt: Im Zweifel nicht hochladen, sondern erst lokal prüfen und bereinigen. Diese Regel ist eine verbindliche Vorgabe für unsere gemeinsame Arbeit; ihre technische Durchsetzung erfordert zusätzlich lokale Schutzmaßnahmen.
