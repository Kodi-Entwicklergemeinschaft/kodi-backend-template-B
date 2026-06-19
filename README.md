<div align="center">

# 🏛️ KODI Backend — Main API (Template B)

**The core REST API behind the KODI / Template B city app — listings, services, events and citizen content.**

[![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![Express](https://img.shields.io/badge/Express-4.x-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com)
[![MySQL](https://img.shields.io/badge/MySQL-8.x-4479A1?style=flat-square&logo=mysql&logoColor=white)](https://www.mysql.com)
[![License](https://img.shields.io/badge/License-EUPL%201.2-green?style=flat-square)](LICENSE)

_Template B main backend API — open-sourced from **KODI-Kommunen-Digital**. Licensed under the [EUPL-1.2](LICENSE)._

</div>

---

## Overview

This is the main REST API for the Template B city app. It is one of three backend services:

| Service | Role |
|---|---|
| **`kodi-backend-template-B`** *(this repo)* | Main REST API — listings, categories, events, waste calendar, citizen services, auth |
| `kodi-backend-forum-template-B` | Forums, posts, comments, group membership, and chat (incl. encrypted chat) |
| WebsocketServer | Realtime transport for chat messages |

It runs as an Express service against a MySQL database, issues RS256 JWTs, and stores media in Huawei OBS object storage.

---

## Features

| Area | Endpoints |
|---|---|
| **Listings** | `listings`, `cityListings` — create / browse / filter, recurrence rules, media |
| **Categories & places** | `categories`, `cities`, `village` |
| **Favorites** | `favorites` |
| **Users & auth** | `users` — registration, login, RS256 JWT, roles |
| **Citizen content** | `citizenServices`, `contactUs`, `moreInfo`, `ads` |
| **City features** | `wasteCalender`, `defectReporter` |
| **Ops** | `status` (health) |

Versioned API: legacy routes at the root plus `v1/` and `v2/` (current). Error tracking via Sentry (`instrument.js`); transactional email via templated `emailTemplates/`.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js + Express |
| Database | MySQL (`mysql2`) |
| Auth | RS256 JWT (key pair via env) |
| Push | `firebase-admin` |
| Email | `nodemailer` + templated emails |
| Object storage | Open Telekom Cloud — OBS object storage (`eSDK_Storage_OBS`) for media/images |
| Translation | DeepL (`DEEPL_AUTH_KEY`) |
| Monitoring | Sentry |

---

## Getting Started

### Prerequisites

- Node.js **18+**
- A reachable **MySQL** database
- (Optional) Firebase service account (push), SMTP account (email), Huawei OBS bucket (media), DeepL key (translation)

### Installation

```bash
git clone https://github.com/Kodi-Entwicklergemeinschaft/kodi-backend-template-B.git
cd kodi-backend-template-B
npm install
```

### Configuration

```bash
cp .env.example .env
```

| Key | Description |
|---|---|
| `ENVIRONMENT`, `WEBSITE_DOMAIN`, `PORT` | Deployment identity + bind port |
| `DATABASE_HOST` / `DATABASE_USER` / `DATABASE_PASSWORD` / `DATABASE_NAME` | MySQL connection |
| `ACCESS_PRIVATE` / `ACCESS_PUBLIC` / `REFRESH_PRIVATE` / `REFRESH_PUBLIC` | RS256 JWT key pairs (PEM body, no header/footer) |
| `AUTH_EXPIRATION`, `REFRESH_EXPIRATION`, `SALT` | Token TTLs + bcrypt salt rounds |
| `BUCKET_HOST` / `BUCKET_NAME` / `BUCKET_ACCESS_KEY` / `BUCKET_SECRET_KEY` | Huawei OBS object storage |
| `EMAIL_ID` / `EMAIL_PASSWORD` / `EMAIL_HOST` / `EMAIL_PATH` | SMTP for transactional email |
| `DEEPL_AUTH_KEY` | DeepL translation |
| `API_VERSION`, `BRIDGE_ENABLED` | API version + bridge routes toggle |

> `.env` is gitignored — never commit real credentials. Generate JWT key pairs with `openssl`.

### Run

```bash
npm start
```

The API starts on `PORT` (from your `.env`).

---

## Related Services

- **Forum service:** [`kodi-backend-forum-template-B`](https://github.com/Kodi-Entwicklergemeinschaft/kodi-backend-forum-template-B)
- **Mobile app:** [`kodi-mobile-template-B`](https://github.com/Kodi-Entwicklergemeinschaft/kodi-mobile-template-B)

---

## License

Licensed under the **European Union Public Licence v1.2 (EUPL-1.2)**. See [LICENSE](LICENSE).
