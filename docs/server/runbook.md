# Runbook — muhammedogur.com

> Sunucuda tekrar eden işlerin adımları. Sunucuda dosya elle düzenlenmez: her değişiklik depodan gelir.

## Bağlanma
- `ssh muhammedogur` (yerel `~/.ssh/config`) · komuttan önce prompt'a bak: `@ubuntu` mu?

## Siteyi güncellemek (HTML/CSS)
1. Yerelde `site/` altında değiştir → commit → push
2. Sunucuda: `git -C /var/www/muhammedogur.com pull`
3. Caddy'ye bir şey yapmak gerekmez (dosyaları her istekte diskten okur)
4. Kontrol: tarayıcıda yenile

## Caddyfile'ı değiştirmek
1. Yerelde `server/etc/caddy/Caddyfile` → commit → push
2. Sunucuda: `git -C /var/www/muhammedogur.com pull`
3. `sudo cp /var/www/muhammedogur.com/server/etc/caddy/Caddyfile /etc/caddy/Caddyfile`
4. `caddy validate --config /etc/caddy/Caddyfile`
5. `sudo systemctl reload caddy` (sertifika otoritesi değiştiyse `restart`)
6. Geri dönüş: önceki commit'e dönüp 2–5

## Haftalık bakım
- `sudo apt update && sudo apt upgrade` (Caddy dahil; Removing satırını oku)
- "System restart required" varsa: `sudo reboot`
- `sudo fail2ban-client status sshd` · `sudo ufw status`

## Kontroller
- Sertifika: `curl -vI https://muhammedogur.com 2>&1 | grep -i -E "subject:|issuer:|expire|HTTP/"`
- Yönlendirme: `curl -I http://muhammedogur.com` → 308
- Caddy günlüğü: `sudo journalctl -u caddy --since "1 hour ago" --no-pager`

## Riskli değişiklik (SSH, ufw)
- Önce ikinci oturum açık · sonra ölü adam anahtarı:
  `sudo systemd-run --unit=olu-adam --on-active=5m <geri alan komut>`
- Değişiklik → yeni sekmeden test → `sudo systemctl stop olu-adam.timer`

## Sıfırdan kurulum sırası (sunucu kaybedilirse)
1. DNS A kaydını yeni IP'ye çevir (`dns.md`)
2. Anahtarla giriş → `muhammed` + sudo → SSH sertleştirme (`server/etc/ssh/…`)
3. Güncelleme + ufw + fail2ban
4. Caddy (resmî depo) → depoyu `/var/www/muhammedogur.com`'a klonla → Caddyfile'ı kopyala → reload
5. Kontroller
