---
title: Azure Batch izleyin | Microsoft Docs
description: Azure Batch için Azure izleme Hizmetleri, ölçümleri, tanılama günlükleri ve diğer izleme özellikleri hakkında bilgi edinin.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: ee483c19aa59ca98226f77a5e56b1ee4eb4dede5
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543419"
---
# <a name="monitor-batch-solutions"></a>Batch çözümlerini izleme

Hizmetler, Araçlar ve API'ler, Batch çözümlerinizi izlemek için çeşitli Azure ve Batch hizmeti sağlar. Bu genel bakış makalesi, ihtiyaçlarınıza uygun bir izleme yaklaşımı seçmenize yardımcı olur.

Azure bileşenleri ve Hizmetleri Azure kaynakları izlemek için kullanılabilen genel bakış için bkz: [izleme Azure uygulamalarını ve kaynaklarını](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Abonelik düzeyinde izleme

Batch hesapları içerir, abonelik düzeyinde [Azure etkinlik günlüğü](../azure-monitor/platform/activity-logs-overview.md) operasyonel olay verileri toplayan [çeşitli kategorileri](../azure-monitor/platform/activity-logs-overview.md#categories-in-the-activity-log).

Batch hesabı için özellikle, etkinlik günlüğü hesabı oluşturma ve silme ve anahtar yönetimi ile ilgili olayları toplar.

Etkinlik günlüğünüzü olaylarını almak için bir yolu, Azure portalı kullanmaktır. Tıklayın **tüm hizmetleri** > **etkinlik günlüğü**. Veya Azure CLI, PowerShell cmdlet'leri veya Azure İzleyici REST API'sini kullanarak olayları için sorgu. Ayrıca, Etkinlik günlüğünü dışarı aktarma veya yapılandırma [etkinlik günlüğü uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Batch hesabı düzeyinde izleme

Her Batch hesabı özelliklerini kullanarak izleme [Azure İzleyici](../azure-monitor/overview.md). Azure İzleyici toplar [ölçümleri](../azure-monitor/platform/data-collection.md#metrics) ve isteğe bağlı olarak [tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md) havuzlar, işler ve görevler gibi Batch hesabı düzeyinde kapsamlı kaynaklar. Toplama ve bu verileri el ile veya programlama yoluyla kullanmak Batch hesabınızdaki etkinlikleri izlemek ve sorunlarını tanılamak için. Ayrıntılar için bkz [toplu ölçümleri, uyarılar ve değerlendirme tanılama ve izleme günlüklerini](batch-diagnostics.md).
 
> [!NOTE]
> Varsayılan olarak Batch hesabınıza ek yapılandırma olmadan ölçümler kullanılabilir ve 30 günlük çalışırken geçmişini sahiptirler. Bir Batch hesabı için tanılama günlük kaydını etkinleştirmeniz gerekir ve depolamak ya da tanılama günlük verilerini işlemek için ek ücrete neden. 

## <a name="batch-resource-monitoring"></a>Batch kaynak izleme

Batch uygulamalarınızı izleyebilir veya işler, görevler, düğümleri ve havuzları da dahil olmak üzere kaynaklarınızın durumunu sorgulamak için Batch API'lerini kullanın. Örneğin:

* [Görev sayısı ve işlem düğümleri duruma göre](batch-get-resource-counts.md)
* [Sorguları listesi Batch kaynaklarını verimli bir şekilde oluşturun](batch-efficient-list-queries.md)
* [Görev bağımlılıklarını oluşturma](batch-task-dependencies.md)
* Kullanım bir [iş yöneticisi görevi](/rest/api/batchservice/job/add#jobmanagertask)
* İzleyici [görev durumu](/rest/api/batchservice/task/list#taskstate)
* İzleyici [düğüm durumu](/rest/api/batchservice/computenode/list#computenodestate)
* İzleyici [havuzu durumu](/rest/api/batchservice/pool/get#poolstate)
* İzleyici [havuzu hesap kullanımı](/rest/api/batchservice/pool/listusagemetrics)
* [Sayısı havuz düğümleri duruma göre](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>VM performans sayaçları ve uygulama izleme

* [Application Insights](../application-insights/app-insights-overview.md) programlı olarak kullanılabilirlik, performans ve kullanım toplu işleri ve görevleri izlemek için kullanabileceğiniz bir Azure hizmetidir. Kolayca get performans sayaçları, kümedeki düğümlerin (VM'ler) ve Vm'leri dışına görevler için özel bilgi işlem. 

  Bir örnek için bkz. [izleme ve hata ayıklama Application Insights ile bir Batch .NET uygulaması](monitor-application-insights.md) ve eşlik eden [kod örneği](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights).

  > [!NOTE]
  > Application Insights'ı kullanmak için ek ücrete neden. Bkz: [fiyatlandırma seçenekleri](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* [Batch Explorer](https://github.com/Azure/BatchExplorer) oluşturma, hata ayıklama ve izleme Azure Batch uygulamalarıyla yardımcı olmak için bir ücretsiz ve zengin özellikli tek başına istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchExplorer/) indirebilirsiniz. İsteğe bağlı olarak Batch çözümünüzü yapılandırın [Application Insights verilerini görüntülemek](https://github.com/Azure/batch-insights) Batch Gezgini VM performans sayaçları gibi.


## <a name="next-steps"></a>Sonraki adımlar

* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* Daha fazla bilgi edinin [tanılama günlüğüne kaydetme](batch-diagnostics.md) Batch ile.