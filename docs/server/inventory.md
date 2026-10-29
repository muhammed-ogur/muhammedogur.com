# Sunucu envanteri

> Son doğrulama: 2026-10-29 · Yöntem: aşağıdaki komutlar (salt okuma)
> IP adresi bilerek yazılmadı (bkz. dns.md). Sır yok.

## Sistem
- İşletim sistemi: Ubuntu 26.04.1 LTS (resolute) (`cat /etc/os-release`)
- Çekirdek: 7.0.0-42-generic (`uname -r`)
- Sanallaştırma: VMware (open-vm-tools, vgauth)
- Not: panel etiketi "TR-Ubuntu 22" — kurulum kalıbı; sistem sonradan yükseltilmiş

## Donanım
- İşlemci: 4 çekirdek (`nproc`)
- Bellek: 3.3 GiB, swap 2 GiB (`free -h`)
- Disk: 64 GB, %18 dolu (`df -h /`)

## Ağ — dinleyen portlar (`ss -tlnp`)
| Port | Program | Erişim |
|---|---|---|
| 22 | sshd (systemd socket activation) | dışarıya açık (IPv4 + IPv6) |
| 80 | caddy (HTTP → HTTPS 308) | dışarıya açık |
| 443 | caddy (HTTPS, HTTP/2 + HTTP/3) | dışarıya açık |
| 2019 | caddy admin API | yalnız sunucu içi (localhost) |
| 53 | systemd-resolved | yalnız sunucu içi (127.0.0.53/54) |

## Yazılım
- Caddy 2.11.7 — resmî depo (`/etc/apt/sources.list.d/caddy-stable.list`, `signed-by` anahtarlı); güncelleme elle (bkz. runbook)
- Ayar: `/etc/caddy/Caddyfile` (depoda: `server/etc/caddy/Caddyfile`)
- Site: `/var/www/muhammedogur.com` (depo klonu, sahibi muhammed) → Caddy yalnız `site/` klasörünü sunar
- Sertifika: Let's Encrypt, Caddy otomatik alır ve yeniler

## Kullanıcılar ve zamanlanmış işler
- Oturum açabilen: root (SSH ile girişi kapalı), **muhammed** (sudo) · `sync` sistem hesabı kabuk vermez
- muhammed için yetkili anahtar: 1 adet (`muhammed@MOGUR masaustu`)
- root crontab: yok

## Güvenlik (bugünkü durum)
- SSH: root girişi kapalı, parola kapalı, yalnız anahtar · ayar: `/etc/ssh/sshd_config.d/10-sertlestirme.conf` (depoda: `server/etc/ssh/sshd_config.d/`)
- ufw: aktif · gelen deny, giden allow · izin: 22/tcp, 80,443/tcp (IPv4 + IPv6)
- fail2ban: aktif, `sshd` jail açık
- Otomatik güncelleme: unattended-upgrades açık (yalnız Ubuntu güvenlik deposu; Caddy hariç)

## Değişiklik geçmişi
- 2026-10-27: root'a SSH anahtarı eklendi · nginx kaldırıldı (80'de eski şablon site yayındaydı)
- 2026-10-28: 34 paket güncellendi, yeniden başlatıldı (çekirdek -31 → -42) · ufw etkinleştirildi · fail2ban kuruldu
- 2026-10-29: `muhammed` (sudo) + anahtar; root ve parola ile SSH kapatıldı
- 2026-10-29: Caddy kuruldu (resmî depo); site yayında, Let's Encrypt sertifikası (önce staging ile denendi)

## Açık işler
- `/var/www/html` altındaki eski şablon dosyalarını sil
- root'un `authorized_keys` dosyasını boşalt (etkisiz)
- `muhammed`'i `adm` grubuna ekle (`journalctl` sudo'suz)
- Eski çekirdek paketleri (7.0.0-31): birkaç gün sorunsuz geçince `apt autoremove`
- Gereksiz servisler: ModemManager, upower, udisks2, multipathd (değerlendirilecek)
