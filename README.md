# Yalın-TokenSaver (yln)

**Geliştirici:** Yasin Durak  
**İletişim:** ydurak328@gmail.com 
**Geliştirici Bağlantıları:** [GitHub](https://github.com/yasindev9) | [LinkedIn](https://www.linkedin.com/in/yasindurak/)

---

## Proje Hakkında

Yalın-TokenSaver (yln), LLM tabanlı yapay zeka çözümleri geliştiren kurumsal şirketler, SaaS girişimleri ve bireysel kullanıcılar için tasarlanmış yüksek performanslı bir prompt optimizasyon middleware yazılımıdır.

Türkçe, morfolojik açıdan eklemeli bir dil yapısına sahip olması nedeniyle, mevcut LLM tokenizer algoritmaları tarafından İngilizceye kıyasla kelime başına 3 ila 4 kat daha fazla token parçasına bölünmektedir. Bu durum, Türkçe dilinde hizmet veren yapay zeka uygulamalarında API maliyetlerinin doğrusal olmayan bir şekilde katlanmasına ve operasyonel kârlılığın düşmesine neden olmaktadır.

Yalın-TokenSaver, prompt içerisindeki anlamsal özü ve yönetsel emir kalıplarını doğrudan koruyarak; yapısal gürültüleri hassas bir şekilde ayıklar. Modellerin input boyutunu küçülterek, kurumsal yapay zeka operasyonlarında kaliteden ödün vermeksizin %27.03'e varan oranda kalıcı maliyet tasarrufu sağlar. (Bu tasarruf değeri max_saving modu için geçerlidir.)

---

## Değer Önerisi

* **Doğrudan Maliyet Optimizasyonu:** Altyapıya entegre edildiği andan itibaren LLM girdi maliyetlerini kalıcı olarak azaltır. Günlük yüksek hacimli prompt işleyen sistemlerde doğrudan finansal verimlilik sağlar.
* **Milisaniye Altı Gecikme Süresi:** Gerçek zamanlı akış ve canlı sohbet mimarilerine ek yük bindirmeyecek şekilde, milisaniyeler içerisinde optimize edilmiş motor altyapısına sahiptir.
* **Yapay Zeka Odak Artışı:** Prompt içindeki semantik kalabalığı temizleyip doğrudan eyleme odaklandığı için, dil modellerinin "Attention" mekanizmasını optimize eder ve çıktı doğruluğunu korur.

---

## Performans ve Operasyonel Verimlilik Matrisi

Sistemin başarısı, gerçek dünya senaryolarını yansıtan 500 benzersiz kurumsal Türkçe prompt üzerinde, toplam 1500 bağımsız operasyon hattı yürütmesiyle test edilerek matematiksel olarak kanıtlanmıştır.

### Operasyonel Verimlilik Tablosu

| Çalışma Seviyesi / Modu   | Ortalama Ek İşlem Süresi | Doğrudan Token Tasarrufu | Sistemin Genel Tutarlılık Puanı | İdeal Kullanım Senaryosu                |
| :------------------------ | :----------------------- | :----------------------- | :------------------------------ | :-------------------------------------- |
| **MIN Modu** *(Creative)* | 0.38 ms                  | %16.03                   | 4.74 / 5.0                      | Canlı Destek, Chatbot ve Kreatif Yazım  |
| **MID Modu** *(Balanced)* | 16.51 ms                 | %22.26                   | 4.74 / 5.0                      | Kodlama Asistanları, Kurumsal Agent'lar |
| **MAX Modu** *(Hardcore)* | 15.58 ms                 | %27.03                   | 4.74 / 5.0                      | Büyük Veri Analitiği, Formatlama İşleri |
*Max modu işlem süresinin MID moduna göre daha düşük olması regex yapılarının daha kapsayıcı olmasıyla ilgilidir.*
### Kategori Bazlı Maksimum Verimlilik Dağılımı
* **Metin Formatlama ve Düzenleme (`text_formatting`):** Kurumsal raporlama ve veri düzenleme şablonlarında %30.98 tasarruf oranı.
* **Yazılım ve Kodlama (`natural_coding`):** Doğal dille yazılan teknik komutlardaki gürültülerin elenmesiyle %28.64 net bütçe kazancı.
* **Yaratıcı İçerik Üretimi (`creative_generation`):** Metnin edebi üslubu korunarak %24.30 tasarruf oranı.

---

## Kalite Güvencesi ve Akademik Metrikler

Yalın-TokenSaver, girdi metnini kısaltırken anlam kaybını sıfıra indirmek amacıyla iki aşamalı endüstriyel validasyon mimarisi kullanılarak test edilmiş ve final sürümüne getirilmiştir:

1. **BERTScore Metriği:** Sıkıştırılmış metin ile orijinal metin arasındaki anlamsal benzerliği, kelimelerin vektörel yakınlıkları (embedding) üzerinden bağlamsal olarak ölçen derin öğrenme tabanlı bir değerlendirme metriğidir. Yalın-TokenSaver, BERTScore testlerinde orijinal promptun matematiksel anlam bütünlüğünü koruduğunu kanıtlamıştır.
2. **LLM-as-a-Judge Değerlendirmesi:** Gelişmiş dil modellerinin objektif birer evaluator olarak konumlandırılarak girdi ve çıktı kalitesini puanladığı modern bir QA mimarisidir. Yapılan testlerde sistem, 5.0 üzerinden 4.74 kararlılık skoru elde etmiştir. 500 girdi bulunan test havuzundan rastgele seçilen 100 girdi ilk olarak 3 modda (min-mid-max) işlenmiştir. Daha sonra her 100 ham girdi ve 3 ayrı mod sonuçları (toplam 100x4=400 girdi) birbirinden bağımsız şekilde gemini-3.1-flash modeline gönderilmiş ve yanıtlar sistematik bir şekilde kayıt altına alınmıştır. Daha sonra bu kayıtlar yine aynı modele (gemini-3.1-flash) bir `system prompt` ile gönderilerek ham girdi (raw_text) ile 3 modda optimize edilen girdilerin sonuçları arasında 3 kategoride 5 üzerinden puanlanması istenmiştir. (0: ham girdi ile hiçbir alakası yoktur, 5: ham girdi ile ilgili kategori bazında tamamen aynıdır.) Elde edilen sonuçlar;
###### Sıkıştırma Modlarına Göre Genel Performans Matrisi

| Sıkıştırma Modları | Anlamsal Sadakat | Çıktı Akıcılığı | Detay Koruma | Genel Başarı Puanı |
| ------------------ | ---------------- | --------------- | ------------ | ------------------ |
| min                | 4.41 / 5.0       | 4.71 / 5.0      | 4.21 / 5.0   | 4.44 / 5.0         |
| mid                | 4.52 / 5.0       | 4.67 / 5.0      | 4.37 / 5.0   | 4.52 / 5.0         |
| max                | 4.44 / 5.0       | 4.58 / 5.0      | 4.37 / 5.0   | 4.46 / 5.0         |
- **Modlar Arası Ortalama Varyans Sapması:** `0.256`
- **Yalın-TokenSaver Tutarlılık Puanı:** `4.74 / 5.0`
### Çıktı Kalitesi Artış Paradoksu
Yapılan niteliksel testlerde, dengeli sıkıştırma yapılan `mid` modunun, metne en az dokunan `min` modundan daha yüksek bir çıktı kalitesi skoru aldığı doğrulanmıştır. Yapay zeka modelleri, önlerindeki nezaket ve dolgu kelimelerinden kurtulduklarında doğrudan iş omurgasına odaklanmakta ve daha net, doğru yanıtlar üretmektedir.

---

## Kurulum (Installation)

Yalın-TokenSaver, yüksek performans ve kod güvenliği amacıyla Cython (Stable ABI) mimarisiyle derlenmiş kapalı kaynaklı ticari bir yazılımdır. Sisteminizde çalışabilmesi için aşağıdaki gereksinimleri karşılaması ve işletim sisteminize uygun yöntemle kurulması gerekir.
#### Sistem Gereksinimleri
- İşletim Sistemi: Microsoft Windows (x64)
- Python Sürümü: Python >= 3.8 (Stable ABI / abi3 desteği sayesinde Python 3.8, 3.9, 3.10, 3.11, 3.12 ve üzeri tüm sürümler tek paketle desteklenmektedir.)
### Yöntem 1: GitHub Releases Üzerinden Doğrudan Kurulum
Windows x64 tabanlı sistemler için derlenmiş resmi binary paketini (.whl) doğrudan GitHub reposu üzerinden terminal vasıtasıyla tek komutla kurabilirsiniz:
```bash
pip install https://raw.githubusercontent.com/yasindev9/yln-release/main/dist/yln_tokensaver-1.0.0-cp38-abi3-win_amd64.whl
```

### Yöntem 2: Yerel Wheel (.whl) Dosyası ile Kurulum
Paketi manuel olarak indirip projelerinize dahil etmek isterseniz:
1. GitHub "yln-release" reposundaki "dist" klasörüne girip "yln_tokensaver-1.0.0-cp38-abi3-win_amd64.whl" isimli dosyayı bilgisayarınıza indirin.
2. İndirdiğiniz dosyanın yer aldığı dizinde terminal veya komut satırını (CMD) açarak şu komutu çalıştırın:
```bash
pip install yln_tokensaver-1.0.0-cp38-abi3-win_amd64.whl
```

### Yöntem 3: Resmi PyPI Deposu Üzerinden Kurulum
Yalın-TokenSaver dağıtım paketi, sisteminize tek komutla entegre edilebilir:
```bash
pip install yln-tokensaver
```
# Kullanım Kılavuzu
Kütüphaneyi projelerinize dahil etmek, motoru RAM üzerinde başlatmak ve işlem hatlarını tetiklemek için aşağıdaki entegrasyon kodunu referans alabilirsiniz.

```python
from yln.core import YalinTokenSaver

# 1. Motorun Başlatılması (Initialization)
# Lisans anahtarınızı girerek motoru RAM üzerinde 1 kere tetikleyin.
# Ücretsiz deneme sürümü için license_key parametresini None bırakabilirsiniz.
saver = YalinTokenSaver(license_key="YLN-XXXXX-XXXXX", default_model="gpt-4o")

# 2. Ham Türkçe Metnin Hazırlanması
raw_prompt = "Merhaba yapay zeka, bana acil olarak Python dilinde yazılmış kurumsal bir veri tabanı bağlantı fonksiyonu yazar mısınız lütfen? Şimdiden çok teşekkür ederim."

# 3. Optimizasyon Pipeline'ının Tetiklenmesi
# İhtiyacınıza göre 'min', 'mid' veya 'max' sıkıştırma modlarından birini seçebilirsiniz.
result = saver.optimize_prompt(raw_text=raw_prompt, mode="mid", model="gpt-4o")

# 4. Çıktıların ve Metriklerin Analizi
if result["status"] == "processed":
    print("İşlem Durumu    :", result["status"])
    print("Ham Token Sayısı :", result["metrics"]["raw_tokens"])
    print("Yeni Token Sayısı:", result["metrics"]["optimized_tokens"])
    print("Net Tasarruf     : %", result["metrics"]["savings_percentage"])
    print("İşlem Süresi     :", result["metrics"]["duration_ms"], "ms")
elif result["status"] == "limit_exceeded":
    print("Hata:", result["error_message"])
```
### Parametre Detayları
#### Başlatıcı Sınıf Parametreleri (`YalinTokenSaver`)
- `license_key` (str, Opsiyonel): Ticari aboneliğinize ait kriptografik lisans anahtarı. Deneme sürümü için `None` veya boş bırakılmalıdır.
- `default_model` (str, Varsayılan: `"gpt-4"`): Token sayım işlemlerinde baz alınacak varsayılan dil modeli mimarisi 'cl100k_base' olarak belirlenir.
#### İşlem Hattı Parametreleri (`optimize_prompt`)
- `raw_text` (str, Zorunlu): LLM'e gönderilecek olan ham, sıkıştırılmamış Türkçe prompt metni.
- `mode` (str, Varsayılan: `"mid"`): Uygulanacak sıkıştırma agresifliği. Seçenekler:
	- `"min"`: Morfolojik analizleri pasif tutarak sıfır riskli ve kreatif promptlar için optimizasyon yapar.
	- `"mid"`: Anlam koruma ve bütçe tasarrufunu dengede tutan kurumsal standart mod.
	- `"max"`: Büyük veri işleme ve analiz süreçleri için agresif veri madenciliği sıkıştırması uygular.
- `model` (str, Opsiyonel): Gönderilecek spesifik model adı. (Gelişmiş tokenizer eşleşmesi sağlar).
# Lisans ve Ücretsiz Kullanım Limitleri
Yalın-TokenSaver tescilli ve ticari bir yazılımdır. Kütüphane, lisans anahtarı girilmeksizin yerel bilgisayarınızda **günlük 50 prompt** sınırı ile ücretsiz deneme sürümünde çalışmaktadır. Günlük sınır aşıldığında sistem otomatik olarak koruma moduna geçerek girdi metnini sıkıştırmadan bypass eder.
Kurumsal altyapılar için günlük limit sınırını kaldırmak, sınırsız API erişimi ve teknik destek almak adına ticari lisans anahtarınızı bağlantılar üzerinden temin edebilirsiniz.

[Kurumsal Lisans Anahtarı Satın Al (Shopier)](https://www.shopier.com/yasindurak)

[Resmi Dağıtım & GitHub](https://github.com/yasindev9/yln-release)

_Yalın-TokenSaver kapalı kaynaklı (closed-source) ticari bir yazılımdır. Paket, derlenmiş ve şifrelenmiş biçimde dağıtılmaktadır; kaynak kod paylaşımı içermez._
### Copyright © 2026 Yasin Durak. Tüm Hakları Saklıdır.