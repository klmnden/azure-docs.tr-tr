---
title: Müşteri kabulü Team Data Science Process yaşam döngüsü aşaması
description: Hedefleri, görevleri ve teslim edilebilirler için veri bilimi projelerinizi müşteri kabulü aşaması
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 91d645e6120040870c7c1696c7bfd8f68509cb35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303579"
---
# <a name="customer-acceptance-stage-of-the-team-data-science-process-lifecycle"></a>Müşteri kabulü Team Data Science Process yaşam döngüsü aşaması

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) müşteri kabulü aşama ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İşin gereksinimlerini anlama**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
**Proje teslim edilebilirleri Sonlandır**: İşlem hattı, model ve bunların dağıtım bir üretim ortamında müşterinin amaçlarını karşılamak onaylayın.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada ele iki ana görevi vardır:

   * **Sistem doğrulama**: İşlem hattı ve dağıtılmış bir modelinin müşterinin gereksinimlerini karşıladığını onaylayın.
   * **Proje el alma**: Kapalı proje sistemi üretimde çalıştırılacağı varlık teslim.

Müşteri, sistemin iş gereksinimlerini karşıladığından ve bu, istemci uygulama tarafından kullanım için üretim sistemi dağıtmak için kabul edilebilir doğruluk ile ilgili sorular yanıtlanmaktadır doğrulamalıdır. Tüm belgeleri sonlandırılır ve gözden geçirdi. Proje. Bu sayede işlemleri için sorumlu varlığa kapalı. Bu varlık, örneğin, bir BT veya müşteri veri bilimi takım ya da sistem üretimde çalıştırmaktan sorumludur müşterinin aracı olabilir. 

## <a name="artifacts"></a>Yapıtlar
Bu son aşamasında üretilen ana yapıt **müşteri için rapor projenin çıkış**. Bu teknik rapora sistem çalıştırılması hakkında daha fazla bilgi için yararlı olan tüm proje ayrıntılarını içerir. TDSP sağlayan bir [çıkış rapor](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) şablonu. Şablonu olduğu gibi kullanabilir veya belirli istemci ihtiyaçları için özelleştirebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İşin gereksinimlerini anlama](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

İşlemin belirli senaryolar için tüm adımları gösteren uçtan uca tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantıları ve küçük resim açıklamaları senaryolarıyla bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Adımlar Azure Machine Learning Studio'nun TDSPs yürütmek nasıl bir örnekleri için bkz: [Azure Machine Learning ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
