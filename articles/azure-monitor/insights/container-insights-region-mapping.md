---
title: Bölge eşleştirmeleri kapsayıcılar için Azure İzleyici
description: Bu makalede, Azure İzleyici'kapsayıcıları, Log Analytics çalışma alanı ve özel ölçümler arasında desteklenen bölge eşleştirmeleri açıklanır.
services: azure-monitor
ms.service: azure-monitor
ms.workload: infrastructure-services
author: mgoedtel
ms.author: magoedte
ms.date: 06/26/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 481a2a400be4e983e0a2337a200324061494efa1
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518085"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici tarafından desteklenen bölge eşlemeleri

 Azure İzleyici gönderilen özel ölçümleri toplamaya ve kapsayıcılar için Azure İzleyici etkinleştirirken, yalnızca belirli bölgelerde bir Log Analytics çalışma alanı ve bir AKS kümesi bağlamak için desteklenir.

## <a name="log-analytics-workspace-supported-mappings"></a>Log Analytics çalışma alanı eşlemeleri desteklenir

AKS kümesi kaynakları veya Log Analytics çalışma alanı, başka bölgelerde bulunabilir ve bizim eşlemeler aşağıdaki tabloda gösterilmektedir.

|**AKS kümesi bölge** | **Log Analytics çalışma alanı bölgesi** |
|-----------------------|------------------------------------|
|**Afrika** | |
|SouthAfricaNorth |WestEurope |
|SouthAfricaWest |WestEurope |
|**Avustralya** | |
|AustraliaEast |AustraliaEast |
|AustraliaCentral |AustraliaCentral |
|AustraliaCentral2 |AustraliaCentral |
|AustraliaEast |AustraliaEast |
|**Asya Pasifik** | |
|Ping'in ekran |Ping'in ekran |
|SoutheastAsia |SoutheastAsia |
|**Brezilya** | |
|BrazilSouth | SouthCentralUS |
|**Kanada** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Avrupa** | |
|FranceCentral |FranceCentral |
|FranceSouth |FranceCentral |
|NorthEurope |NorthEurope |
|UKSouth |UKSouth |
|UKWest |UKSouth |
|WestEurope |WestEurope |
|**Hindistan** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japonya** | |
|JapanEast |JapanEast |
|JapanWest |JapanEast |
|**Güney Kore** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**ABD** | |
|CentralUS |CentralUS|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|WestUS |WestUS |
|WestUS2 |WestUS2 |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|

<sup>1</sup> kapasitesi kısıtlamaları nedeniyle bölgeyi yeni kaynakları oluşturulurken kullanılamaz. Bu, bir Log Analytics çalışma alanı içerir. Ancak, önceden var olan bağlı kaynaklar bölgede çalışmaya devam.

## <a name="custom-metrics-supported-regions"></a>Özel ölçümler desteklenen bölgeler

Azure Kubernetes Hizmetleri (AKS) ölçümleri toplamaya düğüm kümeleri ve pod'ların yalnızca, aşağıdaki özel ölçümler olarak yayımlama için desteklenen [Azure bölgeleri](../platform/metrics-custom-overview.md#supported-regions).

## <a name="next-steps"></a>Sonraki adımlar

AKS kümenizi izlemeye başlamak için gözden [kapsayıcılar için Azure İzleyici etkinleştirme](container-insights-onboard.md) izlemeyi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemlerin anlaşılması.  