---
title: Takım veri bilimi işlemi yaşam döngüsü - Azure müşteri kabul aşaması | Microsoft Docs
description: Hedefler, görevler ve veri bilimi projelerinizi müşteri kabul aşaması için teslim edilebilir
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 82819ab59d9bab6f256cd01a376f62ac8dac26b3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837149"
---
# <a name="customer-acceptance"></a>Müşteri kabulü

Bu makalede hedefleri, görevler ve müşteri kabul aşama takım veri bilimi işlem (TDSP) ile ilgili sonuçlara özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

   1. **İş anlama**
   2. **Veri alımı ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

Görsel bir TDSP yaşam döngüsü şöyledir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
**Proje sonuçlara Sonlandır**: ardışık düzen, model ve bunların dağıtım bir üretim ortamında Müşteri'nin hedefleri karşılamak onaylayın.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele iki ana görevleri şunlardır:

   * **Sistem doğrulama**: dağıtılan modeli ve ardışık düzen Müşteri'nin gereksinimlerini karşıladığını onaylayın.
   * **Proje el dışı**: Proje devre dışı üretimde sistemini çalıştırmak için giderek varlık teslim.

Müşteri, sistem kendi iş gereksinimlerini karşıladığını ve bu sistem üretim kullanımı için kendi istemcinin uygulama tarafından dağıtmak için kabul edilebilir doğruluk ile ilgili sorular yanıtlanmaktadır doğrulamalıdır. Tüm belgeleri sonlandırılır ve gözden. Proje karmalayan işlemleri için sorumlu varlığa kapalı. Bu varlık, örneğin, bir BT veya müşteri veri bilimi ekibi veya sistem üretimde çalıştırmaktan sorumludur müşterinin bir aracı olabilir. 

## <a name="artifacts"></a>Yapıtlar
Bu Son aşamada üretilen ana yapay **müşteri için rapor projenin çıkmak**. Bu teknik rapor sistem işletme hakkında bilgi almak için yararlı olan tüm proje ayrıntılarını içerir. TDSP sağlayan bir [çıkmak rapor](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) şablonu. Şablonu olduğu gibi kullanabilir veya belirli istemci ihtiyaçları için özelleştirebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her adımda TDSP yaşam döngüsü bağlantıları verilmiştir:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alımı ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımlar gösteren baştan sona tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantılar ve küçük resim açıklamaları senaryolarla listesini sağlar. İzlenecek yollar bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir. 

Azure Machine Learning Studio kullanan TDSPs adımları yürütmek nasıl örnekleri için bkz: [TDSP Azure Machine Learning ile kullanmak](http://aka.ms/datascienceprocess).
