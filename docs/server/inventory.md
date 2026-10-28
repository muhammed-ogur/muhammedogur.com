# Sunucu envanteri

> Son doğrulama: 2026-10-27 · Yöntem: aşağıdaki komutlar (salt okuma)
> IP adresi bilerek yazılmadı (bkz. dns.md). Sır yok.

## Sistem
- İşletim sistemi: Ubuntu 26.04.1 LTS (resolute) (`cat /etc/os-release`)
- Çekirdek: 7.0.0-31-generic (güncellemeyle değişecek)
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
- ufw: kurulu, kapalı (inactive)
- SSH: root girişi açık, parola açık, anahtar açık (`sshd -T`)
- root için yetkili anahtar: 1 adet (`muhammed@MOGUR masaustu`)
- Otomatik güvenlik güncellemesi: unattended-upgrades çalışıyor (ayarı doğrulanmadı)

## Değişiklik geçmişi
- 2026-10-27: root'a SSH anahtarı eklendi (B2.1)
- 2026-10-27: nginx kaldırıldı (`apt purge nginx nginx-common`); 80 portunda eski bir şablon site yayındaydı

## Açık işler
- 27 paket güncellemesi + yeniden başlatma gerekiyor (B2.3)
- ufw ve fail2ban kurulumu (B2.3)
- Normal kullanıcı aç; root ve parola girişini kapat (B2.2)
- `/var/www/html` altındaki eski şablon dosyalarını sil
- Gereksiz servisler: ModemManager, upower, udisks2, multipathd (değerlendirilecek)
