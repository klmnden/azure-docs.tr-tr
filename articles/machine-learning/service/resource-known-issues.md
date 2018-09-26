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
ms.openlocfilehash: d84040dc440c373ae9bae6dbac7a95109a387ba7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162755"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning hizmeti
 
Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning hizmeti kullanılırken karşılaşılan hatalar yardımcı olur. 


## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

1. Databricks kümesini öneri:
   
   Python 3 v4.x olarak Azure Databricks kümenizi oluşturun. Yüksek eşzamanlılık küme öneririz.
 
1. AML SDK yükleme hatası Databricks üzerinde daha fazla paketleri yüklendiğinde.

   Gibi bazı paketler `psutil upgrade libs`, çakışmaları neden olabilir. Yükleme hataları önlemek için paketleri dondurma LIB sürümüyle yükleyin. Bu sorun için Databricks ilgili ve AML SDK ilişkili değil. Örnek:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="gather-diagnostics-information"></a>Tanılama bilgilerini toplama
Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyaları burada Canlı aşağıda verilmiştir:

## <a name="resource-quotas"></a>Kaynak kotaları

Hakkında bilgi edinin [kaynak kotaları](how-to-manage-quotas.md) Azure Machine Learning'i kullanmaya çalışırken hatalarla karşılaşabilirsiniz.

## <a name="get-more-support"></a>Daha fazla destek alın

Destek isteği gönderin ve teknik destek, forumlar ve diğer Yardım alın. [Daha fazla bilgi edinin...](support-for-aml-services.md)
