---
title: "Takım veri bilimi işlem yaşam döngüsü - Azure dağıtım aşaması | Microsoft Docs"
description: "Hedefler, görevler ve veri bilimi projelerinizi dağıtım aşaması için teslim edilebilir."
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
ms.openlocfilehash: b49b3ab696c22928c5f5a4566e059345fd810588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deployment"></a>Dağıtım

Hedefler, görevler, bu konuda özetlenir ve sonuçlara ilişkili **dağıtım** takım veri bilimi işleminin. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

* **İş anlama**
* **Veri alımı ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel gösterimi işte **takım veri bilimi işlemi yaşam döngüsü**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Hedef
* Veri ardışık modelleriyle, bir üretim veya üretim benzeri bir ortam için son kullanıcı kabul dağıtılır. 

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele ana görevi:

* **Model faaliyete**: bir üretim veya uygulama tüketimi için üretim benzeri bir ortam modeli ve ardışık düzen dağıtın.

### <a name="41-operationalize-a-model"></a>4.1 bir model faaliyete
İyi gerçekleştirmek modelleri kümesi oluşturduktan sonra bunlar kullanacak diğer uygulamalar için operationalized. İş gereksinimlerine bağlı olarak, tahminlerin gerçek zamanlı veya toplu olarak yapılır. Modeller, açık bir API arabirimiyle göstererek dağıtılır. Arabirim kolayca çevrimiçi Web siteleri, elektronik tablolar, panolar veya iş ve arka uç uygulamalarının satır gibi çeşitli uygulamalardan tüketilmesi modeli sağlar. Bir Azure Machine Learning web hizmeti ile modeli operationalization örnekleri için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md). Telemetri ve üretim modeli ve dağıtılan veri ardışık düzen izleme oluşturmak için de en iyi bir uygulamadır. Bu uygulama bildirimi ve sorun giderme sonraki sistem durumuyla yardımcı olur.  

## <a name="artifacts"></a>Yapıtlar
* Sistem durumu ve anahtar ölçümleri durum Panosu.
* Dağıtım ayrıntıları raporla son modelleme.
* Son çözüm mimarisi belgesi.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her takım veri bilimi işlemi yaşam döngüsü adımda bağlantıları verilmiştir:

* [1. İş anlama](lifecycle-business-understanding.md)
* [2. Veri alımı ve anlama](lifecycle-data.md)
* [3. Modelleme](lifecycle-modeling.md)
* [4. Dağıtım](lifecycle-deployment.md)
* [5. Müşteri kabulü](lifecycle-acceptance.md)

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) konu. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu.