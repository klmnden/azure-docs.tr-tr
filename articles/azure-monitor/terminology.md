---
title: Azure İzleyici terminolojisi güncelleştirmeleri | Microsoft Docs
description: Azure izleme hizmetlerine yapılan son terminolojisi değişiklikler açıklanmaktadır.
author: bwren
manager: carmonm
editor: tysonn
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2019
ms.author: bwren
ms.openlocfilehash: 8f645f7d569546a8362d0149806a2b4636567fd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61086761"
---
# <a name="azure-monitor-naming-and-terminology-changes"></a>Azure İzleyici adlandırma ve terminoloji değişiklikleri
Önemli değişiklikler için Azure İzleyici kısa bir süre önce Azure müşterileri için izlemeyi basitleştirmek için birleştirilmiş farklı hizmetlerle yapıldı. Bu makalede, en son adı ve Azure İzleyici belgeleri terminolojisi değişiklikleri açıklar.

## <a name="february-2019---log-analytics-terminology"></a>Şubat 2019 - Log Analytics terminolojisi
Azure İzleyici farklı hizmetler birleştirme sonra sizi daha iyi Azure İzleyici hizmeti ve farklı bileşenlerini açıklamak için belgelerimize terminolojisinde değiştirerek sonraki adıma yönlendiriyoruz. 

### <a name="log-analytics"></a>Log Analytics
Azure İzleyici günlük veri olmasına rağmen bir Log Analytics çalışma alanında depolanan ve yine de toplanır ve aynı Log Analytics hizmeti tarafından çözümlenen ancak biz terimi değiştirme _Log Analytics_ için birçok yerde _Azure İzleyicisi_ . Bu terim daha iyi Azure İzleyici'de, rolü yansıtır ve daha iyi tutarlılık sağlar [Azure İzleyicisi'nde ölçümler](platform/data-platform-metrics.md).

Terim _günlük analizi_ yazma ve sorgular çalıştırma ve günlük verilerini analiz etmek için kullanılan Azure portalının sayfası artık öncelikli olarak uygulanır. Bu işlev eşdeğerdir [ölçüm Gezgini](platform/metrics-charts.md), ölçüm verilerini analiz etmek için kullanılan Azure portal sayfasındaki olduğu.

### <a name="log-analytics-workspaces"></a>Log Analytics çalışma alanları
[Çalışma alanları](platform/manage-access.md) günlük verilerini Azure İzleyici'de yine de adlandırılır Log Analytics çalışma alanları tutun. **Log Analytics** menü Azure Portalı'ndaki adlandırıldı **Log Analytics çalışma alanları** ve yerdir, [yeni çalışma alanları oluşturma](learn/quick-create-workspace.md) ve veri kaynaklarını yapılandıracaksınız. Günlüklerinizi ve diğer izleme verilerinin analiz **Azure İzleyici** ve çalışma alanınızda yapılandırma **Log Analytics çalışma alanları**.

### <a name="management-solutions"></a>Yönetim çözümleri
[Yönetim çözümleri](insights/solutions.md) için adlandırılmış _izleme çözümleri_, işlevleri daha iyi açıklar.


## <a name="august-2018---consolidation-of-monitoring-services-into-azure-monitor"></a>Ağustos 2018 - birleştirme Hizmetleri Azure İzleyici ile izleme
Log Analytics ve Application Insights izleme Azure kaynaklarını ve karma ortamlar için tek bir tümleşik deneyim sağlamak için Azure İzleyici ile birleştirilmiş. Bu hizmetleri hiçbir işlevsellik kaldırıldı ve bunlar her zaman kaybı ya da herhangi bir özellik'ın güvenliğinin tamamladınız aynı senaryoları kullanıcılar gerçekleştirebilir.

Bu hizmetlerin her biri için belgelere içeriği tek bir kümesi için Azure İzleyici birleştirilmiştir. Bu izleme belirli bir senaryo için tüm içeriği tek bir konumda birden fazla içerik başvurmak zorunda aksine bulma, okuyucu yardımcı olur. Birleştirilmiş hizmetini geliştikçe içeriği daha tümleşik hale.

Yeniden konumlandırıldığında Azure İzleyicisi'nin özelliklerini dikkate bölümü Log analytics'in aracıları ve görünümler gibi diğer özellikleri de. İşlevleri deneyimlerini Azure portalında için olası geliştirmeleri dışındaki değişmedi.


## <a name="april-2018---retirement-of-operations-management-suite-brand"></a>Nisan 2018 - marka Operations Management Suite devre dışı bırakma
Operations Management Suite (OMS), aşağıdaki Azure Yönetim Hizmetleri Lisans amaçları için paketleme şöyleydi:

- Application Insights
- Azure Otomasyonu
- Azure Backup
- Log Analytics
- Site Recovery

[Yeni fiyatlandırma bu hizmetler için sunulmuş](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/), OMS paketleme artık yeni müşteriler için kullanılabilir. OMS parçası olan hizmetlerin hiçbiri, yukarıda açıklanan Azure İzleyici ile birleştirme dışında değiştirildi. 




## <a name="next-steps"></a>Sonraki adımlar

- Okuma bir [Azure İzleyicisi'ne genel bakış](overview.md) farklı bileşenleri ve özelliklerini açıklar.
- Hakkında bilgi edinin [OMS portalında geçişin](../log-analytics/log-analytics-oms-portal-transition.md).