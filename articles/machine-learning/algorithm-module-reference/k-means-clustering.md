---
title: 'K-ortalamaları kümeleme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Kümeleme modeli eğitmek için Azure Machine Learning hizmetinde K-ortalamaları kümeleme modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7e9b7c8f2cf86245322679198b84b50d2c5edce8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65464662"
---
# <a name="module-k-means-clustering"></a>Modül: K Ortalamaları Kümeleme

Bu makalede nasıl kullanılacağını *K-ortalamaları Kümeleme* deneyimsiz K-ortalamaları kümeleme modeli oluşturmak için Azure Machine Learning Studio'da modülü. 
 
K-ortalamaları, en iyi bilinen bir basit birini ve *Denetimsiz* öğrenimi algoritmaları. Algoritma makine görevler gibi çeşitli için kullanabilirsiniz: 

* [Olağan dışı veri algılama](https://msdn.microsoft.com/magazine/jj891054.aspx).
* Kümeleme metin belgesi.
* Başka bir sınıflandırma veya regresyon yöntemler kullanmadan önce veri kümeleri çözümleniyor. 

Bir kümeleme modeli oluşturmak için:

* Bu modül için denemenizi ekleyin.
* Bir veri kümesine bağlanın.
* Beklediğiniz, kümeleri oluşturmak için uzaklık ölçüm ve VS küme sayısı gibi parametreleri ayarlayın. 
  
Modül hiperparametreleri yapılandırdıktan sonra deneyimsiz modele bağlanmak [kümeleme modeli eğitme](train-clustering-model.md). K-ortalamaları algoritması Denetimsiz öğrenme yöntemi olduğundan, bir etiket sütun isteğe bağlıdır. 

+ Verilerinizi bir etiket içeriyorsa, kümelerin seçimi Kılavuzu ve model iyileştirmek için etiket değerlerini kullanabilirsiniz. 

+ Verilerinizde herhangi bir etiket varsa, algoritma mümkün kategoriler, yalnızca verileri temel alan temsil eden kümeleri oluşturur.  

##  <a name="understand-k-means-clustering"></a>K-ortalamaları kümeleme anlama
 
Genel olarak, yinelemeli teknikler kullanır grubu durumlarda bir veri kümesi için benzer özelliklere sahip kümeler halinde kümelemeyi. Koruyabilmesini verilerdeki anomaliler tanımlama, veri keşfetme ve sonunda tahminleri yapmak için kullanışlıdır. Modelleri kümeleme mantıksal gözatma veya basit gözlem tarafından türettiğiniz değil DataSet ilişkileri belirlemenize de yardımcı olabilir. Bu nedenlerle, kümeleme sık makine öğrenimi görevlerini erken aşamalarında verileri araştırmak ve beklenmeyen bağıntılar bulmak için kullanılır.  
  
 K-ortalamaları yöntemi kullanarak kümeleme modeli yapılandırırken, bir hedef numarası belirtmelisiniz *k* sayısını belirtir *centroids* modelde istiyor. Kütle merkezi temsilcisi her kümenin bir noktadır. K-ortalamaları algoritması küme içinde kareler toplamı en aza indirerek gelen her veri noktası kümelerden birine atar. 
 
Eğitim verileri işlediğinde, K-ortalamaları algoritması rastgele seçilen centroids başlangıç kümesi ile başlar. Centroids noktaları kümeler için başlangıç olarak görev yapar ve bunlar Lloyd'ın algoritması çalıştırmalarınızı konumlarına iyileştirmek için geçerlidir. K-ortalamaları algoritması oluşturmak ve bir veya daha fazla bu koşulları karşıladığında kümeleri iyileştirme durdurur:  
  
-   Artık tek tek noktaları için küme atamaları değiştirmek ve algoritma hiper yakınsanmış bir çözüm üzerinde centroids Sabitle.  
  
-   Algoritmanın, yinelemeleri belirtilen sayıda çalışan tamamlandı.  
  
 Eğitim aşamasını tamamladıktan sonra kullandığınız [Ata veri kümelerine](assign-data-to-clusters.md) modülü birinden K-ortalamaları algoritması kullanılarak bulunan yeni çalışmaları atayın. Küme atama, yeni durum her kümenin kütle merkezi arasındaki mesafeyi hesaplayarak gerçekleştirin. Her yeni örneği yakın kütle merkezi kümeyle atanır.  

## <a name="configure-the-k-means-clustering-module"></a>K-ortalamaları kümeleme modülünü Yapılandır
  
1.  Ekleme **K-ortalamaları Kümeleme** denemenizi modülü.  
  
2.  Eğitim modeli nasıl istediğinizi belirtmek için seçin **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Kümeleme modeli kullanmak istediğiniz tam parametreleri biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.  
  
3.  İçin **numarası, Centroids**, algoritması şununla istediğiniz küme sayısını yazın.  
  
     Model, tam olarak bu küme sayısını üretmek için garanti yoktur. Algoritma, veri noktalarının sayısı ile başlar ve en uygun yapılandırma bulmak için yinelenir.  
  
4.  Özellikleri **başlatma** ilk küme yapılandırmasına tanımlamak için kullanılan algoritma belirtmek için kullanılır.  
  
    -   **İlk N**: Bazı ilk veri noktası sayısı seçilen veri kümesinden ve ilk şekilde kullanılır. 
    
         Bu yöntem ayrıca çağrılır *Forgy yöntemi*.  
  
    -   **Rastgele**: Algoritma, rastgele bir veri noktasının bir kümeye yerleştirir ve ardından, kümenin kütle merkezi rastgele olarak ilk ortalama noktalarına atanan hesaplar. 

         Bu yöntem ayrıca çağrılır *rastgele bölüm* yöntemi.  
  
    -   **K-anlamına gelir ++** : Bu kümeleri başlatma için varsayılan yöntemdir.  
  
         **K-anlamına gelir ++** algoritması önerilen 2007'de David Arthur ve Sergei Vassilvitskii düşük standart K-ortalamaları algoritma tarafından kümeleme önlemek için. **K-anlamına gelir ++** ilk küme merkezleri seçmek için farklı bir yöntem kullanarak, standart K-ortalamaları artırır.  
  
    
5.  İçin **rastgele sayı doldurma**, isteğe bağlı olarak küme başlatma için temel kullanılacak bir değer yazın. Bu değer, küme seçimi önemli bir etkiye sahip olabilir.  
  
6.  İçin **ölçüm**, küme vektör arasında ya da yeni veri noktaları rastgele seçilen kütle merkezi arasındaki uzaklığı ölçmek amacıyla kullanılacak işlevi seçin. Azure Machine Learning aşağıdaki küme uzaklık ölçümleri destekler:  
  
    -   **Euclidean**: Euclidean uzaklık K-ortalamaları kümeleme için küme dağılım bir ölçü yaygın olarak kullanılır. Bu ölçüm tercih edilen çünkü centroids noktaları arasındaki ortalama uzaklığı en aza indirir.
  
7.  İçin **yinelemeler**, algoritma centroids seçimini sonlandırır önce eğitim verileri yinelemek sayısını yazın.  
  
     Bu parametre için Bakiye doğruluğu eğitim zamana karşı ayarlayabilirsiniz.  
  
8.  İçin **Ata etiket modu**, bir etiket sütun kümesinde varsa nasıl işleneceğini belirten bir seçenek belirleyin.  
  
     K-ortalamaları kümeleme yöntemi öğrenme Denetimsiz bir makine olduğundan, etiketler isteğe bağlıdır. Ancak, veri kümeniz zaten bir etiket sütun varsa, kümelerin seçimi yol göstermesi için bu değerleri kullanabilirsiniz veya değerleri yoksayılacak belirtebilirsiniz.  
  
    -   **Etiket sütunu yoksayın**: Etiket sütundaki değerleri yok sayılır ve modeli oluşturma kullanılmaz.
  
    -   **Eksik değerleri doldurun**: Etiket sütun değerleri, kümeler oluşturmanıza yardımcı olmak üzere özellikleri kullanılır. Herhangi bir satır etiket eksikse değerin diğer özelliklerini kullanarak imputed.  
  
    -   **En yakın Merkezi'ne gelen üzerine**: Etiket sütun değerleri, geçerli kütle merkezi için en yakın olan noktası etiketini kullanarak, tahmin edilen etiket değerleri ile değiştirilir.  

8.  Seçin **Normalleştir özellikleri** eğitim önce Özellikleri'leri normalleştirmek isterseniz seçeneği.
  
     Eğitim önce bir normalleştirme uygularsanız, veri noktaları için normalleştirilir `[0,1]` MinMaxNormalizer tarafından.

10. Modeli eğitme.  
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**etiketli bir veri kümesi ekleyin ve kullanarak modeli eğitmek [kümeleme modeli eğitme](train-clustering-model.md) modülü.  
  
### <a name="results"></a>Sonuçlar

Modeli eğitmek ve yapılandırma bitirdikten sonra puanları oluşturmak için kullanabileceğiniz bir modeline sahiptir. Ancak, modeli eğitmek için birden çok yol ve sonuçları görüntüleyip için birden çok yolu vardır: 

#### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Model çalışma alanınızdaki bir anlık görüntüsünü yakalama

Kullandıysanız [kümeleme modeli eğitme](train-clustering-model.md) Modülü:

1. Sağ **kümeleme modeli eğitme** modülü.

2. Seçin **Trained modeli**ve ardından **eğitilen modeli kaydedin**.

Kaydedilmiş modeli model kaydettiğiniz anda eğitim verileri temsil eder. Daha sonra deneme kullanılan eğitim verilerini güncelleştirirseniz, kaydedilmiş modeli güncelleştirmez. 

#### <a name="see-the-clustering-result-dataset"></a>Kümeleme sonuç veri kümesini görürsünüz 

Kullandıysanız [kümeleme modeli eğitme](train-clustering-model.md) Modülü:

1. Sağ **kümeleme modeli eğitme** modülü.

2. Seçin **sonuçları veri kümesi**ve ardından **Görselleştir**.

### <a name="tips-for-generating-the-best-clustering-model"></a>Kümeleme modelinin yerini, en iyi oluşturmaya yönelik ipuçları  

İle eşleşmeyebileceği bilinmektedir *dengeli dağıtım* kümeleme sırasında kullanılan işlem modeli önemli ölçüde etkileyebilir. Dengeli dağıtım noktaları ilk yerleşimini olası centroids içine anlamına gelir.
 
Örneğin, veri kümesini birçok aykırı değerleri içeren ve bir aykırı değer kümeleri sağlamak için seçilir, diğer veri noktası da bu kümeyle yerleştirin ve küme tek olabilir. Diğer bir deyişle, yalnızca bir noktasına sahip olabilir.  
  
Çeşitli şekillerde bu sorunu önleyebilirsiniz:  
  
-   Centroids sayısını değiştirmek ve birden çok çekirdek değer deneyin.  
  
-   Ölçüm değişen veya daha fazla yineleme birden çok modeller oluşturun.  
  
Genel olarak, kümeleme modeli, herhangi bir yapılandırma kümeleri yerel olarak en iyi duruma getirilmiş kümesindeki sonuçlanır mümkündür. Diğer bir deyişle, modeli tarafından döndürülen kümeleri kümesini yalnızca geçerli veri noktalarının uygun ve diğer verilerle genelleştirilebilir desteklenmez. Farklı bir başlangıç yapılandırmasını kullanırsanız, K-ortalamaları yöntemi farklı, belki de daha üstün bir yapılandırmada bulabilirsiniz. 
