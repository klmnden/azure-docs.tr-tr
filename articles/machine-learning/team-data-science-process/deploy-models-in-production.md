---
title: Üretim - Azure Machine Learning modellerini dağıtma | Microsoft Docs
description: Üretim iş kararları alımında etkin bir rol oynar etkinleştirmeden modelleri dağıtmak nasıl.
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
ms.date: 11/30/2017
ms.author: deguhath
ms.openlocfilehash: ce12247f9b3e4ad3ffff307ee8848662ed7f5046
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="deploy-models-in-production"></a>Üretimde modelleri dağıtma

Üretim dağıtımı etkin bir rol bir işletmede yürütmek bir model sağlar. Dağıtılan modelden tahminleri iş kararları için kullanılabilir.

## <a name="production-platforms"></a>Üretim platformları
Çeşitli yaklaşımlar ve platformları, modelleri üretime sokmak için vardır. Bazı seçenekler şunlardır:


- [Azure Machine Learning modeli dağıtımında](../desktop-workbench/model-management-overview.md)
- [SQL Server'daki bir model dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


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
 


