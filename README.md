# MY STORE – Retail Management System (Phase 1)

**MY STORE** is a mobile-first, completely offline-capable Progressive Web Application (PWA) built for supermarket and retail operations in Bangladesh, storing all business data locally inside **IndexedDB** via **Dexie.js** with **BDT (`৳`)** integer minor-unit (**poisha**) currency precision.

## Architecture Highlights (Phase 1)

- **100% Local Offline Database (Dexie.js + IndexedDB)**: 20 versioned stores (`products`, `categories`, `subcategories`, `vendors`, `vendorDocuments`, `batches`, `stockLedger`, `purchases`, `purchaseItems`, `sales`, `saleItems`, `customers`, `memberships`, `returns`, `returnItems`, `vendorPayments`, `cheques`, `expenses`, `settings`, `activityLogs`).
- **Integer Poisha Money Storage**: Financial figures are stored as integer poisha (`1 BDT = 100 poisha`) to eliminate floating-point rounding drift.
- **Common Admin Password System**: Salted `PBKDF2-SHA256` (100,000 iterations via Web Crypto API) protects sensitive operations without storing plaintext passwords. Note: This is local-device application authorization, not server-side multi-user security.
- **Backup & Restore**: Versioned JSON snapshot export across all 20 stores, pre-restore schema/ID validation, overwrite confirmation dialog, and atomic restoration.
- **PWA Offline Shell**: Configured with `vite-plugin-pwa`, Web App Manifest, custom Android/iOS icons, and static asset caching (excluding business transactions & vendor documents from service worker HTTP cache).

## Running & Testing Locally

```bash
npm install
npm run dev
```

Run TypeScript check, production build, and unit tests:

```bash
npm run lint
npm run build
npx --yes tsx --test src/utils/currency.test.ts src/services/cryptoService.test.ts src/services/backupService.test.ts src/database/schema.test.ts
```

## Installing on Android Chrome (Phone-Only Workflow)

1. Open the deployed HTTPS URL in **Android Chrome**.
2. Tap the **Install App** button in the MY STORE header (or tap Chrome menu `⋮` → **Add to Home screen** / **Install app**).
3. Launch **MY STORE** from your Android Home Screen. Once loaded once, it continues operating without an internet connection.

## Publishing on GitHub Pages (Using Android Mobile + GitHub)

1. Push this repository to GitHub (via GitHub Mobile / mobile browser).
2. Add a GitHub Actions workflow `.github/workflows/deploy.yml` that builds with `VITE_BASE_PATH=/<your-repo-name>/ npm run build` and deploys `dist/` to `gh-pages` (or GitHub Pages artifact action).
3. Enable **Settings → Pages → Source: GitHub Actions** in your repository.

## Known Limitations (Phase 1)

- Operational CRUD workflows for POS checkout, Products, Categories, Subcategories, Vendors, Purchases, Customers, Memberships, Returns, Payments, Expiry write-offs, Reports, and Excel import/export are structured as Phase 1 readiness modules and will be implemented in subsequent phases.
- Clearing Android Chrome site storage deletes IndexedDB data; export regular JSON backups in **Settings → JSON Backup & Restore**.
