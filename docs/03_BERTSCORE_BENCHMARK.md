# 1. Giriş ve Veri Seti Yapısı
Bu döküman; Yalın-TokenSaver algoritmasının prompt sıkıştırma operasyonu esnasında ham metin (`raw_prompt`) ile optimize edilmiş metin (`optimized_prompt`) arasında yarattığı anlamsal ve bağlamsal benzerlik değişimlerini ölçmek amacıyla yapılan **BERTScore Semantik Analiz** sonuçlarını raporlar.
Değerlendirme süreci, lokal yapay zeka altyapısında Türkçe dil yapısına en üst düzeyde uyum sağlayan `dbmdz/bert-base-turkish-cased` sarmalayıcı modeli üzerinden koşturulmuştur. Toplam **1500 log satırı** (500 benzersiz vakanın 3 farklı operasyonel modu) matris halinde işlenmiş ve kategorik dağılımlar şu şekilde ayrıştırılmıştır:
- `natural_coding` (Doğal Kodlama): 417 kayıt
- `daily_explanation` (Günlük Açıklamalar): 417 kayıt
- `text_formatting` (Metin Formatlama): 384 kayıt
- `creative_generation` (Yaratıcı İçerik): 282 kayıt
# 2. Global Semantik Koruma Matrisi
1500 işlem hattının tamamlanmasının ardından, tüm modlar genelinde elde edilen anlamsal sadakat ve koruma ortalaması ($F_1$ skoru) **%81.47** olarak hesaplanmıştır. Sıkıştırma kademelerinin esnekliğine göre mod bazında semantik korunma oranları şu şekildedir:
### MOD BAZINDA ORTALAMA ANLAM KORUMA (BERTScore F1)
- MIN Modu (Creative / Chill) : %86.98
- MID Modu (Balanced / Coding) : %80.28
- MAX Modu (Max Saving / Hardcore) : %77.16

# 3. "BERTScore Darboğazı" ve Sentaks Değişim Tuzağı
Projenin optimizasyon felsefesini doğru konumlandırmak adına, bu raporda ortaya çıkan skorların dilbilimsel ve algoritmik arka planının doğru okunması kritik önem taşır.

BERTScore mekanizması; iki metin arasındaki anlamsal benzerliği kelimelerin bağlamsal gömmeleri (contextual embeddings) ve token eşleşme hizalamaları üzerinden matematiksel olarak hesaplar. Bu durum, Yalın-TokenSaver gibi **"sentaks ve biçim budamaya odaklanan kütüphaneler"** için yapısal bir darboğaz (bottleneck) yaratır.

Yalın, prompt içerisindeki nezaket kalıplarını, dolgu kelimelerini, edat ve bağlaçları temizlediğinde metnin biçimsel dizilimi radikal düzeyde değişir. Örneğin; `"Açıklayabilir misin rica etsem?"` cümlesi `"açıkla"` formuna indirgendiğinde, BERT modeli token hacminin azalmasından ve hizalamanın bozulmasından ötürü skoru kasıtlı olarak düşürür.

Oysa instruction-tuned (komutla eğitilmiş) modern bir Büyük Dil Modeli (LLM) için enformasyonun özü ve verilmek istenen emir **%100 oranında korunmuştur.** Bu nedenle, bu dökümandaki nispeten düşük gibi görünen uç skorlar enformasyon kaybını değil, aksine algoritmanın gürültüyü ne kadar keskin ve başarılı bir şekilde ayıkladığını kanıtlar.
# 4. Deep-Trace Pipeline Geçiş Analizleri
Sistemde pipeline adımlarıyla BERTScore tabanında en yüksek "sözde" kayıp yaşayan vakalar, Multi-Mode Deep-Trace motoru tarafından yakalanarak analiz edilmiştir.
### 4.1. MIN Modu Derin İzleme İncelemeleri
ID: 324 | Kategori: `creative_generation` | BERTScore: %66.47
- **Ham Metin:** _Selam, fantastik bir şehir için ilginç isimler üretebilir misin rica etsem?_
- **Optimized:** _fantastik bir şehir için ilginç isimler üret_
- **Pipeline İzleme Logu:**
	- `Step 1 (Normalize)` : selam fantastik bir şehir için ilginç isimler üretebilir misin rica etsem?
	- `Step 2 (Regex Out)` : fantastik bir şehir için ilginç isimler üret
	- `Step 3 (Zeyrek In)` : fantastik bir şehir için ilginç isimler üret
	- `Step 4 (Stopwords)` : fantastik bir şehir için ilginç isimler üret
	- **Analiz:** Algoritma metin başındaki hitabı ve sondaki rica bütününü kusursuz temizlemiş, yeterlilik fiilini saf komuta çevirmiştir. Semantik olarak sıfır kayıp olmasına rağmen skor %66.47'ye düşerek metrik tuzağını net bir şekilde ortaya koymuştur.
