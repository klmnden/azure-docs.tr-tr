---
title: 'Artırmalı karar ağacı regresyonu: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Artırılmış karar ağacı regresyonu modülü Azure Machine Learning hizmetinde bir topluluğu, regresyon ağaçlarında artırma kullanarak oluşturmak için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 67e54f10074ee566ce974dbd27485904bfe0a653
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411540"
---
# <a name="boosted-decision-tree-regression-module"></a>Artırmalı karar ağacı regresyonu Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Regresyon ağaçlarında artırma kullanarak, bir topluluğu oluşturmak için bu modülü kullanın. *Artırma* ağaçlarının her önceki ağaçları bağımlı olduğu anlamına gelir. Algoritma, öncesinde ağaçları fazlalığı sığdırma tarafından öğrenir. Bu nedenle, küçük riskiyle karşı daha az kapsamı ile doğruluğunu artırmak için karar ağacı topluluğu içinde artırma eğilimlidir.  
  
Bu regresyon yöntemi denetimli öğrenme yöntemidir ve bu nedenle bir *veri kümesi olarak etiketlenmiş*. Etiket sütunun sayısal değerler içermesi gerekir.  

> [!NOTE]
> Sayısal değişkenler kullanan kümeleriyle bu modülü kullanın.  

Model tanımladıktan sonra bunu kullanarak eğitme [modeli eğitme](./train-model.md).

> [!TIP]
> Oluşturulan ağaçları hakkında daha fazla bilgi edinmek ister misiniz? Modeli eğitilir sonra çıktısını sağ tıklayarak [modeli eğitme](./train-model.md) modülü ve select **Görselleştir** her yineleme üzerinde oluşturuldu ağaç görmek için. Her ağacının bölmelerini incelemek ve her düğüm için bkz.  
  
## <a name="more-about-boosted-regression-trees"></a>Artırmalı regresyon ağaçlarında hakkında daha fazla bilgi  

Artırma bagging, rastgele ormanları ve benzeri birlikte topluluğu modelleri oluşturmak için Klasik çeşitli yöntemler biridir.  Azure Machine Learning'de artırmalı karar ağacı algoritması artırma REYONU geçişin verimli bir uygulama kullanın. Gradyan artırma bir machine learning teknik regresyon sorunları var. Bu, her adımda hata ölçün ve sonraki düzeltmek için önceden tanımlanmış kaybı işlevini kullanarak tarafınızdaki bir biçimde her regresyon ağacını oluşturur. Böylece öngörü aslında bir topluluğu zayıf tahmin modellerinin modelidir.  
  
Regresyon sorunları artırma ağaçları tarafınızdaki bir biçimde bir dizi oluşturur ve ardından bir rastgele Diferansiyeli kaybı işlevi kullanarak en iyi ağacı seçer.  
  
Ek bilgi için şu makalelere bakın:  
  
