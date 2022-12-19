# -------------------------Makine Öğrenmesine Giriş Dersinin PCA(Principal Component Analysis) Konu Sunum Raporu-------------------------

## PCA Nedir?

Temel bileşen analizi veya PCA, büyük bir değişken kümesini, büyük kümedeki bilgilerin çoğunu hala içeren daha küçük bir değişkene dönüştürerek, büyük veri kümelerinin boyutsallığını azaltmak için sıklıkla kullanılan bir boyut indirgeme yöntemidir.

Bir veri setinin değişken sayısını azaltmak, doğal olarak doğruluk pahasına gelir, ancak boyutsallığı azaltmanın püf noktası, basitlik için biraz doğrulukla ticaret yapmaktır. Çünkü daha küçük veri kümelerinin keşfedilmesi ve görselleştirilmesi daha kolaydır ve işlenecek harici değişkenler olmadan makine öğrenimi algoritmaları için verilerin analizini çok daha kolay ve hızlı hale getirir.

Sonuç olarak, özetlemek gerekirse, PCA fikri basittir - mümkün olduğu kadar çok bilgiyi korurken bir veri kümesindeki değişken sayısını azaltmadır.

## PCA Nasıl Çalışır ?

Temel bileşen analizi beş adımda gerçekleştirilebilir. PCA'nın ne yaptığına dair mantıklı açıklamalar sunarak ve bunların nasıl hesaplanacağına odaklanmadan standardizasyon, kovaryans, özvektörler ve özdeğerler gibi matematiksel kavramları basitleştirerek her adımı gözden geçireceğim.

1. Standardize the range of continuous initial variables...Standardization
2. Compute the covariance matrix to identify correlations...Covariance Matrix Computation 
3. Compute the eigenvectors and eigenvalues of the covariance matrix to identify the principal components
4. Create a feature vector to decide which principal components to keep... Feature Vector 
5. Recast the data along the principal components axes 

### STEP 1: STANDARDIZATION “Sürekli ilk değişken aralığını standartlaştırma’’

Bu adımın amacı, sürekli başlangıç değişkenlerinin aralığını, her birinin analize eşit katkıda bulunacağı şekilde standardize etmektir.

Daha spesifik olarak, PCA'dan önce standardizasyon gerçekleştirmenin kritik olmasının nedeni, PCA'nın başlangıç değişkenlerinin varyansları konusunda oldukça hassas olmasıdır. Yani, başlangıç değişkenlerinin aralıkları arasında büyük farklar varsa, daha geniş aralıklara sahip değişkenler, küçük aralıklara sahip olanlara göre baskın olacaktır (örneğin, 0 ile 100 arasında değişen bir değişken, 0 ile 1 arasında değişen bir değişken üzerinde baskın olacaktır), bu da yanlı sonuçlara yol açacaktır. Dolayısıyla verilerin karşılaştırılabilir ölçeklere dönüştürülmesi bu sorunu önleyebilir. 

