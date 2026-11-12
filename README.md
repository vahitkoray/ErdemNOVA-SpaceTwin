# ErdemNOVA-SpaceTwin

NASA Space Apps Challenge Niğde için geliştirilen SpaceTwin, Dünya üzerindeki bölgeleri Ay ve Mars araştırmaları açısından incelemeye yardımcı olan bir eğitim prototipidir. Şematik haritada bilinen analog örnekleri ve Türkiye’den araştırma adayları keşfedilebilir. Seçilen Dünya konumu için meteoroloji verileri sorgulanabilir.

## Challenge

[Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars](https://www.spaceappschallenge.org/2026/challenges/permanent-moon-base-locations-and-mars/)

## Takım · ErdemNOVA

- Göktürkhan YOZBATIRAN — 10/C
- Zeynep Su DOĞAÇ — 10/C
- Mehmet Berat AVLANMAZ — 10/A Anadolu
- Yusuf AKDEMİR — 10/C

**Rehber öğretmen:** Vahit Koray ŞENPEKMEZCİ

## Veri kaynakları

- [NASA POWER](https://power.larc.nasa.gov/) — Uygulama, seçilen Dünya konumu için aylık 2 metre hava sıcaklığı (T2M) ve günlük yağış ortalaması (PRECTOTCORR) verilerini canlı API üzerinden alır. Yıllık ortalamalar, ayların gün sayılarına göre ağırlıklandırılır.
- [NASA MOLA](https://pds-geosciences.wustl.edu/missions/mgs/mola.html) — Mars topoğrafyası için araştırma kaynağı.
- [NASA LOLA / LRO](https://lunar.gsfc.nasa.gov/lola/) — Ay topoğrafyası için araştırma kaynağı.

MOLA ve LOLA raster analizi prototipte henüz uygulanmadı. POWER verileri Dünya meteorolojisini tanımlar. Türkiye’deki adaylar ön araştırma niteliğindedir.

## Canlı prototip

[ErdemNOVA-SpaceTwin · Netlify](https://erdemnova-spacetwin.netlify.app)

## Etkinlik öncesi hazırlık ve sprint planı

GitHub deposu ve ilk web prototipi oluşturuldu, Netlify yayın adresi hazırlandı, NASA POWER bağlantısı entegre edilip düzeltildi ve proje kapsamı ile araştırma kaynakları belirlendi. Bu çalışmalar etkinlik öncesi hazırlıktır. 14–15 Kasım 2026 için aşağıdaki sprintler planlanmıştır.

1. [Sprint 1 · Kapsam ve görev planı](scrum/sprint-1.md)
2. [Sprint 2 · Veri akışı ve teknik test](scrum/sprint-2.md)
3. [Sprint 3 · Bilimsel içerik ve kullanıcı deneyimi](scrum/sprint-3.md)
4. [Sprint 4 · Demo ve teslim hazırlığı](scrum/sprint-4.md)
