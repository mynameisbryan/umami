# Umami Setup für AXIOM

## ✅ Status

- [x] Umami als Git Submodule hinzugefügt
- [ ] Dependencies installieren
- [ ] Environment Variables konfigurieren
- [ ] Auf Vercel deployen

## Nächste Schritte

### 1. Dependencies installieren

```bash
cd umami
npm install --legacy-peer-deps
```

> **Hinweis:** `--legacy-peer-deps` ist nötig wegen React 18/19 Dependency-Konflikt.

### 2. Environment Variables konfigurieren

Erstelle `.env.local`:

```bash
cd umami
cat > .env.local << EOF
DATABASE_URL=postgresql://user:password@host:port/database
APP_SECRET=$(openssl rand -base64 32)
EOF
```

**Datenbank-Optionen:**
- **Vercel Postgres:** Im Vercel Dashboard → Storage → Postgres erstellen
- **Supabase:** Free Tier nutzen (Connection String aus Settings → Database)

### 3. Lokal testen (Optional)

```bash
cd umami
npm run dev
```

Öffne http://localhost:3001

### 4. Auf Vercel deployen

1. **Neues Vercel Projekt erstellen:**
   - Vercel Dashboard → Add New Project
   - Repository auswählen
   - **Root Directory:** `umami` (wichtig!)
   - Framework: Next.js (auto-detected)

2. **Environment Variables setzen:**
   - `DATABASE_URL` (von Vercel Postgres oder Supabase)
   - `APP_SECRET` (generiere mit `openssl rand -base64 32`)

3. **Deploy**

4. **Umami initialisieren:**
   - Öffne deine Umami URL
   - Erstelle Admin-Account
   - Gehe zu Settings → Websites → Add Website
   - Kopiere die **Website ID**

5. **Next.js App konfigurieren:**
   - In deinem `web/` Vercel Projekt:
     - `NEXT_PUBLIC_UMAMI_WEBSITE_ID` = Website ID
     - `NEXT_PUBLIC_UMAMI_SCRIPT_URL` = `https://deine-umami-url.vercel.app/script.js`

## Troubleshooting

### npm install schlägt fehl

```bash
# Cache löschen
rm -rf node_modules package-lock.json
npm cache clean --force
npm install --legacy-peer-deps
```

### Vercel Build schlägt fehl

- Stelle sicher, dass **Root Directory** auf `umami` gesetzt ist
- Prüfe Environment Variables sind gesetzt
- Prüfe Build Logs in Vercel Dashboard

## Weitere Infos

- Siehe `doc/UMAMI-DEPLOYMENT-STRATEGY.md` für vollständige Anleitung
- Siehe `doc/UMAMI-ANALYTICS-SETUP.md` für Client-Integration