+ [https://wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting](https://wikipedia.org/wiki/Gradient_boosting)

    Gradyan artırma bu Wikipedia makalesi artırmalı ağaçları bazı arka plan bilgileri sağlar. 
  
-  [https://research.microsoft.com/apps/pubs/default.aspx?id=132652](https://research.microsoft.com/apps/pubs/default.aspx?id=132652)  

    Microsoft Research: LambdaRank LambdaMART için için RankNet: Genel bir bakış. J.C. tarafından Burges.

Gradyan yöntemi yükseltmeli için uygun kaybı işlevi regresyonla azaltarak sınıflandırma sorunlar için de kullanılabilir. Sınıflandırma görevleri için artırmalı ağaçları uygulaması hakkında daha fazla bilgi için bkz. [iki sınıflı artırılmış karar ağacı](./two-class-boosted-decision-tree.md).  

## <a name="how-to-configure-boosted-decision-tree-regression"></a>Artırılmış karar ağacı regresyonu yapılandırma

1.  Ekleme **artırılmış karar ağacı** denemenizi modülü. Bu modül altında bulabilirsiniz **Machine Learning**, **başlatmak**altında **regresyon** kategorisi. 
  
2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Nasıl model yapılandırın ve bağımsız değişkenler olarak, belirli bir dizi sağlamak istediğinizi biliyorsanız, bu seçeneği belirleyin.  
   
  
3. **Yapraklar ağaç başına en fazla sayısını**: Tüm ağacında oluşturulabilir terminal düğümlerinden (Yapraklar) sayısını gösterir.  

    Bu değer artırarak, büyük olasılıkla ağaç boyutunu artırın ve overfitting ve uzun süre eğitim at the risk of daha iyi duyarlılık, alın.  

4. **Örnek yaprak düğümü başına en düşük sayısı**: Terminal düğümlerinden (yaprak) oluşturmak için bir ağaç biçiminde gereken durumlarda en az sayısını belirtir.

    Bu değer artırarak yeni kurallar oluşturma için eşik artırın. Örneğin, varsayılan değer olan 1 ile tek bir çalışmasını oluşturulacak yeni bir kural neden olabilir. 5 değerine artırmak istiyorsanız, eğitim verilerini aynı koşulları karşılayan en az 5 servis taleplerini içerir etmesi gerekir.

5. **Öğrenme oranı**: 0 ile öğrenme sırasında adım boyutu tanımlayan 1 arasında bir sayı yazın. Öğrenme oranı ne kadar hızlı veya yavaş learner en iyi çözüm üzerinde uygun sonuç verir belirler. Adım boyutu çok büyük ise, en iyi çözüm overshoot. Adım boyutu çok küçükse, eğitim en iyi çözümün yakınsanmasını daha uzun sürer.

6. **Oluşturulan ağaçları sayısı**: Karar ağaçları topluluğu içinde oluşturmak için toplam sayısını gösterir. Daha fazla karar ağaçları oluşturarak potansiyel olarak daha iyi kapsamı elde edebilirsiniz, ancak eğitim süresini artırır.

    Bu değer, eğitilen model görselleştirirken kullanılacak görüntülenen ağaçları sayısını da denetler. bakın veya bir tek ağacı yazdırma istiyorsanız, değer 1 olarak ayarlayabilirsiniz; Ancak, yalnızca bir ağaç oluşturulur (ilk parametre kümesiyle ağacı) ve herhangi bir yinelemenin gerçekleştirilir.

7. **Rastgele sayı kaynağı**: Rasgele çekirdek değeri kullanmak için isteğe bağlı bir negatif olmayan tamsayı türü. Bir çekirdek belirterek yeniden üretilebilirliğini verilere ve parametreleri olan çalıştırmaları arasında sağlar.

    Varsayılan olarak, rastgele bir tohum ilk çekirdek değeri sistem saatinden elde edilen anlamına gelen 0 olarak ayarlanır.
  
8. **Bilinmeyen kategorik düzeyleri izin**: Eğitim ve doğrulama kümelerinde bilinmeyen değerleri için bir grup oluşturmak için bu seçeneği belirleyin. Bu seçeneğin işaretini kaldırırsanız, model eğitim verilerde bulunan değerleri kabul edebilir. Model için bilinen değerleri daha az kesin olabilir, ancak daha iyi tahminler elde etmek için yeni (bilinmiyor) değerler sağlayabilirsiniz.

9. Bir eğitim veri kümesi ve eğitim modülleri birini ekleyin:

    - Ayarlarsanız **Oluştur trainer modu** seçeneğini **tek parametre**, kullanın [modeli eğitme](train-model.md) modülü.  
  
    

10. Denemeyi çalıştırın.  
  
### <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Çıktısı, her yineleme üzerinde oluşturuldu ağaç görmek için sağ [modeli eğitme](train-model.md) modülü ve select **Görselleştir**.
  
     Her ağaç gruplama detaya gitme ve her düğüm için kuralları görmek için tıklayın.  

+ Modeli Puanlama için kullanmak için ona bağlanın [Score Model](./score-model.md), yeni giriş örnekleri için değerleri tahmin etmek için.

+ Eğitim modeli anlık görüntüsünü kaydetmek için sağ **Trained modeli** seçin ve eğitim modülünün çıkışını **Kaydet**. Denemeyi art arda gelen çalışır bir kopyasını kaydettiğiniz eğitilen model güncelleştirilmez.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 