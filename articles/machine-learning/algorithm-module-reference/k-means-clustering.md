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
ms.openlocfilehash: 29aeb460581655190b7c3f4256647059f4642d3e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029708"
---
# <a name="k-means-clustering"></a>K-ortalamaları kümeleme

Bu makalede nasıl kullanılacağını **K-ortalamaları Kümeleme** deneyimsiz K-ortalamaları kümeleme modeli oluşturmak için Azure Machine Learning Studio'da modülü. 
 
K-ortalamaları, en iyi bilinen bir basit birini ve *Denetimsiz* öğrenme algoritmaları ve makine öğrenimi görevlerini çeşitli için gibi kullanılabilir [anormal veri algılama](https://msdn.microsoft.com/magazine/jj891054.aspx), metnin kümeleme belgeler ve sınıflandırma veya regresyon diğer yöntemleri kullanarak önce bir veri kümesinin analizi. Kümeleme modeli oluşturmak için denemenizi için bu Modülü Ekle, bir veri kümesine bağlanmak ve beklediğiniz, kümeleri oluşturmak için uzaklık ölçüm ve VS küme sayısı gibi parametrelerini ayarlayın. 
  
Modül hiperparametreleri yapılandırdıktan sonra deneyimsiz modele bağlanmak [kümeleme modeli eğitme](train-clustering-model.md) K-ortalamaları algoritması Denetimsiz öğrenme yöntemi olduğundan, bir etiket sütun isteğe bağlıdır. 

+ Verilerinizi bir etiket içeriyorsa, kümelerin seçimi Kılavuzu ve model iyileştirmek için etiket değerlerini kullanabilirsiniz. 

+ Verilerinizde herhangi bir etiket varsa, algoritma mümkün kategoriler, yalnızca verileri temel alan temsil eden kümeleri oluşturur.  
  

  
##  <a name="understanding-k-means-clustering"></a>Anlama k-ortalamaları kümeleme
 
Genel olarak, yinelemeli teknikler kullanır grubu durumlarda bir veri kümesi için benzer özelliklere içeren kümeler halinde kümelemeyi. Koruyabilmesini verilerdeki anomaliler tanımlama, veri keşfetme ve sonunda tahminleri yapmak için kullanışlıdır. Modelleri kümeleme mantıksal gözatma veya basit gözlem tarafından türettiğiniz değil DataSet ilişkileri belirlemenize de yardımcı olabilir. Bu nedenlerle, kümeleme sık makine öğrenimi görevlerini erken aşamalarında verileri araştırmak ve beklenmeyen bağıntılar bulmak için kullanılır.  
  
 K-ortalamaları yöntemini kullanarak bir kümeleme modeli yapılandırırken, bir hedef numarası belirtmelisiniz *k* sayısını gösteren *centroids* modelde istiyor. Kütle merkezi temsilcisi her kümenin bir noktadır. K-ortalamaları algoritması küme içinde kareler toplamı en aza indirerek gelen her veri noktası kümelerden birine atar. 
 
Eğitim verileri işlerken K-ortalamaları algoritması başlangıç noktaları her küme için başlangıç olarak kullanılırlar ve yinelemeli olarak centroids konumlarını iyileştirmek için Lloyd'ın algoritması uygular, centroids seçilen randomnly kümesi ile başlar. K-ortalamaları algoritması oluşturmak ve bir veya daha fazla bu koşulları karşıladığında kümeleri iyileştirme durdurur:  
  
-   Artık tek tek noktaları kümesi atamaları değiştirmek ve algoritma hiper yakınsanmış bir çözüm üzerinde centroids Sabitle.  
  
-   Algoritmanın, yinelemeleri belirtilen sayıda çalışan tamamlandı.  
  
 Kullandığınız eğitim aşaması tamamlandıktan sonra [Ata veri kümelerine](assign-data-to-clusters.md) birinden k-ortalamaları algoritma tarafından bulunan yeni çalışmaları atamak için modülü. Küme atama, yeni durum her kümenin kütle merkezi arasındaki mesafeyi hesaplayarak gerçekleştirilir. Her yeni örneği yakın kütle merkezi kümeyle atanır.  

## <a name="how-to-configure-k-means-clustering"></a>K-ortalamaları kümeleri yapılandırma
  
1.  Ekleme **K-ortalamaları Kümeleme** denemenizi modülü.  
  
2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Kümeleme modeli kullanmak istediğiniz tam parametreleri biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.  
  
   
  
3.  İçin **numarası, Centroids**, algoritması şununla istediğiniz küme sayısını yazın.  
  
     Küme sayısı tam olarak bu üretmek için model garanti edilmez. Algorithn, bu sayıda veri noktası ile başlar ve en uygun yapılandırma bulmak için yinelenir.  
  
     
  
4.  Özellikleri **başlatma** ilk küme yapılandırmasına tanımlamak için kullanılan algoritma belirtmek için kullanılır.  
  
    -   **İlk N**: Bazı ilk veri noktası sayısı seçilen veri kümesinden ve ilk şekilde kullanılır.  
  
         Olarak da adlandırılan *Forgy yöntemi*.  
  
    -   **Rastgele**: Algoritma, rastgele bir veri noktasının bir kümeye yerleştirir ve ardından, kümenin kütle merkezi rastgele olarak ilk ortalama noktalarına atanan hesaplar.  
  
         Olarak da adlandırılan *rastgele bölüm* yöntemi.  
  
    -   **K-anlamına gelir ++**: Bu kümeleri başlatma için varsayılan yöntemdir.  
  
         **K-ortalamaları ++** algoritması tarafından David Arthur 2007'de önerilen ve düşük ile standart kümeleme önlemek için Sergei Vassilvitskii k-ortalamaları algoritması. **K-ortalamaları ++** ilk küme merkezleri seçmek için farklı bir yöntem kullanarak, standart K-ortalamaları artırır.  
  
    
5.  İçin **rastgele sayı doldurma**, isteğe bağlı olarak küme başlatma için temel kullanılacak bir değer yazın. Bu değer, küme seçimi önemli bir etkiye sahip olabilir.  
  
    
  
6.  İçin **ölçüm**, küme vektör arasında ya da yeni veri noktaları rastgele seçilen kütle merkezi arasındaki uzaklığı ölçmek amacıyla kullanılacak işlevi seçin. Azure Machine Learning aşağıdaki küme uzaklık ölçümleri destekler:  
  
    -   **Euclidean**: Euclidean uzaklık K-ortalamaları kümeleme için küme dağılım bir ölçü yaygın olarak kullanılır. Bu ölçüm tercih edilen çünkü centroids noktaları arasındaki ortalama uzaklığı en aza indirir.
  

  
7.  İçin **yinelemeler**, algoritma yineleme eğitim verilerinde centroids seçimini kesinleştirmeden önce sayısını yazın.  
  
     Bu parametre için Bakiye doğruluğu eğitim zamanla karşılaştırmalı olarak ayarlayabilirsiniz.  
  
8.  İçin **Ata etiket modu**, bir etiket sütun kümesinde varsa nasıl işleneceğini belirten bir seçenek belirleyin.  
  
     K-ortalamaları kümeleme yöntemi öğrenme Denetimsiz bir makine olduğundan, etiketler isteğe bağlıdır. Ancak, veri kümeniz zaten bir etiket sütun varsa, kümelerin seçimi yol göstermesi için bu değerleri kullanabilirsiniz veya değerleri yoksayılacak belirtebilirsiniz.  
  
    -   **Etiket sütunu yoksayın**: Etiket sütundaki değerleri yok sayılır ve modeli oluşturma kullanılmaz.
  
    -   **Eksik değerleri doldurun**: Etiket sütun değerleri, kümeler oluşturmanıza yardımcı olmak üzere özellikleri kullanılır. Herhangi bir satır etiket eksikse değerin diğer özelliklerini kullanarak imputed.  
  
    -   **En yakın Merkezi'ne gelen üzerine**: Etiket sütun değerleri, geçerli kütle merkezi için en yakın olan noktası etiketini kullanarak, tahmin edilen etiket değerleri ile değiştirilir.  

8.  Seçeneğini **Normalleştir özellikleri**eğitim önce Özellikleri'leri normalleştirmek istiyorsanız.
  
     Eğitim önce bir normalleştirme uygularsanız, veri noktaları için normalleştirilir `[0,1]` MinMaxNormalizer tarafından.

10. Modeli eğitme.  
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**etiketli bir veri kümesi ekleyin ve kullanarak modeli eğitmek [kümeleme modeli eğitme](train-clustering-model.md) modülü.  
  

### <a name="results"></a>Sonuçlar

Yapılandırma ve modeli eğitimi tamamladıktan sonra puanları oluşturmak için kullanabileceğiniz bir modeline sahiptir. Ancak, modeli eğitmek için birden çok yol ve sonuçları görüntüleyip için birden çok yolu vardır: 

#### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Model çalışma alanınızdaki bir anlık görüntüsünü yakalama

+ Kullandıysanız [kümeleme modeli eğitme](train-clustering-model.md) Modülü
    1. Sağ [kümeleme modeli eğitme](train-clustering-model.md) modülü.
    2. Seçin **Trained modeli** ve ardından **eğitilen modeli kaydedin**.


Kaydedilmiş modeli model kaydettiğiniz anda eğitim verileri temsil eder. Daha sonra deneme kullanılan eğitim verilerini güncelleştirirseniz, kaydedilmiş model güncelleştirilmez. 



#### <a name="see-the-clustering-result-dataset"></a>Kümeleme sonuç veri kümesini görürsünüz 

+ Kullandıysanız [kümeleme modeli eğitme](train-clustering-model.md) Modülü
    1. Sağ [kümeleme modeli eğitme](train-clustering-model.md) modülü.
    2. Seçin **sonuçları veri kümesi** ve ardından **Görselleştir**.


### <a name="tips-for-generating-the-best-clustering-model"></a>Kümeleme modelinin yerini, en iyi oluşturmaya yönelik ipuçları  

İle eşleşmeyebileceği bilinmektedir **dengeli dağıtım** kümeleme sırasında kullanılan işlem modeli önemli ölçüde etkileyebilir. Dengeli dağıtım noktaları ilk yerleşimini potental centroids içine anlamına gelir.
 
Örneğin, veri kümesini birçok aykırı değerleri içeren ve bir aykırı değer kümeleri sağlamak için seçilir, diğer veri noktası da bu kümeyle yerleştirin ve tekil küme olabilir: diğer bir deyişle, yalnızca bir nokta olan bir küme.  
  
Bu sorunu önlemek için çeşitli yollar vardır:  
  
-   Centroids sayısını değiştirmek ve birden çok çekirdek değer deneyin.  
  
-   Ölçüm değişen veya daha fazla yineleme birden çok modeller oluşturun.  
  
  
Genel olarak, kümeleme modeli, herhangi bir yapılandırma kümeleri yerel olarak en iyi duruma getirilmiş kümesindeki sonuçlanır mümkündür. Diğer bir deyişle, modeli tarafından döndürülen kümeleri kümesini yalnızca geçerli veri noktalarının uygun ve diğer verilerle genelleştirilebilir değildir. Farklı bir başlangıç yapılandırmasını kullandıysanız, K-ortalamaları yöntemi farklı, belki de daha üstün bir yapılandırmada bulabilirsiniz. 
