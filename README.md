# PrivateVault Safari PWA — prototype

## Install
Host the **entire folder** at a static HTTPS URL you control (e.g. Cloudflare Pages, Netlify, GitHub Pages). Do not upload any vault backup files. Open the live HTTPS URL on iPhone in Safari → Share → Add to Home Screen → Open as Web App. Launch from its Home Screen icon and create a new vault there.

## Storage / backup design
- Vault envelope is encrypted using AES-256-GCM, with key from PBKDF2-SHA256 (600k iterations) and 16-byte random salt. IV is random per save.
- Encrypted envelope stored in the browser's localStorage for this origin. Its storage can be evicted or deleted, so it is NOT durable enough as sole copy.
- On unlock the app requests persistent browser storage (heuristic-dependent); this does NOT guarantee safety.
- After every save, backup status changes to Backup needed. Use Save encrypted backup and the iPhone Files / iCloud Drive share action. Verify the file in Files. Status indicates export invoked, not that file arrived.
- Import from Files replaces the local vault. Restore with original master password.
- A Home Screen app may have storage separated from Safari, so begin and restore inside the Home Screen app itself.
- No background sync or automatic iCloud Drive writing is possible in this PWA without server/cloud account integration.

## Security limitations
This is a **prototype, not security-audited**. No Face ID, Safari Password AutoFill, hardware-protected key, rollback protection, or cross-device sync. Service worker caches only static app assets and never vault data. Hosting provider can change served JavaScript (supply-chain risk); choose a trusted domain with careful access controls. Never upload an unencrypted vault. Do not entrust critical credentials to this prototype. Verify backups by restoring on a separate test browser/device. Master password cannot be recovered. Keep backups in trusted encrypted storage.

## Local preview
Run `python3 -m http.server 8000` in this folder on a computer, then visit http://localhost:8000. To install on iPhone, serve from HTTPS.
