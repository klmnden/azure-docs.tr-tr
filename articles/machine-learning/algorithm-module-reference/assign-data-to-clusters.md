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
ms.openlocfilehash: 1c2d2a02ecfb617551dd9174b87f363d57b151a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65467208"
---
# <a name="module-assign-data-to-clusters"></a>Modül: Veri kümelerine atama

Bu makalede nasıl kullanılacağını *Ata veri kümelerine* modülü Azure Machine Learning visual arabiriminde. Modül ile eğitilmiş bir kümeleme modeli aracılığıyla tahminler oluşturur *K-ortalamaları Kümeleme* algoritması.

Ata veri kümeleri modülü için yeni her veri noktası için olası atamaları içeren bir veri kümesi döndürür. 


## <a name="how-to-use-assign-data-to-clusters"></a>Ata veri kümelerine kullanma
  
1. Azure Machine Learning görsel arabirim önceden eğitilmiş bir kümeleme modeli bulun. Oluşturun ve aşağıdaki yöntemlerden birini kullanarak bir kümeleme modeli eğitme:  
  
    - K-ortalamaları kümeleme algoritması kullanarak yapılandırma [K-ortalamaları Kümeleme](k-means-clustering.md) modülü ve bir veri kümesi ve kümeleme modeli eğitme modülünü (Bu makale) kullanarak model eğitme.  
  
    - Mevcut bir eğitilen kümeleme modelden da ekleyebilirsiniz **kaydettiğiniz modelleri** çalışma alanınızda grup.

2. Eğitilen modelin sol giriş bağlantı noktasına ekleme **Ata veri kümelerine**.  

3. Yeni bir veri kümesinin girdi olarak ekleyin. 

   Bu veri kümesinde, etiketler isteğe bağlıdır. Genellikle, kümeleme bir Denetimsiz öğrenme yöntemidir. Kategorileri önceden bilmeniz beklenmez. Ancak, giriş sütunları eğitim kümeleme modeli veya bir hata oluştuğunda kullanılan sütunları aynı olmalıdır.

    > [!TIP]
    > Arabirimi küme tahminleri yazılır sütun sayısını azaltmak için kullanma [veri kümesinde sütun seçme](select-columns-in-dataset.md), sütunların bir alt kümesini seçin. 
    
4. Bırakın **ekleme veya işaretini denetlemek için yalnızca sonuç** sonuçları (küme atamaları) sütununu dahil olmak üzere tam giriş veri kümesi, içerecek şekilde sonuçları istiyorsanız onay kutusu.
  
    Bu onay kutusunun işaretini kaldırırsanız, yalnızca sonuçlar döndürülür. Öngörüler bir web hizmeti bir parçası olarak oluşturduğunuzda, bu seçenek yararlı olabilir.
  
5.  Denemeyi çalıştırın.  
  
### <a name="results"></a>Sonuçlar

+  Değerleri veri kümesini görüntülemek için modülü sağ tıklayın, **neden veri kümeleri**ve ardından **Görselleştir**.

