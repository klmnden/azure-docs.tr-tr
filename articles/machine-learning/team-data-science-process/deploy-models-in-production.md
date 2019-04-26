---
title: Üretimde - Team Data Science Process modelleri dağıtma
description: Üretim iş kararları etkin bir rol oynamaya dönemlik modelleri dağıtma
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1a75c842989cfbaf7bb1880831fda2bc6994622b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60238341"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>Modelleri, iş kararları etkin bir rol oynamaya üretime dağıtın.

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
