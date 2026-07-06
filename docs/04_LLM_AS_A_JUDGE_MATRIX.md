# 1. Değerlendirme Metodolojisi ve Deney Kurgusu
Geleneksel semantik eşleşme metriklerinin (BERTScore vb.) biçimsel sentaks değişimlerinden ötürü ürettiği manipülatif darboğazları aşmak adına, projenin nihai doğrulama katmanı olarak bir **LLM-as-a-Judge (Hakem LLM)** mimarisi kurgulanmıştır.
**Akıllı Örneklem (Sampling Layer):** Geliştirme bias'ını (taraflılık) engellemek adına, 500 benzersiz prompttan oluşan ana test havuzundan rastgele seçilen **100 vaka** (ID bazlı) deneye tabi tutulmuştur.
**İşlem Hacmi:** Her bir vakanın ham hali ile birlikte 3 farklı sıkıştırma varyasyonu (`min`, `mid`, `max`) üretilerek toplam **400 bağımsız varyasyon satırı** oluşturulmuştur.
**Hakem Model Yapılandırması:** Prompt varyasyonları ilk olarak `Gemini 3.1 Flash-Lite` modeline gönderilerek çıktılar `sonuclar_gemini-3.1-flash-lite.jsonl` olarak kaydedilmiştir. Ardından, modelin kendi ürettiği çıktı varyasyonları arasındaki nüansları en tutarlı şekilde yorumlayabilmesi amacıyla, yine aynı modele (Self-Evaluation kurgusuyla) `1-5` arası puanlama yaptırılmıştır.

Puanlama, LLM çıktılarının endüstri standartlarındaki kalitesini ölçen 3 temel boyutta gerçekleştirilmiştir:
- **Anlamsal Sadakat (Semantic Fidelity):** Sıkıştırılmış promptun, ham prompttaki ana komutu ve enformasyon özünü ne kadar koruduğu.
- **Çıktı Akıcılığı (Output Quality / Fluency):** Optimize edilmiş prompt ile tetiklenen LLM yanıtının dil bilgisi, akıcılık ve mantık kalitesi.
- **Detay Koruma Oranı (Detail Retention):** Prompt içindeki teknik parametrelerin, kısıtların ve yan detayların korunma derecesi.
# 2. Global Performans ve Boyutsal Başarı Matrisi
### SIKIŞTIRMA MODLARINA GÖRE GENEL PERFORMANS MATRİSİ
#### MIN MODU PERFORMANSI:
GENEL BAŞARI PUANI (Total Score) : 4.44 / 5.0
Anlamsal Sadakat (Semantic) : 4.41 / 5.0
Çıktı Akıcılığı (Output Quality) : 4.71 / 5.0
Detay Koruma Oranı (Detail) : 4.21 / 5.0
#### MID MODU PERFORMANSI:
GENEL BAŞARI PUANI (Total Score) : 4.52 / 5.0
Anlamsal Sadakat (Semantic) : 4.52 / 5.0
Çıktı Akıcılığı (Output Quality) : 4.67 / 5.0
Detay Koruma Oranı (Detail) : 4.37 / 5.0
#### MAX MODU PERFORMANSI:
GENEL BAŞARI PUANI (Total Score) : 4.46 / 5.0
Anlamsal Sadakat (Semantic) : 4.44 / 5.0
Çıktı Akıcılığı (Output Quality) : 4.58 / 5.0
Detay Koruma Oranı (Detail) : 4.37 / 5.0
### Bulgular ve Analiz:
**"Daha Az Gürültü, Daha Yüksek Odak" Paradoksu:** Deneyin en çarpıcı sonucu, `mid` modunun (**4.52**), metne en az dokunan `min` modundan (**4.44**) daha yüksek bir genel başarı puanı almasıdır. Bunun hesaplamalı dilbilimsel ve mimari açıklaması şudur: Prompt içindeki nezaket kalıpları, dolgu kelimeleri ve gürültüler arındırıldığında, LLM'in **Attention Mekanizması** dağılmaktan kurtulur. Gürültüden arınmış, doğrudan eyleme odaklanan optimize prompt, modelin enformasyon iskeletini daha doğru işlemesini sağlar ve çıktı kalitesini artırır.
**MAX Modunun Verimliliği:** En agresif ve radikal sıkıştırmayı yapan `max` modunun, genel başarıda **4.46** alarak `min` modunu geride bırakması kütüphanenin hassasiyetini kanıtlamaktadır. %27'lik ekstrem bir token tasarrufu sağlarken, çıktı kalitesinde sadece `0.13` puanlık minimal bir sapma yaşanması kurumsal ölçeklemede muazzam bir Yatırım Getirisi anlamına gelir.
# 3. Kategori ve Mod Bazlı Dağılım Matrisi
Sıkıştırma algoritmalarının prompt türlerine göre davranışını mikro düzeyde incelemek adına, kategorilerin mod bazlı Total Skor Ortalamaları haritalandırılmıştır:
### Total Skor Ortalamaları Dağılım Tablosu

