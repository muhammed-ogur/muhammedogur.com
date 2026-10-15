# 0003 — Faz 6 sonuna kadar IntelliSense ve Tab tamamlama açık

- **Durum:** Kabul edildi
- **Tarih:** 2026-10-06
- **Karar veren:** Muhammed Ogur

## Bağlam

Öğrenme programında C# ve komut satırı, kod tamamlama olmadan, bakarak değil ezberden yazılarak çalışılıyordu. Amaç canlı kod mülakatlarındaki IDE'siz yazma zayıflığını gidermekti.

Uygulamada bu yöntem ilerlemeyi belirgin biçimde yavaşlattı. Asıl hedef olan muhammedogur.com'u kurmak geride kaldı ve motivasyon düştü. Gerçek işte kod her zaman IDE ve kod tamamlamayla yazılıyor.

## Karar

Faz 6 sonuna kadar şunlar açık:

- IDE'de IntelliSense (kod tamamlama), otomatik formatlama, snippet ve quick fix
- Kabukta Tab tamamlama

Satır ya da blok yazan AI asistanları (Copilot, tam satır öneren IntelliCode, Rider Full Line Completion) B16'ya kadar kapalı kalır. IntelliSense yalnız isimleri ve sözdizimini tamamlar. AI asistanı ise mantığı yazar; öğrenilmesi gereken kısım da mantıktır.

Ezberden yazma çalışmaları kalkar. Yerine anlama ölçülür: kodu kendim kurarım ve neden öyle yazdığımı açıklarım.

## Değerlendirilen seçenekler

1. **Ezberden yazmaya devam etmek.** Reddedildi. Bedeli hız ve motivasyon kaybıydı.
2. **IntelliSense ve AI asistanının ikisini birden açmak.** Reddedildi. Mantığı araç yazınca öğrenme gerçekleşmez.
3. **Yalnız IntelliSense açık, AI asistanı kapalı.** Seçildi.

## Sonuçlar

- **Olumlu:** Proje işi hızlanır. Çalışma ortamı gerçek işe benzer. Pratik ve okumayla doğal bir ezber oluşur.
- **Olumsuz:** IDE'siz canlı kod becerisi kendiliğinden gelişmez. Bu bedel bilerek kabul edildi.

## Yeniden bakma tetikleyicisi

Faz 6 sonunda IntelliSense'siz bir test yapılacak. O tarihte iş bulunamamışsa ve görüşmelerde IDE'siz kod yazma isteniyorsa, ezber çalışması yeniden ele alınacak.
