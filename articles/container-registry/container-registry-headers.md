---
title: "Azure kapsayıcı kayıt defteri depoları | Microsoft Docs"
description: "Azure kapsayıcı kayıt defteri depoları için Docker görüntüleri kullanma"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 2090d4c951e2261529bf1b7b361510d5822060a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-container-registry-repositories"></a>Azure kapsayıcı kayıt defteri depoları

Azure kapsayıcı kayıt defterleri, hizmetleri ve orchestrators birçok ile uyumludur. ACR kullanıldığı aracıları ve kaynak hizmetleri izlemek kolaylaştırmak için biz Docker üstbilgi alanı Docker.config dosyasında kullanmaya.



## <a name="viewing-repositories-in-the-portal"></a>Portalda depoları görüntüleme

ACR üstbilgi biçimi izleyin:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Bulut: Azure, Azure yığın veya diğer kamu veya Ülkeye özel Azure bulut. Azure yığını ve kamu Bulutlar şu anda desteklenmemektedir karşın, bu parametre gelecekteki destek sağlar.
* Hizmet: hizmetin adını.
* Optionalservicename: isteğe bağlı bir parametre alt servisleri ile ya da bir SKU belirlemek için hizmetler için (örn: web uygulamaları Azure/app-service/web-apps ile karşılık gelir).

İş ortağı Hizmetleri ve orchestrators bizim telemetri ile yardımcı olmak için özel üstbilgi değerleri kullanmak için önerilir. Kullanıcılar, bu nedenle isterlerse başlığına geçirilen değeri değiştirebilirsiniz.

"X-Meta-kaynak-Client" alanını doldurmak için kullanılacak ACR ortakları istiyoruz değerleri aşağıdaki şunlardır:

| Hizmet Adı              | Üstbilgi                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | Azure/işlem/azure-kapsayıcı-hizmeti |
| Uygulama hizmeti - Web uygulamaları    | Azure/app-service/web-apps            |
| Uygulama hizmeti - Logic Apps  | Azure/app-service/logic-apps          |
| Batch                     | işlem/Azure/toplu                   |
| Bulut konsol             | Bulut/Azure-konsol                   |
| İşlevler                 | işlem/Azure/işlevleri               |
| Nesnelerin interneti - Hub  | IOT/Azure/hub                         |
| HDInsight                 | Veri/Azure/hdınsight                  |
| Jenkins                   | Azure/jenkins                         |
| Machine Learning          | Azure/data/machile-öğrenme           |
| Service Fabric            | Azure/işlem/service-yapı          |
| VSTS                      | Azure/vsts                            |


## <a name="next-steps"></a>Sonraki adımlar
[Kayıt defterleri ve desteklenen Hizmetleri ve orchestrators hakkında daha fazla bilgi edinin](container-registry-intro.md)
