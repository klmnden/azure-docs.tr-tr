---
title: Azure Batch analizi | Microsoft Docs
description: Azure Batch analizi referansı.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 999c3037196044250b8a12d6b6b380553e58c6ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60721564"
---
# <a name="batch-analytics"></a>Batch Analizi
Toplu analiz konularındaki olayları ve Uyarıları Batch hizmet kaynakları için kullanılabilir yönelik başvuru bilgileri içerir.

Bkz: [Azure Batch tanılama günlüğüne kaydetme](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) etkinleştirme ve Batch tanılama günlükleri'ni kullanma hakkında daha fazla bilgi.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Azure Batch hizmeti, belirli Batch kaynaklarını ömrü boyunca aşağıdaki tanılama günlüğü olaylar gönderir.

**Hizmet günlüğü olayları**
* [Havuz oluşturma](batch-pool-create-event.md)
* [Havuz silme başlangıç](batch-pool-delete-start-event.md)
* [Tam havuzunu Sil](batch-pool-delete-complete-event.md)
* [Havuz yeniden boyutlandırma başlangıç](batch-pool-resize-start-event.md)
* [Tam havuzunu boyutlandırma](batch-pool-resize-complete-event.md)
* [Görev Başlat](batch-task-start-event.md)
* [Görev tamamlandı](batch-task-complete-event.md)
* [Görev başarısız](batch-task-fail-event.md)