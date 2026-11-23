# Görev: Blender'da foto-gerçekçi 3D açıklayıcı Shorts üreten bir Claude Code skill sistemi kur

Sen benim bilgisayarımda çalışan Claude Code'sun. Amacın, Zack D. Films tarzında (gerçekçi insan vücudu, anatomi, "Eğer ... yaparsan ne olur?" açıklayıcıları) **dikey 1080×1920 YouTube Shorts / Reels / TikTok videolarını tamamen Blender içinde, gerçek 3D render olarak** üreten, tekrar kullanılabilir bir **Claude Code skill + ajan sistemi** kurmak. AI video üretici (Veo, Sora, Kling vb.) kullanma; her kare Blender render'ı olacak.

Hedef kalite: Mr omi'nin ücretli "3D Animated Shorts" skill'inin vardığı seviyeye (aşağıdaki "Referans iş akışı" bölümü) ulaşmak ve onu geçmek. Bu sistemi bir kez kuracaksın, sonra ben tek bir kısa komutla ("3d-short skill'ini kullan, şu konuyu yap" ya da "şu referans videoyu beat-for-beat ama daha iyi yeniden yap") video ürettireceğim.

---

## 0. Çalışma kuralları

- **Önce ortamı doğrula:** işletim sistemi, Blender sürümü (4.5 LTS veya 5.x), GPU (NVIDIA/AMD/Apple), VRAM, RAM, disk, ffmpeg, Python, uv/pip, git. Eksik olanı söyle, kurulumu benden onay alarak yap.
- **Her büyük aşamanın sonunda dur**, ne yaptığını 5-10 satırda özetle, örnek kare (still) göster ve devam için onay iste. Küçük kararları kendin ver.
- Her şeyi tek bir git reposunda tut (`~/3d-shorts-studio` gibi), sık commit at.
- **Hesap açman veya giriş yapman gereken hiçbir şeyi kendin yapma.** API anahtarı gerekiyorsa bana sor; anahtarları asla repoya yazma (`.env` + `.gitignore`).
- Ücretsiz/açık kaynak öncelikli. Ücretli servis (ElevenLabs vb.) yalnızca isteğe bağlı eklenti olsun; varsayılan yol tamamen ücretsiz çalışmalı.
- Her indirilen asset için lisans ve gerekli atfı `licenses.json` dosyasına kaydet. Ticari kullanıma kapalı (non-commercial) ya da paylaşımda atıf zorunluluğu dayatan kaynakları varsayılan olarak kullanma; kullanman gerekirse bana sor.
- Başkasının videosunu, senaryosunu veya karakterini kopyalama: referansları **ölçmek ve öğrenmek** için kullan, içerik ve metin bizim olsun. Bilimsel iddiaları doğrula; kaynağı zayıfsa "Bilim insanları ... tahmin ediyor" gibi ihtiyatlı dil kullan.

---

## 1. Önce mevcut açık kaynak çalışmaları incele (tekerleği yeniden icat etme)