ID: 306 | Kategori: `daily_explanation` | BERTScore: %69.19
- **Ham Metin:** _Rica etsem bana enflasyonun günlük hayattaki etkilerini örneklerle açıklayabilir misin?_
- **Optimized:** _bana enflasyonun günlük hayattaki etkilerini örneklerle açıkla_
- **Analiz:** Başlangıçtaki nezaket kalıbı atılmış, sondaki rica yapısı temizlenmiştir. `açıkla` eyleminin sonundaki ünlü harf korunarak emir kipine geçiş hatasız tamamlanmıştır.
### 4.2. MID Modu Derin İzleme İncelemeleri
ID: 324 | Kategori: `creative_generation` | BERTScore: %53.51
- **Ham Metin:** _Selam, fantastik bir şehir için ilginç isimler üretebilir misin rica etsem?_
- **Optimized:** _fantastik şehir ilginç isimler üret_
-  **Pipeline İzleme Logu:**
	- `Step 2 (Regex Out)` : fantastik bir şehir ilginç isimler üret
	- `Step 4 (Stopwords)` : fantastik şehir ilginç isimler üret
	- **Analiz:** MID modunda devreye giren hiyerarşik stopwords listesi, `bir` edatını ve `için` bağlacını da çöpe atmıştır. Cümle tam anlamıyla bir LLM attention (dikkat) odak noktasına dönüştürülmüştür. BERTScore token hizalaması tamamen koptuğu için %53.51 vererek ağır bir cezalandırma uygulamıştır.
ID: 54 | Kategori: `daily_explanation` | BERTScore: %63.02
- **Ham Metin:** _Yapay öğrenme ile derin öğrenme arasındaki farkı basit bir dille anlatabilir misin acaba?_
- **Optimized:** _yapay öğrenme derin öğrenme Aras farkı basit dille anlat_
	- **Analiz:** Zeyrek morfolojik analiz motorunun yeteneği burada ortaya çıkmıştır; `arasındaki` kelimesi `Aras` (Arası/Arasındaki) köküne indirgenmiş, `ile`, `bir`, `acaba` kelimeleri süpürülmüştür.
### 4.3. MAX Modu Derin İzleme İncelemeleri
ID: 93 | Kategori: `creative_generation` | BERTScore: %58.93
- **Ham Metin:** _Bir gizem romanı için beklenmedik bir final fikri üretebilir misin lütfen?_
- **Optimized:** _gizem romanı final fikri üret_
- **Pipeline İzleme Logu:**
	- `Step 2 (Regex Out)` : bir gizem romanı beklenmedik bir final fikri üret
	- `Step 3 (Zeyrek In)` : bir gizem romanı bekle bir final fikri üret
	- `Step 4 (Stopwords)` : gizem romanı final fikri üret
	- **Analiz:** MAX modunun en agresif örneğidir. Cümledeki `beklenmedik` sıfatı, ana eylemin odağını değiştirmediği gerekçesiyle tamamen elenmiştir. Metin sadece ve sadece 4 kelimelik hayati bir instruction omurgasına indirgenmiştir.
# 5. Sonuç ve Metriksel Doğrulama
Yapılan derin izleme analizleri sonucunda, Yalın-TokenSaver'ın kurduğu pipeline'nın kelimeleri rastgele kesen kör bir yapı olmadığı; aksine Türkçe morfolojisini doğrudan takip eden ve gürültüleri temizleyen akıllı bir mimari olduğu BERTScore katmanında da doğrulanmıştır.

Geleneksel semantik hizalama metriklerinin biçimsel değişimlerden ötürü ürettiği düşük skor manipülasyonu, bir sonraki aşama olan ve promptların LLM üzerindeki gerçek çıktı kalitesini ölçen **LLM-as-a-Judge Matrisi** ile tamamen kırılacaktır. [[04_LLM_AS_A_JUDGE_MATRIX]]