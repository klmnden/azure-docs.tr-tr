---
title: Azure tanılama günlükleri'ne genel bakış
description: Azure tanılama günlükleri, Azure İzleyici ve bir Azure kaynağı içinde gerçekleşen olaylar anlamak için bunları nasıl kullanabileceğiniz hakkında bilgi edinin.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: 8902e29baa5802e3416bcda97ca59a5576d41829
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244871"
---
# <a name="overview-of-azure-diagnostic-logs"></a>Azure tanılama günlükleri'ne genel bakış

**Tanılama günlükleri** bir Azure kaynağının işlemiyle ilgili zengin, sık kullanılan veriler sağlar. Azure İzleyici, kullanılabilir iki tür Tanılama Günlüğü yapar:

* **Kiracı günlükleri** -Azure Active Directory günlükleri gibi mevcut bir Azure aboneliği dışında Kiracı düzeyi hizmetler bu günlükleri gelir.
* **Kaynak günlükleri** -Bu günlükleri, ağ güvenlik grupları veya depolama hesapları gibi bir Azure aboneliğinde kaynakları dağıtma, Azure hizmetlerinden gelir.

    ![Kaynak tanılama günlükleri diğer türleri vs günlükleri](media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

Bu günlüklerin içeriği, Azure hizmeti ve kaynak türüne göre değişir. Örneğin, ağ güvenliği Grup Kuralı sayaçları ve Key Vault denetimleri iki, tanılama günlüğü türleridir.

Bu günlükler farklı [etkinlik günlüğü](activity-logs-overview.md). Etkinlik günlüğü, Kaynak Yöneticisi'ni kullanarak, örneğin, bir sanal makine oluştururken veya bir mantıksal uygulama siliniyor, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüğünün abonelik düzeyinde günlüktür. Kaynak düzeyinde tanılama günlükleri, kaynağının içinde Örneğin, Key Vault'tan bir gizli dizi alma gerçekleştirilen işlemler hakkında bilgi sağlar.

Bu günlükler de konuk işletim sistemi düzeyinde tanılama günlüklerinden farklıdır. Konuk işletim sistemi tanılama günlükleri, bu sanal makine içinde çalışan bir aracının tarafından toplanan veya diğer desteklenen kaynak türü ' dir. Konuk işletim sistemi düzeyinde tanılama günlükleri, işletim sistemi ve bir sanal makinede çalışan uygulamalardan veri yakalama işlemi sırasında kaynak düzeyinde tanılama günlükleri, Azure platformu kendisine hiçbir aracı ve yakalama kaynağa özgü veri gerektirir.

Tüm hizmetler burada açıklanan tanılama günlükleri'ni destekler. [Bu makalede hangi hizmetlerin Desteği Tanılama günlükleri bir bölüm listesi içeriyor](./../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>Tanılama günlükleri ile yapabilecekleriniz
Tanılama günlükleri ile yapabileceklerinizden bazıları şunlardır:

![Mantıksal yerleşimini tanılama günlükleri](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* Kaydetmek için bir [ **depolama hesabı** ](../../azure-monitor/platform/archive-diagnostic-logs.md) denetim veya el ile İnceleme. Bekletme süresi (gün cinsinden) kullanarak belirtebilirsiniz **kaynak tanılama ayarlarını**.
* [Bunları Stream **Event Hubs** ](diagnostic-logs-stream-event-hubs.md) alımı bir üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.
* Bunları analiz [Azure İzleyici](../../azure-monitor/platform/collect-azure-metrics-logs.md), verileri hemen Azure İzleyici ile ilk veri depolama alanına yazmaya gerek yazıldığı.  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Günlükleri yayan biri ile aynı abonelikte değil Event Hubs ad alanı veya bir depolama hesabını kullanabilirsiniz. Ayarı yapılandıran kullanıcının her iki aboneliğin uygun RBAC erişiminiz olması gerekir.

> [!NOTE]
>  Şu anda, ağ akışı günlükleri güvenli bir sanal ağda olduğu bir depolama hesabına arşivlenemiyor.

## <a name="diagnostic-settings"></a>Tanılama ayarları

Kaynak tanılama günlükleri, kaynak tanılama ayarlarını kullanarak yapılandırılır. Kiracı tanılama günlükleri, Kiracı tanılama ayarı kullanılarak yapılandırılır. **Tanılama ayarları** hizmet denetimi için:

* (Depolama hesabı, olay hub'ları ve/veya Azure İzleyici) burada tanılama günlükleri ve ölçümleri gönderilir.
* Ölçüm verilerini de gönderilip ve hangi günlük kategorileri gönderilir.
* Günlük kategorileri bir depolama hesabında ne kadar süre tutulacağını.
    - Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değeri herhangi bir sayıda gün 1 ile 365 arasında olabilir.
    - Bekletme ilkeleri ayarlayın, ancak yalnızca (örneğin, Event Hubs veya Log Analytics seçeneği seçili) günlükleri bir depolama hesabında depolama devre dışı, bekletme ilkeleri bir etkisi yoktur.
    - Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar.

Tanılama ayarlarını portalında, Azure PowerShell ve CLI komutları ile yapılandırılmış veya kullanarak bu ayarları [Azure İzleyici REST API](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Desteklenen hizmetler, kategoriler ve şemalar tanılama günlükleri

[Bu makaleye bakın](../../azure-monitor/platform/diagnostic-logs-schema.md) desteklenen hizmetler ve günlük kategorileri ve bu hizmetler tarafından kullanılan şemalar tam listesi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak tanılama günlükleri için Stream **olay hub'ları**](diagnostic-logs-stream-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/)
* [Azure İzleyici ile Azure depolama biriminden günlüklerini çözümleme](collect-azure-metrics-logs.md)
