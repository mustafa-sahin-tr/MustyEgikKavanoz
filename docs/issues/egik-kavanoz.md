<!-- game: Eğik Kavanoz -->
<!-- repo: MustyEgikKavanoz -->
# EPIC: Eğik Kavanoz — bırak-birleştir, ama kavanozu yatırabilirsin
labels: epic, enhancement

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

## STORY: Eğik Kavanoz 1: Core paketini bağla, MergePhysics paketi ve proje iskeleti
labels: tema:backend, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **1. story'si** (epic #EPIC). Tema: Altyapı.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** Unity projesi, `MustyCozyCore` ve `MustyMergePhysics` UPM bağımlılıkları, sahne düzeni, UGS kurulumu

### Kullanıcı hikâyesi
Bir geliştirici olarak Eğik Kavanoz projesinin ortak paketi (reklam, IAP, UGS, yerelleştirme, ayarlar) ve Daily Drop'un fizik çekirdeğini ilk günden kullanmasını isterim, böylece yalnızca yatırma, su ve akvaryum katmanlarını yazarım.

### Kabul kriterleri
- [ ] **Senaryo: Temiz kurulum** · Verilen boş bir Unity 2022 LTS projesi · Eğer `MustyCozyCore` ve `MustyMergePhysics` paketleri git URL ile eklenirse · O zaman proje derlenir ve `Bootstrap` sahnesi açılınca UGS oturumu (anonim Player Accounts) kurulur.
- [ ] **Senaryo: MergePhysics örnek sahnesi** · Verilen paketin `Samples~/BasicJar` sahnesi · Eğer Play'e basılırsa · O zaman daire gövdeli 8 kademe birleşme, taşma tespiti ve zincir puanı oyun kodu olmadan çalışır (paketin kendi smoke testi).
- [ ] **Senaryo: Sahne akışı** · Verilen `Bootstrap → Home → Play → Result → Aquarium` sahneleri · Eğer uygulama açılırsa · O zaman 2 sn içinde Home görünür; geri tuşu Home'da uygulamadan çıkar, diğer sahnelerde bir geri gider (Play'de önce "Çık?" onayı).
- [ ] **Senaryo: Yerelleştirme** · Verilen TR ve EN tabloları · Eğer cihaz dili TR ise · O zaman tüm metinler TR, aksi halde EN; dil Ayarlar'dan değiştirilebilir; 8 balık adı iki dilde de tabloda.
- [ ] **Senaryo: Build** · Verilen Android build ayarları · Eğer `release` build alınırsa · O zaman paket adı `com.mtafasahin.tiltjar`, min SDK 24, 64-bit, boyut < 70 MB.
- [ ] **Senaryo: Remote Config** · Verilen UGS Remote Config · Eğer `tilt_angle_deg` 15'ten 12'ye çekilirse · O zaman yeni oturumda kavanoz 12 derece yatar.

### Teknik görevler (Unity/C#)
- [ ] Repo iskeleti: `Assets/_Game/{Scripts,Prefabs,Art,Audio,Data,Shaders}`, `.gitignore`, `docs/game-design.md` (bu epic'in kuralları), `docs/analytics.md`, `docs/tilt-physics.md` (7. story kararları).
- [ ] `MustyMergePhysics` paketi (ayrı repo, UPM): Daily Drop'tan `Ball → MergeBody`, `BallDefinition → MergeDefinition` (collider tipi: Circle | Capsule), `MergeSystem`, `BallPool → MergePool`, `OverflowDetector` (dünya y tabanlı, referans çizgi dışarıdan verilir), `ChainScorer`, `DropController` (bırakma yüksekliği ve x aralığı delegeyle), `MergeEvent`. Daily Drop bu pakete geçirilir (ayrı issue o repoda).
- [ ] `MustyCozyCore` bağımlılığı (UPM git URL, sürüm etiketi): `AdsService`, `IapService`, `UgsBootstrap`, `Localization`, `SettingsScreen`, `BackButtonRouter`, `Haptics`, `ShareService`.
- [ ] Sahne yöneticisi: `SceneFlow` (async yükleme, geçiş perdesi), `AppState` (ScriptableObject tabanlı oturum durumu), `RunState` (tur içi durum).
- [ ] UGS: Player Accounts anonim giriş, Cloud Save, Analytics, Remote Config, Leaderboards projesi ve ortamları (`dev`, `prod`).
- [ ] Remote Config şeması: `tilt_angle_deg`, `tilt_duration_ms`, `tilt_start_charges`, `tilt_max_charges`, `tilt_score_step`, `tilt_rewarded_per_run`, `rescue_enabled`, `overflow_grace_ms`, `drop_cooldown_ms`, `tier_weights`, `interstitial_daily_cap`, `interstitial_min_run_s`.
- [ ] CI: GitHub Actions ile Android debug build (GameCI); artefakt olarak APK; paket testleri (`MustyMergePhysics` Edit Mode) CI'da koşar.
- [ ] Kod stili ve analiz: `.editorconfig`, Roslyn analizörleri, `Assembly Definition` dosyaları (Game, Game.Editor, Tests).

### Analitik
- `app_open` {first_open: bool, locale: string, build: int} — her açılışta
- `session_start` {runs_today: int} — Home görünür olduğunda

### Notlar
- Karar: fizik çekirdeği paket olarak ayrışır; Eğik Kavanoz'a özgü olan yalnızca `Jar` (döner kavanoz), `WaterSurface`, `TiltSystem`, `Aquarium`. Daily Drop 3 kapanmadan paket ayrıştırılamaz; o durumda bu story'nin eforu L olur ve ilk hafta burada geçer.
- Karar: reklam mediation AppLovin MAX (Core ile aynı); bu oyunda iki ödüllü yerleşim (`tilt_extra`, `rescue`) ve bir interstitial yerleşimi (`run_end`).

## STORY: Eğik Kavanoz 2: Kavanoz, su yüzeyi ve taşma çizgisi
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **2. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `Play` sahnesi, `Jar` prefab, `WaterSurface`, `OverflowDetector` (paket), `GameOverFlow`

### Kullanıcı hikâyesi
Bir oyuncu olarak balıkların içine düştüğü akvaryum kavanozunu, suyun yüzeyini ve üstündeki taşma çizgisini net görmek isterim, böylece ne zaman risk aldığımı bilirim ve kavanoz yatsa bile su "su gibi" dursun.

### Kabul kriterleri
- [ ] **Senaryo: Kavanoz ölçüsü** · Verilen 390×844 referans ekran · Eğer Play sahnesi açılırsa · O zaman kavanoz genişliği ekranın %84'ü, yüksekliği %56'sı; iki duvar ve taban `BoxCollider2D`, alt köşeler 32 px yuvarlatılmış (`EdgeCollider2D`, 6 segment); pivot kavanozun geometrik merkezi.
- [ ] **Senaryo: Su yüzeyi** · Verilen kavanoz 0° · Eğer oyun başlarsa · O zaman su yüzeyi kavanozun üst kenarının 96 px altında, kavanozun içinde maskelenmiş (`SpriteMask`), hafif sinüs dalgalı (genlik 3 px, periyot 2,4 sn).
- [ ] **Senaryo: Su dünya yatayında** · Verilen kavanoz −15° yatık · Eğer su çizilirse · O zaman su yüzeyi dünya yatayında ve aynı dünya y'sinde kalır; kavanozun alçak köşesinde su kenara daha yakın, yüksek köşesinde daha uzak görünür.
- [ ] **Senaryo: Taşma çizgisi** · Verilen su yüzeyi · Eğer oyun başlarsa · O zaman su yüzeyinin 36 px üstünde dünya yatayında kesikli çizgi ve sağında "taşma" etiketi görünür; çizgi kavanozla dönmez.
- [ ] **Senaryo: Taşma tespiti** · Verilen durağan bir balık · Eğer balığın merkezi çizginin üstünde `overflow_grace_ms` (1200) boyunca kalırsa · O zaman tur biter, girdi kilitlenir ve Kurtar teklifi (11. story) ya da Result akışı başlar.
- [ ] **Senaryo: Geçici taşma** · Verilen yeni düşen bir balık · Eğer sıçrayıp 1,2 sn içinde çizginin altına inerse · O zaman tur bitmez; yatırma sırasında kayan balık da aynı kurala tabidir.
- [ ] **Senaryo: Alçak köşe dudağı** · Verilen kavanoz +15° · Eğer bir balık alçak köşeye kayarsa · O zaman kavanoz ağzından dışarı çıkmaz (ağız kenarında 24 px içe dönük görünmez dudak collider).
- [ ] **Senaryo: Uygulama arka plana** · Verilen devam eden tur · Eğer uygulama arka plana alınırsa · O zaman fizik durur, dönünce kaldığı yerden sürer.

### Teknik görevler (Unity/C#)
- [ ] `Jar` prefab: kök `Rigidbody2D` (bodyType Kinematic, `interpolation Interpolate`), child collider'lar (2 duvar, taban, 2 köşe edge, 2 dudak edge), görsel cam sprite (9-slice + kenar parıltısı), `JarGeometry` (ağız uç noktalarını dünya koordinatında döndüren yardımcı).
- [ ] `WaterSurface`: kavanoz içini dolduran `SpriteRenderer` + `SpriteMask` (kavanozun iç şekli, kavanozla döner); su sprite'ı dünya yatayında sabit bir `Transform` altında (kavanozun child'ı değil); yüzey çizgisi `LineRenderer` 32 nokta sinüs (shader yerine CPU, 8. story shader'a taşır).
- [ ] `OverflowLine`: dünya uzayında sabit `Transform`; `OverflowDetector`'a referans y verir (paket API'si: `SetThresholdY(float)`).
- [ ] `OverflowDetector` (paket): her `FixedUpdate`'te aktif balıkların `Rigidbody2D.position.y` ve `velocity.magnitude < 0.05`; balıklara bağlı `OverflowTimer`; yatırma sırasında (`TiltSystem.IsTilting`) hız eşiği 0,3'e gevşetilir (kayan balık "durağan" sayılmaz).
- [ ] `GameOverFlow`: girdi kilidi, `RunState.Finished`, Kurtar teklifi kancası, Result'a geçiş; `RunState` enum {Idle, Playing, Tilting, Overflow, Finished}.
- [ ] Fizik ayarları: `Time.fixedDeltaTime = 1/60`, `Physics2D.gravity = (0, -22)`, `velocityIterations 8`, `positionIterations 3`; katmanlar: Fish, Jar, Lip.
- [ ] Güvenli alan: `SafeArea` bileşeni; Pause: `OnApplicationPause` ile `Time.timeScale` ve girdi kilidi.
- [ ] Edit Mode testi: `OverflowDetector` zamanlayıcı ve yatırma gevşetmesi; `JarGeometry` ağız noktaları (0°, ±15°).

### Analitik
- `run_end` {run_id: string, drop_count: int, score: int, cause: "overflow"|"quit", duration_s: int, tilts_used: int} — tur bittiğinde

### Notlar
- Karar: su hacmi simüle edilmez; su yüzeyi dünya y'sinde sabittir. Kavanoz yatınca suyun "köşeye dolması" görsel olarak maske sayesinde kendiliğinden doğru görünür.
- Yerçekimi, grace ve geometri değerleri başlangıç; Play modunda ayarlanır, Remote Config'ten gelir.
- Tasarım taslağında kavanoz 12° çizilmiş; v1 kararı 15° (daha belirgin kayma), `tilt_angle_deg` ile 12'ye çekilebilir.

## STORY: Eğik Kavanoz 3: 8 kademeli balık seti ve birleşme kuralı
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **3. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `FishDefinition` (SO, `MergeDefinition` türevi), `Fish` prefab, `MergeSystem` (paket), `MergePool`

### Kullanıcı hikâyesi
Bir oyuncu olarak aynı türden iki balığın değince bir üst türe dönüştüğünü ve iki Balina'nın birleşince kaybolduğunu görmek isterim, böylece yer açma stratejisi kurabilirim.

### Kabul kriterleri
- [ ] **Senaryo: Kademe tablosu** · Verilen `FishDefinition` listesi · Eğer oyun yüklenirse · O zaman 8 kademe vardır; elips ölçüleri (genişlik × yükseklik px, 390 referans): 28×20, 36×26, 46×32, 58×40, 72×52, 90×64, 112×76, 136×92; adlar TR/EN (Boncuk, Güppi, Lepistes, Palyaço, Melek, Kaplumbağa, Yunus, Balina / Bead, Guppy, Molly, Clownfish, Angelfish, Turtle, Dolphin, Whale).
- [ ] **Senaryo: Birleşme** · Verilen aynı kademeden iki balık · Eğer temas ederlerse · O zaman ikisi yok olur ve temas noktasında bir üst kademe balık oluşur; yeni balığın hızı ikisinin ortalaması, yönü hızın işaretine göre.
- [ ] **Senaryo: Üçlü temas** · Verilen aynı kademeden üç balık aynı anda temas · Eğer birleşme tetiklenirse · O zaman yalnızca bir çift birleşir, üçüncü kalır.
- [ ] **Senaryo: Balina** · Verilen iki 8. kademe balık · Eğer temas ederlerse · O zaman ikisi de yok olur, 1.000 puan, ekran parıltısı ve 1 yatırma hakkı (6. story) verilir.
- [ ] **Senaryo: Yön** · Verilen hareket eden balık · Eğer `velocity.x` işareti değişirse · O zaman sprite 120 ms içinde yüzer gibi diğer yöne döner (kuyruk arkada), collider değişmez.
- [ ] **Senaryo: Havuz** · Verilen 60 balık · Eğer sürekli birleşme olursa · O zaman `Instantiate/Destroy` çağrısı yapılmaz; GC alloc 0 (Profiler).
- [ ] **Senaryo: Renk körü** · Verilen Ayarlar'da "Tür sembolleri" açık · Eğer balıklar çizilirse · O zaman her kademede farklı sembol (1–8 nokta/şekil) balığın gövdesinde görünür.

