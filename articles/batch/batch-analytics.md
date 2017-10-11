---
title: Azure toplu analizi | Microsoft Docs
description: "Azure toplu analizi için başvuru."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="batch-analytics"></a>Toplu analizi
Konular toplu analizi, olaylar ve Uyarılar için toplu service kaynakları kullanılabilir için başvuru bilgileri içerir.

Bkz: [Azure Batch tanılama günlük](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) etkinleştirme ve toplu tanılama günlüklerini kullanma hakkında daha fazla bilgi.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Azure Batch hizmeti aşağıdaki tanılama günlük olayları belirli Batch kaynaklarını ömrü boyunca yayar.

**Hizmeti oturum açma olayları**
* [Havuz oluşturma](batch-pool-create-event.md)
* [Havuzu silme Başlat](batch-pool-delete-start-event.md)
* [Havuzu silme tamamlandı](batch-pool-delete-complete-event.md)
* [Havuzu yeniden boyutlandırma Başlat](batch-pool-resize-start-event.md)
* [Tam havuzu yeniden boyutlandırma](batch-pool-resize-complete-event.md)
* [Görev Başlat](batch-task-start-event.md)
* [Görev tamamlandı](batch-task-complete-event.md)
* [Görev başarısız](batch-task-fail-event.md)