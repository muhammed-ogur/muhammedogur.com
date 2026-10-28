# 0002 Klavye düzeni: US tabanlı hibrit düzen

- Durum: Kabul edildi · 2026-10-12 · **Uygulama değişti · 2026-10-24** (MSKLC → Keyman) · **2026-10-26** (Keyman → PowerToys; bkz. Ek 2)

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

## Ek — 2026-10-24: MSKLC yerine Keyman
**Ne oldu:** Windows 11 Smart App Control (Akıllı Uygulama Denetimi) "Açık" konumda. Açılışta `LogonUI.exe`, MSKLC'nin ürettiği imzasız `usTRmlz.dll`'i yüklemeye çalıştı, Code Integrity engelledi (Policy ID `{0283ac0f-fff1-49ae-ada1-8a933130cad6}`); düzen çalışmadı, her kilit/açılışta "Bu uygulamanın bir kısmı engellendi" bildirimi geldi.

**Değerlendirilen yollar:**
- Smart App Control'ü kapatmak → reddedildi: geri açmak için Windows'u sıfırdan kurmak gerekir, güvenlik katmanı kaybı.
- DLL'i imzalatmak → reddedildi: self-signed kabul edilmiyor; Azure Artifact Signing bireysele yalnız ABD/Kanada; OV sertifika 150–300 $/yıl (bütçe ₺0).
- DLL'i elle System32'ye kopyalayıp kaydetmek → işe yaramaz: engellenen şey kurulum dosyası değil, DLL'in yüklenmesi.
- KbdEdit → aynı sorun (imzasız DLL üretir). Kanata / KMonad → imzasız, sınırlı.
- PowerToys Keyboard Manager → imzalı ama yönetici pencereleri ve giriş ekranında çalışmaz, AltGr sorunlu.
- US + Türkçe Q arasında geçiş → i/ı ve noktalama tuzağı, mod hatası.
- **Keyman (SIL Global)** → seçildi.

**Karar:** Aynı eşleme Keyman klavyesi olarak (`usTRmlz.kmn` → `.kmx` → `.kmp`), kmc 18.0.254 ile derlendi. Kaynak: `Claude outputs/klavye/keyman/`. Windows'ta yalnız Türkçe Q kurulu; US tabanı ve Caps(RAlt) ile ş ğ ü ö ç ı İ â ₺ € dosyada tanımlı. MSKLC paketi ve programı kaldırıldı.
- Neden geçti: Keyman kurulumu ve motor DLL'i (`keyman64.dll`) SIL imzalı, zincir Microsoft Identity Verification Root (Microsoft ID Verified) → Smart App Control güveniyor. Düzenin kendisi `.kmx` veri dosyası, kod değil.
- Doğrulama (24.10): Not Defteri'nde harf + sembol testi doğru; yönetici terminalinde ş, `:` `"` doğru; yeniden başlatmadan sonra engelleme bildirimi yok.

**Yeni bedeller:**
- Keyman ayrı bir program: açılıştan kısa süre sonra devreye giriyor; o arada Türkçe Q geçerli.
- Giriş/parola ekranında büyük olasılıkla çalışmaz (doğrulanmadı).
- Başka bilgisayarda kurulum: Keyman + `.kmp` + Keychron firmware ayarı.

**Açık:** Varsayılan giriş yöntemi ayarı (Gelişmiş klavye ayarları → Override for default input method) seçildi; açılışta varsayılanın gelişi gözlenecek. İleride düzeni Keychron firmware'ine (QMK) taşıma seçeneği Park'ta.

## Ek 2 — 2026-10-26: Keyman yerine Windows US + PowerToys
**Ne oldu:** Keyman düzeni Electron uygulamalarında (Claude masaüstü, Obsidian) AltGr (Caps = sağ Alt) kombinasyonlarında hiçbir şey yazmadı; AltGr'sız tuşlar (`; ' [ ]`) doğruydu. `LCTRL RALT` kuralları eklenip 1.1 derlendi → düzelmedi. Electron'da AltGr'ın yanlış yorumlandığına dair açık hatalar var (electron/electron #13895); kesin neden doğrulanmadı.

**Karar (kullanıcı):** Keyman ve Keyman Developer kaldırıldı. Yeni kurulum:
- Windows: Türkçe dilinin altında Microsoft'un imzalı **US** düzeni (`041F:00000409`) + eş için Turkish Q (`041F:0000041F`). Varsayılan giriş yöntemi: Turkish – US. US düzeninde sağ Alt düz Alt'tır (sahte LCtrl yok).
- Keychron firmware: Caps = sağ Alt (değişmedi).
- **PowerToys Keyboard Manager**: 16 kısayol → metin (`RAlt+s`=ş, `RAlt+Shift+s`=Ş … ğ ü ö ç ı İ â Â ₺ €; `exactMatch: true`). Ayar dosyası yedeği: `Claude outputs/klavye/powertoys/`.
- PowerToys: başlangıçta çalıştır + her zaman yönetici olarak çalıştır.
- Keyman'den kalan hayalet giriş yöntemi kaydı (`041F:{FE0420F1-…}`) `Set-WinUserLanguageList` ile silindi.

**Doğrulama (26.10):** Claude ve Not Defteri'nde 16 karakter; yönetici terminalinde ş; yeniden başlatma sonrası varsayılan US + kısayollar çalışıyor.

**Bedeller:**
- PowerToys arka planda çalışmalı; başlamadan önceki saniyelerde Caps + harf = Alt + harf (menü kısayolu tetikleyebilir).
- Giriş/parola ekranında çalışmaz.
- Alt tabanlı başka global kısayollarla çakışma riski (ChatGPT Appshots bir kez Alt ile açıldı, sonra tekrarlamadı).
- Başka bilgisayarda kurulum: US düzeni ekle + PowerToys + `default.json` + firmware ayarı.
