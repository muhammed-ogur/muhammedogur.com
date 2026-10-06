# 0001. Depo nerede durur

- Durum: Kabul edildi · Tarih: 2026-09-25

## Bağlam
Geliştirme Windows 11 + WSL2 (Ubuntu) üzerinde. Depo ya Windows dosya
sisteminde (`/mnt/c/...`) ya da WSL'in Linux dosya sisteminde (`~/projects`)
durabilir. Endişeler: dosya izinleri (güvenlik), derleme/IDE hızı, Windows
araçlarından erişim. Bütçe ₺0.

## Seçenekler
1. Windows (`/mnt/c/...`)
   - Artı: Windows araçları ve yedekleme doğrudan erişir.
   - Eksi: WSL'den bakınca tüm dosyalar 777 (`-rwxrwxrwx`); chmod etkisiz,
     600 gerektiren dosyalar (ör. SSH anahtarı) burada tutulamaz. Kanıt:
     30.09.2026, `ls -la /mnt/c/Users/Muhammed/`.
2. WSL (`~/projects`)
   - Artı: Linux izinleri gerçek çalışır; Microsoft'un WSL belgesi performans
     için projelerin Linux dosya sisteminde tutulmasını önerir (kendi ölçümümüz yok).
   - Eksi: Windows araçları `\\wsl.localhost` üzerinden erişir; Rider
     performansı bilinmiyor; Windows yedeğine girmez.

## Karar
WSL (`~/projects/muhammedogur.com`). Gerekçe: izinlerin doğru çalışması
ve önerilen performans yolu.

## Sonuçlar
- Kazanç: izinler kontrol altında; sunucuyla (Linux) aynı ortam.
- Bedel: dosyalar yalnız WSL içinde; push edilmemiş iş bilgisayar
  arızasında risk altında (10.2026'da laptop arızası yaşandı) → sık push.
  Windows araçlarıyla erişim dolaylı.

## Yeniden bakma tetikleyicisi
D2'de Rider, WSL'deki depoda ölçülmüş ve belirgin şekilde yavaş çalışırsa
ya da yalnızca Windows'ta çalışan bir araç gerekirse.
