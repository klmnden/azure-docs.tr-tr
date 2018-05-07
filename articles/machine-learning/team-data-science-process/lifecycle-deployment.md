---
title: Takım veri bilimi işlemi yaşam döngüsü - Azure dağıtım aşaması | Microsoft Docs
description: Hedefler, görevler ve veri bilimi projelerinizi dağıtım aşaması için teslim edilebilir
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: a56aa9c317703186e1ba2ab6bed9aa1875d318c2
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="deployment"></a>Dağıtım

Bu makalede hedefleri, görevler ve takım veri bilimi işlem (TDSP) dağıtımla ilişkili sonuçlara özetlenmektedir. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

   1. **İş anlama**
   2. **Veri alımı ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

Görsel bir TDSP yaşam döngüsü şöyledir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
Veri ardışık modelleriyle üretim veya üretim benzeri bir ortam için son kullanıcı kabul dağıtın. 

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele ana görevi:

**Model faaliyete**: bir üretim veya uygulama tüketimi için üretim benzeri bir ortam modeli ve ardışık düzen dağıtın.

### <a name="operationalize-a-model"></a>Bir modeli kullanıma hazır hale getirme
İyi gerçekleştirmek modelleri kümesi oluşturduktan sonra bunları kullanmak diğer uygulamalar için faaliyete geçirebilirsiniz. İş gereksinimlerine bağlı olarak, tahminlerin gerçek zamanlı veya toplu olarak yapılır. Modelleri dağıtmak için bunları açık API arabirimiyle açın. Arabirim kolayca çeşitli uygulamalardan gibi tüketilmesi modeli sağlar:

   * Çevrimiçi Web siteleri
   * Elektronik tablolar 
   * Panolar
   * Satır iş kolu uygulamaları 
   * Arka uç uygulamaları 

Bir Azure Machine Learning web hizmeti ile modeli operationalization örnekleri için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md). Telemetri ve üretim modeli ve dağıttığınız veri ardışık düzen izleme oluşturmak için en iyi bir uygulamadır. Bu uygulama bildirimi ve sorun giderme sonraki sistem durumuyla yardımcı olur.  

## <a name="artifacts"></a>Yapıtlar

* Sistem durumu ve anahtar ölçümleri görüntüler durumu Panosu
* Dağıtım ayrıntıları son modelleme raporla
* Son çözüm mimarisi belge


## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her adımda TDSP yaşam döngüsü bağlantıları verilmiştir:

   1. [İş anlama](lifecycle-business-understanding.md)
   2. [Veri alımı ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımlar gösteren baştan sona tam talimatlara sunuyoruz. [Örnek izlenecek yollar](walkthroughs.md) makale bağlantılar ve küçük resim açıklamaları senaryolarla listesini sağlar. İzlenecek yollar bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir. 

Azure Machine Learning Studio kullanan TDSPs adımları yürütmek nasıl örnekleri için bkz: [TDSP Azure Machine Learning ile kullanmak](http://aka.ms/datascienceprocess).
