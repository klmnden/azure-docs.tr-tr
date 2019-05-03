---
title: 'Kümeleme modeli eğitme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Kümeleme modeli eğitmek için Azure Machine Learning hizmetinde kümeleme modeli eğitme modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 41cdec1d7f1c3932b17da6f9b1de518071f3f542
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028088"
---
# <a name="train-clustering-model"></a>Kümeleme modeli eğitme

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Kümeleme modeli eğitmek için bu modülü kullanın.

Modülü kullanarak zaten yapılandırmış olduğunuz deneyimsiz bir kümeleme modeli alır [K-ortalamaları Kümeleme](k-means-clustering.md) modülü ve etiketli ya da etiketsiz bir veri kümesi kullanarak modeli eğitir. Modül, tahmin ve küme atamalarını her durumda bir eğitim veri kümesi için kullanabileceğiniz iki eğitilen bir modelin oluşturur.

> [!NOTE]
> Bir kümeleme modeli bürünülemiyor olması kullanılarak eğitilmiş [modeli eğitme](train-model.md) makine öğrenimi modelleri eğitim için genel modül modülü. Çünkü [modeli eğitme](train-model.md) denetimli öğrenme algoritmaları yalnızca ile çalışır. K-ortalamaları ve diğer küme algoritmalar Denetimsiz öğrenme algoritmasını etiketlenmemiş verilerden bilgi edinebilirsiniz anlamına gelir izin verir.  
  
## <a name="how-to-use-train-clustering-model"></a>Kümeleme modeli eğitme kullanma  
  
1.  Ekleme **kümeleme modeli eğitme** denemenizde Studio modülü. Modül altında bulabilirsiniz **Machine Learning modüllerinin**, **eğitme** kategorisi.  
  
2. Ekleme [K-ortalamaları Kümeleme](k-means-clustering.md) modül veya uyumlu bir küme oluşturur ve özel başka bir modül model ve kümeleme modeli parametrelerini ayarlayın.  
    
3.  Bir eğitim veri kümesi eklemek için sağ girişine **kümeleme modeli eğitme**.
  
5.  İçinde **sütun kümesi**, küme oluşturma ile kullanmak için veri kümesindeki sütunları seçin. İyi özellikler sütunları seçtiğinizden emin olun: Örneğin, kimlikleri veya diğer benzersiz değerlere sahip bir sütun veya aynı değerlere sahip sütun kullanarak kaçının.

    Bir etiket varsa, bir özelliği olarak kullanabilir veya onu bırakın.  
  
6. Seçeneğini **ekleme veya işaretini denetlemek için yalnızca sonuç**, yeni küme etiket birlikte eğitim verileri çıkışı istiyorsanız.

    Bu seçeneğin işaretini kaldırırsanız, yalnızca küme atamaları çıktısı alınır. 

7. Denemeyi çalıştırın ya da tıklayın **kümeleme modeli eğitme** modülü ve select **Seçileni**.  
  
### <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:


+  Değerleri veri kümesini görüntülemek için modülü sağ tıklayın, **neden veri kümeleri**, tıklatıp **Görselleştir**.

+ Daha sonra yeniden kullanmak için eğitilen modeli kaydetmek için modülün sağ tıklayın, **Trained modeli**, tıklatıp **eğitilen modeli olarak Kaydet**.

+ Modelden puan oluşturmak için [Ata veri kümelerine](assign-data-to-clusters.md).



## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 