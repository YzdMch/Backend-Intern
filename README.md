# Backend Intern

Aplikasi backend berbasis Laravel 13 dengan PostgreSQL. Aplikasi dijalankan menggunakan Docker Compose.

## Prasyarat

- Docker Desktop sudah terpasang dan sedang berjalan.
- Git sudah terpasang.

## Menjalankan Aplikasi

1. Clone repository dan masuk ke folder project:

   ```bash
   git clone <url-repository>
   cd Backend-Intern
   ```

2. Buat file environment jika belum tersedia:

   ```bash
   cp .env.example .env
   ```

   Pada Windows PowerShell, gunakan:

   ```powershell
   Copy-Item .env.example .env
   ```

3. Jalankan container aplikasi dan database:

   ```bash
   docker compose up -d --build
   ```

4. Generate application key jika `APP_KEY` masih kosong:

   ```bash
   docker compose exec app php artisan key:generate
   ```

5. Jalankan migration database:

   ```bash
   docker compose exec app php artisan migrate --force
   ```

6. Buka aplikasi di [http://localhost:8000](http://localhost:8000).

Container `app` akan menunggu PostgreSQL sampai berstatus healthy sebelum dijalankan.

## Perintah Docker

Melihat status container:

```bash
docker compose ps
```

Melihat log backend:

```bash
docker compose logs -f app
```

Menghentikan aplikasi:

```bash
docker compose down
```

Menjalankan perintah Artisan lain:

```bash
docker compose exec app php artisan <command>
```

Contoh:

```bash
docker compose exec app php artisan migrate:status
```

## Konfigurasi Database

Konfigurasi default PostgreSQL di `compose.yaml` adalah:

```text
Host: postgres
Port: 5432
Database: ewf_system
Username: postgres
Password: postgres
```

Dari aplikasi Laravel, gunakan `DB_HOST=postgres`. Dari komputer host, database dapat diakses melalui `localhost:5432`.

## Troubleshooting

Jika backend tidak berjalan, periksa log:

```bash
docker compose logs app postgres
```

Jika perubahan dependency belum masuk ke image, rebuild container:

```bash
docker compose down
docker compose up -d --build
```

Jangan menghapus volume PostgreSQL kecuali data database memang boleh dihapus. Jika password database pada `.env` berbeda dari password saat volume pertama kali dibuat, gunakan password lama atau reset volume secara sengaja:

```bash
docker compose down -v
docker compose up -d --build
```

Perintah `down -v` akan menghapus data PostgreSQL.

## Testing

Jalankan test Laravel dari dalam container:

```bash
docker compose exec app php artisan test
```