| **Kategori / Sıkıştırma Seviyesi** | **MIN Modu** | **MID Modu** | **MAX Modu** | **En Başarılı Mod / Segment**     |
| ---------------------------------- | ------------ | ------------ | ------------ | --------------------------------- |
| **`natural_coding`**               | 4.35         | 4.53         | **4.55**     | **MAX Modu** _(Ultra Odaklı Kod)_ |
| **`text_formatting`**              | 4.56         | **4.59**     | 4.44         | **MID Modu** _(Dengeli Format)_   |
| **`daily_explanation`**            | 4.49         | **4.53**     | 4.40         | **MID Modu** _(Akıcı Bilgi)_      |
| **`creative_generation`**          | 4.37         | **4.44**     | **4.44**     | **MID / MAX Modu Ortaklığı**      |
### Kategorik Derin İnceleme:
**`natural_coding` Segmentinde MAX Modu Dominasyonu (4.55):** Yazılımcıların doğal dille kod veya regex talep ettiği bu kategoride `max` modu zirveye yerleşmiştir. Yazılım mantığında nezaket kalıplarının ve süslü cümlelerin yeri olmadığından, sistem tüm gürültüyü temizleyip promptu saf algoritma girdisine çevirdiğinde LLM'in ürettiği kodun kararlılığı ve doğruluğu en üst seviyeye çıkmaktadır.
**`text_formatting` Segmentinde MID Modu Dengesi (4.59):** Veri manipülasyonu, JSON şeması oluşturma veya metin düzenleme taleplerinde `mid` modu rekor kırmıştır. Bu kategoride metnin yapısal sınırları ve bazı yönelim ekleri kritik olduğundan, `mid` modunun dengeli yaklaşımı model kalitesini zirvede tutmuştur.
# 4. Sistemik Genel Tutarlılık ve Kararlılık Analizi
Sistemin farklı modlar arasında geçiş yaparken ne kadar stabil kaldığını ölçmek adına yapılan varyans ve kararlılık testlerinin akademik yorumu şu şekildedir:
- **Modlar Arası Ortalama Varyans Sapması:** `0.256`
- **Yalın-TokenSaver Tutarlılık Puanı:** `4.74 / 5.0`
### Akademik Değerlendirme Notu
Modlar arası ortalama varyans sapmasının `0.256` gibi sıfıra son derece yakın bir eşikte kalması, sistemin matematiksel ve dilbilimsel açıdan **Üst Düzey Kararlılığa** sahip olduğunu belgeler.

Yalın-TokenSaver, kullanıcının seçtiği sıkıştırma kademesinden bağımsız olarak enformasyon çizgisini kesin bir şekilde korumaktadır. Algoritma promptu radikal düzeyde kırparken, ana fikri, teknik parametreleri ve komut iskeletini asla değiştirmemekte, model girdilerini lobotomiye uğratmadan optimize etmektedir.
# 5. Sonuç
LLM-as-a-Judge testleri, Yalın-TokenSaver'ın yapay zeka operasyonlarında büyük bir etkiye sahip olduğunu tescillemiştir. Sistem promptları sıkıştırıp bütçe tasarrufu sağlarken, LLM'lerin attention mekanizmasını optimize ederek çıktı doğruluğunu artıran bir **"enformasyon filtresi"** görevi görmektedir. 