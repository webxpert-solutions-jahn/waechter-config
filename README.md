# waechter-config

Öffentliche Ereignis-Liste für einen privaten Ereignis-Wächter.

- `watches.json` wird automatisch vom Trading-Radar gepflegt.
- Ein Skript auf einem Server liest die Datei alle 5 Minuten und prüft die Einträge in ihrem Zeitfenster.
- Enthält bewusst keine Zugangsdaten und kein Push-Thema.

Felder eines Eintrags in `watches[]`:
`id` (eindeutig), `type` (`contains` | `regex` | `changed`), `url`, `needle` / `pattern` / `extract`,
`active_from`, `active_until` (ISO 8601 mit Zeitzone, z. B. `2026-11-20T21:55:00+01:00`),
`title`, `message` (`{wert}` = Treffer aus Regex-Gruppe 1).
