---
title: 'İki sınıflı Artırmalı karar ağacı: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir machine learning artırmalı karar ağacı algoritması üzerinde alan modeli oluşturmak için Azure Machine Learning hizmetinde iki sınıflı artırılmış karar ağacı modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09ea530cac5bdbd62208f134177e5ceaccb545e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65027953"
---
# <a name="two-class-boosted-decision-tree-module"></a>İki sınıflı artırılmış karar ağacı Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Artırmalı karar ağacı algoritması üzerinde dayalı bir machine learning modeli oluşturmak için bu modülü kullanın. 

Bir yükseltmeli karar ağacı üçüncü ağaç birinci ve ikinci ağaçları ve benzeri hatalarını düzeltir, ikinci ağaç ilk ağacı hatalarını düzeltir yöntemi öğrenme bir topluluğu olur.  Öngörüler öngörü sağlayan tüm topluluğu ağaçları birlikte üzerinde temel alır.
  
Genel olarak, düzgün bir şekilde yapılandırıldığında, artırmalı karar ağaçları, çok çeşitli makine öğrenimi görevlerini üzerinde en iyi performansı elde etmek kolay yöntemler şunlardır. Ancak, bunlar ayrıca daha fazla bellek kullanımı yoğun öğrencileriyle biridir ve geçerli uygulama bellekte her şeyi içerir. Bu nedenle, artırmalı karar ağacı modeli bazı doğrusal öğrencileriyle işleyebilen büyük veri kümelerini işlemeye mümkün olmayabilir.

## <a name="how-to-configure"></a>Yapılandırma

Bu modül deneyimsiz sınıflandırma modeli oluşturur. Sınıflandırma modeli eğitmek için ek olarak, denetimli öğrenme yöntemi olduğundan ihtiyacınız bir *etiketli veri kümesini* , tüm satırlar için bir değer içeren bir etiketi sütun içerir.

Bu tür bir model kullanarak eğitebilirsiniz [modeli eğitme](././train-model.md). 

1.  Azure Machine Learning'de ekleme **artırılmış karar ağacı** denemenizi modülü.
  
2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.
  
    + **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.
  
  
3.  İçin **bırakır ağaç başına en fazla sayısı**, tüm ağacında oluşturulabilir terminal düğümlerinden (Yapraklar) sayısını gösterir.
  
     Bu değer artırarak, büyük olasılıkla ağaç boyutunu artırın ve overfitting ve uzun süre eğitim at the risk of daha iyi duyarlılık, alın.
  
4.  İçin **yaprak düğüm başına örnek sayısı alt sınırı**, terminal düğümlerinden (yaprak) oluşturmak için bir ağaç biçiminde gerekli servis talebi sayısını gösterir.  
  
     Bu değer artırarak yeni kurallar oluşturma için eşik artırın. Örneğin, varsayılan değer olan 1 ile tek bir çalışmasını oluşturulacak yeni bir kural neden olabilir. 5 değerine artırmak istiyorsanız, eğitim verilerini aynı koşulları karşılayan en az beş servis taleplerini içerir etmesi gerekir.
  
5.  İçin **öğrenme oranı**, 0 ile öğrenme sırasında adım boyutu tanımlayan 1 arasında bir sayı yazın.  
  
     Öğrenme oranı ne kadar hızlı veya yavaş learner en iyi çözüm üzerinde uygun sonuç verir belirler. Adım boyutu çok büyük ise, en iyi çözüm overshoot. Adım boyutu çok küçükse, eğitim en iyi çözümün yakınsanmasını daha uzun sürer.
  
6.  İçin **oluşturulmuş ağaçları sayısı**, topluluğu içinde oluşturmaya karar ağaçları toplam sayısını gösterir. Daha fazla karar ağaçları oluşturarak potansiyel olarak daha iyi kapsamı elde edebilirsiniz, ancak eğitim süresini artırır.
  
     Bu değer, eğitilen model görselleştirirken kullanılacak görüntülenen ağaçları sayısını da denetler. bakın veya tek bir ağaç yazdırmak, değeri 1 olarak ayarlayın. Ancak bunu yaptığınızda, yalnızca bir ağaç oluşturulur (ilk parametre kümesiyle ağacı) ve herhangi bir yinelemenin gerçekleştirilir.
  
7.  İçin **rastgele sayı doldurma**, isteğe bağlı olarak rasgele çekirdek değeri kullanılacak bir negatif olmayan tamsayı yazın. Bir çekirdek belirterek yeniden üretilebilirliğini verilere ve parametreleri olan çalıştırmaları arasında sağlar.  
  
     Rastgele bir tohum varsayılan ilk çekirdek değeri sistem saatinden elde edilen anlamına gelen 0 olarak ayarlanır.  Rastgele bir tohum kullanarak art arda gelen çalışmalarını farklı sonuçlar olabilir.
  

9. Modeli eğitme.
  
    + Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, ekli bir veri kümesine bağlanmak ve [modeli eğitme](./train-model.md) modülü.  
  
   
## <a name="results"></a>Sonuçlar

Model eğitiminin tamamlandıktan sonra çıktısını sağ [modeli eğitme](./train-model.md) sonuçlarını görüntülemek için:

+ Her yineleme üzerinde oluşturuldu ağaç görmek için seçin **Görselleştir**. 
+ Gruplama detaya gitme ve her düğüm için kuralları görmek için her ağaç tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 