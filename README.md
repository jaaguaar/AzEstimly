# AzEstimly

> Visual Azure architecture estimator for teams — drag, drop, and get a cost estimate in minutes.

AzEstimly is an internal tool that lets managers quickly sketch Azure architectures using a drag-and-drop canvas and get an instant ballpark cost estimate — without wrestling with the official Azure pricing calculator.

**⚠️ Estimates only.** Final costs depend on actual usage and may include charges not reflected here. Always validate with the [official Azure calculator](https://azure.microsoft.com/en-us/pricing/calculator/) before committing to a budget.

---

## Features

- 🎨 **Visual canvas** — drag Azure services from a library, connect them with arrows, organize into groups
- 💰 **Live cost estimate** — real-time pricing table powered by the Azure Retail Prices API
- 🏷️ **Naming conventions** — define a naming template once, see named resources everywhere (e.g. `acme-dev-sql-westeu`)
- 🌍 **Multi-region** — West Europe, North Europe, East US, Southeast Asia
- 📋 **Environments** — create Dev, clone it to Prod with one click, then adjust tiers
- 👥 **Team sharing** — invite colleagues via Microsoft Entra ID, assign editor or viewer roles
- 💾 **Auto-save** — canvas saves automatically every 30 seconds

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 14 (App Router) |
| Canvas | Xyflow (React Flow) |
| State | Zustand |
| UI | Tailwind CSS + shadcn/ui |
| Animations | Framer Motion |
| ORM | Prisma |
| Database | PostgreSQL (Azure Flexible Server) |
| Auth | NextAuth.js + Microsoft Entra ID |
| Validation | Zod |
| Pricing | Azure Retail Prices API |
| CI/CD | GitHub Actions → Azure Web App |

## Getting Started

### Prerequisites

- Node.js 20+
- PostgreSQL instance (local or Azure Flexible Server)
- Microsoft Entra ID App Registration ([setup guide](docs/entra-setup.md))

### Installation

```bash
git clone https://github.com/your-org/azestimly.git
cd azestimly
npm install
```

### Environment Variables

Copy `.env.example` to `.env.local` and fill in the values:

```bash
cp .env.example .env.local
```

```env
DATABASE_URL=               # PostgreSQL connection string
NEXTAUTH_URL=               # app URL (e.g. http://localhost:3000)
NEXTAUTH_SECRET=            # random secret (openssl rand -base64 32)
AZURE_AD_CLIENT_ID=         # Entra ID App Registration client ID
AZURE_AD_CLIENT_SECRET=     # Entra ID App Registration client secret
AZURE_AD_TENANT_ID=         # Entra ID tenant ID
```

### Database Setup

```bash
npx prisma migrate dev
```

### Run Locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Project Structure

```
azestimly/
├── app/
│   ├── (auth)/               # login, callback pages
│   ├── (dashboard)/
│   │   ├── projects/         # project list
│   │   └── projects/[id]/
│   │       ├── canvas/       # main canvas page
│   │       └── settings/     # naming convention, sharing
│   └── api/
│       ├── auth/             # NextAuth endpoints
│       ├── projects/         # CRUD
│       ├── diagrams/         # canvas state persistence
│       └── prices/           # Azure Retail Prices API proxy
├── components/
│   ├── canvas/               # Xyflow canvas + custom Azure nodes
│   ├── library/              # left panel — service library
│   ├── estimator/            # right panel — cost breakdown table
│   └── ui/                   # shadcn components
├── lib/
│   ├── azure/                # Azure services catalog + SVG icons
│   ├── pricing/              # cost calculation logic
│   └── naming/               # naming convention engine
└── prisma/
    └── schema.prisma
```

## Development

```bash
npm run dev          # dev server
npm run build        # production build
npm run lint         # ESLint
npx prisma studio    # Prisma Studio (DB GUI)
```

## Deployment

The app runs as a single **Azure Web App** (Node.js 20 LTS). Push to `main` triggers a GitHub Actions workflow that builds and deploys automatically.

Infrastructure:
- **Web App** — B2 (dev) / P1v3 with auto-scale (prod)
- **PostgreSQL Flexible Server** — B2s (dev) / Zone Redundant (prod)
- **Key Vault** — stores all secrets

Estimated infrastructure cost: ~$60–90/month (dev) · ~$200–250/month (prod)

## Contributing

This is an internal tool. To contribute, open a PR against `main`. Tag a reviewer from the team.

## License

Internal use only.
