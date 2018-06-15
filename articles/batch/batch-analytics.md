---
title: Azure toplu analizi | Microsoft Docs
description: Azure toplu analizi için başvuru.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: 4c81acb282b24bbd899227c4dcc449fed4ba6c7d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30312686"
---
# <a name="batch-analytics"></a>Batch Analizi
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