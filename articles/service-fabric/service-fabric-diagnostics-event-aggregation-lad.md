---
title: Linux Azure Tanılama ile Azure Service Fabric olay toplama | Microsoft Docs
description: Toplama ve izleme ve tanılama Azure Service Fabric kümelerinin LAD kullanarak olaylarını toplama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: 212158d9a76fa2e49c60be0b5c52f281497c155b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669427"
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Olay toplama ve Linux Azure Tanılama'yı kullanarak koleksiyon
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Bir Azure Service Fabric kümesi çalıştırırken, merkezi bir konumda tüm düğümlerden günlükleri toplamak için iyi bir fikirdir. Günlükleri sahip merkezi bir konumda, kümenizdeki sorunları veya uygulamalar ve hizmetler, kümede çalışan sorunları gidermek ve çözümlemenize yardımcı olur.

Karşıya yükleme ve günlükleri toplamak için bir yolu, günlükleri, Azure Depolama'ya yükler ve ayrıca Azure Application Insights veya olay hub'larına günlükleri gönderme seçeneği olan Linux Azure tanılama (LAD) uzantısı kullanmaktır. Olayları depolamadan okuyun ve bunları bir analiz platformu ürün gibi yerleştirmek için bir dış işlem kullanabilirsiniz [Azure İzleyici günlükleri](../log-analytics/log-analytics-service-fabric.md) veya başka bir günlük ayrıştırma çözümü.

## <a name="log-and-event-sources"></a>Günlük ve olay kaynakları

### <a name="service-fabric-platform-events"></a>Service Fabric platform olaylarına
Service Fabric aracılığıyla birkaç Giden kutusu günlükleri yayan [LTTng](https://lttng.org)işletimsel olaylar veya çalışma zamanı olayları dahil olmak üzere. Bu günlükler, kümenin Resource Manager şablonu belirten konumda depolanır. Alma veya depolama hesabı ayrıntıları belirlemek için etiketini ara **AzureTableWinFabETWQueryable** ve Ara **StoreConnectionString**.

### <a name="application-events"></a>Uygulama olayları
 Olaylar, belirtildiği gibi uygulamaların ve hizmetlerin koddan yazılımınızı işaretlerken yayılan. Metin tabanlı günlük dosyalarını--örneğin LTTng Yazar herhangi bir günlük çözümü kullanabilirsiniz. Daha fazla bilgi için uygulamanızı izlemeyi LTTng belgelerine bakın.

[İçinde bir yerel makine dağıtım kurulumunda Hizmetleri izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).

## <a name="deploy-the-diagnostics-extension"></a>Tanılama uzantısını dağıtma
İlk adımı günlüklerinin toplanması tanılama uzantısını her bir Service Fabric kümesindeki Vm'leri dağıtmaktır. Tanılama uzantısını her VM'de günlükleri toplar ve bunları belirttiğiniz depolama hesabına yükler. 

Kümeyi oluşturmanın bir parçası tanılama uzantısını kümesindeki vm'lere dağıtmak için ayarlanmış **tanılama** için **üzerinde**. Kümeyi oluşturduktan sonra Resource Manager şablonunda gerekli değişiklikleri yapmak zorunda portalı kullanarak bu ayarı değiştiremezsiniz.

Bu, belirtilen günlük dosyalarını izlemek için LAD aracıyı yapılandırır. Yeni bir satır, dosyanın sonuna olduğunda, belirttiğiniz depolama alanına (tablo) gönderilen bir syslog girişi oluşturur.


## <a name="next-steps"></a>Sonraki adımlar

1. Sorun giderme sırasında incelemeniz hangi olayların daha ayrıntılı olarak anlamak için bkz [LTTng belgeleri](https://lttng.org/docs) ve [kullanarak LAD](https://docs.microsoft.com/azure/virtual-machines/extensions/diagnostics-linux).
2. [Log Analytics aracısını ayarlama](service-fabric-diagnostics-event-analysis-oms.md) ölçümleri toplamak amacıyla, kümenizde dağıttığınız kapsayıcılarını izleme ve günlüklerinizi görselleştirin 
