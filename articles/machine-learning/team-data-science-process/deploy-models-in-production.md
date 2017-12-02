---
title: "Üretim - Azure Machine Learning modellerini dağıtma | Microsoft Docs"
description: "Üretim iş kararları alımında etkin bir rol oynar etkinleştirmeden modelleri dağıtmak nasıl."
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
ms.date: 11/30/2017
ms.author: bradsev;
ms.openlocfilehash: c7be9eda2d6f37951eb120250861cf4a39b0141b
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-models-in-production"></a>Üretim modellerinde dağıtma

Üretim dağıtımı etkin bir rol bir işletmede yürütmek bir model sağlar. Dağıtılan modelden tahminleri iş kararları için kullanılabilir.

## <a name="production-platforms"></a>Üretim platformları
Çeşitli yaklaşımlar ve platformları, modelleri üretime sokmak için vardır. Bazı seçenekler şunlardır:


- [Azure Machine Learning modeli dağıtımında](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)
- [SQL Server'daki bir model dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning sunucu](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


>[!NOTE]
>Dağıtımdan önce modeli Puanlama gecikme süresi üretimde kullanmak üzere düşük güvence altına almaya sahip.
>


>[!NOTE]
>Azure Machine Learning Studio kullanarak dağıtım için bkz: [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B testi
Birden fazla modeli üretimde olduğunda gerçekleştirmek yararlı olabilir [A / B testi](https://en.wikipedia.org/wiki/A/B_testing) modelleri performansını karşılaştırmak için. 

 
## <a name="next-steps"></a>Sonraki adımlar

İşlem için tüm adımları gösteren talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 
 


