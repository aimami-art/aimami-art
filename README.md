ML with Stellar Classification Dataset - SDSS17

PROJENİN AMACI:

Astronomi'de yıldız sınıflandırması, yıldızların spektral özelliklerine dayanarak sınıflandırılmasıdır. 
Galaksilerin, kuasarların ve yıldızların sınıflandırma şeması, astronomideki en temel sınıflandırmalardan biridir. 
Yıldızların erken kataloglanması ve gökyüzündeki dağılımları, onların kendi galaksimizi oluşturduğunu anlamamıza yol açmıştır ve 
Andromeda'nın kendi galaksimizden ayrı bir galaksi olduğu ayırt edildikten sonra, daha güçlü teleskoplar yapıldıkça sayısız galaksi incelenmeye başlanmıştır. 
Bu veri seti, yıldızları, galaksileri ve kuasarları spektral özelliklerine göre sınıflandırmayı amaçlamaktadır.

VERİ SETİ:

Veri, SDSS (Sloan Dijital Gökyüzü Taraması) tarafından alınan 100.000 uzay gözleminden oluşmaktadır. 
Her gözlem, 17 özellik sütunu ve bir sınıf sütunu ile tanımlanır; bu sınıf sütunu, gözlemin bir yıldız, galaksi veya kuasar olduğunu belirtir.

obj_ID = Nesne Tanımlayıcısı, CAS tarafından kullanılan görüntü kataloğundaki nesneyi tanımlayan benzersiz değer
alpha = Sağ Açıklık açısı (J2000 döneminde)
delta = Dik açıklık açısı (J2000 döneminde)
u = Fotometrik sistemde ultraviyole filtre
g = Fotometrik sistemde yeşil filtre
r = Fotometrik sistemde kırmızı filtre
i = Fotometrik sistemde yakın kızılötesi filtre
z = Fotometrik sistemde kızılötesi filtre
run_ID = Belirli taramayı tanımlamak için kullanılan tarama numarası
rereun_ID = Görüntünün nasıl işlendiğini belirtmek için kullanılan tekrar numarası
cam_col = Tarama sırasında kameranın tarama çizgisini belirlemek için kullanılan kamera sütunu
field_ID = Her alanı tanımlamak için kullanılan alan numarası
spec_obj_ID = Optik spektroskopik nesneler için kullanılan benzersiz ID (bu, aynı spec_obj_ID'ye sahip iki farklı gözlemin aynı sonuç sınıfını paylaşması gerektiği anlamına gelir)
class = Nesne sınıfı (galaksi, yıldız veya kuasar nesnesi)
redshift = Dalga boyundaki artışa dayalı kırmızıya kayma değeri
plate = SDSS'teki her plakayı tanımlayan plaka ID'si
MJD = SDSS verisinin ne zaman alındığını belirtmek için kullanılan Modifiye Edilmiş Julian Tarihi
fiber_ID = Her gözlemdeki odak düzlemine ışık gönderen fiberi tanımlayan fiber ID'si

MODEL SEÇİMİ:

Veriyi başarıyla yükledim ve her sütunun eksik olmayan veri sayısını bir çubuk grafikle görselleştirdim. Bu, veri setinin tamamlanmışlık durumu hakkında bilgi veriyor.
Veri seti, dataset.info() ile gösterildiği gibi 100.000 satır ve 18 sütundan oluşuyor ve eksik veri yok.

Ön işleme (Preprocessing)

Python'un ayrılmış anahtar kelimeleriyle çakışmayı önlemek için class sütununu Class olarak yeniden adlandırdım.
Kimlik (ID) ve anlamlı olmayan özellikler gibi bazı sütunlar çıkardım çünkü bunlar tahmin modeli için katkı sağlamıyor. Analiz için alpha, delta, u, g, vb. gibi ilgili özelliklere odaklandım.
Tüm özelliklerin ortalamasının 0 ve varyansının 1 olmasını sağlamak için StandardScaler kullanarak veri standartlaştırdım. Bu, birçok makine öğrenimi algoritması için kritik öneme sahip.
LabelEncoder, Class sütununu sayısal etiketlere dönüştürmek için kullandım.

Korelasyon Analizi (Correlation Analysis)

Özellikler arasındaki doğrusal ilişkiyi anlamak için Pearson korelasyon matrisini hesapladım. Bu, çoklu doğrusal ilişkiyi tespit etmeye ve en iyi özellikleri seçmeye yardımcı olur.

Denetimli Öğrenme (Supervised Learning - Random Forest)

Veri setini %80 eğitim ve %20 test olarak böldüm.
100 tahmincili bir RandomForestClassifier uyguladım ve modeli doğruluk (accuracy), kesinlik (precision), geri çağırma (recall), F1 skoru gibi metriklerle değerlendirdim.
Modelin doğruluğu %98 oldu, ki bu oldukça etkileyici.Diğer gözetimli öğrenmeleri de uyguladım fakat en yüksek değer randomforestta çıktı.

unsupervised da ise başlangıç metrik değerlerim düşük çıktı fakat daha sonrasında elbow yöntemi ile küme sayımı belirledim zaten daha öncesinde verilerimi scale etmiş ve azaltmıştım.
İyi değerler yakalayamasam da elbow yöntemi ve scale ile daha doğru sonuçlar elde ettim.


https://www.kaggle.com/code/muhammeduyur/sc-ml
