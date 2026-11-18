# waechter-config

Öffentliche Ereignis-Liste für einen privaten Ereignis-Wächter.

- `watches.json` wird automatisch vom Trading-Radar gepflegt.
- Ein Skript auf einem Server liest die Datei alle 5 Minuten und prüft die Einträge in ihrem Zeitfenster.
- Enthält bewusst keine Zugangsdaten und kein Push-Thema.

Felder eines Eintrags in `watches[]`:
`id` (eindeutig), `type` (`contains` | `regex` | `changed`), `url`, `needle` / `pattern` / `extract`,
`active_from`, `active_until` (ISO 8601 mit Zeitzone, z. B. `2026-11-20T21:55:00+01:00`),
`title`, `message` (`{wert}` = Treffer aus Regex-Gruppe 1).

### Typ `sec8k` (SEC-8-K-Wächter)

Fragt die öffentlichen EDGAR-Einreichungen einer US-Firma ab (data.sec.gov) und meldet, sobald
im Zeitfenster ein 8-K mit SEC-Annahmezeit innerhalb des Fensters erscheint. Kein `url`, kein Muster.

- `ticker` (z. B. `"NVDA"`) **oder** `cik`
- `items` (optional): z. B. `["2.02"]` = Quartalszahlen; weglassen = jedes 8-K
- `forms` (optional, Standard `["8-K"]`)
- `repeat` (optional, `true` = jedes neue 8-K im Fenster melden statt nur das erste)
- `message`-Platzhalter: `{wert}` = Items, `{zeit}` = Annahmezeit (deutsche Zeit), `{form}`, `{link}`

```json
{
  "id": "nvda-zahlen-2027-02-25",
  "type": "sec8k",
  "ticker": "NVDA",
  "items": ["2.02"],
  "active_from": "2027-02-25T22:00:00+01:00",
  "active_until": "2027-02-25T23:30:00+01:00",
  "title": "NVDA: Zahlen-8-K da",
  "message": "Items {wert} um {zeit}. Plan: ..."
}
```

Die von der SEC verlangte Absender-Kennung liegt nur auf dem Server, nicht in diesem Repo.
