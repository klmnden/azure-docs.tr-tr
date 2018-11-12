---
title: Team Data Science Process yaşam döngüsü - Azure müşteri kabulü aşaması | Microsoft Docs
description: Hedefleri, görevleri ve teslim edilebilirler için veri bilimi projelerinizi müşteri kabulü aşaması
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
ms.openlocfilehash: 9dd3ab8c9451ecfe6b095b52f5af083a7c7e9550
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232768"
---
# <a name="customer-acceptance"></a>Müşteri kabulü

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) müşteri kabulü aşama ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İşin gereksinimlerini anlama**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
**Proje teslim edilebilirleri Sonlandır**: işlem hattı, model ve bunların dağıtım bir üretim ortamında müşterinin amaçlarını karşılamak onaylayın.

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada ele iki ana görevi vardır:

   * **Sistem doğrulama**: dağıtılan model ve işlem hattı müşterinin gereksinimlerini karşıladığını doğrulayın.
   * **Proje el alma**: Proje kapalı sistemi üretimde çalıştırılacağı varlık teslim.

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

Adımlar Azure Machine Learning Studio'nun TDSPs yürütmek nasıl bir örnekleri için bkz: [Azure Machine Learning ile TDSP kullanma](https://aka.ms/datascienceprocess).
