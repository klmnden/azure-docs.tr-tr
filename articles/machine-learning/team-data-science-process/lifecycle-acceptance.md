---
title: "Takım veri bilimi işlem yaşam döngüsü - Azure müşteri kabul aşaması | Microsoft Docs"
description: "Hedefler, görevler ve veri bilimi projelerinizi müşteri kabul aşaması için teslim edilebilir."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: 953f090f775da5e48b2f4ed36550a5624ae4596b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="customer-acceptance"></a>Müşteri kabulü

Hedefler, görevler, bu konuda özetlenir ve sonuçlara ilişkili **müşteri kabul aşama** takım veri bilimi işleminin. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

* **İş anlama**
* **Veri alımı ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel gösterimi işte **takım veri bilimi işlemi yaşam döngüsü**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
* **Proje sonuçlara Sonlandır**: ardışık düzen, model ve bunların dağıtım bir üretim ortamında çağıran müşteri hedefleri olduğundan emin olun.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele iki ana görevleri şunlardır:

* **Sistem doğrulama**: dağıtılan modeli ve ardışık düzen müşteri gereksinimlerine toplantı onaylayın.
* **Proje el dışı**: üretimde sistem çalıştırmaktır varlığa.

Müşteri, sistem iş gereksinimlerine ve üretim için sistemi dağıtmak için kabul edilebilir doğruluğu sorularla kendi istemci uygulama tarafından kullanılıyor yanıtlar karşıladığını doğrulamalıdır. Tüm belgeleri sonlandırılır ve gözden. Bir yandan dışı işlemleri için sorumlu varlığa projenin tamamlanır. Bu varlık, örneğin, bir BT veya müşteri veri bilimi ekibi veya sistem üretimde çalıştırmaktan sorumludur müşterinin bir aracı olabilir. 

## <a name="artifacts"></a>Yapıtlar
Bu Son aşamada üretilen ana yapay **çıkış proje rapor müşteri için**. Bu projenin hakkında bilgi edinin ve sistemi çalıştırmak bu kullanışlı tüm ayrıntılarını içeren teknik rapor eder. Bir [çıkış rapor](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) şablon olarak kullanılan veya gereken belirli istemci için özelleştirilmiş TDSP tarafından sağlanır. 


## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her takım veri bilimi işlemi yaşam döngüsü adımda bağlantıları verilmiştir:

* [1. İş anlama](lifecycle-business-understanding.md)
* [2. Veri alımı ve anlama](lifecycle-data.md)
* [3. Modelleme](lifecycle-modeling.md)
* [4. Dağıtım](lifecycle-deployment.md)
* [5. Müşteri kabulü](lifecycle-acceptance.md)

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) konu. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu.