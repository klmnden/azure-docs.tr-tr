---
title: 'Veri kümesine atayın: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Ata veri kümesi modülü için kümeleme modeli puanlamak için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: b370c6ef5be311ed9c8df2737fa1b01144d61011
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028778"
---
# <a name="assign-data-to-clusters"></a>Veri kümelerine atama

Bu makalede nasıl kullanılacağını [Ata veri kümelerine](assign-data-to-clusters.md) K-ortalamaları kümeleme algoritması kullanılarak eğitilmiş bir kümeleme modeli kullanarak tahmin oluşturmak için görsel arabirim modülünde.

Modül, her yeni bir veri noktası için olası atamaları içeren bir veri kümesi döndürür. 


## <a name="how-to-use-assign-data-to-clusters"></a>Ata veri kümelerine kullanma
  
1.  Görsel arabirim içinde önceden eğitilmiş bir kümeleme modeli bulun. Oluşturun ve bu yöntemlerden birini kullanarak bir kümeleme modeli eğitme:  
  
    - K-ortalamaları algoritmasını kullanarak yapılandırma [K-ortalamaları Kümeleme](k-means-clustering.md) modülü ve ardından tren bir veri kümesini kullanarak model ve [kümeleme modeli eğitme](train-clustering-model.md) modülü.  
  
  
    Mevcut bir eğitilen kümeleme modelden da ekleyebilirsiniz **kaydettiğiniz modelleri** çalışma alanınızda grup.

2. Eğitilen modelin sol giriş bağlantı noktasına ekleme [Ata veri kümelerine](assign-data-to-clusters.md).  

3. Yeni bir veri kümesinin girdi olarak ekleyin. Bu veri kümesinde, etiketler isteğe bağlıdır. Kategorileri önceden bilirsiniz beklenmiyor için genel olarak, kümeleme bir Denetimsiz öğrenme yöntemidir.

    Ancak, giriş sütunları eğitim kümeleme modeli veya bir hata oluştuğunda kullanılan sütunları aynı olmalıdır.

    > [!TIP]
    > Küme tahminleri sütunları çıkış sayısını azaltmak için kullanmak [kümesindeki sütunları seçme](select-columns-in-dataset.md), sütunların bir alt kümesini seçin. 
    
4. Seçeneğini bırakın **ekleme veya işaretini denetlemek için yalnızca sonuç** tam giriş veri kümesi, birlikte sonuçları (küme atamaları) gösteren bir sütun içerecek şekilde sonuçları istiyorsanız.
  
    Bu seçeneğin işaretini kaldırırsanız, yalnızca sonuçları geri alın. Öngörüler bir web hizmeti bir parçası olarak oluştururken bu yararlı olabilir.
  
5.  Denemeyi çalıştırın.  
  
### <a name="results"></a>Sonuçlar

 
+  Değerleri veri kümesini görüntülemek için modülü sağ tıklayın, **neden veri kümeleri**, tıklatıp **Görselleştir**.

