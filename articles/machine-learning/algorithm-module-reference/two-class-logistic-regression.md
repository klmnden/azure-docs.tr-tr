---
title: 'İki sınıflı Lojistik regresyon: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: İki (ve yalnızca iki) sonuçları tahmin etmek için kullanılan bir Lojistik regresyon modeli oluşturmak için Azure Machine Learning hizmetinde iki sınıflı Lojistik regresyon modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: aacaf6c64ef77d0e694f97e3675060eca33794ed
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029258"
---
# <a name="two-class-logistic-regression-module"></a>İki sınıflı Lojistik regresyon Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

İki (ve yalnızca iki) sonuçları tahmin etmek için kullanılan bir Lojistik regresyon modeli oluşturmak için bu modülü kullanın. 

Lojistik regresyon pek çok sorunları modellemek için kullanılan iyi bilinen bir istatistik tekniğidir. Bu algoritma bir *denetimli öğrenme* yöntemi  Bu nedenle modeli eğitmek için sonuçlarını zaten içeren bir veri kümesi sağlamanız gerekir.  

### <a name="about-logistic-regression"></a>Lojistik regresyon hakkında  

Lojistik regresyon sonucu olasılığını tahmin etmek için kullanılır ve sınıflandırma görevleri için özellikle olarak popüler istatistikleri iyi bilinen bir yöntemidir. Algoritma, veri Lojistik işlevine sığdırma tarafından bir olayın oluşum olasılığını tahmin eder.
  
Bu modülde, sınıflandırma algoritmasıdır dichotomous veya ikili değişkenleri için optimize edilmiştir. birden çok sonuç sınıflandırmak ihtiyacınız varsa [veya çoklu sınıflar Lojistik regresyon](./multiclass-logistic-regression.md) modülü.

##  <a name="how-to-configure"></a>Yapılandırma  

Bu modeli eğitmek için bir etiket veya sınıf sütunu içeren bir veri kümesi sağlamanız gerekir. Bu modül iki sınıflı sorunlar için tasarlandığından, etiket veya sınıf sütun tam olarak iki değerler içermelidir. 

[Oy] Örneğin, etiket sütunla olası değerler "Evet" veya "Hayır". Veya, "Yüksek" veya "Düşük" olası değerler [kredi riski] olabilir. 
  
1.  Ekleme **iki sınıflı Lojistik regresyon** denemenizi modülü.  
  
2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.  
  
3.  İçin **iyileştirme dayanıklılık**, model iyileştirilmesi sırasında kullanmak için bir eşik değerini belirtin. Geliştirme yinelemeleri arasında belirtilen eşiğin altına düşerse, algoritma bir çözüm üzerinde yakınsanmış olarak kabul edilir ve eğitim durdurur.  
  
4.  İçin **L1 Kurallaştırma ağırlık** ve **L2 Kurallaştırma ağırlık**, L1 ve L2 Kurallaştırma parametrelerini kullanmak için bir değer yazın. Sıfır olmayan bir değer, her ikisi için önerilir.  
  
     *Kurallaştırma* modelleri aşırı katsayısı değerlerle penalizing tarafından overfitting engelleyen bir yöntemdir. Kurallaştırma varsayım hata katsayısı değerlerle ilişkili cezası ekleyerek çalışır. Bu nedenle, daha doğru bir model aşırı katsayısı değerlerle ceza, ancak daha az doğru bir modeli daha pasif değerlerle daha az ceza.  
  
     L1 ve L2 Kurallaştırma farklı etkileri ve kullanır.  
  
    -   Yüksek boyutlu verilerle çalışırken kullanışlı, L1 seyrek modelleri için uygulanabilir.  
  
    -   Buna karşılık, L2 Kurallaştırma seyrek olmayan veriler için tercih edilir.  
  
     Bu algoritma L1 ve L2 Kurallaştırma değerleri doğrusal birleşimi destekler: diğer bir deyişle, varsa <code>x = L1</code> ve <code>y = L2</code>, ardından <code>ax + by = c</code> doğrusal aralık düzenleme koşullarını tanımlar.  
  
    > [!NOTE]
    >  L1 ve L2 düzenleme hakkında daha fazla öğrenmek ister misiniz? Aşağıdaki makalede L1 ve L2 Kurallaştırma nasıl farklıdır ve lojistik regresyon ve sinir ağı modelleri için kod örnekleri ile model sığdırma etkilemesi hakkında ayrıntılı bilgi sağlar:  [L1 ve Machine Learning için L2 düzenleme](https://msdn.microsoft.com/magazine/dn904675.aspx)  
    >
    > L1 ve L2 koşulları farklı doğrusal bileşimleri, lojistik regresyon modellerini çıkabilecek: Örneğin, [esnek net Kurallaştırma](https://wikipedia.org/wiki/Elastic_net_regularization). Modelinizde maliyetli doğrusal birlikte tanımlamak için aşağıdaki bileşimler başvuru öneririz.
      
5.  İçin **L BFGS için bellek boyutu**, için kullanılacak bellek miktarını belirtin *L BFGS* iyileştirme.  
  
     L-BFGS "sınırlı için bellek Fletcher Goldfarb Shanno Broyden" anlamına gelir. Bu parametre tahmini için popüler bir iyileştirme algoritması olur. Bu parametre, son konumları ve sonraki adıma hesaplama için depolamak için gradyan sayısını gösterir.  
  
     Bu iyileştirme parametresi sonraki adım ve yön hesaplamak için kullanılan bellek miktarını sınırlar. Daha az bellek belirttiğinizde, eğitim daha hızlı ancak daha az doğru.  
  
6.  İçin **rastgele sayı doldurma**, bir tamsayı girin. Sonuçları aynı denemede birden çok çalıştırılan yeniden üretilebilen olmasını istiyorsanız, bir çekirdek değeri tanımlamak önemlidir.  
  
  
8. Denemeye etiketli bir veri kümesi ekleyin ve birine bağlanabilmeleri [eğitim modülleri](module-reference.md).  
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](./train-model.md) modülü.  
  
9. Denemeyi çalıştırın.  
  
## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Çıkışı modelin parametreler, eğitimleri, öğrenilen özellik ağırlıkları özetini görmek için sağ [modeli eğitme](./train-model.md) seçip **Görselleştir**.   
  
+ Yeni veri tahminlerde için eğitilen modeli ve yeni veriler giriş olarak kullanın [Score Model](./score-model.md) modülü. 


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 