---
title: Log Analytics'te aracı veri kaynakları yapılandırma | Microsoft Docs
description: Günlük verileri Log Analytics toplanan aracıları ve diğer kaynakları bağlı veri kaynakları tanımlayın.  Bu makalede nasıl Log Analytics veri kaynaklarını kullanmaktadır, nasıl yapılandırılacakları ayrıntılarını açıklayan ve farklı veri kaynaklarının bir özetini sunar kavramını açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2018
ms.author: bwren
ms.openlocfilehash: d9bedeeb2e354dab8bc6a7be56826f28914326be
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101539"
---
# <a name="agent-data-sources-in-log-analytics"></a>Log Analytics aracısını veri kaynakları
Log Analytics aracılarını topladığı veriler, yapılandırdığınız veri kaynakları tarafından tanımlanır.  Aracılardan gelen veri olarak depolanır [günlük verilerini](data-collection.md) bir kayıt kümesi ile.  Her veri kaynağı kendi özellikler kümesini sahip her türüyle belli bir türdeki kayıtları oluşturur.

![Günlük veri toplama](media/agent-data-sources/overview.png)

## <a name="summary-of-data-sources"></a>Veri kaynakları özeti
Aşağıdaki tabloda, Log Analytics'te şu anda kullanılabilir aracı veri kaynaklarını listeler.  Bu veri kaynağı için ayrıntı sağlayan ayrı bir makale için bir bağlantı vardır.   Ayrıca kendi yöntemi ve toplama sıklığı bilgi sağlar. 


| Veri kaynağı | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Özel günlükler](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | geldiğinde |
| [Özel günlükler](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | geldiğinde |
| [IIS günlükleri](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |Günlük dosyası aktarma ayarına bağlıdır |
| [Performans sayaçları](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Performans sayaçları](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Azure depolama biriminden: 10 dakika; Aracısı'ndan: işle |
| [Windows olay günlükleri](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | geldiğinde |


## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Veri kaynaklarından yapılandırma **veri** menüde **Gelişmiş ayarlar** çalışma alanı için.  Herhangi bir yapılandırma çalışma alanınızdaki tüm bağlı kaynaklar teslim edilir.  Herhangi bir aracı şu anda bu yapılandırmayı dışarıda tutulamaz.

![Windows olayları Yapılandır](./media/agent-data-sources/configure-events.png)

1. Azure portalında **Log Analytics** > çalışma alanınızı > **Gelişmiş ayarlar**.
2. Seçin **veri**.
3. Yapılandırmak istediğiniz veri kaynağında'a tıklayın.
4. Kendi yapılandırma hakkında ayrıntılar için yukarıdaki tabloda her bir veri kaynağı için belgelere bağlantıyı izleyin.


## <a name="data-collection"></a>Veri toplama
Veri kaynağı yapılandırmalarını birkaç dakika içinde doğrudan Log Analytics'e bağlı aracılara teslim edilir.  Belirtilen verileri Aracıdan toplanan ve her bir veri kaynağı için belirli aralıklarla doğrudan Log Analytics'e teslim.  Bu özellikleri için her veri kaynağı için belgelere bakın.

System Center Operations Manager aracıları için bir bağlı yönetim grubu, veri kaynağı yapılandırmalarını yönetim paketleri çevrilen ve yönetim grubuna varsayılan olarak 5 dakikada bir teslim.  Aracı gibi diğer yönetim paketi indirir ve belirtilen verileri toplar. Veri kaynağına bağlı olarak ya da Log Analytics'e veri ileten bir yönetim sunucusuna gönderilen veriler olacaktır ya da aracı verileri Log Analytics için yönetim sunucusu üzerinden geçmeden gönderir. Bkz: [çözümlerini azure'da izleme için veri koleksiyonu ayrıntıları](../../azure-monitor/insights/solutions-inventory.md) Ayrıntılar için.  Operations Manager ve Log Analytics'e bağlama ve sıklığını değiştirme hakkında ayrıntılar bu yapılandırma okuyabilir, teslim [System Center Operations Manager tümleştirmesini yapılandırma](../../log-analytics/log-analytics-om-agents.md).

Aracının Log Analytics ya da Operations Manager bağlanamıyor ise, bağlantı kurduğunda teslim eder veri toplamaya devam eder.  Veri miktarı istemci için en yüksek önbellek boyutunu ulaşırsa veya aracıyı 24 saat içinde bir bağlantı kurmak mümkün değilse, verileri kaybolabilir.

## <a name="log-records"></a>Günlük kayıtları
Log Analytics tarafından toplanan tüm veriler, çalışma alanında kayıt olarak depolanır.  Farklı veri kaynağı tarafından toplanan kayıtlarını kendi özellikler kümeniz ve tarafından tanımlanan kendi **türü** özelliği.  Her bir kayıt türü, her veri kaynağı için belgeler ve Ayrıntılar için çözüm bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [izleme çözümleri](../../azure-monitor/insights/solutions.md) işlevselliği eklemek için Azure İzleyici ve ayrıca çalışma alanına veri toplayın.
* Hakkında bilgi edinin [oturum sorguları](../../log-analytics/log-analytics-queries.md) izleme çözümleri ve veri kaynaklarından toplanan verileri çözümlemek için.  
* Yapılandırma [uyarılar](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktif olarak size veri kaynaklarından toplanan ve izleme çözümlerinin kritik verilerin bildirmek için.