### Teknik görevler (Unity/C#)
- [ ] `FishDefinition : MergeDefinition` ScriptableObject: tier, size (Vector2), color, bodySprite, eyeSprite, tailSprite, symbolSprite, mergeScore, nameKey, themeId (13. story koleksiyon için).
- [ ] `Fish` prefab: `CapsuleCollider2D` (yatay, size = elips), `Rigidbody2D` (mass alanla orantılı, `collisionDetectionMode Continuous`, `angularDrag 2.0`), `SpriteRenderer` gövde + child göz ve kuyruk, `FishView` (yön flip, kuyruk sallanma 2 Hz), `MergeBody` bileşeni (paket).
- [ ] `MergeSystem` (paket): `OnCollisionEnter2D` → `mergeLock`, aynı karede çift birleşmeyi önleyen `HashSet<int>`, birleşme kuyruğu `FixedUpdate` sonunda; `OnCollisionStay2D` ile yatırma sonrası kayan balıkların temas-ı birleşmesi (Enter kaçırılırsa).
- [ ] `MergePool` (ObjectPool<MergeBody>), kademe başına ön ısıtma 12.
- [ ] Fizik materyali: friction 0.25, bounciness 0.08; kademe büyüdükçe `linearDrag` 0.2 → 0.6.
- [ ] `MergeEvent` (tier, position, chainIndex, causedByTilt: bool) — 5, 6, 9 ve 13. story'ler dinler.
- [ ] Play Mode testi: aynı kademe iki balığın 1 sn içinde tek balığa dönüştüğü; yatık kavanozda (±15°) kayarak temas eden çiftin birleştiği.
- [ ] Play modu karşılaştırması: serbest dönüş + `angularDrag 2.0` ile `freezeRotation = true`; karar `docs/game-design.md`'ye yazılır.

