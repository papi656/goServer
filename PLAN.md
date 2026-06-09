# Multi-Media Payload Server
**Branch Name:** `feature/multi-media-payload-server`

## Description
A Go HTTP server that accepts POST requests containing JSON, Markdown, PDF, or media files. Binary files are stored in MinIO; structured/text content is stored in PostgreSQL.

## Stack
- **Language:** Go
- **HTTP:** net/http (stdlib)
- **Database:** PostgreSQL
- **DB Driver:** pgx/v5
- **Object Storage:** MinIO (Docker)
- **MinIO SDK:** minio-go/v7
- **Migrations:** golang-migrate
- **Config:** godotenv
- **Auth:** API Key (stored in DB)
- **Cron:** robfig/cron/v3

## Project Structure
- `main.go`: entry point
- `.env`: configuration
- `handler/`: HTTP handlers
- `middleware/`: auth middleware
- `processor/`: content-type handlers (JSON, Markdown, binary)
- `storage/`: DB and MinIO clients
- `jobs/`: scheduled tasks
- `model/`: data structures
- `db/migrations/`: SQL migrations

## Build Phases

### Phase 1: Foundation
Set up PostgreSQL + MinIO via docker-compose, config loader, migrations, connection pooling.

### Phase 2: Auth
API key authentication with bootstrap key generation on first run.

### Phase 3: Upload
Handle JSON, Markdown, PDF, and image payloads.

### Phase 4: Download
Retrieve payload metadata and content.

### Phase 5: Cleanup
Monthly cron job to delete old records.

### Phase 6: Polish
Validation, error responses, logging.

## Study Resources

### Multipart form parsing
- [Go net/http docs](https://pkg.go.dev/net/http) — ParseMultipartForm, FormFile, FormValue
- [Go blog: Testing multipart/form-data](https://go.dev/blog/http-handler-gen)

### MIME type detection
- [DetectContentType](https://pkg.go.dev/net/http#DetectContentType)
- [Go 1.24 MIME sniffing](https://go.dev/blog/go1.24-mime)

### Object storage (S3/MinIO)
- [S3 object keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html)
- [S3 presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html)
- [MinIO presigned ops](https://min.io/docs/minio/linux/reference/minio-mc/mc-share.html)

### PostgreSQL JSONB with pgx
- [pgx JSON/JSONB docs](https://pkg.go.dev/github.com/jackc/pgx/v5#section-readme)
- [pgx wiki JSONB guide](https://github.com/jackc/pgx/wiki/JSONB-Guide)
- [CockroachDB blog](https://www.cockroachlabs.com/blog/jsonb-go/)

### HTTP middleware chaining
- [Go blog: middleware pattern](https://go.dev/blog/httptreemux)
- [justinas/alice](https://github.com/justinas/alice)
- [Gopher Academy](https://blog.gopheracademy.com/middleware/)

### API key auth / hashing
- [crypto/sha256](https://pkg.go.dev/crypto/sha256)
- [OWASP password storage cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)

### robfig/cron
- [GitHub repo](https://github.com/robfig/cron)
- [pkg.go.dev](https://pkg.go.dev/github.com/robfig/cron/v3)

### MinIO Go SDK
- [minio-go repo](https://github.com/minio/minio-go)
- [MinIO Go SDK docs](https://min.io/docs/minio/linux/developers/go/minio-go.html)
- [Examples](https://github.com/minio/minio-go/tree/main/examples)

### golang-migrate
- [GitHub repo](https://github.com/golang-migrate/migrate)
- [DigitalOcean tutorial](https://www.digitalocean.com/community/tutorials/using-golang-migrate)

### pgx connection pooling
- [pgxpool docs](https://pkg.go.dev/github.com/jackc/pgx/v5/pgxpool)
- [pgx wiki getting started](https://github.com/jackc/pgx/wiki/Getting-started-with-pgx/v5)
