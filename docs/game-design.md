<!-- Kaynak: docs/issues/egik-kavanoz.md (epic bölümü). GitHub'daki epic issue ile aynı metin; karar değişirse önce burası, sonra epic güncellenir. -->

# Eğik Kavanoz — bırak-birleştir, ama kavanozu yatırabilirsin

## Neden
Sahibi (2026-10-14): Suika tarzı bırak-birleştir mekaniğinde oyuncunun tek kararı "nereye bırakayım"dır; yığın kötü kurulunca geri dönüş yoktur ve tur "kaderine terk edilir". Eğik Kavanoz bu mekaniğe ikinci bir stratejik kaynak ekler: oyuncu kısıtlı sayıda "yatırma" hakkıyla kavanozu 15 derece sola/sağa yatırır, fizik yeniden akar, balıklar köşeye kayar, aynı türler birleşir ve zincir patlar. Yatırma hakkı kıttır (tur başı 2, üst sınır 3) ve birleşmeyle kazanılır; Screwdom'un kıtlık mekaniğinin (5 vida yuvası, D30 %17) bırak-birleştir'e uyarlanmış hali. İzlemesi keyifli olan "yığın kayması" anı Twitch/TikTok klibi için doğal bir "an" üretir.

Araştırma raporu: Daily Drop epic'indeki raporun 5. bölümü (fizik merge ailesi) ve tasarım taslağı **Eğik Kavanoz · Oyun** (12 derece yatık akvaryum kavanozu, köşeye kayan balıklar, Zincir ×3 rozeti, yatırma hakkı 1/2 ve iki yön düğmesi, "Ek yatırma · reklam").

Portföydeki rolü: Daily Drop'un fizik çekirdeğinin (`MustyMergePhysics`) ikinci tüketicisi; aynı çekirdek, farklı kanca. Solo MVP 3 hafta. Daily Drop "ritüel" kancasını, Eğik Kavanoz "kıt kaynak + koleksiyon" kancasını test eder; soft launch verisi hangisinin D7'yi taşıdığını gösterir.

