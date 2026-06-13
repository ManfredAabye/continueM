## TODO

### Empfohlene Vorgehensweise zur Internationalisierung

Da die Architektur in TypeScript (83,9%), JavaScript (7,7%) und anderen Sprachen vorliegt, empfehle ich folgende Schritte:

1.  **Standard-i18n-Bibliothek für VS Code Extensions wählen**  
    Verwenden Sie die etablierte Erweiterung [`vscode-l10n`](https://code.visualstudio.com/api/references/vscode-api#l10n) von Microsoft. Sie ist offiziell für VS Code-Extensions gedacht und unterstützt die Auswahl der Anzeigesprache basierend auf der VSCode-UI-Sprache des Benutzers. Alternativ können Sie `i18next` verwenden, das flexibler, aber auch aufwändiger in der Integration ist.

2.  **Schlüsselstellen im Code identifizieren**  
    Suchen Sie nach allen Benutzeroberflächentexten:
    *   Befehlsnamen (`package.json` → `contributes.commands`)
    *   Statusleistenmeldungen
    *   QuickPick-Menüs
    *   Benachrichtigungen und Fehlermeldungen
    *   Konfigurationsbeschreibungen (`package.json` → `contributes.configuration`)
    *   Tooltips und Hover-Informationen

3.  **Übersetzungsdateien anlegen**  
    Erstellen Sie für jede der vier Sprachen eine eigene Datei im Format `package.nls.{locale}.json` (z.B. `package.nls.de.json`). Die Standarddatei ist `package.nls.json` (Englisch). Strukturieren Sie sie so:
    ```json
    {
        "extension.command.runContinue": "Run Continue Check",
        "extension.status.running": "Continue is analyzing code...",
        "extension.error.noConfig": "No Continue configuration found."
    }
    ```

4.  **Build-Prozess anpassen**  
    Da das Repository bereits ein komplexes Build-Setup mit TypeScript und mehreren Paketen hat, müssen Sie:
    *   `package.json` um Skripte zum Generieren der Übersetzungsdateien erweitern.
    *   Sicherstellen, dass die `l10n`-Dateien im endgültigen VSIX-Paket mitgeliefert werden.
    *   Die Webpack-Konfiguration (falls vorhanden) entsprechend anpassen.

5.  **Platzhalter und Pluralisierung beachten**  
    Nutzen Sie die `vscode.l10n.t()`-Funktion mit Platzhaltern:  
    `vscode.l10n.t('Found {0} issues in {1} files.', issueCount, fileCount)`.  
    Für Pluralformen ist `vscode-l10n` auf die Standardregeln der jeweiligen Sprache vorbereitet.

6.  **Community-getriebene Übersetzung**  
    Da das Repository keine aktive Community zu haben scheint (0 Sterne, 0 Forks, keine Releases), könnten Sie die Übersetzungsarbeit selbst leisten oder:
    *   Ein Crowdsourcing-Tool wie **POEditor** oder **Weblate** einbinden.
    *   Die rohen JSON-Dateien direkt im Repository pflegen und Contributions per Pull Request erlauben.

### Fallstricke und Besonderheiten dieses Repos
*   **Veraltete Codebasis**: Das Originalprojekt wurde "final 2.0.0" released und dann in den reinen Lesemodus versetzt. Prüfen Sie vor dem Internationalisierungsaufwand, ob das Plugin überhaupt noch wie gewünscht funktioniert (insbesondere die Hugging Face-Offline-Nutzung ist noch als ToDo markiert).
*   **JetBrains-Plugin**: Die Readme empfiehlt, stattdessen die CLI zu nutzen. Konzentrieren Sie Ihre Übersetzungsbemühungen auf die **VS Code Extension** und die **CLI**, wenn Sie konsistent sein wollen.
*   **Keine Sprachpaket-Struktur vorhanden**: Sie müssen wirklich bei null anfangen – es gibt keinerlei Vorbereitung für Übersetzungen.

### Konkreter nächster Schritt
1.  **Forken** Sie das Repository lokal.
2.  **Installieren** Sie `@vscode/l10n-dev` als Dev-Dependency.
3.  Ersetzen Sie **alle hartkodierten Strings** im Code durch `l10n.t()`-Aufrufe.
4.  **Erstellen** Sie die vier Sprachdateien (en, de, fr, es).
5.  **Testen** Sie, indem Sie VS Code mit `--locale=de` starten.
