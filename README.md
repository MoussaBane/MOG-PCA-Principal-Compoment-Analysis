# Makine Öğrenmesine Giriş Dersinin PCA(Principal Component Analysis) Konu Sunumu

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


