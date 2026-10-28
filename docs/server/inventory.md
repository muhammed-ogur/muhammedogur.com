# Sunucu envanteri

> Son doğrulama: 2026-10-28 · Yöntem: aşağıdaki komutlar (salt okuma)
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
- Oturum açabilen: yalnız root (`sync` sistem hesabı, kabuk vermez)
- root crontab: yok

## Güvenlik (bugünkü durum)
- ufw: **aktif** · varsayılan: gelen deny, giden allow · izin: 22/tcp, 80,443/tcp (IPv4 + IPv6) (`ufw status verbose`)
- fail2ban: aktif, `sshd` jail açık (`fail2ban-client status sshd`; kurulumdan dakikalar sonra 3 IP yasaklandı)
- Otomatik güncelleme: unattended-upgrades açık, günlük (`/etc/apt/apt.conf.d/20auto-upgrades`)
- SSH: root girişi açık, parola açık, anahtar açık (`sshd -T`) — **B2.2'de kapanacak**
- root için yetkili anahtar: 1 adet (`muhammed@MOGUR masaustu`)

## Değişiklik geçmişi
- 2026-10-27: root'a SSH anahtarı eklendi (B2.1)
- 2026-10-27: nginx kaldırıldı (`apt purge nginx nginx-common`); 80 portunda eski bir şablon site yayındaydı
- 2026-10-28: 34 paket güncellendi, yeniden başlatıldı (çekirdek 7.0.0-31 → 7.0.0-42)
- 2026-10-28: ufw etkinleştirildi (ölü adam anahtarıyla: `systemd-run --on-active=5m ufw disable`, test sonrası iptal)
- 2026-10-28: fail2ban kuruldu

## Açık işler
- Normal kullanıcı aç; root ve parola girişini kapat (B2.2) — botlar parola deniyor, acil
- `/var/www/html` altındaki eski şablon dosyalarını sil
- Eski çekirdek paketleri (7.0.0-31): birkaç gün sorunsuz geçince `apt autoremove`
- Gereksiz servisler: ModemManager, upower, udisks2, multipathd (değerlendirilecek)
