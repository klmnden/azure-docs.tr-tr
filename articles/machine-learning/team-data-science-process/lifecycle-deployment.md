---
title: Team Data Science Process yaşam döngüsü dağıtım aşaması
description: Hedefleri, görevleri ve teslim edilebilirler için veri bilimi projelerinizi dağıtım aşaması
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
ms.openlocfilehash: 00710183828892c81d3ea887e4394237288eb6bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303545"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Team Data Science Process yaşam döngüsü dağıtım aşaması

Bu makalede, hedeflerinizi, görevleri ve teslim edilebilirler ile Team Data Science işlem (TDSP) dağıtımı ilişkili özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

   1. **İşin gereksinimlerini anlama**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsü görsel bir temsilini şu şekildedir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
Bir üretim ya da üretim ortamına benzer son kullanıcı onay için bir işlem hattıyla veri modelleri dağıtın. 

## <a name="how-to-do-it"></a>Nasıl yapılır
Bu aşamada ele ana görev:

**Modeli hazır hale getirmek**: Üretim ya da üretim ortamına benzer uygulama tüketim modelini ve işlem hattı dağıtın.

### <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme
Bir dizi iyi gerçekleştirilip modelleri oluşturduktan sonra bunları kullanmak, diğer uygulamalar için çalışır hale getirebilirsiniz. İş gereksinimlerine bağlı olarak, Öngörüler, gerçek zamanlı olarak veya toplu olarak yapılır. Modelleri dağıtmak için bunları açık bir API arabirimi ile kullanıma gerekir. Arabirim modelinin çeşitli uygulamalardan gibi kolayca kullanılabilmesini sağlar:

   * Çevrimiçi Web siteleri
   * Elektronik tablolar 
   * Panolar
   * Satır iş kolu uygulamaları 
   * Arka uç uygulamaları 

Modeli kullanıma hazır hale getirme ile bir Azure Machine Learning web hizmeti örnekleri için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md). Telemetri ve üretim modeli ve dağıttığınız veri işlem hattını izleme oluşturmak için en iyi bir uygulamadır. Bu yöntem, raporlama ve sorun giderme sonraki sistem durumu ile yardımcı olur.  

## <a name="artifacts"></a>Yapıtlar

* Sistem durumunu ve anahtar ölçümlerin gösteren bir durum Panosu
* Dağıtım ayrıntıları son modelleme raporla
* Nihai çözüm mimarisi belge


## <a name="next-steps"></a>Sonraki adımlar

TDSP yaşam döngüsü içinde her adım için bağlantılar şunlardır:

   1. [İşin gereksinimlerini anlama](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

İşlemin belirli senaryolar için tüm adımları gösteren uçtan uca tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantıları ve küçük resim açıklamaları senaryolarıyla bir listesini sağlar. İzlenecek bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut, şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir. 

Adımlar Azure Machine Learning Studio'nun TDSPs yürütmek nasıl bir örnekleri için bkz: [Azure Machine Learning ile TDSP kullanma](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
