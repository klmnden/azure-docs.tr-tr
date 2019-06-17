---
title: 'İki sınıflı destekli vektör makinesi: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Nasıl kullanacağınızı öğrenin **iki sınıflı destekli vektör makinesi** destekli vektör makinesi algoritması üzerinde dayalı bir model oluşturmak için Azure Machine Learning hizmetindeki modülü.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 2f076dd3a5b1ceb9e24548652a71fda5b9aa48b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65027938"
---
# <a name="two-class-support-vector-machine-module"></a>İki sınıflı destekli vektör makinesi Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Destekli vektör makinesi algoritması üzerinde dayalı bir model oluşturmak için bu modülü kullanın. 

Destek vektörü makineler (SVMs), denetimli öğrenme yöntemleri iyi researched bir sınıfa dahildir. Bu belirli uygulama sürekli veya kategorik değişkenleri esas alarak, iki olası sonuçlarını, tahmini için uygundur.

Model parametreleri tanımladıktan sonra modeli eğitimi modüllerini kullanma ve sağlama eğitme bir *etiketli veri kümesini* , etiket veya sonucu bir sütun içerir.

## <a name="about-support-vector-machines"></a>Destek vektörü makineler hakkında

Destek vektör makineler arasında erken makine öğrenimi algoritma ve modelleri SVM birçok uygulamalar, metin ve görüntü sınıflandırma için bilgi alma kullanılmış olan. SVMs hem sınıflandırma ve regresyon görevler için kullanılabilir.

Etiketli veri gerektiren bir denetimli öğrenme modeli bu SVM modelidir. Eğitim işlem algoritma, giriş verileri analiz eder ve adlı bir çok boyutlu özellik alanı desenleri tanır *hyperplane*.  Tüm giriş örnekleri bu alanı noktaları olarak temsil edilir ve kategorisi kategori tarafından geniş ayrılır ve mümkün olduğunca bir boşluk Temizle şekilde çıktı olarak eşleştirilir.

Tahmin için bir kategori veya diğer, bunları aynı alana eşleme yeni örnekler SVM algoritması atar. 

## <a name="how-to-configure"></a>Yapılandırma 

Bu model türü için veri kümesi sınıflandırıcı eğitmek için kullanmadan önce Normalleştir önerilir.
  
1.  Ekleme **iki sınıflı destekli vektör makinesi** denemenizi modülü.  
  
2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.  

3.  İçin **yineleme sayısı**, modeli oluştururken kullanılan yineleme sayısını gösteren bir sayı yazın.  
  
     Bu parametre, eğitim hızı ve doğruluğu arasındaki dengeyi denetlemek için kullanılabilir.  
  
4.  İçin **Lambda**, L1 Kurallaştırma için ağırlık kullanılacak bir değer yazın.  
  
     Bu düzenleme katsayısı, model ayarlamak için kullanılabilir. Daha büyük değerler daha karmaşık modellerin kapatılması.  
  
5.  Seçeneğini **Normalleştir özellikleri**eğitim önce Özellikleri'leri normalleştirmek istiyorsanız.
  
     Eğitim önce bir normalleştirme uygularsanız veri noktalarını Orta Orta ve standart sapma bir birim için ölçeklendirilebilir.
  
6.  Seçeneğini **birim küre projeye**, katsayıları'leri normalleştirmek için.
  
     Birim alanı değerleri yansıtma eğitim önce veri noktaları 0'da ortalanmış bir birim standart sapma sağlamak için ölçeği anlamına gelir.
  
7.  İçinde **rastgele sayı doldurma**, çalıştırmaları arasında yeniden üretilebilirliğini emin olmak istiyorsanız, bir çekirdek kullanılacak bir tamsayı girin.  Aksi takdirde, bir sistem saati değeri çalıştırmaları arasında biraz farklı sonuca neden olabilir bir kaynağı olarak kullanılır.
  
9. Etiketlenmiş bir veri kümesi ve bir bağlama [eğitim modülleri](module-reference.md):
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](train-model.md) modülü.
  

10. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Çıkışı modelin parametreler, eğitimleri, öğrenilen özellik ağırlıkları özetini görmek için sağ [modeli eğitme](./train-model.md)seçip **Görselleştir**.

+ Eğitilen modelleri tahminlerde bulunmak üzere kullanmak için eğitilen modele bağlanmak [Score Model](score-model.md) modülü.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 