Bunları klonla (veya fork'la) ve README, SKILL.md, betikleri oku. Hangi parçayı alacağını bir tabloda bana raporla. Lisanslarına uy (çoğu MIT).

| Repo | Ne için bakıyorsun |
|---|---|
| `Hitid123/kylo-3d-shorts` | Konuya en yakın temel: MPFB gerçekçi insanlar, röntgen/anatomi ikizi, Poly Haven, referans ölçer, −14 LUFS mastering, QA kapısı |
| `goinsidemachines/blender-shorts-agent` | Üretim mühendisliği: sahne/hareket/kare kontrolleri, test karesinden render süresi tahmini, parça parça render, bellek koruması, ses mix, yerel TTS (Kokoro/Chatterbox) |
| `mromidev/claude-blender-skills` | Gerçekçilik kuralları, render süresini 5-8 saatten ~1 saate indirme yöntemi, Blender'a soket köprüsü |
| `AVENGERSTHE2102/videoeditor` içindeki `library/nocodehack/make-short/` | Orijinali silinmiş make-short skill'inin SKILL.md'si: bağımsız eleştirmen (critic) ajan tasarımı, referans ölçüm (measure) mantığı, "asla kendini puanlama" kuralı |
| `luispablovillasenor-afk/shorts-animados-claude` → `realista/` | Dikiş (stitches) sahnesinin ücretsiz Blender yeniden yapımı: deri, yara, iplik, tüyler |
| `ahujasid/mcp-for-blender` (eski adı blender-mcp) | Claude ↔ Blender canlı bağlantı, Poly Haven/Sketchfab asset arama, `look` ile sahneye bakma. Telemetriyi `DISABLE_TELEMETRY=true` ile kapat |
| `kinnuworks/faceless-shorts`, `sunxiayi/make-blender-education-video-skill`, `squall01337/mixamo-llm-mocap`, `usefulwrong/human-models-mcp` | Ek fikirler: foto-gerçekçi Poly Haven sahneleri, bilimsel eğitim videoları, referans videodan Mixamo hareket, Z-Anatomy kasları |

Bu repolar küçük ve kaybolabiliyor; kullandıklarını yerel olarak sakla (vendor/ veya submodule).

---

## 2. Referans iş akışı (bunu öğren ve uygula)

Mr omi videosunda şu yöntemi anlattı. Sistemin bunların hepsini **otomatik ve tekrar kullanılabilir** biçimde yapmalı:

**Referansı ölç:**
- Referans kanalın en çok izlenen short'larını kare kare analiz et (30 fps tam çözünürlük).
- Kamera hareketini **phase correlation** ile ölç (her kareyi frekans alanına çevirip kareden kareye kayma ve zoom miktarını hesapla). Sonuç: yana kaydırma (pan) yok; tek eksende düz dolly-in, bekleme, yavaş sürünme ve yumuşak (eased) geri çekilme.
- Kamera **jerk** değerini ölç (ivmenin değişim hızı). İlk testte bizimki referansın 8 katıydı; kamerayı tek dolly eksenine kilitleyip her hareketi ease'leyince yarıya indi.
- Parlaklık, kontrast, doygunluk, arka plan mavisinin tam RGB değeri, mix'in ses yüksekliği (LUFS).
- Senaryo kalıbı: çoğu "If you..." ikinci tekil şahıs varsayımıyla açılıyor. Konuşma hızı medyan ~2,77 kelime/sn (sakin).
- Geçişlerin yalnızca ~1/7'si sert kesme; geri kalanı hareketli geçiş (whip pan, kameranın bir sonraki sahneye uçtuğu zoom'lar).
- Mikro detay: dikiş sahnesinde her 6-8 karede yeni bir dikiş, iplik kendi yolunu çiziyormuş gibi animasyonlu.
- **Asıl püf noktası:** kendi render'ını da aynı ölçüm hattından geçir. İki veri seti arasındaki fark = yapılacaklar listesi.

