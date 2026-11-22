# Petunjuk Deploy ke Cloudflare Workers

## 1. File yang ditambahkan ke repo (root)
- `wrangler.jsonc`  -> ganti "name" dengan nama Worker Anda di dashboard Cloudflare
- `.gitignore`

## 2. Ubah package.json
Tambahkan script dan devDependencies (jangan hapus yang sudah ada):

```json
"scripts": {
  "start": "node index.js",
  "dev": "wrangler dev",
  "deploy": "wrangler deploy"
},
"devDependencies": {
  "wrangler": "^4"
}
```

Jika index.js memakai `require()`, tambahkan juga `"type": "module"`
dan ubah `require` menjadi `import`.

## 3. Ubah index.js
Tambahkan di baris paling atas:

```js
import { httpServerHandler } from 'cloudflare:node';
```

Tambahkan di baris paling bawah (sesuaikan port dengan app.listen Anda):

```js
export default httpServerHandler({ port: 3000 });
```

## 4. Pengaturan di dashboard Cloudflare (Build configuration)
- Build command   : (kosongkan)
- Deploy command  : npx wrangler deploy
- Version command : npx wrangler versions upload
- API token       : klik "Create new token" (yang lama tidak tersedia)
- Variables and secrets: pindahkan Secrets dari Replit

## 5. Catatan penting
- Folder `views` (template EJS/Pug) dibaca dari sistem file, sehingga
  kemungkinan tidak jalan di Workers tanpa perubahan.
- File seperti icon.png perlu disajikan lewat static assets Cloudflare.
- Database lokal (SQLite, Replit DB) tidak bisa dipakai. Gunakan D1, KV,
  atau database eksternal.