## Hedef kitle ve konumlandırma
- 16–35, küresel; Suika/Watermelon Game sevenler; Twitch/TikTok'ta "yığın kayması" kliplerini izleyen/paylaşan kitle ikincil dağıtım kanalı.
- Mağaza adı: **Tilt Jar: Aquarium Merge** (EN) / **Eğik Kavanoz: Akvaryum Birleştirme** (TR; 30 karakter sınırı için 17. story'de kısaltma kararı). Anahtar kelimeler: watermelon game, physics merge, aquarium, fish merge, suika.
- Konumlandırma cümlesi: "Balıkları bırak, birleştir; sıkışınca kavanozu yatır."
- Rakiplerden farkı: yatırma hakkı (kıt, kazanılan kaynak), su yüzeyi ve taşma (dünya yatayında kalan su), akvaryum koleksiyonu (24 tür, 3 tema). Oyun ekranında banner yok; interstitial yalnızca tur arası ve günde en fazla 3.

## Core loop ve kurallar (v1 kararları)
1. Tur: sonsuz yüksek skor modu. Oyuncu balık bırakır, aynı türler birleşir, taşınca tur biter; rekor ve günlük en iyi tutulur. Süre sayacı yok.
2. Kavanoz: dikey, 9:16; 390×844 referansta genişlik ekranın %84'ü, yükseklik %56'sı; pivot kavanozun geometrik merkezi. Su yüzeyi kavanozun üst kenarının 96 px altında ve **dünya yatayında sabittir** (kavanoz döner, su dönmez). Taşma çizgisi su yüzeyinin 36 px üstünde, kesikli.
3. Balıklar 8 kademe: Boncuk, Güppi, Lepistes, Palyaço, Melek, Kaplumbağa, Yunus, Balina. Aynı kademeden iki balık temas edince bir üst kademeye birleşir; iki Balina birleşince ikisi de yok olur, 1.000 puan.
4. Sıra: rastgele (tur seed'i `Random` tohum, loglanır); gelen kademeler 1–5, ağırlık {1: 0.30, 2: 0.25, 3: 0.20, 4: 0.15, 5: 0.10}; sıradaki 3 balık önizlemede.
5. Kontrol: parmağı yatay sürükle, bırakınca düşer; bir sonraki bırakma 0,45 sn sonra. Bırakma yüksekliği kavanoz ağzının o anki en yüksek noktasının 24 px üstü.
6. Yatırma: üç açı {−15°, 0°, +15°}. "Sola" düğmesi açıyı bir adım sola, "Sağa" bir adım sağa götürür; uçtaki yönde düğme devre dışı. Her adım 1 yatırma hakkı. Dönüş 1,2 sn, ease-in-out; yatırma boyunca ve sonrasında balıklar durana kadar (en çok 2,5 sn) düşürme kilitli. Kavanoz yatık **kalır**; yerçekimi sabit (0, −22).
7. Yatırma hakkı: tur başı 2; her Balina oluşumunda +1; her 500 puan eşiği geçildiğinde +1; stok üst sınırı 3. Ödüllü videoyla +1 (tur başına en çok 2 kez). HUD'da "1/2" sayaç ve iki yön düğmesi.
8. Puan: kademe t birleşmesi `10 × t²`; zincir (0,8 sn penceresinde ardışık birleşme) ×1,5 / ×2 / ×3 (üst sınır); yatırmayla tetiklenen zincir aynı tabloyu kullanır, HUD'da "Zincir ×3" rozeti.
9. Taşma: bir balığın merkezi taşma çizgisinin üstünde 1,2 sn kalırsa tur biter. Tur başına bir kez "Kurtar" (ödüllü video): dünya y'si en yüksek 3 balık silinir ve tur sürer.
10. Akvaryum meta: bir temada ulaşılan her kademe o temanın türünü koleksiyona ekler (8 kademe × 3 tema = 24 tür). Temalar: Tropik (ücretsiz), Buz ve Derin Deniz (her biri 2,99 USD). Rekor duvarı: en yüksek skor, en uzun zincir, en çok Balina, en çok yatırma kazancı.
11. Reklam ilkesi: oyun ekranında ve Home'da reklam yok; interstitial yalnızca tur arası, günde ≤ 3, ilk 2 turda yok, 45 sn'den kısa turdan sonra yok; ödüllü video her zaman oyuncunun seçimi. No Ads (3,99 USD) tüm interstitial'ı kapatır, ödüllü videoyu kapatmaz. Coin/para birimi yok.
12. Erişilebilirlik: Reduce Motion açıkken yatırma anlık (animasyonsuz, su dalgası yok), tür sembolleri (renk körü), büyük yazı modu, Android geri tuşu, ana menü düğmesi, TR/EN birinci sınıf.
13. Fizik: `MustyMergePhysics` paketi (Daily Drop'tan ayrıştırılır), Unity Physics2D, sabit zaman adımı 1/60. Kavanoz `Rigidbody2D` kinematic, `MoveRotation` ile döner; `Physics2D.gravity` döndürme alternatifi **reddedildi** (su yüzeyi ve HUD dünya yatayında kalmalı; yerçekimi döndürülürse su ve görsel karmaşa). Determinizm hedeflenmez; replay/doğrulama SONRA.

## Kapsam dışı (v1)
- Seviye/harita, enerji, booster envanteri, coin.
- Gerçek zamanlı çok oyunculu, turnuva, klan; günlük seed modu (SONRA, Daily Drop ile çapraz tanıtım).
- Klip/video paylaşımı (SONRA); v1'de yalnızca PNG paylaşım kartı.
- Replay kaydı ve sunucu tarafı skor doğrulama (SONRA, Daily Drop 25 ile ortak).
- iOS çıkışı MVP'de yok (Android soft launch sonrası).

## Başarı ölçütleri (kapılar)
- Soft launch (TR, PH, CA; Android; 14 gün): D1 ≥ %35, D7 ≥ %15, oturum başı tur ≥ 3, paylaşım oranı (paylaş dokunan / sonuç gören) ≥ %3, ortalama oturum ≥ 4 dk, çökme oranı < %0,5.
- Yatırma sinyali: turların ≥ %70'inde en az 1 yatırma kullanılmış; yatırma başına ortalama puan ≥ 150 (aksi halde 6–8. story his/kural iterasyonu).
- Kapı geçilirse: iOS build, tema paketi fiyat testi, haftalık liderlik tablosu, Canlı.
- Kapı geçilmezse: iki hafta iterasyon (yatırma kuralları, taşma gerilimi, kurtar teklifi), yine geçilmezse rafa.

## Temalar ve story'ler (sıra)
**P1 · MVP**
- [ ] Eğik Kavanoz 1: Core paketini bağla, MergePhysics paketi ve proje iskeleti
- [ ] Eğik Kavanoz 2: Kavanoz, su yüzeyi ve taşma çizgisi
- [ ] Eğik Kavanoz 3: 8 kademeli balık seti ve birleşme kuralı
- [ ] Eğik Kavanoz 4: Düşürme kontrolü ve sıradaki önizleme
- [ ] Eğik Kavanoz 5: Puanlama, zincir çarpanı ve koşu istatistiği
- [ ] Eğik Kavanoz 6: Yatırma hakkı sistemi ve kazanma kuralları
- [ ] Eğik Kavanoz 7: Kavanoz döndürme fiziği
- [ ] Eğik Kavanoz 8: Yatırma animasyonu, su dalgası, haptik ve girdi kilitleri
- [ ] Eğik Kavanoz 9: His ve juice: birleşme, perde, taşma gerilimi
- [ ] Eğik Kavanoz 10: Tur yapısı: sonsuz yüksek skor, oyun sonu ve yeniden başlat
- [ ] Eğik Kavanoz 11: Ödüllü video: ek yatırma hakkı ve taşma sonrası kurtar
- [ ] Eğik Kavanoz 12: Sonuç ekranı, rekor anı ve günlük en iyi
- [ ] Eğik Kavanoz 13: Akvaryum: tür koleksiyonu
- [ ] Eğik Kavanoz 14: Paylaşım kartı (rekor anı PNG + metin) ve sistem paylaşımı
- [ ] Eğik Kavanoz 15: İlk tur deneyimi (FTUE) ve yatırma öğretimi
- [ ] Eğik Kavanoz 16: Interstitial kotası ve No Ads
- [ ] Eğik Kavanoz 17: Analitik olay şeması ve kapı raporu
- [ ] Eğik Kavanoz 18: Mağaza sayfası, ASO ve soft launch yapılandırması
**P2 · Soft launch**
- [ ] Eğik Kavanoz 19: Akvaryum temaları (Buz, Derin Deniz) ve tema paketleri
- [ ] Eğik Kavanoz 20: Haftalık liderlik tablosu
- [ ] Eğik Kavanoz 21: Ayarlar ve erişilebilirlik
**P3 · Canlı**
- [ ] Eğik Kavanoz 22: Rekor duvarı ve günlük hedefler
**SONRA**
- [ ] Eğik Kavanoz 23: Klip paylaşımı (son 8 sn GIF)
- [ ] Eğik Kavanoz 24: Replay kaydı ve skor doğrulama (Daily Drop ile ortak)
- [ ] Eğik Kavanoz 25: Günlük seed modu ve Daily Drop çapraz tanıtımı
- [ ] Eğik Kavanoz 26: Özel kavanoz şekilleri

## Riskler ve açık sorular
- `MustyMergePhysics` ayrıştırması: Daily Drop'un `Ball`/`MergeSystem`/`OverflowDetector` sınıfları önce pakete taşınmalı; Daily Drop 3 kapanmadan bu paket çıkmaz. 1. story'nin eforu buna bağlı (M → L).
- Hareketli duvar fiziği: kinematic duvar dönerken dinamik balıkların tünelleme (tunneling) ve "fırlama" riski; `Continuous` çarpışma ve yavaş açısal hız (15° / 1,2 sn ≈ 12,5°/sn) ile sınırlanır; 7. story'de Play modunda ölçülür.
- Yatık kavanozda alçak köşe: ağız kenarı 43 px alçalır; balığın ağızdan dışarı kaymasını önleyen görünmez "dudak" collider gerekir (2. ve 7. story).
- Elips balıklar dairelerden daha az öngörülebilir yığılır; `freezeRotation` alternatifi 3. story'de Play modunda karşılaştırılır.
- Klon algısı: balık seti ve akvaryum teması Suika/Watermelon'dan görsel olarak ayrışır; "watermelon game" yalnızca ASO anahtar kelimesidir, mağaza görsellerinde meyve yok.
- TR mağaza adı 30 karakteri aşıyor (34); 18. story'de karar.
- Yatırma dengesi: hak çok bolsa mekanik "ücretsiz karıştırma"ya, çok kıtsa unutulan düğmeye dönüşür; başlangıç değerleri Remote Config'te, soft launch'ta ayarlanır.

## Done when
P1 story'leri kapalı, soft launch 14 gün tamamlanmış, kapı raporu (D1, D7, oturum başı tur, paylaşım, yatırma sinyali) epic'e yorum olarak eklenmiş ve karar (Canlı / iterasyon / raf) yazılmış.
