# YRewrite Integration

## Übersicht

Der Consent Manager bietet eine nahtlose Integration mit dem YRewrite-Addon für die Verwaltung von Domains.

## Features

### 1. Domain-Auswahl aus YRewrite

Beim Anlegen einer neuen Domain im Consent Manager können Sie:

- **Aus YRewrite wählen**: Alle in YRewrite konfigurierten Domains werden in einem Dropdown angezeigt
- **Manuell eingeben**: Option "_Manuell eingeben_" für Domains, die nicht in YRewrite konfiguriert sind

**Vorteile:**
- Keine Tippfehler bei Domain-Namen
- Konsistente Schreibweise zwischen YRewrite und Consent Manager
- Schnellere Einrichtung

### 2. Automatische Synchronisation

Wenn eine Domain in YRewrite gelöscht wird, wird sie automatisch auch aus dem Consent Manager entfernt.

**Technische Details:**
- Extension Point: `YREWRITE_DOMAIN_DELETED`
- Automatische Cache-Aktualisierung
- Backend-Benachrichtigung über gelöschte Domains

**Vorteile:**
- Keine verwaisten Einträge
- Datenbank-Konsistenz
- Weniger manuelle Wartung

## Verwendung

### Domain hinzufügen

1. **Consent Manager → Domains** aufrufen
2. "Hinzufügen" klicken
3. Im Dropdown:
   - YRewrite-Domain auswählen ODER
   - "Manuell eingeben" wählen und eigene Domain eingeben
4. Weitere Einstellungen vornehmen
5. Speichern

**Domain-Bereinigung:**
Domains werden automatisch bereinigt und normalisiert:
- `https://example.com` → `example.com`
- `http://www.example.com` → `example.com`
- `example.com/subfolder` → `example.com`
- `example.com:8080` → `example.com`
- Automatische Konvertierung zu Kleinbuchstaben

Dies stellt sicher, dass Domains immer im korrekten Format gespeichert werden, unabhängig davon, wie sie in YRewrite eingegeben wurden.

### Domain bearbeiten

- Bereits angelegte Domains können wie gewohnt bearbeitet werden
- Der Domain-Name ist bei bestehenden Einträgen ein normales Textfeld
- **Hinweis:** Bei YRewrite-Domains wird automatisch eine Info angezeigt, dass die Domain synchronisiert wird

### Domain löschen

Zwei Wege zum Löschen:

1. **Manuell im Consent Manager** - Normal über die Lösch-Funktion
2. **Automatisch via YRewrite** - Domain wird in YRewrite gelöscht → Consent Manager entfernt sie automatisch

## Technische Implementierung

### Extension Point

```php
// boot.php
rex_extension::register('YREWRITE_DOMAIN_DELETED', function($ep) {
    $deletedDomain = $ep->getParam('domain');
    $domainName = $deletedDomain->getName();
    
    // Consent Manager Domain löschen
    // Cache aktualisieren
});
```

### JavaScript für manuelle Eingabe

Das UI bietet einen nahtlosen Wechsel zwischen Dropdown und manuellem Eingabefeld:

- Select zeigt YRewrite-Domains
- Option "_Manuell eingeben_" blendet Textfeld ein
- "Zurück zur Auswahl"-Button für Rückkehr zum Dropdown
- Automatische Konvertierung zu Kleinbuchstaben bei Submit

## Anforderungen

- **REDAXO**: >= 5.x
- **YRewrite**: Installiert und aktiviert
- **Consent Manager**: Aktuelle Version

## Fallback

Wenn YRewrite nicht installiert oder deaktiviert ist:
- Normales Textfeld für Domain-Eingabe (wie zuvor)
- Keine Änderung am bestehenden Verhalten
- Vollständig rückwärtskompatibel

## Best Practices

1. **Domains zuerst in YRewrite anlegen** - dann im Consent Manager auswählen
2. **Kleinschreibung beachten** - Domains werden automatisch in Kleinbuchstaben konvertiert
3. **Protokoll weglassen** - Nur `example.com`, nicht `https://example.com`

## Vorteile für Multisite-Setups

- **Zentrale Verwaltung**: Alle Domains in YRewrite pflegen
- **Konsistenz**: Keine Abweichungen zwischen Addons
- **Effizienz**: Ein Klick statt manueller Eingabe
- **Fehlerfreiheit**: Keine Tippfehler bei Domain-Namen

## Changelog

**Version 4.5.0+**
- YRewrite-Integration hinzugefügt
- Automatische Synchronisation bei Domain-Löschung
- Domain-Auswahl aus YRewrite-Dropdown

---

**Hinweis:** Diese Integration ist optional und funktioniert auch ohne YRewrite. Bei Nicht-Verfügbarkeit von YRewrite wird das Standard-Eingabefeld verwendet.
