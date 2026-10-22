# DNS kaydı — muhammedogur.com

- **Alan adı:** muhammedogur.com
- **Kayıt türü:** A
- **Hedef:** `<sunucu IP — Hostlab paneli>`
- **TTL:** Auto
- **Ad sunucuları:** `nataly.ns.cloudflare.com.`, `coleman.ns.cloudflare.com.`

## Proxy kararı

- **Seçim:** DNS only (gri bulut)
- **Neden:**
  - Katmanlar alttan üste sırayla kuruluyor; sorun çıkarsa hangi katmanda olduğu belli olsun.
  - Cloudflare proxy yalnız 80/443 portlarını taşır; açık olsaydı `ssh muhammedogur.com` çalışmazdı.
  - Caddy, Let's Encrypt sertifikasını istek doğrudan sunucuya geldiği için ek ayarsız alır.
- **Ne zaman değişir:** Caddy ve sertifika tamamlandıktan sonra yeniden değerlendirilir; proxy açılırsa SSH için ayrı bir yol gerekir.

## Doğrulama

```bash
dig muhammedogur.com +short
```
Beklenen: `<sunucu IP — Hostlab paneli>`

```bash
dig muhammedogur.com NS +short
```
Beklenen:
```text
nataly.ns.cloudflare.com.
coleman.ns.cloudflare.com.
```
