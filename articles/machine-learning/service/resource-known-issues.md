---
title: Azure Machine Learning hizmeti için bilinen sorunlar ve sorun giderme
description: Geçici çözümler, bilinen sorunların bir listesini almak ve sorun giderme
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 27e73bc75c5f04190bad3dab49c1d46782982a18
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034153"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning hizmeti
 
Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning hizmeti kullanılırken karşılaşılan hatalar yardımcı olur. 


## <a name="databricks-and-azure-machine-learning"></a>Databricks ve Azure Machine Learning

**Databricks kümesini öneri:** 

Python 3 v4.x olarak Azure Databricks kümenizi oluşturun. Yüksek eşzamanlılık küme öneririz.
 
**AML SDK yükleme hatası Databricks üzerinde daha fazla paketleri yüklendiğinde** gibi bazı paketler `psutil upgrade libs`, çakışmaları neden olabilir. Yükleme hataları önlemek için paketleri dondurma LIB sürümüyle yükleyin. Bu sorun için Databricks ilgili ve AML SDK ilişkili değil. Örnek:
```
pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
```


## <a name="gather-diagnostics-information"></a>Tanılama bilgilerini toplama
Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyaları burada Canlı aşağıda verilmiştir:

## <a name="resource-quotas"></a>Kaynak kotaları

Hakkında bilgi edinin [kaynak kotaları](how-to-manage-quotas.md) Azure Machine Learning'i kullanmaya çalışırken hatalarla karşılaşabilirsiniz.

## <a name="get-more-support"></a>Daha fazla destek alın

Destek isteği gönderin ve teknik destek, forumlar ve diğer Yardım alın. [Daha fazla bilgi edinin...](support-for-aml-services.md)
