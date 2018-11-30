---
title: Üretimde - Azure Machine Learning modelleri dağıtma | Microsoft Docs
description: Üretim iş kararları etkin bir rol oynamaya dönemlik modelleri dağıtma
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 5b1614f92f7633e008f4f7176723002dc7730b15
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495964"
---
# <a name="deploy-models-in-production"></a>Üretimde modelleri dağıtma

Üretim dağıtımı, etkin bir rol bir iş yürütmek bir model sağlar. Dağıtılan bir modelde tahminleri iş kararları için kullanılabilir.

## <a name="production-platforms"></a>Üretim platformları

Çeşitli yaklaşımlar ve modelleri üretime koymak için Platform vardır. Bazı seçenekler şunlardır:

- [Modeller Azure Machine Learning hizmeti ile dağıtılacağı yeri](../service/how-to-deploy-and-where.md)
- [Bir modeli SQL Server dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Dağıtımdan önce bir üretim ortamında kullanmak için düşük Puanlama modeli, gecikme süresi sağlamak üzere vardır.
>

>[!NOTE]
>Azure Machine Learning Studio'yu kullanarak bir dağıtım için bkz [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B testi

Birden çok modelleri üretimde olduğunda yapmak yararlı olabilir [A / B testi](https://en.wikipedia.org/wiki/A/B_testing) modelleri performansını karşılaştırmak için. 
 
## <a name="next-steps"></a>Sonraki adımlar

İşlem için tüm adımları gösteren talimatlara **belirli senaryoları** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir. 
