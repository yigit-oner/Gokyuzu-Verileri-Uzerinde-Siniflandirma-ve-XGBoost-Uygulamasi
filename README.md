# Gokyuzu-Verileri-Uzerinde-Siniflandirma-ve-XGBoost-Uygulamasi
Sloan Dijital Gökyüzü Anketi (SDSS) verilerini kullanarak yıldızlar, galaksiler ve kuasarlar (QSO) arasında ayrım yapan bir Makine Öğrenmesi Sınıflandırma projesidir. Yüksek korelasyonlu özniteliklerin çıkarılması, veri ölçeklendirme ve XGBoost modelinin optimizasyonu yapılmıştır.

Bu projede, gök cisimlerinin temel özelliklerini (konum, parlaklık, kırmızıya kayma) içeren Sloan Dijital Gökyüzü Anketi (SDSS) verilerini kullandım. Ana amacım, bu veriyi kullanarak bir gök cisminin Yıldız (Star), Galaksi (Galaxy) veya Kuasar (QSO) olduğunu yüksek doğrulukla tahmin edebilen bir sınıflandırma modeli geliştirmekti. Proje, hem veri hazırlık hem de model optimizasyon aşamalarını içeriyor.

1. Veri Ön İşleme ve Keşifçi Analiz (EDA)
Gereksiz Özniteliklerin Çıkarılması: Analiz veya modelleme için gereksiz olan, ya da veri setinde tekil tanımlayıcı (ID) görevi gören objid, specobjid, rerun, camcol, field, run gibi sütunları veri setinden çıkardım.

Kategorik Değişken Dönüşümü: Hedef değişkenim olan class (sınıf) sütununu, makine öğrenmesi algoritmalarının anlayacağı hale getirmek için Label Encoding kullanarak sayısal değerlere (0, 1, 2) dönüştürdüm.

Keşifçi Görselleştirme: redshift (kırmızıya kayma) değişkeninin her bir sınıf (Star, Galaxy, QSO) içindeki dağılımlarını histogramlar ile görselleştirdim. Bu, sınıflar arasındaki ayrışmayı anlamam için kritikti.

2. Yüksek Korelasyonlu Özniteliklerin Çıkarılması
Sorun: Veri setindeki birçok parlaklık filtresi (r, i, z gibi) birbiriyle çok yüksek korelasyon gösteriyordu. Bu durum, modelin performansını düşürebilir ve yorumlanabilirliğini zorlaştırabilirdi (Çoklu Doğrusallık sorunu).

Çözüm: %95 korelasyon eşiği (threshold) belirleyen özel bir Python fonksiyonu (drop_high_corr) yazdım. Bu fonksiyon, üst üçgen matrisini kontrol ederek belirlenen eşiğin üzerindeki değişkenleri tespit etti.

Sonuç: Bu temizleme adımıyla r, i, z ve mjd gibi yüksek korelasyonlu sütunları veri setinden güvenli bir şekilde çıkardım.

3. Model Eğitimi ve Değerlendirme
Veri Bölme: Veri setini eğitim ve test (test_size = 0.33) setlerine ayırdım.

Ölçeklendirme (Scaling): Tüm özniteliklerin aynı etkiye sahip olması için, modeli eğitmeden önce StandardScaler kullanarak veriyi standartlaştırdım (normalleştirdim).

Model Seçimi: Güçlü performansı ve karmaşık ilişkileri yakalama yeteneği nedeniyle XGBoost Classifier modelini seçtim.

İlk Sonuçlar: Modelin ilk eğitiminden sonra %99'un üzerinde bir test doğruluğu (accuracy_score) elde ettim. Başarıyı Karmaşıklık Matrisi (Confusion Matrix) ve Sınıflandırma Raporu (Classification Report) ile detaylı olarak değerlendirdim.

4. Model Optimizasyonu (Grid Search)
Modelin en iyi performansı vermesi için hiperparametre optimizasyonuna geçtim.

GridSearchCV metodunu kullanarak n_estimators, learning_rate, max_depth ve colsample_bytree gibi kritik XGBoost parametreleri için geniş bir arama alanı tanımladım.

Bu optimizasyon sürecini çalıştırarak, modelin test doğruluğunu daha da artıracak en iyi parametre kombinasyonunu bulmayı hedefledim.

Kullanılan Teknolojiler: Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn (LabelEncoder, train_test_split, StandardScaler, accuracy_score, confusion_matrix, classification_report, GridSearchCV), XGBoost.