### Analitik
- `merge` {tier: int, chain: int, by_tilt: bool, drop_count: int} — her birleşmede (tier ≥ 5 için gönderilir; küçükler sayılıp `run_end`'de toplam olarak gider)

### Notlar
- Suika'dan ayrışma: meyve değil akvaryum; 8. kademe Balina. Sanat: düz renk + tek vurgu gölgesi + göz; Tropik tema varsayılan set.
- Elipsler dairelere göre daha "kilitli" yığılır; yatırma tam da bu kilidi açmak için var. `freezeRotation` denemesi balıkları "yüzer" tutar ama yığın fiziğini düzleştirir; Play modunda karar.
## STORY: Eğik Kavanoz 4: Düşürme kontrolü ve sıradaki önizleme
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **4. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `DropController` (paket), `JarDropBounds`, `InputRouter`, `NextPreview` HUD, `RunSequence`

### Kullanıcı hikâyesi
Bir oyuncu olarak parmağımı sürükleyip bıraktığım yere balığın düşmesini ve sıradaki üç balığı görmeyi isterim; kavanoz yatıkken de bırakma aralığının kavanozun ağzına uymasını isterim.

### Kabul kriterleri
- [ ] **Senaryo: Sürükleme** · Verilen elde tutulan balık · Eğer parmak yatay sürüklenirse · O zaman balık kavanoz ağzının iki ucu arasında (yarı genişlik payıyla) parmağı izler; dikey hareket yok sayılır.
- [ ] **Senaryo: Yatık ağız** · Verilen kavanoz +15° · Eğer sürüklenirse · O zaman bırakma aralığı ağzın dünya x izdüşümüdür ve bırakma yüksekliği ağzın yüksek ucunun 24 px üstüdür; balık düşerken ağız kenarına çarpmaz.
- [ ] **Senaryo: Bırakma** · Verilen sürüklenen balık · Eğer parmak kaldırılırsa · O zaman balık o x konumundan düşer; noktalı kılavuz çizgisi (dünya dikeyinde) bırakma anında kaybolur.
- [ ] **Senaryo: Bekleme** · Verilen yeni bırakılmış balık · Eğer `drop_cooldown_ms` (450) dolmadan dokunulursa · O zaman yeni balık gelmez; dolunca sıradaki balık elde belirir.
- [ ] **Senaryo: Yatırma kilidi** · Verilen `RunState.Tilting` · Eğer dokunulursa · O zaman sürükleme başlamaz ve eldeki balık ağzın ortasında bekler; kilit kalkınca kaldığı yerden sürer (8. story kilidi tanımlar).
- [ ] **Senaryo: Önizleme** · Verilen tur sırası · Eğer tur sürüyorsa · O zaman HUD'da sıradaki 3 balık küçükten büyüğe soluklaşarak görünür ve her düşüşte kayar.
- [ ] **Senaryo: Dokunma dışı** · Verilen HUD düğmesine (yatırma, duraklat) dokunma · Eğer parmak düğme üzerindeyse · O zaman balık sürüklenmez (UI raycast önceliği).

### Teknik görevler (Unity/C#)
- [ ] `InputRouter`: Unity Input System, `Pointer` aksiyonları; `EventSystem.IsPointerOverGameObject` ile UI önceliği.
- [ ] `DropController` (paket): durum makinesi {Waiting, Holding, Dropped, Locked}, cooldown, elde balık (kinematic) → bırakınca dynamic; `IDropBounds` delegesi ile x aralığı ve y yüksekliği dışarıdan.
- [ ] `JarDropBounds : IDropBounds`: `JarGeometry`'den ağız uçlarının dünya x'i ve en yüksek ucun y'si; her karede güncellenir (yatırma sırasında da doğru).
- [ ] Kılavuz çizgi: `LineRenderer` noktalı materyal, dünya dikeyinde; Reduce Motion'da düz ince çizgi.
- [ ] `RunSequence`: xoshiro256** PRNG (paket `WeightedSequence`), tur seed'i `run_id`'den; `Peek(3)` ve `Next()`.
- [ ] `NextPreview`: UI ikonları `FishDefinition.bodySprite`; 3 slot, kayma tween 120 ms.
- [ ] Haptik: bırakmada hafif (Core `Haptics.Light`).
- [ ] Hata yolu: parmak ağız dışında kaldırılırsa en yakın geçerli x'e kırpılır; yatırma sırasında kaldırılırsa bırakma iptal, balık elde kalır.

### Analitik
- `drop` {run_id: string, n: int, tier: int, x_norm: float, jar_angle: int} — her bırakmada (yalnızca ilk 10 düşüş ve her 10'da bir)

### Notlar
- Cooldown ve kılavuz görünürlüğü Remote Config.
- Karar: eldeki balık kavanozla dönmez; dünya dikeyinde bekler ve dünya dikeyinde düşer. Yatık ağızda alçak uca bırakılan balık ağız kenarına çarpabilir; `JarDropBounds` aralığı alçak uçtan balığın yarı genişliği + 8 px içeri çeker.

## STORY: Eğik Kavanoz 5: Puanlama, zincir çarpanı ve koşu istatistiği
labels: tema:cekirdek, P1, efor:S
milestone: MVP

**Eğik Kavanoz** epic'inin **5. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** S · **Milestone:** MVP · **Dokunduğu yer:** `ScoreSystem`, `ChainScorer` (paket), HUD skor ve zincir rozeti, `RunStats`

### Kullanıcı hikâyesi
Bir oyuncu olarak büyük birleşmelerin ve özellikle yatırmanın tetiklediği zincirlerin çok puan getirdiğini anında görmek isterim, böylece yatırma hakkını "doğru anda" harcamanın karşılığını hissederim.

### Kabul kriterleri
- [ ] **Senaryo: Temel puan** · Verilen kademe t birleşmesi · Eğer birleşme olursa · O zaman puan `10 × t²` (t=1: 10 … t=7→8: 490) eklenir; Balina+Balina 1.000 puan.
- [ ] **Senaryo: Zincir** · Verilen ardışık birleşmeler · Eğer ikinci ve sonraki birleşmeler 0,8 sn içinde gelirse · O zaman çarpan ×1,5, ×2, ×3 (üst sınır) uygulanır ve HUD'da "Zincir ×2" rozeti görünür; rozet pencere kapanınca 300 ms'de söner.
- [ ] **Senaryo: Yatırma zinciri** · Verilen bir yatırmanın tetiklediği 4 birleşme · Eğer hepsi 0,8 sn pencereleriyle art arda gelirse · O zaman zincir ×3'e ulaşır, rozet "Zincir ×3" ve altında "yatırma" etiketi; `RunStats.tiltChainMax` güncellenir.
- [ ] **Senaryo: Skor animasyonu** · Verilen puan artışı · Eğer skor değişirse · O zaman sayaç 250 ms'de sayarak yükselir ve %105 pop yapar; Reduce Motion'da anında.
- [ ] **Senaryo: Puan eşiği** · Verilen skor 480 → 530 · Eğer 500 eşiği geçilirse · O zaman `ScoreThresholdCrossed(500)` olayı yayınlanır (6. story yatırma hakkı için dinler); eşik tur içinde yalnızca bir kez tetiklenir.
- [ ] **Senaryo: Koşu istatistiği** · Verilen tur sonu · Eğer Result açılırsa · O zaman `RunStats` {runId, score, dropCount, maxTier, chainMax, tiltChainMax, tiltsUsed, tiltsEarned, whales, durationS, rescued} doludur.
- [ ] **Senaryo: Deterministik puan** · Verilen aynı birleşme dizisi · Eğer iki kez hesaplanırsa · O zaman skor aynıdır (int aritmetik).

### Teknik görevler (Unity/C#)
- [ ] `ChainScorer` (paket): `MergeEvent` dinler, zincir zamanlayıcı, int aritmetik; `ScoreTable` SO.
- [ ] `ScoreSystem`: `ChainScorer` üstünde eşik olayları (`tilt_score_step` = 500), `RunStats` yazımı, `causedByTilt` bayrağıyla yatırma zinciri istatistiği.
- [ ] `RunStats` struct + `RunStatsRecorder`; `run_id` = `Guid` kısa biçim (8 karakter), tur başında üretilir.
- [ ] HUD: `ScoreView` (sayaç tween, pop), `ChainBadge` (çarpan + "yatırma" alt etiketi).
- [ ] Remote Config: `chain_window_ms`, `chain_multipliers`, `tilt_score_step`.
- [ ] Edit Mode testi: puan tablosu, zincir çarpanı, eşik olayının tek seferlik tetiklenmesi.

### Analitik
- `run_end` parametrelerine ekle: {max_tier: int, chain_max: int, tilt_chain_max: int, whales: int}

### Notlar
- Puan tablosu başlangıç; soft launch'ta "yatırma başına ortalama puan ≥ 150" kapısına göre ayarlanır.
- Tartışma: yatırma zincirine ek bonus (×1,25) verilsin mi? Karar v1: hayır; yatırmanın ödülü zaten zincir. Veriye göre P2'de yeniden bakılır.

## STORY: Eğik Kavanoz 6: Yatırma hakkı sistemi ve kazanma kuralları
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **6. story'si** (epic #EPIC). Tema: Yatırma.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `TiltCharges`, `TiltSystem` (durum), HUD yatırma sayacı ve yön düğmeleri

### Kullanıcı hikâyesi
Bir oyuncu olarak elimde kaç yatırma hakkım olduğunu, hangi yöne yatırabileceğimi ve yeni hakkı nasıl kazanacağımı her an görmek isterim, böylece yatırmayı "panik düğmesi" değil, planladığım bir hamle olarak kullanırım.

### Kabul kriterleri
- [ ] **Senaryo: Başlangıç** · Verilen yeni tur · Eğer Play açılırsa · O zaman `TiltCharges.current = 2`, HUD'da "2/3" sayaç, kavanoz 0°, "Sola" ve "Sağa" düğmeleri aktif.
- [ ] **Senaryo: Harcama** · Verilen 2 hak, kavanoz 0° · Eğer "Sağa" basılırsa · O zaman hak 1'e düşer, hedef açı +15°, `TiltRequested(+1)` olayı yayınlanır (7. story döndürür); sayaç 150 ms'de "1/3"e küçülerek güncellenir.
- [ ] **Senaryo: Uç sınır** · Verilen kavanoz +15° · Eğer HUD çizilirse · O zaman "Sağa" devre dışı (soluk), "Sola" aktif; +15°'den "Sola" 0°'ye götürür (yine 1 hak).
- [ ] **Senaryo: Hak yok** · Verilen 0 hak · Eğer HUD çizilirse · O zaman iki yön düğmesi de devre dışı ve altında "Ek yatırma · reklam" düğmesi görünür (11. story; reklam yüklü değilse gizli).
- [ ] **Senaryo: Balina kazancı** · Verilen 1 hak · Eğer bir Balina oluşursa (7→8 birleşmesi) · O zaman hak 2 olur ve sayaç yanında "+1" uçan etiket; Balina+Balina yok oluşunda da +1.
- [ ] **Senaryo: Puan kazancı** · Verilen 1 hak · Eğer skor 500, 1.000, 1.500 … eşiklerinden birini geçerse · O zaman hak +1; aynı eşik iki kez vermez.
- [ ] **Senaryo: Üst sınır** · Verilen 3 hak · Eğer yeni kazanç gelirse · O zaman hak 3'te kalır, sayaç "3/3" ve 200 ms sarı titreme ("dolu"); kazanç yanmış sayılır ve `tilt_charge_wasted` olayı gider.
- [ ] **Senaryo: Yatırma sırasında** · Verilen `RunState.Tilting` · Eğer yön düğmesine basılırsa · O zaman girdi yok sayılır (ardışık yatırma kuyruklanmaz).

### Teknik görevler (Unity/C#)
- [ ] `TiltCharges`: {current, max, earnedThisRun, usedThisRun, rewardedThisRun}; `TrySpend()`, `Grant(source)`; `tilt_start_charges`, `tilt_max_charges` Remote Config.
- [ ] `TiltSystem` (durum katmanı): `targetStep ∈ {-1, 0, +1}`, `CanTilt(direction)`, `RequestTilt(direction)` → `TiltRequested` olayı; `IsTilting` bayrağı 7/8. story'den okunur.
- [ ] Kazanç dinleyicileri: `MergeEvent` (tier 8 oluşumu ve 8+8 yok oluşu), `ScoreThresholdCrossed`.
- [ ] HUD: `TiltChargeView` (sayaç, "+1" uçan etiket, dolu titremesi), `TiltButtons` (iki yön, devre dışı durumları, ödüllü video düğmesi yuvası).
- [ ] `RunState`'e `jarStep` ve `TiltCharges` anlık görüntüsü (10. story kaldığı yerden devam için).
- [ ] Edit Mode testi: harcama/kazanç/üst sınır/uç yön kuralları.

### Analitik
- `tilt_use` {run_id: string, direction: "left"|"right", from_step: int, to_step: int, charges_left: int, drop_count: int, score: int} — her yatırmada
- `tilt_charge_earned` {source: "whale"|"score"|"rewarded", charges: int} — her kazançta
- `tilt_charge_wasted` {source: string} — üst sınırda yanan kazançta

### Notlar
- Denge riski epic'te: hak bol → "ücretsiz karıştırma", hak kıt → unutulan düğme. Tüm sayılar Remote Config; soft launch'ta `tilt_use` / `run_end.tilts_used` dağılımına bakılır.
- Karar: 0°'ye geri dönüş de hak harcar (aksi halde her yatırma iki hamle eder, kıtlık bozulur).

## STORY: Eğik Kavanoz 7: Kavanoz döndürme fiziği
labels: tema:cekirdek, P1, efor:L
milestone: MVP

**Eğik Kavanoz** epic'inin **7. story'si** (epic #EPIC). Tema: Yatırma.

**Öncelik:** P1 · **Efor:** L · **Milestone:** MVP · **Dokunduğu yer:** `JarRotator`, `Jar` Rigidbody2D (kinematic), `OverflowDetector` gevşetmesi, `docs/tilt-physics.md`

### Kullanıcı hikâyesi
Bir oyuncu olarak kavanozu yatırdığımda balıkların gerçek bir kavanozdaki gibi kayıp köşede toplanmasını, duvarları delip geçmemesini ve fırlamamasını isterim, böylece yatırma hamlesi güvenilir ve öngörülebilir olsun.

### Kabul kriterleri
- [ ] **Senaryo: Döndürme** · Verilen `TiltRequested(+1)` · Eğer kavanoz 0°'deyse · O zaman `Rigidbody2D.MoveRotation` ile `FixedUpdate`'te 1,2 sn boyunca +15°'ye döner; duvar collider'ları kavanozla döner; `Physics2D.gravity` değişmez.
- [ ] **Senaryo: Balıklar kayar** · Verilen 20 balıklı yığın · Eğer kavanoz +15°'ye dönerse · O zaman balıklar alçak köşeye doğru kayar; dönüş bittikten en çok 2,5 sn sonra tüm balıkların hızı < 0,05 (dinlenme).
- [ ] **Senaryo: Tünelleme yok** · Verilen 60 balık, 200 ardışık yatırma (otomatik test) · Eğer simülasyon biterse · O zaman hiçbir balığın merkezi kavanoz collider'ının dışında değildir.
- [ ] **Senaryo: Fırlama yok** · Verilen dönüş · Eğer balık hızları ölçülürse · O zaman dönüş sırasında hiçbir balığın `velocity.magnitude` 14 birimi (serbest düşüş ~ 1 sn) aşmaz.
- [ ] **Senaryo: Aynı tür temas** · Verilen alçak köşede aynı kademeden iki balık · Eğer kayarak temas ederlerse · O zaman birleşirler (`OnCollisionStay2D` yolu dahil); zincir çarpanı uygulanır.
- [ ] **Senaryo: Yatık kalış** · Verilen kavanoz +15° ve dönüş bitti · Eğer 10 balık daha bırakılırsa · O zaman kavanoz +15°'de kalır, balıklar yatık tabanda birikir ve taşma dünya yatayındaki çizgiye göre ölçülür.
- [ ] **Senaryo: Taşma gevşetmesi** · Verilen yatırma sırasında çizgi üstüne kayan balık · Eğer 1,2 sn içinde çizgi altına inerse · O zaman tur bitmez; dönüş bittikten sonra çizgi üstünde 1,2 sn durursa biter.
- [ ] **Senaryo: Performans** · Verilen orta sınıf Android (Snapdragon 680 sınıfı) · Eğer 60 balıkla yatırma yapılırsa · O zaman `FixedUpdate` + render 16,6 ms altında kalır; kare düşüşü yok.

### Teknik görevler (Unity/C#)
- [ ] `JarRotator`: `FixedUpdate`'te `AnimationCurve` (ease-in-out) ile `MoveRotation(startAngle + curve(t) × delta)`; `Interpolate` açık; bitince `TiltFinished` olayı; `IsTilting` true, balıklar dinlenince (`RestDetector`) `TiltSettled`.
- [ ] `Jar` Rigidbody2D kinematic; tüm duvar/taban/köşe/dudak collider'ları `Jar` child'ı; `useFullKinematicContacts = true` (kinematic-dinamik temas olayları için).
- [ ] Balık `Rigidbody2D`: `collisionDetectionMode Continuous`; `Physics2D` ayarları `defaultContactOffset 0.01`, `velocityThreshold 1`; duvar kalınlığı 24 px (ince duvar tünellemesine karşı).
- [ ] `RestDetector`: aktif balıkların toplam kinetik enerjisi; eşik altında 0,3 sn kalınca `Settled`; üst sınır 2,5 sn (zorunlu kilit açma).
- [ ] `OverflowDetector` ile entegrasyon: `IsTilting || !Settled` iken hız eşiği 0,3.
- [ ] Alternatiflerin kaydı `docs/tilt-physics.md`: (a) `Physics2D.gravity` döndürme — reddedildi: su yüzeyi, taşma çizgisi ve HUD dünya yatayında kalmalı, görsel karmaşa; (b) kavanozu `Transform` ile döndürme — reddedildi: statik collider taşıma fizik senkronunu bozar; (c) seçilen: kinematic `MoveRotation`.
- [ ] Otomatik stres testi (Play Mode, Editor): 60 balık, 200 yatırma, dış-collider ve hız ihlalleri sayılır; CI'da koşar (batchmode).
- [ ] Profiler ölçümü ve `docs/tilt-physics.md`'ye tablo (cihaz, balık sayısı, ms).

### Analitik
- `tilt_physics_anomaly` {run_id: string, kind: "escape"|"velocity", tier: int} — canlıda ihlal yakalanırsa (balık kavanoz dışına çıktıysa geri yerleştirilir ve olay gider)

### Notlar
- Bu story epic'in en riskli parçası; ilk haftanın sonunda Play modunda "kayma hissi" onaylanmadan 8. story'ye geçilmez (başlamadan refine: mockup/prototip, onay, sonra kod).
- Açısal hız 15° / 1,2 sn ≈ 12,5°/sn düşük; tünelleme riski asıl küçük balıkların (28×20) ince duvarla ilişkisinde. Duvar 24 px ve Continuous yeterli gelmezse `fixedDeltaTime` 1/120 denemesi (pil maliyeti).
- Balıkların kavanoz dışına kaçması canlıda yakalanırsa `tilt_physics_anomaly` ile ölçülür ve balık en yakın iç noktaya kinematic yerleştirilir (tur bozulmaz).

## STORY: Eğik Kavanoz 8: Yatırma animasyonu, su dalgası, haptik ve girdi kilitleri
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **8. story'si** (epic #EPIC). Tema: Yatırma.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `TiltPresenter`, `WaterSurface` shader, `Haptics`, `AudioDirector`, `DropController.Locked`

### Kullanıcı hikâyesi
Bir oyuncu olarak kavanozu yatırdığımda suyun çalkalandığını, telefonun titrediğini ve "su sesi" duyduğumu hissetmek isterim; yatırma bitene kadar yanlışlıkla balık bırakamamak isterim, böylece hamle hem keyifli hem güvenli olsun.

### Kabul kriterleri
- [ ] **Senaryo: Kavanoz görseli** · Verilen `TiltRequested` · Eğer dönüş başlarsa · O zaman cam sprite ve kenar parıltısı collider'larla birebir aynı açıda döner (görsel-fizik farkı 0, `Interpolate` sayesinde kare atlamaz).
- [ ] **Senaryo: Su dalgası** · Verilen dönüş · Eğer su yüzeyi çizilirse · O zaman yüzey dünya yatayında kalır; dönüş boyunca sinüs genliği 3 → 10 px, frekans 0,4 → 1,2 Hz yükselir ve bitiminden sonra 1,5 sn'de sönümlenir (shader `_WaveAmp`, `_WaveFreq`).
- [ ] **Senaryo: Köpük** · Verilen dönüş · Eğer su alçak köşeye yaklaşırsa · O zaman kenarda 12–20 küçük kabarcık parçacığı 0,6 sn yükselir.
- [ ] **Senaryo: Haptik ve ses** · Verilen cihaz haptik destekli · Eğer dönüş başlarsa · O zaman Medium haptik ve "su çalkantısı" sesi (1,2 sn, dönüşle senkron); dönüş bitince hafif "tık"; Ayarlar'dan kapatılabilir.
- [ ] **Senaryo: Düşürme kilidi** · Verilen dönüş başladı · Eğer oyuncu sürükler/bırakırsa · O zaman `DropController.Locked`; `TiltSettled` gelince kilit kalkar ve eldeki balık yeniden sürüklenebilir; kilit süresi HUD'da düğmelerin soluklaşmasıyla gösterilir (sayaç yok).
- [ ] **Senaryo: Reduce Motion** · Verilen Reduce Motion açık · Eğer yatırılırsa · O zaman kavanoz anında hedef açıya gider (fizik yine `MoveRotation`, tek adım değil 0,2 sn lineer), su dalgası ve köpük yok, haptik ve ses kalır; kilit `TiltSettled`'a kadar sürer.
- [ ] **Senaryo: Arka plan** · Verilen dönüş ortasında uygulama arka plana · Eğer geri dönülürse · O zaman dönüş kaldığı açıdan sürer; kilit korunur.

### Teknik görevler (Unity/C#)
- [ ] `TiltPresenter`: `TiltRequested/TiltFinished/TiltSettled` dinler; `AnimationCurve` ease-in-out (0,0 → 1,0; 7. story `JarRotator` aynı eğriyi SO'dan okur), haptik, ses, parçacık tetikleri.
- [ ] Su shader (URP 2D, Shader Graph): dünya uzayında UV, `_SurfaceY`, `_WaveAmp`, `_WaveFreq`, `_Time` sinüs; 2. story'deki CPU `LineRenderer` kaldırılır; `SpriteMask` ile kavanoz içine kırpma korunur.
- [ ] Köpük: `ParticleSystem` prefab, dünya uzayı, alçak köşe konumu `JarGeometry`'den.
- [ ] Ses varlıkları: su çalkantısı (1,2 sn), bitiş tıkı; `AudioDirector.PlayTilt(direction)` (sol/sağ hafif pan).
- [ ] `DropController.Locked` durumu (paket API'sine `Lock()/Unlock()`), `TiltButtons` soluklaşma.
- [ ] Reduce Motion dalı: `tilt_duration_ms` → 200, dalga/köpük kapalı; Core `Accessibility.ReduceMotion` bayrağı.
- [ ] Pause senkronu: `OnApplicationPause` sırasında `JarRotator` zamanlayıcısı `Time.timeScale`'e bağlı kalır.

### Analitik
- `tilt_locked_input` {run_id: string} — kilit sırasında dokunma denemesi (kilit süresinin sinir bozucu olup olmadığını ölçmek için)

### Notlar
- Yoğunluk artsın süre uzamasın: dönüş 1,2 sn sabit, efektler bunun içinde biter; kilit süresi (dönüş + dinlenme ≤ 2,5 sn) soft launch'ta `tilt_locked_input` oranına göre kısaltılabilir.
- Reduce Motion'da "anlık" 0,2 sn seçildi: tek karede 15° sıçrama fizikte balıkları duvar içinde bırakabilir.
## STORY: Eğik Kavanoz 9: His ve juice: birleşme, perde, taşma gerilimi
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **9. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `MergeFx`, `AudioDirector`, `Haptics`, `CameraShake`, `OverflowTension`

### Kullanıcı hikâyesi
Bir oyuncu olarak her birleşmenin ses, titreşim ve kabarcıkla "oturduğunu" hissetmek isterim; taşmaya yaklaşınca gerilimi duymak isterim, böylece oyun izlemesi ve oynaması keyifli olsun (klipler için şart).

### Kabul kriterleri
- [ ] **Senaryo: Birleşme efekti** · Verilen kademe t birleşmesi · Eğer yeni balık doğarsa · O zaman 0 → 1,15 → 1,0 ölçek (180 ms), temas noktasında t ile büyüyen halka ve 6–12 kabarcık parçacığı yukarı süzülür.
- [ ] **Senaryo: Perde merdiveni** · Verilen zincir · Eğer ardışık birleşmeler gelirse · O zaman her birleşme sesi yarım ton yükselir (en fazla 8 adım), zincir bitince sıfırlanır; yatırma zincirinde merdiven "su altı" filtresiyle (low-pass 1,2 kHz) çalar.
- [ ] **Senaryo: Haptik** · Verilen cihaz haptik destekli · Eğer kademe ≥ 5 birleşirse · O zaman Medium haptik, Balina oluşumunda ve 8+8'de Heavy; Ayarlar'da kapatılabilir.
- [ ] **Senaryo: Ekran sarsıntısı** · Verilen kademe ≥ 7 birleşme · Eğer oynarsa · O zaman kamera 3 px / 120 ms sarsılır; Reduce Motion açıkken sarsıntı yok.
- [ ] **Senaryo: Taşma gerilimi** · Verilen en yüksek balık çizginin 60 px altında · Eğer 0,5 sn durursa · O zaman çizgi kırmızıya döner, düşük tonlu "uyarı" vızıltısı döngüye girer, su yüzeyi hafif kızarır; balık inince hepsi 300 ms'de söner.
- [ ] **Senaryo: Yatırma ipucu** · Verilen taşma gerilimi aktif ve yatırma hakkı ≥ 1 · Eğer 2 sn geçerse · O zaman yön düğmeleri 1 kez nabız atar (ipucu, girdi kilitlemez); Reduce Motion'da yalnızca kenarlık.
- [ ] **Senaryo: Sessiz cihaz** · Verilen telefon sessizde · Eğer oynanırsa · O zaman efektler görsel ve haptik olarak eksiksiz kalır.

### Teknik görevler (Unity/C#)
- [ ] `MergeFx`: `ParticleSystem` prefab (halka + kabarcık), ölçek tween (kendi `Tween` yardımcı sınıfı; DOTween bağımlılığı yok), havuzlu.
- [ ] `AudioDirector`: birleşme klibi + `pitch = 2^(step/12)`, zincir adımı sayacı; `AudioMixer` snapshot "underwater" (low-pass) yatırma zincirinde; ses varlıkları: 1 birleşme, 1 büyük birleşme, 1 bırakma, 1 taşma uyarısı, 1 oyun sonu, 1 su çalkantısı (8. story), 1 rekor; müzik: 1 ambient akvaryum döngüsü.
- [ ] `CameraShake` (basit offset), `Reduce Motion` bayrağına bağlı.
- [ ] `OverflowTension`: `OverflowDetector` yakınlık olayı → çizgi rengi, su shader `_Tint`, vızıltı döngüsü, yatırma ipucu nabzı.
- [ ] `Haptics` (Core) çağrıları; efekt yoğunluğu ilkesi: yoğunluk artsın, süre uzamasın (zincirleme efektler sırayla ≤ 200 ms).

### Analitik
- `tilt_hint_shown` {run_id: string, charges_left: int} — taşma geriliminde ipucu nabzı gösterildiğinde (sonraki 5 sn içinde `tilt_use` gelirse ipucu işe yaramış sayılır; SQL'de birleştirilir)

### Notlar
- Ses kimliği: yumuşak, "su altı" ailesi; Daily Drop'un "su damlası" ailesiyle akraba ama daha boğuk.
- Tüm ölçüler Play modunda ayarlanır.

## STORY: Eğik Kavanoz 10: Tur yapısı: sonsuz yüksek skor, oyun sonu ve yeniden başlat
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **10. story'si** (epic #EPIC). Tema: Tur yapısı.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `RunManager`, `RunState` kalıcılığı, Home kartı, Play duraklat menüsü

### Kullanıcı hikâyesi
Bir oyuncu olarak istediğim zaman yeni bir tur başlatmak, yarım kalan turu kaldığım yerden sürdürmek ve rekorumu Home'da görmek isterim, böylece "bir tur daha" döngüsü sürtünmesiz olsun.

### Kabul kriterleri
- [ ] **Senaryo: Home** · Verilen Home · Eğer açılırsa · O zaman kart "Rekor · 4.820" ve "Bugünün en iyisi · 1.960", birincil "Oyna" (yarım tur varsa "Devam et · 23. bırakma"), ikincil "Akvaryum", "Ayarlar".
- [ ] **Senaryo: Yeni tur** · Verilen Oyna · Eğer basılırsa · O zaman `run_id` üretilir, kavanoz 0°, hak 2/3, skor 0, sıra yeni seed'le; Play 1 sn içinde açılır.
- [ ] **Senaryo: Duraklat** · Verilen Play · Eğer duraklat düğmesine (veya Android geri) basılırsa · O zaman fizik durur; menü: Devam, Yeniden başlat (onaylı), Ana menü (tur korunur), Ayarlar.
- [ ] **Senaryo: Kaldığı yer** · Verilen yarım tur, uygulama kapatıldı · Eğer 2 saat sonra açılırsa · O zaman tahta (balık konumları, kademeler, dönüşler), skor, bırakma sayısı, sıra indeksi, kavanoz açısı ve yatırma hakları geri yüklenir.
- [ ] **Senaryo: Oyun sonu** · Verilen taşma ve kurtar kullanılmış/reddedilmiş · Eğer tur biterse · O zaman 0,6 sn sonra Result; `RunHistory`'ye kayıt (tarih, skor, maxTier, tiltsUsed, durationS); rekor ve günlük en iyi güncellenir.
- [ ] **Senaryo: Yeniden başlat** · Verilen duraklat menüsü · Eğer "Yeniden başlat" onaylanırsa · O zaman mevcut tur `cause: "quit"` ile kapanır (RunHistory'ye yazılır, rekor sayılmaz) ve yeni tur başlar.
- [ ] **Senaryo: Günlük en iyi** · Verilen yerel tarih değişti · Eğer Home açılırsa · O zaman "Bugünün en iyisi" sıfırlanır; rekor kalır.

### Teknik görevler (Unity/C#)
- [ ] `RunManager`: tur yaşam döngüsü {Create, Resume, Pause, Finish, Abort}; `run_id`; `RunHistory` (yerel JSON + Cloud Save, son 100 tur).
- [ ] `BoardSnapshot`: balık listesi (tier, x, y, rotation), skor, dropCount, sequenceIndex, jarStep, `TiltCharges`; `Save()` her 5 bırakmada, her yatırma sonrası (`TiltSettled`) ve `OnApplicationPause`.
- [ ] Geri yüklemede fizik: kavanozu hedef açıya anında koy, balıkları kinematic yerleştir, 1 kare sonra dynamic (patlamayı önler).
- [ ] Home kartı UI: rekor, günlük en iyi, devam durumu; `DailyBest` yerel tarihle anahtarlanır.
- [ ] Duraklat menüsü (Core `PauseMenu` iskeleti), `BackButtonRouter` bağlama.
- [ ] Cloud Save çakışması: en yüksek `bestScore` kazanır; snapshot cihazda kalır.

### Analitik
- `run_start` {run_id: string, source: "home"|"result_retry"|"restart", runs_today: int} — her tur başında
- `run_resume` {run_id: string, drop_count: int} — kaldığı yerden devam
- `home_view` {has_unfinished: bool, best: int} — Home her açıldığında

### Notlar
- Kapı metriği "oturum başı tur ≥ 3" bu story'nin `run_start` sayısından; Result'taki "Tekrar oyna" (12. story) birincil olmalı.
- Süre sayacı yok; tur dakikalarca sürebilir, 45 sn altı turlar interstitial tetiklemez (16. story).

## STORY: Eğik Kavanoz 11: Ödüllü video: ek yatırma hakkı ve taşma sonrası kurtar
labels: tema:monetizasyon, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **11. story'si** (epic #EPIC). Tema: Monetizasyon.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `RewardedTiltFlow`, `RescueFlow`, `AdsService` (Core), HUD "Ek yatırma · reklam", taşma anı kurtar paneli

### Kullanıcı hikâyesi
Bir oyuncu olarak hakkım bitince bir reklam izleyip bir yatırma daha almak, taştığımda da bir reklamla üstteki balıkları temizletip devam etmek isterim; geliştirici olarak bu iki yerleşimin dürüst (yalnızca seçimle, ödül yalnızca callback'te) ve ölçülebilir olmasını isterim.

### Kabul kriterleri
- [ ] **Senaryo: Ek yatırma teklifi** · Verilen 0 hak, reklam yüklü, tur içi kullanım < 2 · Eğer HUD çizilirse · O zaman "Ek yatırma · reklam" düğmesi yön düğmelerinin altında görünür; reklam yüklü değilse düğme yok (boş teklif yok), 10 sn sonra yeniden denenir.
- [ ] **Senaryo: Ek yatırma ödülü** · Verilen reklam sonuna kadar izlendi · Eğer `OnRewarded` gelirse · O zaman hak +1 (üst sınır 3'ü aşmaz; 0'dan 1'e), `rewardedThisRun` +1; fizik reklam boyunca durmuş, dönünce kaldığı yerden sürer.
- [ ] **Senaryo: Tur başı sınır** · Verilen `rewardedThisRun = 2` · Eğer hak biterse · O zaman düğme görünmez; "Yeni hak: Balina ya da 500 puan" ipucu.
- [ ] **Senaryo: Kurtar teklifi** · Verilen taşma tespit edildi, `rescue_enabled`, tur içinde kurtar kullanılmamış, reklam yüklü · Eğer tur bitecekse · O zaman fizik durur, panel: "Kurtar · reklam izle (üstteki 3 balık silinir)" ve "Bitir"; 6 sn içinde seçim yoksa "Bitir".
- [ ] **Senaryo: Kurtar ödülü** · Verilen reklam izlendi · Eğer `OnRewarded` gelirse · O zaman dünya y'si en yüksek 3 balık kabarcık efektiyle silinir (puan yok), `OverflowTimer`'lar sıfırlanır, tur sürer; HUD'da "Kurtarıldı" rozeti 1 sn; kurtar düğmesi bu turda bir daha çıkmaz.
- [ ] **Senaryo: Yarıda kapatma** · Verilen reklam yarıda kapatıldı · Eğer geri dönülürse · O zaman ödül yok; ek yatırmada düğme kalır, kurtarmada panel yeniden açılır (6 sn sayaç yeniden başlar).
- [ ] **Senaryo: No Ads** · Verilen No Ads satın alınmış · Eğer teklifler gösterilirse · O zaman ödüllü video düğmeleri aynen çalışır (No Ads yalnızca interstitial'ı kapatır).
- [ ] **Senaryo: Çevrimdışı** · Verilen bağlantı yok · Eğer taşma olursa · O zaman kurtar paneli çıkmaz, doğrudan Result.

### Teknik görevler (Unity/C#)
- [ ] `RewardedTiltFlow`: durum {Hidden, Offered, Loading, Showing, Rewarded, Failed}; `AdsService.ShowRewarded(placement: "tilt_extra")`; `TiltCharges.Grant("rewarded")`.
- [ ] `RescueFlow`: `GameOverFlow` kancası (`RunState.Overflow`), panel UI + 6 sn sayaç, `AdsService.ShowRewarded(placement: "rescue")`, `RescueExecutor` (en yüksek 3 `MergeBody` seç, kabarcık efekti, havuza iade, `OverflowDetector.ResetTimers()`).
- [ ] Core `AdsService`: AppLovin MAX ödüllü yerleşimler, yükleme/yeniden deneme, `OnRewarded` güvenilirlik (ödül yalnızca SDK callback'inde), reklam sırasında `Time.timeScale = 0` ve ses kısma.
- [ ] `RunState`'e `rescueUsed`, `rewardedThisRun` (snapshot'a dahil; uygulama kapanırsa durum korunur).
- [ ] Remote Config: `tilt_rewarded_per_run` (2), `rescue_enabled` (true), `rescue_remove_count` (3), `rescue_decision_s` (6).
- [ ] Test reklam kimlikleri `dev` ortamında; `prod`'da gerçek.

### Analitik
- `tilt_ad_offered` {run_id: string, score: int} · `tilt_ad_start` · `tilt_ad_rewarded` · `tilt_ad_failed` {reason: string}
- `rescue_offered` {run_id: string, score: int, drop_count: int} · `rescue_start` · `rescue_rewarded` · `rescue_declined` {reason: "button"|"timeout"} · `rescue_failed` {reason: string}

### Notlar
- İki ödüllü yerleşim de oyuncunun seçimi; oyun ekranında başka reklam yok. Kurtar, Fluffy Drop'un "her seviye sonu interstitial" hatasının tersi: değer anında, oyuncunun elinde.
- Kurtar 3 balık siler; bu bazen yetmez ve 2 bırakma sonra yine taşar. Kabul: tur başı 1 kurtar, oyuncu bunu bilir ("üstteki 3 balık silinir" metni).

## STORY: Eğik Kavanoz 12: Sonuç ekranı, rekor anı ve günlük en iyi
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **12. story'si** (epic #EPIC). Tema: Tur yapısı.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `Result` sahnesi, `ResultView`, `RecordMoment`, `RunHistory`

### Kullanıcı hikâyesi
Bir oyuncu olarak tur bitince skorumu, en büyük balığımı, kaç yatırma kullandığımı ve rekor kırıp kırmadığımı tek ekranda görmek; yeni türler kazandıysam bunu kutlamak ve hemen yeni tura geçmek isterim.

### Kabul kriterleri
- [ ] **Senaryo: İçerik** · Verilen tamamlanmış tur · Eğer Result açılırsa · O zaman başlık "N. bırakmada taştın" (quit ise "Tur bitti"), skor (büyük), Rekor / Bugünün en iyisi / En büyük balık / Yatırma (kullanılan/kazanılan) / En uzun zincir kutuları, "Tekrar oyna" (birincil), "Paylaş" (ikincil), "Akvaryum" bağlantısı.
- [ ] **Senaryo: Giriş animasyonu** · Verilen Result açılışı · Eğer Reduce Motion kapalıysa · O zaman skor 0'dan sayarak 600 ms'de dolar, kutular 80 ms arayla belirir; açıksa anında.
- [ ] **Senaryo: Yeni rekor** · Verilen önceki rekor 4.820 · Eğer tur skoru 5.100 ise · O zaman "Yeni rekor" kurdelesi, Heavy haptik, rekor sesi, `RecordMoment` kaydı (skor, tarih, en büyük balık, kavanoz açısı anlık görüntüsü PNG 540×960) ve Paylaş birincil olur.
- [ ] **Senaryo: Günlük en iyi** · Verilen bugünün en iyisi 1.960 · Eğer skor 2.400 ise (rekor değil) · O zaman "Bugünün en iyisi" rozeti (kurdele yok, Medium haptik).
- [ ] **Senaryo: Yeni tür** · Verilen koleksiyonda olmayan bir kademeye ulaşıldı · Eğer Result açılırsa · O zaman "Yeni tür: Kaplumbağa" kartı önce gösterilir (13. story), ardından Result.
- [ ] **Senaryo: Tekrar oyna** · Verilen Result · Eğer "Tekrar oyna" basılırsa · O zaman interstitial kotası uygunsa reklam (16. story), ardından Play yeni turla açılır; aksi halde doğrudan Play.
- [ ] **Senaryo: Geri** · Verilen Result · Eğer geri tuşuna basılırsa · O zaman Home'a dönülür.

### Teknik görevler (Unity/C#)
- [ ] `ResultView` (uGUI + TextMeshPro), `ResultViewModel` (RunStats + RunHistory + Aquarium farkı).
- [ ] `RecordMoment`: tur bitiş anında tahtanın ekran görüntüsü (`ScreenCapture` yerine ayrı `Camera` + `RenderTexture`, HUD'suz), `persistentDataPath/records/best.png`; 14. story paylaşım kartı için kaynak.
- [ ] Sayaç ve belirme tween'leri, `Reduce Motion` dalı.
- [ ] Düğme sırası: rekor/günlük en iyide Paylaş birincil, aksi halde Tekrar oyna birincil.
- [ ] TR/EN metinler; sayı biçimi yerel (4.820 / 4,820).

### Analitik
- `result_view` {run_id: string, score: int, drop_count: int, max_tier: int, tilts_used: int, is_record: bool, is_daily_best: bool} — her açılışta
- `record_set` {score: int, prev: int} — yeni rekorda

### Notlar
- Tasarım taslağı: Eğik Kavanoz · Sonuç (Daily Drop · Sonuç ile aynı şablon, kutu seti farklı).
- Rekor anı PNG'si: su yüzeyi ve yatık kavanoz görünür; paylaşım kartının "görsel kancası" bu.

## STORY: Eğik Kavanoz 13: Akvaryum: tür koleksiyonu
labels: tema:meta, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **13. story'si** (epic #EPIC). Tema: Akvaryum meta.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `Aquarium` sahnesi, `SpeciesCollection`, `SpeciesDefinition`, Result "Yeni tür" kartı

### Kullanıcı hikâyesi
Bir oyuncu olarak ulaştığım her balık türünün akvaryumuma eklendiğini ve orada yüzdüğünü görmek isterim, böylece skordan bağımsız bir "biriktirme" nedenim olsun ve Balina'ya ulaşmak için tekrar deneyeyim.

### Kabul kriterleri
- [ ] **Senaryo: Kazanım** · Verilen koleksiyonda Tropik/Kaplumbağa yok · Eğer bir turda 6. kademe oluşursa · O zaman tur sonunda tür kazanılır, Result öncesi "Yeni tür: Kaplumbağa" kartı (balık büyük, kısa açıklama, "Akvaryuma eklendi") ve Medium haptik.
- [ ] **Senaryo: Akvaryum ekranı** · Verilen Akvaryum · Eğer açılırsa · O zaman seçili temanın (v1: Tropik) 8 yuvası; kazanılan türler akvaryumda yüzer (basit sinüs + yön flip), kazanılmayanlar silüet ve "?" ile; üstte "5/8 · Tropik", altta tema sekmeleri (Buz ve Derin Deniz kilitli, 19. story).
- [ ] **Senaryo: Tür kartı** · Verilen kazanılmış tür · Eğer dokunulursa · O zaman kart: ad, kaçıncı turda kazanıldı, kaç kez oluşturuldu, en çok oluşturulduğu tur skoru.
- [ ] **Senaryo: Sayaç** · Verilen her tur · Eğer tür oluşursa · O zaman `SpeciesCollection.count[species]` artar (koleksiyon kazanılmış olsa da sayım sürer).
- [ ] **Senaryo: Tamamlama** · Verilen Tropik 8/8 · Eğer son tür kazanılırsa · O zaman "Tropik akvaryum tamam" kutlaması (konfeti 1,5 sn, Reduce Motion'da rozet) ve Home kartında rozet.
- [ ] **Senaryo: Kalıcılık** · Verilen yeni cihaz · Eğer aynı hesapla açılırsa · O zaman koleksiyon Cloud Save'den gelir; çakışmada birleşim (union) ve sayaçların büyüğü.

### Teknik görevler (Unity/C#)
- [ ] `SpeciesDefinition` SO: speciesId (`tropik_6`), themeId, tier, `FishDefinition` referansı, descriptionKey; 24 tanım (v1'de 8'i görsel olarak hazır, 16'sı 19. story).
- [ ] `SpeciesCollection`: {unlocked: HashSet<string>, count: Dictionary<string,int>, firstRunId}; yerel JSON + Cloud Save `aquarium/v1`; birleşim kuralı.
- [ ] `MergeEvent` dinleyici: tur içinde oluşan en yüksek kademeler `RunStats`'e; tur sonunda `SpeciesCollection.Apply(runStats, themeId)`.
- [ ] `Aquarium` sahnesi: 8 yuva, `AquariumFishView` (yüzme animasyonu, 24 balık için tek shader), silüet materyali, tür kartı UI, tema sekmeleri (kilitli durum).
- [ ] "Yeni tür" kartı prefab'ı (Result akışına eklenir), birden fazla yeni tür varsa sırayla.
- [ ] Edit Mode testi: kazanım, sayım, birleşim.

### Analitik
- `species_unlock` {species: string, theme: string, run_id: string, total_unlocked: int} — her kazanımda
- `aquarium_view` {theme: string, unlocked: int} — ekran açıldığında
- `aquarium_complete` {theme: string} — 8/8

### Notlar
- Tutma kancası: skordan bağımsız ilerleme; "Balina'ya ulaş" hedefi D7 için; soft launch'ta `species_unlock` ile D7 korelasyonuna bakılır.
- Koleksiyon temaya bağlı: Buz temasında oynanan tur Buz türlerini açar (19. story). Tema seçimi Home'da; v1'de yalnızca Tropik.
## STORY: Eğik Kavanoz 14: Paylaşım kartı (rekor anı PNG + metin) ve sistem paylaşımı
labels: tema:paylasim, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **14. story'si** (epic #EPIC). Tema: Sosyal.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `ShareCardRenderer`, `ShareService` (Core), Result Paylaş düğmesi, `RecordMoment`

### Kullanıcı hikâyesi
Bir oyuncu olarak rekor kırdığım anın görüntüsünü (yatık kavanoz, kayan balıklar) skorumla birlikte tek dokunuşla WhatsApp/Instagram/X'e göndermek isterim, böylece arkadaşlarım "bu ne oyunu" desin.

### Kabul kriterleri
- [ ] **Senaryo: Görsel kart** · Verilen Result · Eğer Paylaş'a basılırsa · O zaman 1080×1350 PNG üretilir: üstte `RecordMoment` tahta görüntüsü (HUD'suz, yatık kavanoz ve su), altta skor, en büyük balık ikonu ve adı, kullanılan yatırma sayısı, "Tilt Jar" logosu ve mağaza kısa linki; oyuncu adı yok.
- [ ] **Senaryo: Rekor olmayan tur** · Verilen tur rekor değil · Eğer Paylaş'a basılırsa · O zaman kart o turun bitiş anı görüntüsüyle üretilir (her Result'ta tahta görüntüsü alınır, yalnızca rekor olan `best.png` olarak saklanır).
- [ ] **Senaryo: Metin** · Verilen paylaşım · Eğer sistem paylaşım sayfası açılırsa · O zaman metin: "Tilt Jar · 5.100 puan · Balina'ya ulaştım · 4 yatırma\n<link>" (TR/EN; en büyük balık adı yerel).
- [ ] **Senaryo: Platform** · Verilen Android · Eğer paylaşılırsa · O zaman `ACTION_SEND` ile PNG + metin birlikte gider; iOS'ta `UIActivityViewController`.
- [ ] **Senaryo: Hız** · Verilen orta sınıf cihaz · Eğer Paylaş'a basılırsa · O zaman kart 300 ms içinde hazır (RenderTexture, ana iş parçacığı > 1 kare bloklanmaz).
- [ ] **Senaryo: Varyant** · Verilen Remote Config `share_card_variant` = B · Eğer kart üretilirse · O zaman tahta görüntüsü yerine en büyük balığın büyük illüstrasyonu kullanılır (A/B: "an" mı "tür" mü paylaştırır).
- [ ] **Senaryo: Kaydet** · Verilen paylaşım sayfası · Eğer "Galeriye kaydet" seçilirse · O zaman izin akışı doğru çalışır; reddedilirse yalnızca metin paylaşılır.

### Teknik görevler (Unity/C#)
- [ ] `ShareCardRenderer`: ayrı `Camera` + `Canvas` (ekran dışı), `RenderTexture` → `Texture2D.EncodeToPNG`, `persistentDataPath/share/last.png`; `RecordMoment` görüntüsünü `RawImage` ile kompozit.
- [ ] `ShareService` (Core): Android `FileProvider` + `Intent`, iOS native plugin (NativeShare paketi).
- [ ] Kart şablonu prefab: 2 varyant, TR/EN font (Nunito), logo; su/akvaryum zemin.
- [ ] Kısa link: düz mağaza linki + UTM (`utm_source=share&utm_content=record`).
- [ ] Hata yolu: paylaşım iptalinde `share_cancel`; `RecordMoment` görüntüsü yoksa (ilk tur, kapanma) B varyantı yedek.

### Analitik
- `share_tap` {run_id: string, score: int, is_record: bool, variant: string} — düğmeye dokunma
- `share_done` {target: string|null} — sayfa kapandığında

### Notlar
- Kapı metriği: paylaşım oranı = share_tap / result_view ≥ %3 (Daily Drop'un %5'inden düşük; ritüel kancası yok, "an" kancası var).
- Klip (8 sn GIF) paylaşımı SONRA (23. story); PNG kartı öncelikli çünkü ucuz ve her platformda çalışır.

## STORY: Eğik Kavanoz 15: İlk tur deneyimi (FTUE) ve yatırma öğretimi
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **15. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `TutorialDirector`, `TutorialFlags`, Play ipuçları, Home ⓘ kartı

### Kullanıcı hikâyesi
Bir yeni oyuncu olarak kuralları ve özellikle yatırmayı 45 saniyede, oynarken öğrenmek isterim, böylece ilk turda yatırma düğmesini "fark etmeden" bırakmayayım.

### Kabul kriterleri
- [ ] **Senaryo: Dört ipucu** · Verilen ilk açılış · Eğer Play'e girilirse · O zaman 1) "Sürükle ve bırak" eli (ilk bırakmaya kadar), 2) ilk birleşmede "Aynı balıklar birleşir" balonu, 3) 6. bırakmada ya da ilk kez çizgiye yaklaşınca "Kavanozu yatır" balonu yön düğmelerini işaret eder ve düğmeler nabız atar, 4) ilk yatırmadan sonra "Her Balina ve her 500 puan +1 hak" balonu; hiçbiri girdiyi kilitlemez.
- [ ] **Senaryo: İlk tur ön yükü** · Verilen ilk tur · Eğer Play açılırsa · O zaman kavanozda önceden 6 küçük balık (kademe 1–2, bir tarafta yığılı) hazır gelir, böylece ilk yatırmanın etkisi görünür olur; bu ön yük yalnızca ilk turda ve skor saymaz (balıklar işaretli, birleşince puan 0).
- [ ] **Senaryo: Yatırmasız 15 bırakma** · Verilen ilk tur · Eğer 15 bırakmada yatırma kullanılmadıysa · O zaman 3. ipucu bir kez daha gösterilir (toplam en fazla 2).
- [ ] **Senaryo: Tekrar yok** · Verilen ipuçları görüldü · Eğer ikinci tur oynanırsa · O zaman ipucu çıkmaz; Ayarlar'dan "İpuçlarını sıfırla" ile geri gelir.
- [ ] **Senaryo: Nasıl oynanır** · Verilen Home'daki ⓘ · Eğer basılırsa · O zaman 4 slaytlık kart açılır (sürükle, birleştir, yatır, akvaryum); slaytlar kısa döngü animasyonlu (Reduce Motion'da statik).
- [ ] **Senaryo: Ritüel değil, koleksiyon** · Verilen ilk Result · Eğer açılırsa · O zaman "Her tür akvaryumuna eklenir. Balina'ya ulaşabilir misin?" kartı bir kez gösterilir.

### Teknik görevler (Unity/C#)
- [ ] `TutorialFlags` (Cloud Save), `TutorialDirector` (olay dinleyici: FirstDrop, FirstMerge, DropCount == 6 || NearOverflow, FirstTilt, DropCount == 15 && TiltsUsed == 0).
- [ ] El animasyonu ve balon prefab'ı (Core `HintBubble`), yön düğmesi nabız efekti (9. story ile ortak).
- [ ] `FirstRunPreload`: 6 balığın konum listesi (SO), `scoreless` bayrağı `MergeBody`'de (paket API'si: `MergeBody.ScoreMultiplier = 0`).
- [ ] "Nasıl oynanır" kartı: 4 slayt, TR/EN, döngü animasyonları (sprite sheet).
- [ ] FTUE olayları ve `tilt_use` ilişkisi için `docs/analytics.md`'de huni tanımı.

### Analitik
- `ftue_step` {step: string, drop_count: int} — her ipucu gösteriminde
- `ftue_first_tilt` {drop_count: int, hint_shown_count: int} — ilk turdaki ilk yatırmada

### Notlar
- İlke: zorunlu, girdiyi kilitleyen eğitim yok (MustyBlockBlast FTUE kararıyla aynı).
- Ön yük kararı: yatırma "görmeden anlaşılmaz"; boş kavanozda ilk yatırma anlamsız. Ön yüklü balıklar skor saymaz ki ilk skor şişmesin.

## STORY: Eğik Kavanoz 16: Interstitial kotası ve No Ads
labels: tema:monetizasyon, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **16. story'si** (epic #EPIC). Tema: Monetizasyon.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `InterstitialPolicy`, `AdsService` (Core), `IapService` (Core), `NoAdsGate`, Result "Tekrar oyna" akışı

### Kullanıcı hikâyesi
Bir oyuncu olarak tur aralarında ara sıra, asla oyun ekranında olmayan ve günde en fazla 3 kez çıkan bir reklam görmeyi kabul ederim; istemiyorsam tek seferlik 3,99 USD ile kapatmak isterim ve bunun gerçekten çalışmasını isterim.

### Kabul kriterleri
- [ ] **Senaryo: Kota** · Verilen bugün 0 interstitial · Eğer Result'ta "Tekrar oyna" basılırsa (3. tur ve sonrası) · O zaman interstitial gösterilir; günde en fazla `interstitial_daily_cap` (3); kota yerel tarihle sıfırlanır.
- [ ] **Senaryo: Koruma kuralları** · Verilen tur süresi < `interstitial_min_run_s` (45) ya da oturumun ilk 2 turu ya da son interstitial'dan bu yana < 180 sn ya da bu turda ödüllü video izlendi · Eğer Tekrar oyna basılırsa · O zaman interstitial çıkmaz.
- [ ] **Senaryo: Yüklü değil** · Verilen interstitial yüklenmemiş · Eğer sıra geldiyse · O zaman beklenmez, doğrudan Play; kota tüketilmez.
- [ ] **Senaryo: No Ads satın alma** · Verilen Ayarlar ya da Result'taki "Reklamları kaldır · 3,99 USD" · Eğer satın alınırsa · O zaman `NoAdsGate.IsActive` true, interstitial bir daha hiç çıkmaz, ödüllü video düğmeleri kalır; fiyat yerel biçimde gösterilir.
- [ ] **Senaryo: Geri yükleme** · Verilen yeni cihaz · Eğer "Satın alımları geri yükle" basılırsa · O zaman No Ads tanınır (Cloud Save'de de işaretli; çevrimdışı açılışta Cloud Save bayrağı yeterli).
- [ ] **Senaryo: Reklam sonrası** · Verilen interstitial kapandı · Eğer Play açılırsa · O zaman ses ve `Time.timeScale` geri gelir, yeni tur 0°'den başlar.
- [ ] **Senaryo: Oyun ekranı temiz** · Verilen Play · Eğer herhangi bir an incelenirse · O zaman banner yok, overlay reklam yok.

### Teknik görevler (Unity/C#)
- [ ] `InterstitialPolicy`: `ShouldShow(runStats, session)` saf fonksiyon; durum {shownToday, lastShownAt, runsThisSession}; yerel JSON.
- [ ] Core `AdsService.ShowInterstitial(placement: "run_end")`, yükleme ve `OnClosed` callback; MAX yerleşimi.
- [ ] `NoAdsGate` (Core `IapService` üstünde): tüketilemeyen ürün `no_ads`; Unity IAP, Google Play Billing; makbuz yerel doğrulama (sunucu tarafı doğrulama SONRA); Cloud Save `iap/no_ads`.
- [ ] Ayarlar ve Result'ta No Ads düğmesi, fiyat `metadata.localizedPrice`.
- [ ] Remote Config: `interstitial_daily_cap`, `interstitial_min_run_s`, `interstitial_min_gap_s`, `interstitial_skip_runs`.
- [ ] Edit Mode testi: `InterstitialPolicy` kuralları (kota, süre, ilk turlar, ödüllü sonrası).

### Analitik
- `interstitial_shown` {run_id: string, shown_today: int} · `interstitial_skipped` {reason: "cap"|"short_run"|"first_runs"|"gap"|"rewarded"|"not_loaded"|"no_ads"}
- `noads_view` {source: string} · `noads_purchase` {price_local: string} · `noads_restore` {success: bool}

### Notlar
- Fluffy Drop'un üç hatası (her seviye sonu interstitial, çalışmayan No Ads, işlevsiz coin) burada tersine çevrilir: kota + koruma kuralları, No Ads Cloud Save bayrağıyla çevrimdışı da çalışır, coin yok.
- Karar: interstitial yalnızca "Tekrar oyna" yolunda; Home'a dönüşte çıkmaz (çıkış anını cezalandırma).

## STORY: Eğik Kavanoz 17: Analitik olay şeması ve kapı raporu
labels: tema:analitik, P1, efor:S
milestone: MVP

**Eğik Kavanoz** epic'inin **17. story'si** (epic #EPIC). Tema: Analitik.

**Öncelik:** P1 · **Efor:** S · **Milestone:** MVP · **Dokunduğu yer:** UGS Analytics olay tanımları, `docs/analytics.md`, SQL raporları

### Kullanıcı hikâyesi
Bir geliştirici olarak soft launch kapısını (D1, D7, oturum başı tur, paylaşım oranı, yatırma sinyali) tek sorguyla görmek isterim, böylece karar veriyle verilir.

### Kabul kriterleri
- [ ] **Senaryo: Şema** · Verilen `docs/analytics.md` · Eğer okunursa · O zaman tüm olaylar (app_open, session_start, home_view, run_start, run_resume, drop, merge, tilt_use, tilt_charge_earned, tilt_charge_wasted, tilt_locked_input, tilt_hint_shown, tilt_physics_anomaly, run_end, result_view, record_set, species_unlock, aquarium_view, aquarium_complete, share_tap, share_done, tilt_ad_*, rescue_*, interstitial_*, noads_*, ftue_step, ftue_first_tilt, settings_change) parametre tipleriyle listelidir ve UGS'de tanımlıdır.
- [ ] **Senaryo: Doğrulama** · Verilen dev build · Eğer 3 tur oynanırsa (biri yatırmalı, biri kurtarmalı) · O zaman UGS Event Browser'da olaylar sırayla ve doğru parametrelerle görünür.
- [ ] **Senaryo: Kapı sorgusu** · Verilen UGS SQL · Eğer `gate_report.sql` çalıştırılırsa · O zaman D1, D7, oturum başı tur, paylaşım oranı, ortalama oturum, çökme oranı tek tabloda gelir.
- [ ] **Senaryo: Yatırma raporu** · Verilen UGS SQL · Eğer `tilt_report.sql` çalıştırılırsa · O zaman yatırmalı tur oranı, tur başı ortalama yatırma, yatırma başına ortalama puan (yatırma zincirinden gelen `by_tilt` birleşme puanı), kaynak dağılımı (balina/puan/reklam), yanan hak oranı gelir.
- [ ] **Senaryo: Gizlilik** · Verilen olaylar · Eğer incelenirse · O zaman kişisel veri (ad, e-posta, kesin konum) yoktur.

### Teknik görevler (Unity/C#)
- [ ] `Analytics` (Core) sarmalayıcısı: `Track(string, Dictionary<string, object>)`, kuyruk + çevrimdışı tampon.
- [ ] UGS Analytics custom event tanımları (dashboard'dan), parametre tipleri.
- [ ] `docs/analytics.md` ve `docs/reports/gate_report.sql`, `tilt_report.sql`, `funnel_first_run.sql` (home_view → run_start → ftue_first_tilt → run_end → result_view → share_tap), `monetization.sql` (ödüllü teklif → izleme oranı; interstitial gösterim/atlama nedenleri).
- [ ] Crashlytics (Firebase) veya UGS Crash raporlama: çökme oranı için.

### Analitik
- Bu story şemanın kendisidir.

### Notlar
- MustyBlockBlast'taki `docs/UGS SQL` yaklaşımı aynen uygulanır; Daily Drop ile ortak olaylar (`app_open`, `share_*`, `settings_change`) aynı isim ve parametrelerle (portföy genelinde karşılaştırma).

## STORY: Eğik Kavanoz 18: Mağaza sayfası, ASO ve soft launch yapılandırması
labels: tema:magaza, P1, efor:M
milestone: MVP

**Eğik Kavanoz** epic'inin **18. story'si** (epic #EPIC). Tema: Mağaza ve çıkış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** Play Console, mağaza metinleri, ekran görüntüleri, kapalı test

### Kullanıcı hikâyesi
Bir geliştirici olarak oyunu üç ülkede kapalı test ve ardından soft launch'a çıkarmak isterim, böylece gerçek oyuncu verisi gelsin.

### Kabul kriterleri
- [ ] **Senaryo: Başlık ve metin** · Verilen Play listesi · Eğer yayımlanırsa · O zaman EN başlık "Tilt Jar: Aquarium Merge" (24 karakter), TR "Eğik Kavanoz: Akvaryum" (22; "Akvaryum Birleştirme" 30'u aştığı için kısa açıklamaya taşınır); kısa açıklama ≤ 80 karakter, uzun tire yok; tam açıklamada "watermelon game", "physics merge", "aquarium", "fish merge", "no timer" geçer.
- [ ] **Senaryo: Görseller** · Verilen ekran görüntüleri · Eğer yüklenirse · O zaman 8 görüntü (ilk ikisi yatık kavanoz ve kayan balıklar, sonra birleşme zinciri, Balina, akvaryum koleksiyonu, sonuç, paylaşım kartı, temalar), 1 özellik grafiği, 15 sn video (yatırma anı; isteğe bağlı ama önerilir).
- [ ] **Senaryo: Kapalı test** · Verilen 12+ test kullanıcısı · Eğer 14 gün sürerse · O zaman Play üretim erişimi formu doldurulur (MustyBlockBlast deneyimi).
- [ ] **Senaryo: Ülkeler** · Verilen soft launch · Eğer yayımlanırsa · O zaman yalnızca TR, PH, CA; IAP fiyatları (No Ads 3,99 USD, tema paketleri 2,99 USD) yerel kademede.
- [ ] **Senaryo: Sürüm bilgisi** · Verilen build · Eğer yayımlanırsa · O zaman versionCode ve versionName CHANGELOG ile eşleşir; gizlilik politikası ve veri güvenliği formu dolu (reklam SDK'sı, analitik, Cloud Save, IAP).

### Teknik görevler (Unity/C#)
- [ ] Mağaza metinleri `docs/store/{en,tr}.md`; ASO anahtar kelime listesi; rakip başlıkları (Suika, Watermelon Game klonları, Fish merge).
- [ ] Ekran görüntüsü şablonu (Design kanvası), 1080×1920; yatık kavanoz ilk görsel.
- [ ] Veri güvenliği formu; `docs/privacy.md`.
- [ ] Play Console kurulumu: iç test → kapalı test → üretim (3 ülke); IAP ürünleri `no_ads`, `theme_ice`, `theme_deep`.
- [ ] Sürümleme: `CHANGELOG.md`, `versionCode` CI'da otomatik.

### Analitik
- Mağaza: Play Console dönüşüm oranı; UTM'li paylaşım linkleri.

### Notlar
- Başlık testi: soft launch sırasında "Aquarium Merge" ile "Watermelon Game" A/B (Play listing experiments); klon algısı riskine karşı görsellerde meyve yok.
## STORY: Eğik Kavanoz 19: Akvaryum temaları (Buz, Derin Deniz) ve tema paketleri
labels: tema:meta, P2, efor:L
milestone: Soft launch

**Eğik Kavanoz** epic'inin **19. story'si** (epic #EPIC). Tema: Akvaryum meta.

**Öncelik:** P2 · **Efor:** L · **Milestone:** Soft launch · **Dokunduğu yer:** `ThemeDefinition`, `ThemeGate`, `IapService` (Core), Home tema seçici, Akvaryum tema sekmeleri, 16 yeni `FishDefinition`

### Kullanıcı hikâyesi
Bir oyuncu olarak kavanozumu Buz ya da Derin Deniz temasında oynamak, o temanın 8 türünü toplamak isterim, böylece koleksiyon 24'e çıksın; geliştirici olarak bu temaların küçük, dürüst IAP paketleri olmasını isterim.

### Kabul kriterleri
- [ ] **Senaryo: Tema tanımı** · Verilen 3 `ThemeDefinition` · Eğer oyun yüklenirse · O zaman Tropik (ücretsiz), Buz (2,99 USD), Derin Deniz (2,99 USD); her temada 8 `FishDefinition` (aynı elips ölçüleri, farklı sprite/renk/ad: Buz 1–8 örn. Buz Boncuğu … Narval; Derin Deniz 1–8 örn. Fener Balığı … Dev Mürekkep), kavanoz camı/su rengi/ambient ses teması.
- [ ] **Senaryo: Seçim** · Verilen Buz satın alınmış · Eğer Home'da Buz seçilirse · O zaman sonraki tur Buz balıkları ve Buz su rengiyle açılır; yarım kalmış Tropik tur varsa önce "Tur devam ediyor" uyarısı (tema tur ortasında değişmez).
- [ ] **Senaryo: Koleksiyon** · Verilen Buz temasında tur · Eğer 5. kademe oluşursa · O zaman Buz/5 türü kazanılır; Tropik koleksiyonu etkilenmez; Akvaryum'da Buz sekmesi "1/8".
- [ ] **Senaryo: Önizleme** · Verilen tema kilitli · Eğer Akvaryum'da Buz sekmesine dokunulursa · O zaman 8 silüet ve tema önizlemesi (kavanoz + 3 balık animasyonlu), "Aç · 2,99 USD" düğmesi, "Satın alımları geri yükle".
- [ ] **Senaryo: Satın alma** · Verilen Google Play Billing · Eğer Buz alınırsa · O zaman anında seçilebilir, Cloud Save'de kalıcı, yeni cihazda geri yüklenir.
- [ ] **Senaryo: Adalet** · Verilen herhangi tema · Eğer oynanırsa · O zaman fizik, ölçüler, puan ve yatırma kuralları aynıdır (yalnızca görsel/ses ve koleksiyon).
- [ ] **Senaryo: Paylaşım kartı** · Verilen Buz temasında rekor · Eğer kart üretilirse · O zaman kart Buz zemini ve Buz balık adlarını kullanır.

### Teknik görevler (Unity/C#)
- [ ] `ThemeDefinition` SO: themeId, nameKey, 8 `FishDefinition`, jarSprite, waterTint, ambientClip, iapSku (null = ücretsiz); `ThemeGate.IsUnlocked(themeId)`.
- [ ] 16 balık sprite seti (gövde, göz, kuyruk, sembol) + 2 kavanoz camı + 2 su rengi + 2 ambient döngü; `Addressables` ile tema paketleri (APK boyutu < 70 MB korunur).
- [ ] Unity IAP tüketilemeyen ürünler `theme_ice`, `theme_deep`; `IapService` geri yükleme; Cloud Save `iap/themes`.
- [ ] Home tema seçici (3 kart), Akvaryum tema sekmeleri ve kilitli önizleme; `RunState.themeId`.
- [ ] `SpeciesCollection` tema anahtarı zaten `speciesId`'de (13. story); `SpeciesDefinition` 24'e tamamlanır.
- [ ] Fiyat testi: Remote Config `theme_price_tier` ile iki fiyat noktası (Play'de iki ürün: 2,99 / 1,99).

### Analitik
- `theme_select` {theme: string} · `theme_view` {theme: string, unlocked: bool} · `theme_purchase` {sku: string, price_local: string} · `theme_restore` {success: bool}

### Notlar
- Karar: tema = koleksiyon dilimi; tema satın almayan oyuncu 8/24'te kalır ve bunu Akvaryum'da görür (dürüst kilit: kazanılabilir değil, satın alınır; açıkça yazılır).
- Üçüncü paket (örn. "Gece Neon") Canlı'da LiveOps içeriği; şablon bu story'den çıkar.

## STORY: Eğik Kavanoz 20: Haftalık liderlik tablosu
labels: tema:backend, P2, efor:M
milestone: Soft launch

**Eğik Kavanoz** epic'inin **20. story'si** (epic #EPIC). Tema: Sosyal.

**Öncelik:** P2 · **Efor:** M · **Milestone:** Soft launch · **Dokunduğu yer:** UGS Leaderboards, `LeaderboardService`, Home "Bu hafta" sekmesi, Result sıralama satırı

### Kullanıcı hikâyesi
Bir oyuncu olarak bu haftaki en yüksek skorumun dünyada kaçıncı olduğunu görmek isterim, böylece hafta boyunca rekorumu geçmek için bir sebebim olsun.

### Kabul kriterleri
- [ ] **Senaryo: Gönderim** · Verilen tamamlanmış tur (`cause: overflow`) · Eğer skor haftanın en iyisinden yüksekse · O zaman `weekly_{isoWeek}` tablosuna gönderilir (yalnızca artış).
- [ ] **Senaryo: Sıralama** · Verilen tablo yanıtı · Eğer gelirse · O zaman Result'ta "Bu hafta dünyada #1.284 · ilk %8"; yanıt 2 sn'de gelmezse "—" ve arka planda güncellenir.
- [ ] **Senaryo: Tablo ekranı** · Verilen Home'daki "Bu hafta" sekmesi · Eğer açılırsa · O zaman ilk 50 + oyuncunun kendi sırası; isimler takma ad (21. story) ya da rastgele deniz canlısı adı; haftanın bitimine geri sayım.
- [ ] **Senaryo: Hafta değişimi** · Verilen Pazartesi 00:00 UTC · Eğer geçilirse · O zaman yeni tablo; önceki hafta "Geçen hafta" olarak 7 gün görünür.
- [ ] **Senaryo: Çevrimdışı** · Verilen bağlantı yok · Eğer Result açılırsa · O zaman gönderim kuyruklanır, bağlantı gelince gider.
- [ ] **Senaryo: Hile filtresi** · Verilen skor > 60.000 ya da `duration_s` < skor/200 · Eğer gönderilirse · O zaman Cloud Code reddeder (tam doğrulama SONRA 24).

### Teknik görevler (Unity/C#)
- [ ] UGS Leaderboards: haftalık sıfırlanan tablo (reset schedule), tutma süresi 2 hafta.
- [ ] `LeaderboardService.Submit(score, runStats)`, `GetPlayerRange`, kuyruk; Cloud Code `submitWeeklyScore` (üst sınır, süre oranı, oyuncu kimliği).
- [ ] Result sıralama satırı ve Home "Bu hafta" UI; geri sayım.
- [ ] Takma ad Player Accounts profilinde (21. story ile ortak).

### Analitik
- `leaderboard_submit` {score: int, rank: int, percentile: float} · `leaderboard_view` {tab: "week"|"last_week"}

### Notlar
- Günlük değil haftalık: Daily Drop'un ritüeli burada yok; haftalık tablo "rekoru geç" döngüsüne uyar.
- UGS Leaderboards ücretsiz katman soft launch hacminde yeterli; kontrol edilecek.

## STORY: Eğik Kavanoz 21: Ayarlar ve erişilebilirlik
labels: tema:erisilebilirlik, P2, efor:S
milestone: Soft launch

**Eğik Kavanoz** epic'inin **21. story'si** (epic #EPIC). Tema: Erişilebilirlik ve ayarlar.

**Öncelik:** P2 · **Efor:** S · **Milestone:** Soft launch · **Dokunduğu yer:** `SettingsScreen` (Core), oyuna özgü ayar bayrakları, `TiltPresenter` Reduce Motion dalı

### Kullanıcı hikâyesi
Bir oyuncu olarak ses, haptik, hareket azaltma, dil, takma ad ve reklam ayarlarını tek yerden yönetmek isterim; renk körü bir oyuncu olarak türleri sembolle ayırt etmek, hareket hassasiyeti olan bir oyuncu olarak yatırmayı dalgasız ve anlık görmek isterim.

### Kabul kriterleri
- [ ] **Senaryo: Ayarlar listesi** · Verilen Ayarlar · Eğer açılırsa · O zaman Ses, Müzik, Haptik, Reduce Motion, Tür sembolleri, Sol/sağ düğme yerleşimi (yön düğmeleri alt köşelerde ya da ortada), Dil, Takma ad, Reklamları kaldır, İpuçlarını sıfırla, Uygulamayı puanla, Gizlilik, Sürüm.
- [ ] **Senaryo: Reduce Motion** · Verilen açık · Eğer oynanırsa · O zaman yatırma 0,2 sn lineer ve dalgasız (8. story), sarsıntı/konfeti/köpük kapalı, su yüzeyi düz çizgi, akvaryum balıkları yavaş (0,5×); fonksiyon kaybı yok.
- [ ] **Senaryo: Tür sembolleri** · Verilen açık · Eğer balıklar çizilirse · O zaman kademe sembolleri gövdede görünür (3. story); önizleme ve akvaryumda da.
- [ ] **Senaryo: Düğme yerleşimi** · Verilen "Köşeler" seçili · Eğer Play çizilirse · O zaman "Sola" sol alt, "Sağa" sağ alt köşede (tek el, 6,7" cihaz); "Orta" seçiliyse ikisi yan yana altta (tasarım taslağı).
- [ ] **Senaryo: Büyük yazı** · Verilen sistem yazı boyutu büyük · Eğer HUD çizilirse · O zaman metinler kırpılmaz (TextMeshPro auto-size sınırları); yatırma sayacı ve zincir rozeti okunur kalır.
- [ ] **Senaryo: Android geri** · Verilen Ayarlar açık · Eğer geri basılırsa · O zaman önceki ekrana dönülür.
- [ ] **Senaryo: Değerlendirme isteği** · Verilen 5. tamamlanan tur ve yeni rekor ya da ilk 8/8 akvaryum · Eğer Result kapanırsa · O zaman mağaza içi değerlendirme isteği bir kez gösterilir.

### Teknik görevler (Unity/C#)
- [ ] Core `SettingsScreen` bağlama; oyuna özgü anahtarlar: `species_symbols`, `tilt_button_layout`.
- [ ] `TiltButtons` iki yerleşim prefab varyantı; `SafeArea` ile köşe yerleşimi.
- [ ] TextMeshPro auto-size ve TR karakter atlası.
- [ ] In-app review (Google Play In-App Review, iOS SKStoreReviewController) tetik kuralı.
- [ ] Takma ad doğrulama (uygunsuz kelime listesi, 3–12 karakter).

### Analitik
- `settings_change` {key: string, value: string} · `review_prompt` {shown: bool}

### Notlar
- Kontrast: HUD metinleri 4,5:1; su üstündeki taşma çizgisi ve etiket koyu zemin paletinde doğrulandı.
- Reduce Motion tüm efektleri kapsar (MustyBlockBlast ilkesi).

## STORY: Eğik Kavanoz 22: Rekor duvarı ve günlük hedefler
labels: tema:meta, P3, efor:M
milestone: Canlı

**Eğik Kavanoz** epic'inin **22. story'si** (epic #EPIC). Tema: Akvaryum meta.

**Öncelik:** P3 · **Efor:** M · **Milestone:** Canlı · **Dokunduğu yer:** `RecordWall`, `DailyGoals`, Akvaryum alt sekmesi, Home günlük hedef kartı

### Kullanıcı hikâyesi
Bir oyuncu olarak akvaryumumun yanında rekorlarımı (en yüksek skor, en uzun zincir, en çok Balina, en çok yatırma kazancı) ve her gün küçük bir hedef görmek isterim, böylece "rekor kır" dışında da bir günlük nedenim olsun.

### Kabul kriterleri
- [ ] **Senaryo: Rekor duvarı** · Verilen 10+ tur · Eğer Akvaryum'daki "Rekorlar" sekmesi açılırsa · O zaman 6 plaket: En yüksek skor, En uzun zincir, Bir turda en çok Balina, Bir turda en çok kazanılan yatırma, En uzun tur (bırakma), Toplam tür (x/24); her plaket tarih ve `RecordMoment` küçük görüntüsüyle.
- [ ] **Senaryo: Günlük hedef** · Verilen yeni gün · Eğer Home açılırsa · O zaman 1 hedef kartı (havuzdan: "Bir turda 3 yatırma zinciri", "Kaplumbağa'ya ulaş", "1.500 puan geç", "Hiç yatırmadan 800 puan") ve ilerleme; tamamlanınca rozet ve Medium haptik; ödül yok (coin yok), yalnızca seri sayacı "3 gün üst üste".
- [ ] **Senaryo: Remote hedef havuzu** · Verilen Remote Config `daily_goal_pool` · Eğer değişirse · O zaman sonraki gün yeni havuzdan seçilir; seçim tarihe göre deterministik.
- [ ] **Senaryo: Kalıcılık** · Verilen yeni cihaz · Eğer aynı hesapla açılırsa · O zaman rekorlar ve hedef serisi Cloud Save'den gelir.

### Teknik görevler (Unity/C#)
- [ ] `RecordWall`: `RunHistory` üstünden türetilen 6 metrik + `RecordMoment` küçük görüntüleri (240×426).
- [ ] `DailyGoals`: `GoalDefinition` SO (tür, hedef değer, metin anahtarı), tarih tabanlı seçim, `RunStats` dinleyici, seri sayacı; Cloud Save.
- [ ] Akvaryum alt sekmesi ve Home kartı UI; Remote Config `daily_goal_pool`.
- [ ] Edit Mode testi: tarih tabanlı deterministik hedef seçimi ve seri kırılma mantığı.

### Analitik
- `goal_assigned` {goal: string} · `goal_complete` {goal: string, streak: int} · `record_wall_view`

### Notlar
- LiveOps'un ucuz biçimi: içerik yok, hedef var. Günlük seed modu (SONRA 25) gelirse hedefler o moda bağlanabilir.
## STORY: Eğik Kavanoz 23: Klip paylaşımı (son 8 sn GIF)
labels: tema:paylasim, P3, efor:L, SONRA
milestone: SONRA

**Eğik Kavanoz** epic'inin **23. story'si** (epic #EPIC). Tema: Sosyal.

**Öncelik:** P3 · **Efor:** L · **Milestone:** SONRA · **Dokunduğu yer:** `ClipRecorder`, `GifEncoder`, Result "Klibi paylaş" düğmesi

### Kullanıcı hikâyesi
Bir oyuncu olarak kavanozu yatırıp zincir patlattığım son 8 saniyeyi GIF olarak paylaşmak isterim, böylece arkadaşlarım "yığın kayması" anını görsün (TikTok/Twitch kitlesi için asıl kanca).

### Kabul kriterleri
- [ ] **Senaryo: Halka tampon** · Verilen Play · Eğer tur sürüyorsa · O zaman son 8 sn'nin kareleri 15 fps, 360×640'ta `RenderTexture` halkasında tutulur (120 kare, < 40 MB VRAM); kare düşüşü yok.
- [ ] **Senaryo: İşaretleme** · Verilen yatırma zinciri ≥ ×2 ya da Balina oluşumu · Eğer olursa · O zaman o an "klip anı" olarak işaretlenir ve tur sonunda en iyi anın etrafındaki 8 sn seçilir (yoksa son 8 sn).
- [ ] **Senaryo: GIF üretimi** · Verilen Result "Klibi paylaş" · Eğer basılırsa · O zaman GIF 3 sn içinde arka plan iş parçacığında üretilir (< 6 MB, 256 renk, skor overlay'i), ilerleme çubuğu; sistem paylaşımı PNG kartla aynı yol.
- [ ] **Senaryo: Reduce Motion / düşük cihaz** · Verilen 3 GB RAM altı cihaz ya da kayıt kapalı · Eğer Play açılırsa · O zaman tampon devre dışı, düğme gizli.
- [ ] **Senaryo: Alternatif** · Verilen Android 10+ · Eğer `MediaProjection` fizibilitesi olumluysa · O zaman MP4 (ses dahil) seçeneği; iOS'ta ReplayKit; karar `docs/clip.md`.

### Teknik görevler (Unity/C#)
- [ ] `ClipRecorder`: `RenderTexture` halkası, `AsyncGPUReadback`, kare zaman damgaları; "klip anı" işaretleri.
- [ ] `GifEncoder`: LZW + medyan kesme paleti (kendi implementasyonu ya da hafif bir kütüphane), `Task` ile arka plan; `persistentDataPath/share/clip.gif`.
- [ ] Fizibilite: Android `MediaProjection` (izin diyaloğu, ses), iOS ReplayKit; pil ve bellek ölçümü.
- [ ] Result düğmesi, ilerleme UI, `ShareService` bağlama.

### Analitik
- `clip_share_tap` {run_id: string, moment: "tilt_chain"|"whale"|"last"} · `clip_share_done` {size_kb: int, target: string|null}

### Notlar
- MVP'de PNG kartla paylaşım oranı ölçülür; klip, paylaşım kapısı (≥ %3) geçilince ya da yayıncı ilgisi görülünce gelir.
- GIF, MediaProjection'ın izin ve ses karmaşasından kaçınır; kalite düşük ama her platformda gider.

## STORY: Eğik Kavanoz 24: Replay kaydı ve skor doğrulama (Daily Drop ile ortak)
labels: tema:backend, P3, efor:L, SONRA
milestone: SONRA

**Eğik Kavanoz** epic'inin **24. story'si** (epic #EPIC). Tema: Analitik ve backend.

**Öncelik:** P3 · **Efor:** L · **Milestone:** SONRA · **Dokunduğu yer:** `InputRecorder` (paket), Cloud Code doğrulama, `docs/determinism.md` (Daily Drop 25 ile ortak)

### Kullanıcı hikâyesi
Bir geliştirici olarak haftalık liderlik tablosundaki skorların oynanmış bir koşudan geldiğini doğrulamak isterim, böylece hile tabloyu bozmasın; bunu Daily Drop ile aynı altyapıyla yapmak isterim.

### Kabul kriterleri
- [ ] **Senaryo: Kayıt** · Verilen tur · Eğer oynanırsa · O zaman her bırakma {frame, x}, her yatırma {frame, direction}, her kurtar {frame} ve tur seed'i kaydedilir ve skorla gönderilir (< 6 KB).
- [ ] **Senaryo: Tutarlılık** · Verilen kayıt · Eğer sunucuda (headless simülasyon) yeniden oynatılırsa · O zaman skor ± %15 içinde (yatırma fiziği Daily Drop'tan daha kaotik); değilse skor işaretlenir.
- [ ] **Senaryo: Ortak paket** · Verilen `MustyMergePhysics` · Eğer `InputRecorder` pakete eklenirse · O zaman Daily Drop aynı sınıfı kullanır; olay türleri genişletilebilir (`IRecordedAction`).
- [ ] **Senaryo: Araştırma** · Verilen kinematic döner kavanoz · Eğer determinizm ölçülürse · O zaman aynı cihazda birebir, farklı cihazlarda sapma raporu `docs/determinism.md`'ye (Daily Drop ile ortak doküman, oyun başına bölüm).

### Teknik görevler (Unity/C#)
- [ ] `InputRecorder` (paket): `IRecordedAction` (Drop, Tilt, Rescue), sıkıştırma, gönderim.
- [ ] Headless doğrulama servisi (Unity batchmode) — Daily Drop 25 ile aynı servis, oyun kimliğine göre sahne.
- [ ] Alternatif: istatistiksel anomali tespiti (skor/bırakma oranı, skor/süre, yatırma başına puan).

### Analitik
- `score_flagged` {run_id: string, reason: string}

### Notlar
- Önce Daily Drop 25'in fizibilitesi; sonuç olumsuzsa her iki oyunda da yalnızca istatistiksel filtre.

## STORY: Eğik Kavanoz 25: Günlük seed modu ve Daily Drop çapraz tanıtımı
labels: tema:ritual, P3, efor:M, SONRA
milestone: SONRA

**Eğik Kavanoz** epic'inin **25. story'si** (epic #EPIC). Tema: Tur yapısı.

**Öncelik:** P3 · **Efor:** M · **Milestone:** SONRA · **Dokunduğu yer:** `DailySeedMode`, Home "Günün kavanozu" kartı, çapraz tanıtım kartı

### Kullanıcı hikâyesi
Bir oyuncu olarak günde bir kez herkesle aynı balık sırasını oynamak ve skorumu kıyaslamak isterim; iki oyunu da seven biri olarak Daily Drop'u burada, burayı Daily Drop'ta keşfetmek isterim.

### Kabul kriterleri
- [ ] **Senaryo: Günlük seed** · Verilen `seed = SHA256("tiltjar:" + yyyy-MM-dd)` · Eğer "Günün kavanozu" oynanırsa · O zaman balık sırası herkeste aynı, günde tek deneme, ayrı günlük tablo; yatırma kuralları aynı.
- [ ] **Senaryo: Çapraz tanıtım** · Verilen Daily Drop yüklü değil · Eğer günlük mod ilk kez tamamlanırsa · O zaman "Her gün aynı sıra sevdiysen: Daily Drop" kartı bir kez (mağaza linki, UTM); Daily Drop'ta simetrik kart ayrı issue.
- [ ] **Senaryo: Yüklü ise** · Verilen Daily Drop yüklü (paket adı sorgusu) · Eğer kart sırası gelirse · O zaman kart çıkmaz.

### Teknik görevler (Unity/C#)
- [ ] `DailySeedMode`: Daily Drop'un `DailySeed`/`DailySequence` sınıfları `MustyMergePhysics` paketine taşınır (ortak); `DailyState` sadeleştirilmiş.
- [ ] Günlük tablo (UGS Leaderboards, `daily_{dayIndex}`); Home kartı.
- [ ] Çapraz tanıtım kartı (Core `CrossPromoCard`), yüklü uygulama sorgusu (Android `PackageManager`, iOS URL scheme).

### Analitik
- `daily_mode_start` {day_index: int} · `cross_promo_view` {target: string} · `cross_promo_tap` {target: string}

### Notlar
- İki oyunun hangi kancayla D7 taşıdığı görüldükten sonra; bu story "iyi kancayı diğerine taşır".

## STORY: Eğik Kavanoz 26: Özel kavanoz şekilleri
labels: tema:meta, P3, efor:M, SONRA
milestone: SONRA

**Eğik Kavanoz** epic'inin **26. story'si** (epic #EPIC). Tema: Akvaryum meta.

**Öncelik:** P3 · **Efor:** M · **Milestone:** SONRA · **Dokunduğu yer:** `JarShapeDefinition`, `Jar` prefab varyantları, Remote Config haftalık şekil

### Kullanıcı hikâyesi
Bir oyuncu olarak haftanın bir günü farklı bir kavanozda (dar boyunlu, yuvarlak, çift bölmeli) oynamak isterim, böylece yatırma stratejisi tazelensin.

### Kabul kriterleri
- [ ] **Senaryo: Şekil seti** · Verilen 3 `JarShapeDefinition` (Dar boyun, Yuvarlak, Çift bölme) · Eğer uygulanırsa · O zaman her biri kendi collider seti, `JarGeometry` ağız noktaları, `SpriteMask` şekli ve dudak collider'larıyla izole ve stres testinden (7. story) geçer.
- [ ] **Senaryo: Takvim** · Verilen Remote Config `jar_shape_schedule` · Eğer Cumartesi ise · O zaman "Yuvarlak kavanoz günü"; Home kartında rozet; haftalık tabloya ayrı etiket.
- [ ] **Senaryo: Adalet** · Verilen özel şekil · Eğer oynanırsa · O zaman koleksiyon ve rekor duvarı etkilenmez (ayrı "şekil rekorları").

### Teknik görevler (Unity/C#)
- [ ] `JarShapeDefinition` SO: collider listesi, maske sprite'ı, ağız noktaları, önerilen `tilt_angle_deg`; `Jar` prefab varyantları.
- [ ] 7. story stres testi her şekil için CI'da.
- [ ] Remote Config takvim şeması; Home rozeti; tablo etiketi.

### Analitik
- `jar_shape` {shape: string, run_id: string}

### Notlar
- Yuvarlak kavanozda yatırma etkisi zayıf (balıklar zaten kayar); dar boyunda çok güçlü. Dengesi Play modunda; LiveOps içeriği olarak ucuz.