Matematiksel olarak bu, ortalamayı çıkararak ve her değişkenin her değeri için standart sapmaya bölerek yapılabilir. 
                                  
  ![image](https://user-images.githubusercontent.com/75726215/208484034-61b817ce-7a15-4438-bebb-df03f29aaad1.png)

                                  
Standardizasyon yapıldıktan sonra, tüm değişkenler aynı ölçeğe dönüştürülecektir. 

### STEP 2: COVARIANCE MATRIX COMPUTATION (KOVARYANS MATRİSİ HESAPLAMASI) ” Korelasyonları belirlemek için kovaryans matrisini hesaplama”
           
Bu adımın amacı, girdi veri setindeki değişkenlerin birbirlerine göre ortalamadan nasıl farklılaştığını anlamak, başka bir deyişle aralarında herhangi bir ilişki olup olmadığını görmektir. Çünkü bazen değişkenler, gereksiz bilgiler içerecek şekilde yüksek oranda ilişkilidir. Dolayısıyla, bu korelasyonları belirlemek için kovaryans matrisini hesaplıyoruz. 

Kovaryans matrisi, başlangıç değişkenlerinin tüm olası çiftleriyle ilişkili kovaryansları girdi olarak içeren bir p × p simetrik matristir (burada p, boyutların sayısıdır). Örneğin, x, y ve z 3 değişkenli 3 boyutlu bir veri kümesi için kovaryans matrisi, bunun 3×3 matrisidir: 

  ![image](https://user-images.githubusercontent.com/75726215/208484138-a0c9099c-1b48-479d-b85e-de765c45ae86.png)

                                
Bir değişkenin kendisi ile kovaryansı onun varyansı olduğundan (Cov(a,a)=Var(a)) ana köşegende (Sol üstten sağ alta) aslında her bir ilk değişkenin varyansına sahibiz. Ve kovaryans değişmeli olduğundan (Cov(a,b)=Cov(b,a)) kovaryans matrisinin girişleri ana köşegene göre simetriktir, yani üst ve alt üçgen kısımlar eşittir. 

Matris girdileri olarak sahip olduğumuz kovaryanslar, değişkenler arasındaki korelasyonlar hakkında bize ne anlatıyor? 
Aslında önemli olan kovaryansın işaretidir: 
Pozitif ise: iki değişken birlikte artar veya azalır (korelasyonlu) 
Negatif ise: biri azalırken diğeri artar (Ters korelasyonlu) 

Artık kovaryans matrisinin olası tüm değişken çiftleri arasındaki korelasyonları özetleyen bir tablodan daha fazlası olmadığını bildiğimize göre, bir sonraki adıma geçelim. 

### STEP 3: ANA BİLEŞENLERİ BELİRLEMEK İÇİN KOVARYANS MATRİSİNİN ÖZVEKTORLERİNİ(EIGENVECTORS) VE ÖZDEĞERLERİNİ(EIGENVALUES) HESAPLAMA

Özvektörler ve özdeğerler, verilerin temel bileşenlerini belirlemek için kovaryans matrisinden hesaplamamız gereken lineer cebir kavramlarıdır. Bu kavramların açıklamasına geçmeden önce temel bileşenlerden ne anladığımızı anlayalım. 
Temel bileşenler, başlangıç değişkenlerinin doğrusal kombinasyonları veya karışımları olarak oluşturulan yeni değişkenlerdir. Bu kombinasyonlar, yeni değişkenler (yani temel bileşenler) ilintisiz olacak ve ilk değişkenlerdeki bilgilerin çoğu ilk bileşenlere sıkıştırılacak veya sıkıştırılacak şekilde yapılır. Yani, fikir şu ki, 10 boyutlu veriler size 10 ana bileşen verir, ancak PCA mümkün olan maksimum bilgiyi ilk bileşene, ardından ikinci bileşene maksimum kalan bilgiyi vb. 

   ![image](https://user-images.githubusercontent.com/75726215/208484292-cc1fa7f5-4ce2-4c61-a4c9-e530f39a6157.png)

                                 
Bilgileri temel bileşenlerde bu şekilde düzenlemek, çok fazla bilgi kaybetmeden boyutsallığı azaltmanıza olanak tanır ve bu, düşük bilgi içeren bileşenleri atarak ve kalan bileşenleri yeni değişkenleriniz olarak kabul ederek.

Burada anlaşılması gereken önemli bir şey, temel bileşenlerin daha az yorumlanabilir olduğu ve başlangıç değişkenlerinin doğrusal kombinasyonları olarak oluşturuldukları için herhangi bir gerçek anlamının olmadığıdır.

Geometrik olarak konuşursak, ana bileşenler, verilerin maksimum miktarda varyansı açıklayan yönlerini, yani verilerin çoğunu yakalayan çizgileri temsil eder. Buradaki varyans ve bilgi arasındaki ilişki, bir çizginin taşıdığı varyans ne kadar büyükse, veri noktalarının çizgi boyunca dağılımı o kadar büyük ve bir çizgi boyunca dağılım ne kadar büyükse, o kadar fazla bilgiye sahip olur. Tüm bunları basitçe ifade etmek gerekirse, temel bileşenleri, gözlemler arasındaki farkların daha iyi görülebilmesi için verileri görmek ve değerlendirmek için en iyi açıyı sağlayan yeni eksenler olarak düşünün. 

Temel bileşenlere sahip olduktan sonra, her bileşenin açıkladığı varyans (bilgi) yüzdesini hesaplamak için her bileşenin özdeğerini özdeğerlerin toplamına böleriz. 

  # PCA Temel Bileşenleri Nasıl Oluşturulur?
Veride değişken sayısı kadar temel bileşen olduğu için, temel bileşenler, ilk temel bileşen veri kümesindeki olası en büyük varyansı açıklayacak şekilde oluşturulur. Örneğin veri kümemizin dağılım grafiği aşağıdaki gibi olsun, ilk ana bileşeni tahmin edebilir miyiz? Evet, orijinden geçtiği için yaklaşık olarak mor işaretlerle eşleşen çizgidir ve noktaların izdüşümünün (kırmızı noktalar) en fazla yayıldığı çizgidir. Veya matematiksel olarak konuşursak, varyansı en üst düzeye çıkaran çizgidir (yansıtılan noktalardan (kırmızı noktalar) orijine olan mesafelerin karesinin ortalaması).
                          
   ![image](https://user-images.githubusercontent.com/75726215/208484370-36fb5812-506a-49cb-a428-683b57585d56.png)

İkinci temel bileşen, birinci temel bileşenle ilişkisiz olması (yani ona dik olması) ve bir sonraki en yüksek varyansı açıklaması koşuluyla aynı şekilde hesaplanır.

Bu, orijinal değişken sayısına eşit toplam p ana bileşen hesaplanana kadar devam eder.

Artık temel bileşenlerle ne demek istediğimizi anladığımıza göre, özvektörlere ve özdeğerlere geri dönelim. Onlar hakkında bilmeniz gereken ilk şey, her zaman çift olarak geldikleridir, yani her özvektörün bir özdeğeri vardır. Ve sayıları, verilerin boyutlarının sayısına eşittir. Örneğin, 3 boyutlu bir veri kümesi için 3 değişken vardır, dolayısıyla 3 özdeğere karşılık gelen 3 özvektör vardır.

Daha fazla uzatmadan, yukarıda açıklanan tüm sihrin arkasında özvektörler ve özdeğerler vardır, çünkü Kovaryans matrisinin özvektörleri aslında eksenlerin en çok varyansın (en çok bilginin) olduğu ve Temel Bileşenler dediğimiz yönleridir. Ve özdeğerler, her Temel Bileşende taşınan varyans miktarını veren özvektörlere iliştirilmiş katsayılardır.

Özvektörlerinizi özdeğerlerine göre en yüksekten en düşüğe doğru sıralayarak, temel bileşenleri önem sırasına göre elde edersiniz.

   # Örnek:
Veri setimizin x,y 2 değişkenli 2 boyutlu olduğunu ve kovaryans matrisinin özvektörlerinin ve özdeğerlerinin aşağıdaki gibi olduğunu varsayalım:

   ![image](https://user-images.githubusercontent.com/75726215/208484461-6f604122-c400-4b3b-bbb0-67658d2a6871.png)


Özdeğerleri azalan düzende sıralarsak λ1>λ2 elde ederiz, bu da birinci ana bileşene (PC1) karşılık gelen özvektörün v1 ve ikinci bileşene (PC2) karşılık gelen özvektörün v2 olduğu anlamına gelir.

Temel bileşenlere sahip olduktan sonra, her bileşenin açıkladığı varyans (bilgi) yüzdesini hesaplamak için her bileşenin özdeğerini özdeğerlerin toplamına böleriz. Bunu yukarıdaki örneğe uygularsak, PC1 ve PC2'nin sırasıyla verilerin varyansının %96'sını ve %4'ünü taşıdığını görürüz.

### STEP 4: FEATURE VECTOR (ÖZELLİK VEKTÖR) ” HANGİ ANA BİLEŞENLERİN TUTULACAĞINA KARAR VERMEK İÇİN BİR ÖZELLİK VEKTÖRÜ OLUŞTURMA” 

Önceki adımda gördüğümüz gibi, özvektörleri hesaplamak ve bunları özdeğerlerine göre azalan düzende sıralamak, temel bileşenleri önem sırasına göre bulmamızı sağlar. Bu adımda yaptığımız şey, tüm bu bileşenleri tutmayı veya daha az anlamlı olanları (düşük özdeğerleri) atmayı seçip kalanlarla Özellik vektörü dediğimiz bir vektörler matrisi oluşturmayı seçmektir. 

Dolayısıyla, özellik vektörü, tutmaya karar verdiğimiz bileşenlerin özvektörlerini sütunlar halinde içeren bir matristir. Bu, onu boyut indirgemeye yönelik ilk adım yapar, çünkü n'nin yalnızca özvektörlerini (bileşenlerini) tutmayı seçersek, son veri kümesi yalnızca p boyuta sahip olacaktır. 

Aradığınız şeye bağlı olarak tüm bileşenleri saklamayı veya daha az önemli olanları atmayı seçmek size kalmış. Çünkü, verilerinizi boyutsallığı azaltmaya çalışmadan ilişkisiz yeni değişkenler (temel bileşenler) açısından tanımlamak istiyorsanız, daha az önemli bileşenleri dışarıda bırakmanıza gerek yoktur.

  # Örnek:
Önceki adımdaki örneğe devam ederek, v1 ve v2 özvektörlerinin her ikisi ile bir özellik vektörü oluşturabiliriz:
      
   ![image](https://user-images.githubusercontent.com/75726215/208484531-d4ca6808-4de5-415b-8739-1abec9ef1bce.png)

                              
Veya daha az öneme sahip olan v2 özvektörünü atın ve yalnızca v1 ile bir özellik vektörü oluşturun:

   ![image](https://user-images.githubusercontent.com/75726215/208484591-ced65069-9fc4-4203-a5cb-32f91c891df5.png)


Özvektör v2'nin atılması, boyutsallığı 1 azaltacak ve sonuç olarak nihai veri setinde bilgi kaybına neden olacaktır. Ancak v2'nin bilgilerin yalnızca %4'ünü taşıdığı göz önüne alındığında, kayıp bu nedenle önemli olmayacak ve v1 tarafından taşınan bilgilerin %96'sına sahip olacağız.
Bu nedenle, örnekte gördüğümüz gibi, aradığınız şeye bağlı olarak tüm bileşenleri saklamayı veya daha az önemli olanları atmayı seçmek size kalmış. Çünkü, verilerinizi boyutsallığı azaltmaya çalışmadan ilişkisiz yeni değişkenler (temel bileşenler) açısından tanımlamak istiyorsanız, daha az önemli bileşenleri dışarıda bırakmanıza gerek yoktur.


### LAST STEP: VERİLERİ ANA BİLEŞENLER EKSENLERİ BOYUNCA YENİDEN DÖKÜMLEYİN

Önceki adımlarda standardizasyon dışında veriler üzerinde herhangi bir değişiklik yapmıyorsunuz, sadece ana bileşenleri seçip özellik vektörünü oluşturuyorsunuz, ancak girdi veri seti her zaman orijinal eksenler cinsinden (yani, ilk değişkenler). 

Son adım olan bu adımda amaç, kovaryans matrisinin özvektörleri kullanılarak oluşturulan özellik vektörünü kullanmak, verileri orijinal eksenlerden temel bileşenler tarafından temsil edilenlere yeniden yönlendirmek (bu nedenle Temel Bileşenler Analizi adıdır). ). Bu, orijinal veri setinin devriğini özellik vektörünün devriğiyle çarparak yapılabilir. 

   ![image](https://user-images.githubusercontent.com/75726215/208484659-d632deb5-9c8e-4e48-8a52-cea564e9a6b7.png)





## PCA YAPARKEN NELERE DİKKAT ETMELİYİZ? 
İlk olarak tüm değişkenlerimizin nümerik olduğundan emin olmalıyız. Eğer veri setinde kategorik değişkenler varsa önce bunları dummy değişkene çevirip analize öyle devam edilmelidir. 

İkinci olarak dikkat etmemiz gereken nokta PCA nın hangi veri seti üzerinde kullanılacağıdır. Örneğin elimizdeki veri seti insan davranışlarını içeren bir veriyse bazı durumlarda kullanmak doğru olmayabilir. Bu aşamada veri setimizi iyi tanımalıyız. 

Son olarak değişkenlerimizin normal dağılımdan geldiğine emin olmadığımız durumlarda PCA dan önce değişkenlerimize normalleştirme işlemi uygulamamız daha doğru sonuçlar almamızı sağlayacaktır. 

NOT: Analizlerimizin sonucunda değişkenlerimizi de belirterek rapor yazmamız ya da yorum yapmamız gerekiyorsa PCA kullanmamız doğru olmaz çünkü PCA uygularken var olan değişkenlerimizden farklı bir düzlemde birbirine dik yeni bileşenler oluşturulduğundan bu bileşenler hakkında bilgi veremeyiz. 







                                
## Kaynakça:

https://www.cs.toronto.edu/~rgrosse/courses/csc411_f18/slides/lec12-slides.pdf

https://www.cs.cmu.edu/~mgormley/courses/10701-f16/slides/lecture14-pca.pdf

[Lindsay I. Smith]: A tutorial on Principal Component Analysis

















