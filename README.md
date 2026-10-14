# GeoLib — Prototipo web del Project Work

## Requisiti

Node.js 20+, npm, Docker con Docker Compose. Connessione internet necessaria per installare le dipendenze e visualizzare le tessere OpenStreetMap.

## Avvio (3 terminali)

1. Nella cartella radice, avvia il database:

   ```bash
   docker compose up -d
   ```
2. Installa dipendenze e configura il server:

   ```bash
   npm install
   npm run install:all
   cp server/.env.example server/.env
   ```

   **Prima di utilizzare account reali, cambia `JWT_SECRET` e la password demo del database.** La configurazione inclusa è riservata allo sviluppo locale.
3. Avvia entrambi i servizi:

   ```bash
   npm run dev
   ```
4. Apri nel browser **http://localhost:5173**. L'API è su **http://localhost:3001/api**.

   Puoi registrare due utenti (con indirizzi email diversi), pubblicare un libro col primo e richiederlo col secondo. Il proprietario può poi approvare e segnare la restituzione.

## Struttura

```text
GeoLib_prototipo/
├── client/                React + Vite + react-leaflet
│   └── src/
│       ├── main.jsx       Interfaccia e logica frontend
│       └── style.css      Stile responsive
├── server/
│   ├── index.js           API REST Express
│   ├── .env.example       Configurazione locale
│   └── package.json
├── db/
│   └── schema.sql         Database + estensione PostGIS
├── docker-compose.yml    PostgreSQL / PostGIS
└── README.md
```

## API principali

| Metodo | Endpoint                                   | Funzione                              |
| ------ | ------------------------------------------ | ------------------------------------- |
| GET    | `/api/health`                            | Stato database                        |
| POST   | `/api/auth/register`                     | Registrazione                         |
| POST   | `/api/auth/login`                        | Accesso                               |
| GET    | `/api/books?lat=41.9&lon=12.5&radius=20` | Ricerca geografica                    |
| POST   | `/api/books`                             | Pubblicazione (JWT)                   |
| POST   | `/api/books/:id/loans`                   | Richiesta prestito (JWT)              |
| GET    | `/api/loans`                             | Lista prestiti propri (JWT)           |
| PATCH  | `/api/loans/:id`                         | Aggiornamento prestito (proprietario) |
| GET    | `/api/stats`                             | Statistiche account (JWT)             |
| POST   | `/api/books/:id/view`                    | Registra visualizzazione scheda       |

## Limiti e sviluppi futuri

Questo è un **prototipo universitario**, non un prodotto pronto alla pubblicazione. Mancano ancora: verifica email, reset password, sistema reale di upload immagini, notifiche, moderazione, flussi completi di consenso/informativa/privacy, gestione amministrativa, test automatici e un deployment sicuro con HTTPS. Attualmente le coordinate dei libri esistono nel backend e l'API le restituisce. La navigazione sulla mappa dipende dalla connessione internet.
