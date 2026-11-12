# ERDEMNOVA | SPACETWIN
## TEKNİK DOKÜMANTASYON VE TESLİM REHBERİ

**Sürüm:** 1.1, 13 Kasım 2026  
**Durum:** Etkinlik öncesi mevcut prototipin teknik durum kaydı.

## 1. Proje kimliği

- Takım: ErdemNOVA. Proje: SpaceTwin. Etkinlik: NASA Space Apps Challenge 2026 – Niğde, 14–15 Kasım 2026. Yerel etkinlik formatı bu incelemede doğrulanamadı.
- Challenge: [Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars](https://www.spaceappschallenge.org/2026/challenges/permanent-moon-base-locations-and-mars/).
- Rehber öğretmen: Vahit Koray ŞENPEKMEZCİ.
- Öğrenciler: Göktürkhan YOZBATIRAN (10/C), Zeynep Su DOĞAÇ (10/C), Mehmet Berat AVLANMAZ (10/A Anadolu), Yusuf AKDEMİR (10/C).
- [Netlify prototip adresi](https://erdemnova-spacetwin.netlify.app/) ve [GitHub depo adresi](https://github.com/vahitkoray/ErdemNOVA-SpaceTwin). Depo görünürlüğü ve jüri erişimi bu incelemede doğrulanamadı.

## 2. Problem ve bilimsel yaklaşım

- Dünya’da Ay/Mars görev planlaması için benzer çevresel, jeolojik ya da operasyonel koşullar gösteren analog araştırma alanlarının incelenmesi amaçlanır.
- Analog alan, Ay veya Mars’ın tamamının Dünya’daki kopyası değildir. Benzerlik her zaman belirli bir özellik (örneğin kuraklık, volkanik köken, krater jeolojisi) bakımından değerlendirilmelidir.
- Prototip, bilinen örneklerle Türkiye’den araştırma adaylarını ayrı kategorilere yerleştirir; doğrulanmamış keşif veya nicel benzerlik puanı ileri sürmez.

## 3. Mimari ve teknoloji yığını

- [index.html](../index.html) arayüzü, stilleri, bölge kayıtlarını ve JavaScript kodunu içerir. Uygulama tarayıcıda çalışır. Derleme adımı ve uygulamaya ait bir sunucu veya veritabanı gerektirmez.
- Tarayıcı statik sayfayı Netlify’dan yükler. Veri düğmesine basıldığında tarayıcı NASA POWER’a doğrudan HTTPS isteği gönderir ve yanıtı yerel olarak işler.
- Arayüzde ekran genişliğine göre yerleşim kuralları bulunur. SVG harita şematiktir ve koordinatlar yaklaşık konumu gösterir.

## 4. Modüller ve işlevler

- Hero / ana ekran: proje özeti, yarışma teması ve gezinme.
- Explore the Earth: şematik SVG Dünya haritası, enlem-boylamla hesaplanan işaretleyiciler, filtreler ve bölge kartları.
- Known analogs: Atacama Çölü (Şili), Haughton Krateri (Kanada), Death Valley (ABD). Bunlar literatürde bilinen araştırma bağlamıyla sunulur.
- Explore Türkiye: Kapadokya, Karapınar, Nemrut Kalderası, Salda Gölü çevresi. Aday statüleri hipotez/ön araştırmadır, bilimsel doğrulama değildir.
- Compare Worlds: Mars veya Ay araştırma bağlamını seçme, Dünya bölgesi seçme ve NASA POWER sorgusu çalıştırma.
- Transparent Science: POWER/MOLA/LOLA kaynaklarının rolü ve yöntemsel sınırlamalar.
- Team: dört öğrenci, sınıfları ve rehber öğretmen.

## 5. Veri modeli ve konumlar

- `window.SITES` dizisi yedi kayıt içerir. Her kayıtta `id`, `name`, `country`, `lat`, `lon`, `category`, `tags`, `note`, `status` ve `why` alanları bulunur.
- Filtre düğmelerinin değerleri `all`, `known` ve `turkey` şeklindedir. Bunlar tüm kayıtları, `Bilinen analog` kategorisini ve `Türkiye adayı` kategorisini gösterir.
- Kart veya harita işaretleyicisi seçimi profil panelini günceller. POWER sorgusu, karşılaştırma panelindeki ayrı bölge seçicisinden koordinatları alır. Ay/Mars seçimi yalnızca araştırma bağlamı metnini değiştirir.
- `why` alanı araştırma gerekçesini açıklar. Bölge bazında literatür doğrulaması ve aday koordinatlarının kaynak kayıtları henüz belgelenmedi.

## 6. NASA POWER API entegrasyonu

- Uç nokta: `https://power.larc.nasa.gov/api/temporal/monthly/point`. İstek `parameters=T2M,PRECTOTCORR`, `community=RE`, `longitude`, `latitude`, `start`, `end` ve `format=JSON` parametrelerini kullanır. [NASA POWER aylık API belgesi](https://power.larc.nasa.gov/docs/services/api/temporal/monthly/)
- T2M, 2 m hava sıcaklığıdır (°C). PRECTOTCORR, düzeltilmiş günlük yağışın aylık ortalamasıdır (mm/gün).
- `start` ve `end` aynı yıldır. Yıl, tarayıcının UTC yılı eksi iki olarak seçilir. UTC yılı 2026 iken istek 2024 verilerini sorgular.
- Yanıt `properties.parameter` içinden okunur. Kod, iki parametre için de `YYYYMM` anahtarlarını tarar, değerleri `Number()` ile dönüştürür ve sonlu olmayan veya −900 ve altındaki değerleri eler. Hesaplama için 12 ayın tamamı gerekir. `null` ve boş dizeler sıfıra dönüşebildiğinden bu dönüşüm sıkı bir yanıt şeması doğrulaması sağlamaz.
- İstek `AbortSignal.timeout(45000)` kullanır. Başarıda iki ondalık basamaklı özet ve ham JSON bağlantısı, hatada açıklama ve ham JSON bağlantısı gösterilir.

## 7. Yıllık özet hesaplama yöntemi

- Her ay için gerçek takvim gün sayısı d_m kullanılır; artık yıl Şubat 29 gün olarak hesaplanır.
- Yıllık ağırlıklı ortalama sıcaklık T = Σ(T_m × d_m) / Σd_m.
- Yıllık ortalama günlük yağış P = Σ(P_m × d_m) / Σd_m (mm/gün). Bu değer yıllık toplam yağış değildir.
- Yaklaşık yıllık toplam yağışa ihtiyaç olursa Σ(P_m × d_m) mm hesaplanmalıdır. Mevcut arayüz bunu göstermek yerine günlük ortalama sunar.
- Önceki belgede Atacama için 2024 ekran çıktısı 16,14 °C ve 0,05 mm/gün olarak kaydedilmiş. Ham JSON, ekran görüntüsü ve bağımsız yeniden hesaplama bu incelemede doğrulanamadı.

## 8. Ay ve Mars veri setleri: kapsam sınırı

- MOLA (Mars Orbiter Laser Altimeter): Mars yüzey yükseltisi/topografya için olası veri kaynağı. Prototipte raster okuma, yeniden projeksiyon ve bölgesel eşleştirme gerçekleştirilmemiştir.
- LOLA/LRO (Lunar Orbiter Laser Altimeter): Ay yüzey yükseltisi/topografya için olası veri kaynağı. Prototipte raster analizi gerçekleştirilmemiştir.
- POWER Dünya atmosfer verisi üretir. Mars veya Ay sıcaklığı, atmosfer basıncı, yağış veya yerçekimiyle doğrudan eşdeğer değildir.
- Şu aşamada dünya konumları ile bir Ay/Mars konumu arasında kanıtlanmış nicel skor, yüzey mineralojisi kıyası ya da yeni analog keşfi bulunmamaktadır.

## 9. Kullanım yönergesi

- https://erdemnova-spacetwin.netlify.app/ adresini açın.
- Analog alan araştırmasında filtrelerden tümü / bilinen örnekler / Türkiye adaylarını seçin; kart veya harita işaretleyicisine tıklayarak profil açıklamasını okuyun.
- Gerçek veri ile incele bölümünde araştırma hedefini (Mars/Ay) ve Dünya konumunu seçin. NASA POWER verisini getir düğmesine basın.
- Veri dönerse 2024 (2026 yılı için) özetini ve ham JSON bağlantısını görün. Hata varsa ağ, servis erişimi veya CORS durumunu kontrol edin.
- Sonuçlar belirli Dünya koordinatının meteorolojik durumunu anlatır; tek başına Mars/Ay analog statüsünü kanıtlamaz.

## 10. Test ve doğrulama planı

- Kaynak kodundan doğrulananlar: yedi bölge kaydı, aylık POWER isteği, gün sayısıyla ağırlıklandırma, 12 ay koşulu ve hata bildirimi. Dört sprint planı yerel Git geçmişinde `9248bd8` kaydında yer alır.
- Önceki belgede Netlify sayfasının mobil tarayıcıda açıldığı ve Atacama için veri gösterildiği kaydedilmiş. Test tarihi, tarayıcı sürümü, ekran görüntüsü ve ham yanıt bu incelemede doğrulanamadı. Canlı site ve API akışı yeniden test edilmedi.
- Planlanan testler: yedi bölge için ham JSON ve arayüz sonuçlarını karşılaştırma, 12 aylık hesaplamayı bağımsız olarak yeniden yapma, ağ hatasında bildirimi kontrol etme ve mobil/masaüstü tarayıcılarda filtreler ile klavye erişimini deneme.
- Otomatik test çalıştırması ve bilimsel belirsizlik değerlendirmesi belgelenmedi.

## 11. Sorunlar ve çözümler

- Git geçmişinde önceki kod `annual/point` yolunu kullanır. `068a197` kaydı aylık uç noktaya ve gün sayısıyla ağırlıklandırılan yıllık özete geçişi gösterir. Önceki belgede tarayıcıda `Failed to fetch` hatası görüldüğü belirtilmiş. Bu hata için tarayıcı kaydı bulunmadığından nedeni doğrulanamadı.
- Kalan riskler arasında ağ veya CORS hatası, eksik aylık veri ve dış servis kesintisi bulunur. Mevcut hata bildirimi ham veri bağlantısını gösterir. Demo öncesinde yeniden test ve varsa tarihli gerçek bir yanıtın örnek olarak hazırlanması Sprint 4 planındadır.

## 12. Güvenlik, lisans ve etik

- Tarayıcı NASA POWER’a HTTPS GET isteği gönderir. Uygulama kodunda API anahtarı veya kullanıcı hesabı akışı bulunmaz.
- Site bağımsız eğitim prototipi olarak tanıtılır. Üçüncü taraf veri ve görseller için lisans ve atıf denetimi belgelenmedi.

## 13. Kurulum ve dağıtım

- Yerel Git dalı `main` ve `origin` uzak adresi proje deposuna işaret ediyor. Uzak deponun varsayılan dalı ve görünürlüğü bu incelemede doğrulanamadı.
- Yerel ön izleme için depo kökündeki `index.html` dosyasını tarayıcıda açın. Canlı veri sorgusu için NASA POWER’a tarayıcıdan erişim gerekir.
- Prototip için Netlify adresi hazırlandı. Yayın dalı, yayın dizini, derleme ayarı ve GitHub ile otomatik dağıtım bağlantısı Netlify panelinden doğrulanmadı.

## 14. Organizasyon ve Scrum süreci

- GitHub deposu, Netlify yayın adresi, ilk web prototipi, NASA POWER bağlantısı ve proje kapsamının belirlenmesi etkinlik öncesi hazırlıktır. [Proje özeti](../README.md)
- 14 Kasım: [Sprint 1 · Kapsam ve görev planı](../scrum/sprint-1.md), [Sprint 2 · Veri akışı ve teknik test](../scrum/sprint-2.md).
- 15 Kasım: [Sprint 3 · Bilimsel içerik ve kullanıcı deneyimi](../scrum/sprint-3.md), [Sprint 4 · Demo ve teslim hazırlığı](../scrum/sprint-4.md).
- Dört sprint de 13 Kasım 2026 itibarıyla plan aşamasındadır. Gerçekleşen öğrenci katkıları, teknik sorunlar, çözümler ve sprint sonuçları sprint sonunda kaydedilecek. Scrum Master ataması bu incelemede doğrulanamadı.

## 15. Jüri teslim kontrol listesi

- Proje ekibi için hazırlık listesi: takım ve challenge kaydını kontrol etme, canlı prototip bağlantısını deneme, demo provası yapma ve gerçekleşen sprint sonuçlarını belgeleme.
- Niğde’ye özgü teslim kanalı, dosya biçimleri, depo erişimi, video veya slayt zorunluluğu ve yerel son saat doğrulanamadı. Yerel organizatör duyurusu ve güncel küresel teslim yönergesi üzerinden kontrol edilmeli.

## 16. Gelecek geliştirme ve öncelikler

- P0: Daha kapsamlı NASA POWER testleri; veri/hesaplama kanıtları; açık hata senaryoları; çalışır demo.
- P1: MOLA ve LOLA için uygun çözünürlüklü raster/ön işlenmiş veri kaynakları, coğrafi koordinat dönüşümü ve veri lisansı doğrulaması.
- P2: Çok ölçütlü analog karşılaştırma şeması, kalibrasyon, belirsizlik ve bilimsel değerlendirme; benzerlik skorunun ancak yöntem doğrulandıktan sonra eklenmesi.
- P3: Gerçek coğrafi harita/GIS katmanları ve daha erişilebilir mobil kullanıcı deneyimi.

## 17. Resmî referanslar ve erişim bağlantıları

- [NASA Space Apps Challenge 2026](https://www.spaceappschallenge.org/2026/)
- [Resmî challenge sayfası](https://www.spaceappschallenge.org/2026/challenges/permanent-moon-base-locations-and-mars/)
- [NASA POWER aylık API belgesi](https://power.larc.nasa.gov/docs/services/api/temporal/monthly/)
- [Mars MOLA arşivi](https://pds-geosciences.wustl.edu/missions/mgs/mola.html)
- [Ay LOLA / LRO görev kaynağı](https://lunar.gsfc.nasa.gov/lola/)
- [Netlify prototipi](https://erdemnova-spacetwin.netlify.app/)
- [GitHub deposu](https://github.com/vahitkoray/ErdemNOVA-SpaceTwin)
