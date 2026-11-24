# Manager build (AjuLabs)

`manager/dist` is the Evolution Manager bundle served at `/manager`. In this build it
comes from evolution-manager-v2 at `f054b9bc28083152d4948f835e3346fd0add39db`, the same
commit as the bundle shipped with 2.3.8 (the CSS output is byte-identical), plus
`evolution-manager-v2-follow-qr.patch`.

The patch keeps the QR code and pairing code dialogs up to date while they are open.
Since 2026-07-28 WhatsApp retires the secret in the QR right after the first scan
(`companion_reg_refresh`). Baileys then renders a new QR, and the phone has to scan that
one. The stock dialog fetched the QR once, so it kept showing the retired one.

The dialog polls `GET /instance/connectionState/{instance}` every 2 s and reads
`GET /instance/connect/{instance}` only while the state is `connecting`. Calling `connect`
on a closed instance would start a new session. The dialog closes when the state turns
`open`, and polling stops after 5 minutes.

## Rebuild

```bash
git clone https://github.com/evolution-foundation/evolution-manager-v2.git
cd evolution-manager-v2
git checkout f054b9bc28083152d4948f835e3346fd0add39db
git apply ../evolution-api/manager/evolution-manager-v2-follow-qr.patch
npm ci --ignore-scripts
npx vite build   # tsc -b passes on this commit, vite build is enough for the bundle
```

Replace `manager/dist/index.html` and `manager/dist/assets/index-*.js` with the new
output. Check that `index-*.css` and `assets/images/` come out unchanged.
