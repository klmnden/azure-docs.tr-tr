---
title: Azure toplu işlem izleme | Microsoft Docs
description: Azure toplu işlem için Azure izleme Hizmetleri, ölçümleri, tanılama günlüklerini ve diğer izleme özellikleri hakkında bilgi edinin.
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
ms.openlocfilehash: 29ac86ed5c744d37150b0f1b2db17f60306fe77e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789955"
---
# <a name="monitor-batch-solutions"></a>Batch çözümlerini izleme

Azure ve Batch hizmeti hizmetleri, Araçlar ve API'ler, Batch çözümlerinizi izlemek için bir aralığı belirtin. Bu genel bakış makalesi gereksinimlerinize uyan bir izleme yaklaşımı seçmenize yardımcı olur.

Azure bileşenlerini ve hizmetlerini Azure kaynakları izlemek kullanılabilen genel bakış için bkz: [izleme Azure uygulamaları ve kaynakları](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Abonelik düzeyi izleme

Toplu işlem hesaplarını içerir, abonelik düzeyinde [Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) operasyonel olay veri toplar [birkaç kategorisi](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#categories-in-the-activity-log).

Batch hesapları için özellikle, etkinlik günlüğü hesap oluşturma ve silme ve anahtar yönetimi için ilgili olayları toplar.

Olaylar, etkinlik günlüğünden almak için bir yol Azure Portalı'nı kullanmaktır. Tıklatın **tüm hizmetleri** > **etkinlik günlüğü**. Veya Azure CLI, PowerShell cmdlet'leri ya da Azure İzleyici REST API'sini kullanarak olayları için sorgu. Ayrıca, etkinlik günlüğü dışarı aktarma veya yapılandırma [etkinlik günlüğü uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Toplu işlem hesabı düzeyi izleme

Özelliklerini kullanarak her Batch hesabını izleme [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure İzleyici toplar [ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve isteğe bağlı olarak [tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) havuzlar, işler ve görevler gibi Batch hesabı düzeyinde kapsamlı kaynaklar için. Toplamak ve bu verileri el ile veya programlama kullanmak Batch hesabınızdaki etkinlikleri izlemek ve sorunlarını tanılamak için. Ayrıntılar için bkz [toplu ölçümleri, uyarılar ve tanılama değerlendirme ve izleme günlükleri](batch-diagnostics.md).
 
> [!NOTE]
> Ölçümleri ek yapılandırma olmadan Batch hesabınızdaki varsayılan olarak kullanılabilir ve 30 günlük çalışırken geçmişi vardır. Bir toplu işlem hesabı için tanılama günlük kaydını etkinleştirmeniz gerekir ve depolamak ya da tanılama günlük verilerini işlemek için ek ücrete neden olabilir. 

## <a name="batch-resource-monitoring"></a>Batch kaynak izleme

Toplu iş uygulamalarınız, izlemek ve işler, görevler, düğümleri ve havuzları da dahil olmak üzere, kaynakların durumunu sorgulamak için Batch API'lerini kullanın. Örneğin:

* [Görevleri duruma göre sayma](batch-get-task-counts.md)
* [Sorguları listesi Batch kaynaklarını verimli bir şekilde oluşturun](batch-efficient-list-queries.md)
* [Görev bağımlılıkları oluşturma](batch-task-dependencies.md)
* Kullanım bir [iş yöneticisi görevi](/rest/api/batchservice/job/add#jobmanagertask)
* İzleyici [görev durumu](/rest/api/batchservice/task/list#taskstate)
* İzleyici [düğüm durumu](/rest/api/batchservice/computenode/list#computenodestate)
* İzleyici [havuzu durumu](/rest/api/batchservice/pool/get#poolstate)
* İzleyici [havuzu hesabı kullanımı](/rest/api/batchservice/pool/listusagemetrics)
* [Count havuzu düğümleri duruma göre](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>VM performans sayaçları ve uygulama izleme

* [Application Insights](../application-insights/app-insights-overview.md) program aracılığıyla kullanılabilirliği, performansı ve toplu işler ve görevler kullanımını izlemek için kullanabileceğiniz bir Azure hizmetidir. Kolayca get performans sayaçlarını düğümler (VM'ler) ve sanal makineleri kapatmayı görevler için özel bilgi işlem. 

  Bir örnek için bkz: [izleme ve hata ayıklama Batch .NET uygulama Application Insights ile](monitor-application-insights.md) ve eşlik eden [kod örneği](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights).

  > [!NOTE]
  > Application Insights kullanmak için ek ücrete neden olabilir. Bkz: [seçenekleri fiyatlandırma](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* [BatchLabs](https://github.com/Azure/BatchLabs), Azure Batch uygulamalarıyla ilgili oluşturma, hata ayıklama ve izleme işlemlerini gerçekleştirmenize yardımcı olan ücretsiz, gelişmiş özelliklere sahip ve tek başına kullanılan bir istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchLabs/) indirebilirsiniz. İsteğe bağlı olarak Batch çözümünüzü yapılandırın [Application Insights verileri görüntülemek](https://github.com/Azure/batch-insights) BatchLabs VM performans sayaçları gibi.


## <a name="next-steps"></a>Sonraki adımlar

* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* Daha fazla bilgi edinmek [tanılama günlük](batch-diagnostics.md) toplu ile.