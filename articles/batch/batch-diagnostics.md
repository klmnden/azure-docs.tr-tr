---
title: Toplu olaylarını - Azure tanılama günlük kaydını etkinleştir | Microsoft Docs
description: Kayıt ve havuzları ve görevler gibi Azure Batch hesabı kaynaklarına için tanılama günlük olayları analiz edin.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c41c8c9f8fd9302c610ce356b0485e33ea3c967d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a>Tanılama değerlendirme ve toplu çözümlerini izleme olayları günlüğe kaydedin

Çok sayıda Azure hizmetleriyle gibi Batch hizmeti günlüğü olaylarını bazı kaynaklar için kaynak ömrü boyunca yayar. Azure Batch havuzları ve görevler gibi kaynaklar için kayıt olayları tanılama günlüklerini etkinleştirme ve ardından günlükleri tanılama değerlendirme ve izleme için kullanın. Havuzu gibi olaylar oluşturmak, havuzu silme, görev Başlat, görev tamamlandı ve diğerleri toplu tanılama günlüklerine eklenir.

> [!NOTE]
> Batch hesabı kaynaklarını kendileri için olayları günlüğe kaydetmeyi bu makalede ele, değil iş ve görev çıktı verileri. İşlerini ve görevleri çıkış veri depolama hakkında daha fazla bilgi için bkz [kalıcı Azure Batch iş ve Görev çıkış](batch-task-output.md).
> 
> 

## <a name="prerequisites"></a>Önkoşullar
* [Azure toplu işlem hesabı](batch-account-create-portal.md)
* [Azure Depolama hesabınız](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  Toplu iş tanılama günlüklerini kalıcı hale getirmek için Azure günlükleri depolayacağınız bir Azure Storage hesabı oluşturmanız gerekir. Bu depolama hesabı belirtin olduğunda, [tanılama günlük kaydını etkinleştir](#enable-diagnostic-logging) Batch hesabınıza. Depolama hesabı belirtin günlük toplama etkinleştirdiğinizde başvurulan bir bağlantılı depolama hesabı ile aynı değil [uygulama paketleri](batch-application-packages.md) ve [görev çıktısını kalıcılığı](batch-task-output.md) makaleleri.
  
  > [!WARNING]
  > Olduğunuz **ücret** Azure depolama hesabınızda depolanan veriler için. Bu makalede ele alınan tanılama günlükleri içerir. Bu tasarlarken göz önünde bulundurmanız, [günlük Bekletme İlkesi](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).
  > 
  > 

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme
Tanılama günlük varsayılan olarak, toplu işlem hesabı için etkin değil. Açıkça izlemek istediğiniz her toplu işlem hesabı için tanılama günlük kaydını etkinleştirmeniz gerekir:

[Tanılama günlüklerini koleksiyonunu etkinleştirme](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

Tam okumanızı öneririz [genel bakış, Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) günlüğe kaydetme, ancak çeşitli Azure Hizmetleri tarafından desteklenen günlük kategorileri etkinleştirmek yalnızca nasıl anlamak için makale. Örneğin, Azure Batch tek bir günlük kategori şu anda destekler: **hizmeti günlükleri**.

## <a name="service-logs"></a>Hizmet Günlükleri
Azure Batch hizmeti günlükleri havuzu ya da görev gibi bir toplu iş kaynak kullanım ömrü süresince Azure Batch hizmeti tarafından oluşturulan olayları içerir. Batch tarafından gösterilen her olay JSON biçiminde belirtilen depolama hesabında depolanır. Örneğin, bu bir örnek gövdesidir **havuzu oluşturma olayı**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Her olay gövdesinde belirtilen Azure depolama hesabındaki bir .json dosyası bulunur. Günlükleri doğrudan erişmek isterseniz, gözden geçirmek isteyebilirsiniz [tanılama günlüklerini şema depolama hesabındaki](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Hizmeti oturum açma olayları
Batch hizmeti şu anda aşağıdaki hizmeti oturum açma olayları gösterir. Bu makalede son güncelleştirmesinden bu yana ek olaylar eklenmemiş olabilir beri bu liste geniş kapsamlı, olmayabilir.

| **Hizmeti oturum açma olayları** |
| --- |
| [Havuz oluşturma][pool_create] |
| [Havuzu silme Başlat][pool_delete_start] |
| [Havuzu silme tamamlandı][pool_delete_complete] |
| [Havuzu yeniden boyutlandırma Başlat][pool_resize_start] |
| [Tam havuzu yeniden boyutlandırma][pool_resize_complete] |
| [Görev Başlat][task_start] |
| [Görev tamamlandı][task_complete] |
| [Görev başarısız][task_fail] |

## <a name="next-steps"></a>Sonraki adımlar
Tanılama günlük olaylarının bir Azure depolama hesabına depolama yanı sıra, Batch hizmeti günlüğü olaylarını da sağlanabilir bir [Azure olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md)ve göndermenize [Azure günlük analizi](../log-analytics/log-analytics-overview.md).

* [Akış olay hub'ları için Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  Yüksek düzeyde ölçeklenebilir bir veri alım, olay hub'ları için tanılama olayları toplu iş akışı. Olay hub'ları sonra dönüştürmek ve tüm gerçek zamanlı analiz sağlayıcısı kullanarak depolamak saniye başına milyonlarca olayı işleyebilen.
* [Günlük analizi kullanarak Azure tanılama günlüklerini çözümleme](../log-analytics/log-analytics-azure-storage.md)
  
  Tanılama günlüklerinize günlük buradan Operations Management Suite (OMS) Portalı'nda çözümlemek, veya Power BI veya Excel'den analiz için bunları dışarı aktarmak analizi gönderin.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
