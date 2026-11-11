# rrm-api-contract

Contractul API REST pentru **Research Resource Manager**, descris în [`openapi.yaml`](openapi.yaml) (OpenAPI 3.0).

Este sursa unică de adevăr pentru comunicarea dintre:
- [rrm-backend](https://github.com/student-team-origin/rrm-backend) — implementează endpoint-urile descrise aici;
- [rrm-frontend](https://github.com/student-team-origin/rrm-frontend) — își generează tipurile TypeScript din acest fișier.

Ambele repository-uri includ acest repository ca **submodul git**, în folderul `api-contract/`.

```
rrm-backend/                    rrm-frontend/
├── api-contract/  ──┐          ├── api-contract/  ──┐
├── src/             │          ├── src/             │
└── pom.xml          │          └── package.json     │
                     └──────► rrm-api-contract ◄─────┘
```

## Ce conține
- Endpoint-urile pe module: Autentificare, Utilizatori, Granturi, Laboratoare, Resurse, Rezervări, Aprobări, Notificări, Rapoarte.
- Schemele de date. Câmpurile JSON sunt în română, la fel ca în baza de date (`denumire`, `categorieId`, `stare`...).
- Formatul comun al erorilor (`Error`: `code`, `message`, `fields`) și al listelor paginate (`PageMeta` + `items`).

## Lucrul cu submodulul (în backend sau frontend)

Clonare, cu tot cu contractul:
```bash
git clone --recurse-submodules https://github.com/student-team-origin/rrm-backend.git
```

Dacă ai clonat deja fără submodule:
```bash
git submodule update --init
```

După `git pull`, ca submodulul să ajungă la commit-ul folosit de ramura curentă:
```bash
git submodule update
```

## Cum se modifică contractul
1. Ramură în **acest** repository, de forma `feature/TO-<nr>-descriere`. Faci modificarea în `openapi.yaml`, verifici fișierul (mai jos), apoi deschizi un PR. Merge-ul se face cu squash, ca în celelalte repo-uri.
2. După merge, în backend și/sau frontend, pe ramura tichetului care folosește modificarea:
   ```bash
   git -C api-contract fetch
   git -C api-contract switch --detach origin/main
   git add api-contract
   ```
   Commit-ul din acel repository conține noul pointer al submodulului, alături de codul care implementează sau folosește modificarea.
3. În frontend se regenerează tipurile: `pnpm gen:api`.

Reguli:
- **Contractul se modifică întâi aici**, apoi în cod.
- Schimbările incompatibile (câmp redenumit sau șters, cod HTTP schimbat) se anunță echipei: backend-ul și frontend-ul trebuie actualizate împreună.

## Verificare
```bash
npx @redocly/cli@1.34.5 lint openapi.yaml
```
Fișierul trebuie să fie valid (fără erori; avertismentele se pot discuta).

## Echipa
Vladislav Shorodok · Tudor Baciu · Yaroslav Chernov · Erick
