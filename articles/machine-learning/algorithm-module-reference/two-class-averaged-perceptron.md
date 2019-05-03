---
title: 'Karar ormanı regresyon: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir machine learning modeli ortalama perceptron algoritmadan yola çıkılarak oluşturmak için Azure Machine Learning hizmetinde iki sınıflı ortalama Perceptron modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f0fec525ed87f91cf102053383b2934aac4b71c0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029243"
---
# <a name="two-class-averaged-perceptron-module"></a>İki sınıflı ortalama Perceptron Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir machine learning modeli ortalama perceptron algoritmadan yola çıkılarak oluşturmak için bu modülü kullanın.  
  
Bu sınıflandırma algoritmasıdır denetimli öğrenme yöntemidir ve gerektiren bir *etiketli veri kümesini*, bir etiket sütun içerir. Girdi olarak model ve etiketli veri kümesini sağlayarak modeli eğitmek [modeli eğitme](./train-model.md). Eğitim modeli, ardından yeni giriş örnekleri için değerleri tahmin etmek için kullanılabilir.  

### <a name="about-averaged-perceptron-models"></a>Ortalama perceptron modelleri hakkında

*Perceptron yöntemi Ortalama* sinir ağı erken ve basit bir sürümü. Bu yaklaşımda, doğrusal bir işleve göre ve ardından bir dizi özellik öğesinden türetilmiştir ağırlıkları birlikte birkaç olası çıktıları girişleri sınıflandırılır; bu nedenle adı "perceptron."

Daha basit perceptron modelleri doğrusal olarak ayrılabilir desenleri öğrenme ise uygun sinir ağları (özellikle derin sinir ağı) daha karmaşık sınıf sınırları modelleyebilirsiniz. Ancak, perceptrons daha hızlı ve sürekli eğitimlerle perceptrons servis taleplerini seri olarak işlemek için kullanılabilir.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>İki sınıflı ortalama Perceptron yapılandırma

1.  Ekleme **iki sınıflı ortalama Perceptron** denemenizi modülü.  

2.  Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl biliyorsanız, belirli bir dizi bağımsız değişken olarak sağlayın.
  
3.  İçin **öğrenme oranı**, için bir değer belirtin *öğrenme oranı*. Öğrenme oranı denetim stokastik modeli test ve düzeltilmiş her seferinde kullanılır adım boyutunu değerleri.
  
     Hızı daha küçük hale getirerek, model daha sık, içinde yerel bir platoya tıkanıp riskli test. Bir adım daha büyük hale getirerek, daha hızlı ve doğru en düşük değerleri overshooting at the risk of yakınsama.
  
4.  İçin **en yüksek yineleme sayısı**, numarasını yazın eğitim verileri incelemek için algoritma istediğiniz saatleri.  
  
     Erken genellikle durdurma daha iyi Genelleştirme sağlar. Yineleme sayısının artırılması, overfitting at the risk of sığdırma artırır.
  
5.  İçin **rastgele sayı doldurma**, isteğe bağlı olarak temel kullanılacak bir tamsayı girin. Bir çekirdek kullanarak denemeler arasında yeniden üretilebilirliğini çalıştıran emin olmak istiyorsanız önerilir.  
  
1.  Bir eğitim veri kümesi ve bir eğitim modüllerinin bağlayın:
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](train-model.md) modülü.

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Çıkışı modelin parametreler, eğitimleri, öğrenilen özellik ağırlıkları özetini görmek için sağ [modeli eğitme](./train-model.md).


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 