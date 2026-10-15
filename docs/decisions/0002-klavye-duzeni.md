# 0002 Klavye düzeni: US tabanlı hibrit düzen

- Durum: Kabul edildi · 2026-10-12

## Bağlam
Türkçe Q düzeninde kod yazarken sık kullanılan semboller (`; { } [ ] / \ =`) AltGr ya da uzak tuşlarla yazılıyor; bu, kod yazmayı ve Vim gibi klavye odaklı araçları yavaşlatıyor.
US ANSI düzeninde ise Türkçe karakterler (ş ğ ü ö ç ı İ) yok.
US ile TR arasında sürekli geçiş yapmak hem zaman kaybettiriyor hem de kas hafızasını bozuyor.

Kısıtlar: bütçe ₺0 · Keychron V1 Max (US ANSI) alındı · Windows 11 + WSL · bilgisayarı eşim de Türkçe Q ile kullanacak · mülakat dönemi yaklaşıyor (alışma süresi hızımı geçici düşürecek).

## Seçenekler
1. Türkçe Q'da kalmak
   - Artı: Kurulum ve alışma yok.
   - Eksi: Sembol konforu sorunu çözülmüyor; US ANSI klavyede tuş etiketleri uyuşmuyor.
2. US ve TR arasında geçiş yapmak (Win+Space)
   - Artı: Kurulum gerektirmiyor.
   - Eksi: Her geçişte zaman kaybı; iki düzenin kas hafızası birbirine karışıyor.
3. Hibrit düzen — PowerToys Keyboard Manager ile
   - Artı: Kurulumu kolay.
   - Eksi: Arka planda çalışmak zorunda; yönetici pencerelerinde ve parola ekranında çalışmıyor; AltGr / Ctrl+Alt eşlemesinde sorun var.
4. Hibrit düzen — AutoHotkey ile
   - Artı: Çok esnek.
   - Eksi: Arka planda betik çalışmalı; bakım yükü.
5. **Hibrit düzen — MSKLC ile kendi düzenim (`usTRmlz`)**
   - Artı: Windows'un kendi düzeni olarak çalışır, arka planda program gerekmez; dosyası dotfiles'ta saklanır ve yeniden kurulabilir.
   - Eksi: Kurulum gerekiyor; `₺` cp1254'te yok uyarısı (çalışmayı etkilemiyor).

AltGr'ın yeri için değerlendirilenler: sağ Alt (başparmak, tek elle zor kombinasyonlar) · Caps Lock (sol serçe, ana sırada) · mod-tap Esc+AltGr (hızlı yazımda yanlış tetikleme riski).

## Karar
Seçenek 5: MSKLC ile `usTRmlz` (US + AltGr ile ş ğ ü ö ç ı İ â ₺ €).
- Keychron firmware'de Caps Lock → sağ Alt (AltGr); Fn+Caps = Caps Lock.
- Kural: sol el harfi → sağ başparmak RAlt; sağ el harfi → sol serçe Caps.
- Türkçe Q sistemde kalıyor (eşim için); geçiş Win+Space.
- Esc: Vim'de `Ctrl-[`.
- Düzen dosyası: dotfiles `137d38f`.

Gerekçe: semboller US'te tek tuş ya da ana sıraya yakın; Türkçe harfler geçiş yapmadan erişilebilir; çözüm arka plan programı gerektirmiyor.

## Sonuçlar
- Kazanç: Kod sembolleri, kısayollar ve Türkçe karakterler tek düzende, geçişsiz.
- Bedel: Alışma döneminde hız düştü (baz: Türkçe Q 38 wpm, %87 doğruluk → hibrit 1. gün 20 wpm, %85; bazın %53'ü). Başka bir bilgisayarda kurulum gerekiyor (MSKLC paketi + firmware ayarı); kurulumun yasak olduğu bir iş bilgisayarında Win+Space ile US/TR geçişine dönülür.
- Kısıt (2026-10-14): AltGr, Windows'ta Ctrl+Alt sayılır. Bu yüzden hiçbir uygulamaya Ctrl+Alt + s, g, u, o, c, i, a, t, e kısayolu atanmaz; atanırsa o Türkçe harf yazılamaz (Google Drive'ın Ctrl+Alt+G kısayolu ğ harfini engelledi).

## Yeniden bakma tetikleyicisi
- 3 hafta sonunda (2026-11-02) hibrit düzende Türkçe wpm, Türkçe Q bazının (38 wpm) altında kalırsa.
- İş yerinde kendi düzenimi kuramazsam.
