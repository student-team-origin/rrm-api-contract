# CLAUDE.md — rrm-api-contract

Contractul OpenAPI 3.0 al Research Resource Manager (`openapi.yaml`). Este inclus ca submodul (`api-contract/`) în `rrm-backend` și `rrm-frontend`.

## Flux de lucru
- Fiecare modificare are un tichet Jira (`TO-<nr>`). Dacă utilizatorul nu a dat cheia, întreabă-l înainte de a crea ramura. Nu inventa chei.
- Nu face niciodată commit sau push direct pe `main`. Ramura are forma `feature/TO-<nr>-descriere`, iar modificarea intră prin PR cu minim 1 aprobare și squash merge.
- Commit-urile au forma `feat(contract): TO-<nr> descriere` (tipuri: `feat`, `fix`, `docs`, `chore`).

## Reguli pentru `openapi.yaml`
- **Căile sunt în engleză, la plural** (`/resources`, `/resource-categories`). **Câmpurile JSON și enumerările sunt în română**, ca în baza de date (`denumire`, `laboratorId`, `StareResursa: [Activa, Mentenanta, Defecta]`).
- Schemele se definesc în `components/schemas` și se refolosesc prin `$ref`. Schemele de intrare au sufixul `Create` (ex. `ResursaCreate`).
- Toate erorile folosesc schema `Error` (`code`, `message`, `fields`), prin răspunsurile comune din `components/responses` (`BadRequest`, `NotFound`, `Conflict`...). Codurile au forma `UPPER_SNAKE_CASE`, în română (ex. `INTERVAL_OCUPAT`).
- Listele paginate: `allOf` cu `PageMeta` + `items`, cu parametrii `Page` și `Size`.
- Fiecare operație are `operationId`, `tags`, `summary` (în română) și codurile de răspuns relevante.
- **Schimbările incompatibile** (redenumire sau ștergere de câmp, cod HTTP schimbat) trebuie semnalate explicit în descrierea PR-ului: backend-ul și frontend-ul trebuie actualizate.

## Verificare înainte de PR
```bash
npx @redocly/cli@1.34.5 lint openapi.yaml
```
Nu se acceptă erori.