**"Gerçeği varsa indir, elle yapma" kuralı:**
- İnsanlar: taranmış (scan) insanlar güzel görünür ama tek yüzeydir (pantolonun altında bacak yoktur) ve yüz rigi yoktur, oyunculuk yapamaz. Yüz ifadesi gereken karakterler için **MPFB2** (Blender resmi eklenti sitesindeki MakeHuman eklentisi, karakter çıktısı CC0) kullan; 2.0.15+ sürümündeki ARKit isimli yüz shape-key paketi (faceunits01) ve viseme paketleriyle gerçek bir "şaşırmış" ifade yap. MetaHuman hesap/token gerektirdiği için çıkmaz sokak.
- MPFB karakterleri "video oyunu" gibi görünür: mumsu deri, kalın dudaklar, kâse saç. Düzelt: mat deri, gerçek gözenekler (normal/displacement), **subsurface scattering**, gerçekçi saç.
- Gözleri, dişleri, saçı ve kıyafeti **armature'a bağla**; yoksa yürürken vücuttan uçup gider.
- Kemikler: BodyParts3D (CC BY 4.0, atıf gerekli) veya Z-Anatomy (CC BY-SA 4.0). MB-Lab'den kaçın (verisi AGPL).
- Dokular/HDRI/modeller: Poly Haven (CC0; API'yi benzersiz User-Agent ile kullan), ambientCG (CC0). Poly Haven'da "et" dokusu yoksa yaranın içi için taranmış sünger dokusu gibi yaratıcı çözümler bul.
- Dünya: NASA Blue Marble (yüzey, bulut, gece şehir ışıkları) + yükseklikle sönen ince atmosfer. Atıf: "NASA Earth Observatory".
- Şehir: taranmış cephe kitinden binalar; düşük güneş, uzun gölgeler, ağaçlar, arabalar, çatıda klima üniteleri.
- Bulutlar: sahte katı bloblar değil, ışığı saçan ve gölge düşüren **hacimsel (volumetric)** bulutlar.
- 3D çim siyah duvar gibi render olursa: gerçek fotoğraf + yalnızca gölge yakalayan (shadow catcher) zemin.
- Kendi damar üreteci: kalpten çıkan parlak kırmızı arterler, dönen koyu venler, siluet okunsun diye deriye yakın yerlerde daha yoğun dallanma.
- Elle yapılmış her şey "AI slop" gibi durursa referansa geri dön. Örneğin onun kuşbakışı çekiminde stadyum bile yok, sadece açık mavi ışıkta yüzen bir saha.

**Teknik hata dersleri:**
- Deri üstüne konan yara 0,25 mm havada kalırsa, render sırasında Blender deriyi ters köşegenden üçgenler ve deri yaradan dışarı çıkar. Yarayı renderer'ın gördüğü üçgenlerden yeniden kur ve deriye bağla (Surface Deform / Shrinkwrap) ki her adımda birlikte deforme olsun.
- Yaranın etrafında gerçek 3D tüyler (~2.600 adet).
- Dişler yeşil görünüyorsa önce **ışık sekmesini** kontrol et (çimden yansıyan yeşil). Düzeltmeyi gereksiz yere yapma; gerekirse yaptığın düzeltmeyi geri al.

**Gerçekçi görünüm (render'lar "fazla kusursuz" olmasın):**
- View transform: **AgX** (Standard parlak yerleri patlatır, imzalı maviyi de söndürebilir). Maviyi koruyan ama highlight'ları düz beyaza yakmayan ayarı test ederek seç.
- Kompozitör: yumuşak lens / hafif lens distorsiyonu, parlak noktalarda glow (Blender 5.x'te Glare node → Bloom), vinyet, film greni, hızlı hareketlerde motion blur, gerekirse çok hafif kromatik aberasyon.
- Bu zinciri tek bir yeniden kullanılabilir preset/betik yap; her sahneye bir komutla uygulansın.

**Ses:**
- Referans mix'i ölç (onunki ~−5 LUFS, neredeyse hiç dinamik aralık yok). Platform için hedefini belirle ve ölçerek uygula.
- Katmanlar: her dünya için room tone / ambiyans (stadyum havası, uzayda derin drone), büyük anlarda üst üste bindirilmiş vuruşlar, kamerayı takip eden reverb.
- Her efekt tam doğru karede: örneğin "bones" kelimesinde kemik vuruşu. Seslendirmeyi Whisper ile kelime zaman damgalarına böl, efektleri ona hizala.
- Varsayılan mix kuralı: müzik +3 dB, efektler yarı seviye. Bir ses "çok yüksek" denirse kulakla değil **ölçerek** düzelt (LU cinsinden).
- Seslendirme: varsayılan yerel ve ücretsiz (Kokoro / Chatterbox), isteğe bağlı ElevenLabs.

**Render süresi (11 saat → 61 dakika yolculuğu):**
- Önce **render'ın içine kronometre koy**: her karede, her aşamada süre nereye gidiyor ölç, sonra optimize et.
- Bulunan iki büyük darboğaz: (1) sis/haze ışık kaynağı sayılıyordu ve her bina/ağaç/araba her karede ışık listesine yeniden ekleniyordu → şehir kareleri 19 sn'den 8 sn'ye. (2) Bulutlar Blender'ın yeni volume modunda takılıydı ve hız ayarını yok sayıyordu → 40 sn'den 14 sn'ye.
- 1080p render + 2K'ya upscale (telefonda fark görünmez). Upscale için Real-ESRGAN gibi yerel bir araç.
- Adaptive sampling, OpenImageDenoise / OptiX, light tree, makul bounce sayıları; yalnızca **görsel olarak aynı kalan** hızlandırmaları tut (önce/sonra karşılaştır).
- GPU/VRAM yetiyorsa aynı anda 2 Blender örneği, farklı kare aralıklarıyla.
- Taslaklar EEVEE'de, final Cycles'ta (veya test karesine göre hangisi iyiyse).

**Eleştirmen ajan:**
- Her düzeltmeden sonra, sahneyi yapan ajandan **bağımsız, taze bir Claude alt-ajanı** still'leri referans karelerle yan yana değerlendirsin (7 → 7,5 → 8,5 → render'a hazır). Üretici ajan kendini asla puanlamasın.
- Puanlar zamanla şişer; puanın yanında **ikili (geçti/kaldı) kontrol listesi** de kullan: havada objeler, iç içe geçme, yüzen yara, yanlış renk sekmesi, mumsu deri, düz kırmızı doku, sahte bulutlar, AI-slop görünümü, kamera kayması, kompozisyon (dikey kadraj, güvenli alan, altyazı alanı).
- Ayrıca 21 öğreticiden öğrenme benzeri bir mod: bana verilen veya izin verdiğim tutorial transkriptlerini okuyup bulguları kural dosyasına eklesin.

---

## 3. Kurmanı istediğim sistem

Aşağıdaki yapıyı (veya daha iyisini; gerekçesini yaz) oluştur:

```
3d-shorts-studio/
  .claude/
    skills/3d-short/SKILL.md          # ana skill: uçtan uca akış, kısa ve net adımlar
    skills/3d-short/playbook/          # ayrıntılı kurallar: kamera, ışık, deri, ses, render, lisans
    agents/critic.md                   # bağımsız eleştirmen alt-ajan
    agents/researcher.md               # konu araştırma + bilgi doğrulama alt-ajanı
    agents/sound-designer.md           # (isteğe bağlı) ses alt-ajanı
  tools/
    analyze_reference.py   # PySceneDetect (kesmeler), OpenCV phaseCorrelate (kayma/zoom/jerk),
                           # renk/parlaklık/doygunluk/arka plan RGB, ffmpeg ebur128 (LUFS),
                           # Whisper (kelime/sn, açılış kalıbı) → reference_profile.json
    compare_profiles.py    # referans vs bizim render → fark raporu = yapılacaklar listesi
    camera.py              # tek eksen dolly, ease eğrileri, jerk sınırı, hareketli geçişler
    look_preset.py         # AgX + kompozitör post zinciri
    assets.py              # Poly Haven / ambientCG / BodyParts3D / NASA indirme + licenses.json
    humans.py              # MPFB karakter, ARKit yüz anahtarları, deri materyali, armature bağlama
    vessels.py             # dallanan damar ağı üreteci
    sound.py               # TTS, kelime hizalı SFX, ambiyans, mix, LUFS ölçümü/normalizasyonu
    render.py              # kronometreli render, paralel örnekler, upscale, son birleştirme
    qa.py                  # otomatik kare kontrolleri (siyah/donmuş kare, firefly, titreme)
  references/              # ölçülmüş referans profilleri (video dosyalarını repoya koyma)
  projects/<konu>/         # her video: script.md, shotlist.json, .blend, renders, mix, final.mp4
  licenses.json
  README.md                # kurulum + "tek komutla video" kullanım kılavuzu
```

**Video üretim akışı (skill'in uygulayacağı adımlar):**
1. Konu → araştırma ve bilgi doğrulama → ~30-45 sn senaryo ("If you..." kancası, ~2,7 kelime/sn, kendi cümlelerimiz).
2. Shot list: her çekim için kamera hareketi, süre, geçiş tipi, gereken asset'ler.
3. Asset listesini bana göster, onaylayınca indir (lisansları kaydet).
4. Her çekimi önce **tek still** olarak kur ve render et → eleştirmen → düzelt → geçince animasyon.
5. Kamera ve geçişler → taslak (EEVEE / düşük örnek) → `compare_profiles` ile referansa karşı ölç → düzelt.
6. Ses tasarımı ve mix, ölçerek.
7. Final render (kronometreli, optimize, paralel) → upscale → altyazı (isteğe bağlı) → ses birleştirme → `final.mp4` (1080×1920 veya 1440×2560, 30 fps).
8. Başlık, açıklama, hashtag önerisi ve kullanılan asset atıfları.

---

## 4. Kabul testleri (bunları geçmeden "bitti" deme)

1. **9 saniyelik dikiş testi:** ön kolda dikiş atılan bir yara, tek kesintisiz çekim, dolly-in → bekleme → yavaş geri çekilme. Kendi seslendirmemiz: "When you get stitches, a doctor pulls a thread through both sides of the cut and ties it tight." `compare_profiles` raporu kameranın yana kaymasının ~0 olduğunu ve jerk'in referans aralığında olduğunu göstersin. Eleştirmen ≥8,5 versin ve kontrol listesi tamamen geçsin.
2. **Yüz ifadesi testi:** MPFB karakteri nötrden gerçek bir "şaşırmış" ifadeye geçsin (kaşlar yukarı, gözler açık, çene düşük). Mumsu deri yok, kıyafet/göz/diş/saç yürürken vücuttan ayrılmıyor.
3. **Tam short:** kendi seçeceğim (veya senin önereceğin) ~30-40 sn'lik bir anatomi konusu. Örneğin "Kan damarların ne kadar uzun?" fikri, kendi metnimiz ve doğrulanmış bilgiyle: insan + damar ağı + BodyParts3D kemikler + en az iki farklı dünya (stadyum/şehir/Dünya yörüngesi) ve hareketli geçişler.
4. **Render bütçesi:** tam short benim GPU'mda mümkün olduğunca 1 saatin altında render olsun. Her optimizasyonun önce/sonra süresini ve görsel farkını (fark yoksa "yok") raporla.
5. **Tek komut testi:** yeni bir konuşmada yalnızca "3d-short skill'ini kullan: <konu>" yazdığımda sistem bu dosyadaki bağlamın hiçbirine ihtiyaç duymadan baştan sona çalışsın.

---

## 5. Teslimde istediklerim

- Çalışan repo ve README (kurulum + kullanım).
- `9sn_dikis_testi.mp4`, yüz ifadesi testi ve tam short'un final MP4'leri, ayrıca clay/kil (materyalsiz) versiyonu.
- Referans vs bizim render karşılaştırma raporu (tablo).
- Render süresi raporu (optimizasyon başına kazanç).
- `licenses.json` ve video açıklamasına konacak atıf metni.
- Bilinen sınırlamalar ve bir sonraki iyileştirme listesi.

Başla: önce 0. adımdaki ortam kontrolünü yap, sonra 1. adımdaki repoları inceleyip hangi parçaları alacağını tablo halinde raporla ve onayımı bekle.
