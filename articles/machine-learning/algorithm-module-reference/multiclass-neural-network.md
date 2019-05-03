---
title: 'Çok sınıflı sinir ağı: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Birden çok değeri olan bir hedef tahmin etmek için kullanılan bir sinir ağı modeli oluşturmak için Azure Machine Learning hizmetinde veya çoklu sınıflar sinir ağı modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a7bef225c001ebd9bbb9a45c8bcc301cfb49edb6
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029393"
---
# <a name="multiclass-neural-network-module"></a>Çok sınıflı sinir ağı Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Birden çok değeri olan bir hedef tahmin etmek için kullanılan bir sinir ağı modeli oluşturmak için bu modülü kullanın. 

Örneğin, bu tür sinir ağları rakam veya harf tanıma ve sınıflandırma desen tanıma gibi karmaşık bilgisayar işleme görevlerini kullanılabilir.

Sinir ağları kullanarak sınıflandırma denetimli öğrenme yöntemidir ve bu nedenle gerektirir bir *etiketli veri kümesini* , bir etiket sütun içerir.

Girdi olarak model ve etiketli veri kümesini sağlayarak modeli eğitmek [modeli eğitme](./train-model.md). Eğitim modeli, ardından yeni giriş örnekleri için değerleri tahmin etmek için kullanılabilir.  

## <a name="about-neural-networks"></a>Sinir ağları hakkında

Bir sinir ağı, birbirine bağlı katmanları kümesidir. Girişler, ilk katmandır ve bir çıkış katmana ağırlıklı kenarlar ve düğümü oluşan döngüsel olmayan yönlü graf tarafından bağlanırsınız.

Giriş ve çıkış katmanlar arasında birden çok gizli katmanları ekleyebilirsiniz. En Tahmine dayalı görevleri yalnızca bir veya birkaç gizli katmanları ile kolayca gerçekleştirilebilir. Bununla birlikte, son araştırmalara derin sinir ağı (DNN) birçok katmanları ile geçerli görüntü ve konuşma tanıma gibi karmaşık görevler olabilir göstermiştir. Ardışık katmanlarını artan anlam derinlik düzeyini modellemek için kullanılır.

Giriş ve çıkışları arasındaki ilişki, giriş veri çubuğunda sinir ağı eğitiliyor gelen öğrendiniz. Grafik yönünü girişleri gizli katmanı aracılığıyla ve çıkış katmana gelen devam eder. Bir katmandaki tüm düğümleri ağırlıklı kenarları sonraki katmanı düğümlerine bağlı.

Belirli bir giriş için ağ çıkışı hesaplamak için her bir düğümde gizli katmanları ve çıkış katmanında bir değer hesaplanır. Değerin ağırlıklı toplamı düğümleri önceki katmanın değerlerini hesaplayarak ayarlanır. Etkinleştirme işlevi, ardından bu ağırlıklı toplamına uygulanır.

## <a name="configure-multiclass-neural-network"></a>Çok sınıflı sinir ağı yapılandırma

1. Ekleme **veya çoklu sınıflar sinir ağı** denemenizde arabirimi modülü. Bu modül altında bulabilirsiniz **Machine Learning**, **başlatmak**, **sınıflandırma** kategorisi.

2. **Eğitmen modu oluşturma**: Eğitim modeli nasıl istediğinizi belirtmek için bu seçeneği kullanın:

    - **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl zaten biliyorsanız, bu seçeneği belirleyin.

    

3. **Gizli katmanın belirtimi**: Ağ mimarisi oluşturma türünü seçin.

    - **Tam olarak, servis talebi bağlı**: Varsayılan sinir ağı mimarisi kullanarak bir model oluşturmak için bu seçeneği belirleyin. Çok sınıflı sinir ağı modelleri için varsayılan değerler aşağıdaki gibidir:

        - Gizli bir katman
        - Çıkış katmanı tam olarak gizli katmana bağlı.
        - Gizli katmanın tam olarak giriş katmana bağlı.
        - Giriş katmandaki düğüm sayısını, eğitim verilerini özelliklerinde sayısına göre belirlenir.
        - Gizli katmandaki düğüm sayısını, kullanıcı tarafından ayarlanabilir. Varsayılan değer 100'dür.
        - Çıkış katmandaki düğüm sayısını sınıfları sayısına bağlıdır.
  
   

5. **Gizli düğüm sayısını**: Bu seçenek varsayılan mimariyi gizli düğüm sayısını özelleştirmenizi sağlar. Gizli düğüm sayısını yazın. 100 düğümü olan bir gizli katmanın varsayılandır.

6. **Öğrenme oranı**: Her yinelemeyi sizden önce düzeltme alınma adım boyutunu tanımlayın. Öğrenme oranı için daha büyük bir değer daha hızlı yakınsanmasını modeli neden olabilir, ancak yerel en düşük değerleri overshoot.

7. **Sayısı, yinelemeler öğrenme**: En fazla kaç kez algoritma eğitim durumları işlemesi gerektiğini belirtin.

8. **Öğrenme ağırlık verme çapı**: Düğüm ağırlıkları öğrenme sürecinin başlangıcında belirtin.

9. **İtici Güç**: Önceki yinelemelerin gelen düğümlere öğrenme sırasında uygulamak için bir ağırlık belirtin.
  
11. **Karışık örnekler**: Çalışmaları arasında bir yinelemeler karıştırmak için bu seçeneği belirleyin.

    Bu seçeneğin işaretini kaldırırsanız, servis talepleri denemeyi çalıştırma her zaman tam olarak aynı sırada işlenir.

12. **Rastgele sayı kaynağı**: Aynı deneyde çalıştırmaları arasında yinelenebilirliği sağlamak istiyorsanız, çekirdek kullanılacak bir değer yazın.

14. Bir eğitim veri kümesi ve bir bağlama [eğitim modülleri](module-reference.md): 

    - Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](train-model.md).  
  

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

- Ağırlıkları öğrenilen eğitim ve diğer parametreleri sinir ağı modelinin parametreler, bu özellik bir özetini görmek için çıkışını sağ tıklayın [modeli eğitme](./train-model.md) seçip **Görselleştir**.  

- Eğitilen model anlık görüntüsünü kaydetmek için sağ **Trained modeli** seçin ve çıkış **eğitilen modeli olarak Kaydet**. Bu model, aynı denemede bunu izleyen çalışır güncelleştirilmez.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 