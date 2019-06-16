---
title: 'İki sınıflı sinir ağı: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: İki sınıflı sinir ağı modülü yalnızca iki değer içeren bir hedef tahmin etmek için kullanılan bir sinir ağı modeli oluşturmak için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 7ea852fcd312c6f7b1b716278ed538b7accde5bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029228"
---
# <a name="two-class-neural-network-module"></a>İki sınıflı sinir ağı Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Yalnızca iki değer içeren bir hedef tahmin etmek için kullanılan bir sinir ağı modeli oluşturmak için bu modülü kullanın.

Sinir ağları kullanarak sınıflandırma denetimli öğrenme yöntemidir ve bu nedenle bir *etiketli veri kümesini*, bir etiket sütun içerir. Örneğin, ikili sonuçları gibi bir Hasta belirli bir Hastalık etkinleştirilmediğini ya da bir makinenin belirli bir pencere içinde başarısız olma olasılığı yüksek olup olmadığını tahmin etmek üzere bu sinir ağı modelinin kullanabilirsiniz.  

Model tanımladıktan sonra bir giriş olarak etiketlenmiş bir veri kümesi ve model sağlayarak eğitme [modeli eğitme](./train-model.md). Eğitim modeli, ardından yeni girişler için değerleri tahmin etmek için kullanılabilir.

### <a name="more-about-neural-networks"></a>Sinir ağları hakkında daha fazla bilgi

Bir sinir ağı, birbirine bağlı katmanları kümesidir. Girişler, ilk katmandır ve bir çıkış katmana ağırlıklı kenarlar ve düğümü oluşan döngüsel olmayan yönlü graf tarafından bağlanırsınız.

Giriş ve çıkış katmanlar arasında birden çok gizli katmanları ekleyebilirsiniz. En Tahmine dayalı görevleri yalnızca bir veya birkaç gizli katmanları ile kolayca gerçekleştirilebilir. Bununla birlikte, son araştırmalara derin sinir ağı (DNN) birçok katmanları ile geçerli görüntü ve konuşma tanıma gibi karmaşık görevler olabilir göstermiştir. Ardışık katmanlarını artan anlam derinlik düzeyini modellemek için kullanılır.

Giriş ve çıkışları arasındaki ilişki, giriş veri çubuğunda sinir ağı eğitiliyor gelen öğrendiniz. Grafik yönünü girişleri gizli katmanı aracılığıyla ve çıkış katmana gelen devam eder. Bir katmandaki tüm düğümleri ağırlıklı kenarları sonraki katmanı düğümlerine bağlı.

Belirli bir giriş için ağ çıkışı hesaplamak için her bir düğümde gizli katmanları ve çıkış katmanında bir değer hesaplanır. Değerin ağırlıklı toplamı düğümleri önceki katmanın değerlerini hesaplayarak ayarlanır. Etkinleştirme işlevi, ardından bu ağırlıklı toplamına uygulanır.
  
## <a name="how-to-configure"></a>Yapılandırma

1.  Ekleme **iki sınıflı sinir ağı** denemenizi modülü. Bu modül altında bulabilirsiniz **Machine Learning**, **başlatmak**, **sınıflandırma** kategorisi.  
  
2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl zaten biliyorsanız, bu seçeneği belirleyin.  

3.  İçin **katman belirtimi gizli**, ağ mimarisi oluşturma türünü seçin.  
  
    -   **Tam olarak, servis talebi bağlı**: Şu şekilde iki sınıflı sinir ağları için tanımlanan varsayılan sinir ağı mimarisi kullanır:
  
        -   Gizli bir katman vardır.
  
        -   Çıkış katmanı tam olarak gizli katmana bağlı ve gizli katmanın tam olarak giriş katmana bağlı.
  
        -   Giriş katmandaki düğüm sayısını eğitim verileri özelliklerinde sayısına eşittir.
  
        -   Gizli katmandaki düğüm sayısını, kullanıcı tarafından ayarlanır. Varsayılan değer 100’dür.
  
        -   Düğüm sayısını sınıfları sayısına eşittir. İki sınıflı sinir ağı, tüm giriş çıkış katmanında iki düğüm birine eşlenmelidir anlamına gelir.

5.  İçin **öğrenme oranı**, düzeltme önce her bir yineleme sırasında gerçekleştirilen adım boyutunu tanımlayın. Öğrenme oranı için daha büyük bir değer daha hızlı yakınsanmasını modeli neden olabilir, ancak yerel en düşük değerleri overshoot.

6.  İçin **sayısı, yinelemeler öğrenme**, en fazla kaç kez algoritma eğitim durumları işlem belirtin.

7.  İçin **öğrenme ağırlık verme çapı**, düğüm ağırlıkları öğrenme sürecinin başlangıcında belirtin.

8.  İçin **İtici Güç**, önceki yinelemelerin gelen düğümlere öğrenme sırasında uygulamak için bir ağırlık belirtin  

10. Seçin **karışık örnekler** çalışmaları arasında bir yinelemeler Karıştırılmasına seçeneği. Bu seçeneğin işaretini kaldırırsanız, servis talepleri denemeyi çalıştırma her zaman tam olarak aynı sırada işlenir.
  
11. İçin **rastgele sayı doldurma**, kaynağı kullanılacak bir değer yazın.
  
     Aynı deneyde çalıştırmaları arasında yinelenebilirliği sağlamak istediğinizde bir çekirdek belirten değer yararlı olur.  Aksi takdirde, bir sistem saati değeri, denemeyi çalıştırma her zaman biraz farklı sonuçlar oluşabilir çekirdek kullanılır.
  
13. Denemeye etiketli bir veri kümesi ekleyin ve birine bağlanabilmeleri [eğitim modülleri](module-reference.md).  
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](train-model.md) modülü.  
  
14. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Ağırlıkları öğrenilen eğitim ve diğer parametreleri sinir ağı modelinin parametreler, bu özellik bir özetini görmek için çıkışını sağ tıklayın [modeli eğitme](./train-model.md)seçip **Görselleştir**.  

+ Eğitilen model anlık görüntüsünü kaydetmek için sağ **Trained modeli** seçin ve çıkış **eğitilen modeli olarak Kaydet**. Bu model, aynı denemede bunu izleyen çalışır güncelleştirilmez.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 
