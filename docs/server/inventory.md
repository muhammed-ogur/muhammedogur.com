# Sunucu envanteri

> Son doğrulama: 2026-10-29 · Yöntem: aşağıdaki komutlar (salt okuma)
> IP adresi bilerek yazılmadı (bkz. dns.md). Sır yok.

## Sistem
- İşletim sistemi: Ubuntu 26.04.1 LTS (resolute) (`cat /etc/os-release`)
- Çekirdek: 7.0.0-42-generic (`uname -r`; 2026-10-28 güncelleme + yeniden başlatma)
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
| 53 | systemd-resolved | yalnız sunucu içi (127.0.0.53/54) |

## Kullanıcılar ve zamanlanmış işler
- Oturum açabilen: root (SSH ile girişi kapalı), **muhammed** (sudo grubunda) · `sync` sistem hesabı kabuk vermez
- muhammed için yetkili anahtar: 1 adet (`muhammed@MOGUR masaustu`)
- root crontab: yok

## Güvenlik (bugünkü durum)
- SSH: **root girişi kapalı, parola kapalı, yalnız anahtar** (`sshd -T`) · ayar: `/etc/ssh/sshd_config.d/10-sertlestirme.conf` (depoda: `server/etc/ssh/sshd_config.d/`)
  - `10-` öneki: sonradan gelebilecek `50-cloud-init.conf` gibi dosyalardan önce okunsun (ilk okunan değer kazanır)
- ufw: aktif · varsayılan: gelen deny, giden allow · izin: 22/tcp, 80,443/tcp (IPv4 + IPv6)
- fail2ban: aktif, `sshd` jail açık
- Otomatik güncelleme: unattended-upgrades açık, günlük (`/etc/apt/apt.conf.d/20auto-upgrades`)

## Değişiklik geçmişi
- 2026-10-27: root'a SSH anahtarı eklendi (B2.1)
- 2026-10-27: nginx kaldırıldı (`apt purge nginx nginx-common`); 80 portunda eski bir şablon site yayındaydı
- 2026-10-28: 34 paket güncellendi, yeniden başlatıldı (çekirdek 7.0.0-31 → 7.0.0-42)
- 2026-10-28: ufw etkinleştirildi (ölü adam anahtarıyla), fail2ban kuruldu
- 2026-10-29: `muhammed` kullanıcısı (sudo) + anahtar; root ve parola ile SSH girişi kapatıldı (ölü adam anahtarıyla, 3 test)

## Açık işler
- `/var/www/html` altındaki eski şablon dosyalarını sil
- root'un `authorized_keys` dosyası: root girişi kapalı olduğu için etkisiz; temizlikte boşaltılabilir
- Eski çekirdek paketleri (7.0.0-31): birkaç gün sorunsuz geçince `apt autoremove`
- Gereksiz servisler: ModemManager, upower, udisks2, multipathd (değerlendirilecek)
