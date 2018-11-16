---
title: Azure Log Analytics'te veri kaynakları yapılandırma | Microsoft Docs
description: Veri kaynakları, Log Analytics toplanan aracıları ve diğer kaynakları bağlı verileri tanımlar.  Bu makalede nasıl Log Analytics veri kaynaklarını kullanmaktadır, nasıl yapılandırılacakları ayrıntılarını açıklayan ve farklı veri kaynaklarının bir özetini sunar kavramını açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 6f5296844541db774610f5a46161f2e06673d99e
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711574"
---
# <a name="data-sources-in-log-analytics"></a>Log analytics'te veri kaynakları
Log Analytics bağlı kaynaklardan veri toplar ve Log Analytics çalışma alanınızda depolar.  Her birinden toplanan veriler, yapılandırdığınız veri kaynakları tarafından tanımlanır.  Log analytics'te verileri, bir kayıt kümesi depolanır.  Her veri kaynağı kendi özellikler kümesini sahip her türüyle belli bir türdeki kayıtları oluşturur.

![Analytics veri toplama oturum](./media/log-analytics-data-sources/overview.png)

Veri kaynakları farklı [yönetim çözümleri](../azure-monitor/insights/solutions.md), ayrıca bağlı kaynaklardan gelen verileri toplamak ve Log Analytics'te kayıtları oluşturun.  Veri çözümleri genellikle toplamaya ek olarak, günlük aramaları ve belirli bir uygulama veya hizmet işlemi analiz etmenize yardımcı olacak görünümler içerir.


## <a name="summary-of-data-sources"></a>Veri kaynakları özeti
Aşağıdaki tabloda, Log Analytics'te şu anda kullanılabilir veri kaynaklarını listeler.  Bu veri kaynağı için ayrıntı sağlayan ayrı bir makale için bir bağlantı vardır.   Ayrıca kendi yöntemi ve Log Analytics ile veri toplama sıklığı hakkında bilgiler sağlar.  Farklı çözümlerin tanımlamak ve farklı yönetim çözümleri için veri akışı ve bağlantı gereksinimlerini anlamak için bu makaledeki bilgileri kullanabilirsiniz. Sütunların açıklamaları için bkz: [veri koleksiyonu ayrıntıları Azure yönetim çözümlerine için](../azure-monitor/insights/solutions-inventory.md).


| Veri kaynağı | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Özel günlükler](log-analytics-data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | geldiğinde |
| [Özel günlükler](log-analytics-data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | geldiğinde |
| [IIS günlükleri](log-analytics-data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |Günlük dosyası aktarma ayarına bağlıdır |
| [Performans sayaçları](log-analytics-data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Performans sayaçları](log-analytics-data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Syslog](log-analytics-data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Azure depolama biriminden: 10 dakika; Aracısı'ndan: işle |
| [Windows olay günlükleri](log-analytics-data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | geldiğinde |


## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Veri kaynaklarından yapılandırma **veri** Log Analytics menüde **Gelişmiş ayarlar**.  Herhangi bir yapılandırma çalışma alanınızdaki tüm bağlı kaynaklar teslim edilir.  Herhangi bir aracı şu anda bu yapılandırmayı dışarıda tutulamaz.

![Windows olayları Yapılandır](./media/log-analytics-data-sources/configure-events.png)

1. Azure portalında **Log Analytics** > çalışma alanınızı > **Gelişmiş ayarlar**.
2. Seçin **veri**.
3. Yapılandırmak istediğiniz veri kaynağında'a tıklayın.
4. Kendi yapılandırma hakkında ayrıntılar için yukarıdaki tabloda her bir veri kaynağı için belgelere bağlantıyı izleyin.


## <a name="data-collection"></a>Veri toplama
Veri kaynağı yapılandırmalarını birkaç dakika içinde doğrudan Log Analytics'e bağlı aracılara teslim edilir.  Belirtilen verileri Aracıdan toplanan ve her bir veri kaynağı için belirli aralıklarla doğrudan Log Analytics'e teslim.  Bu özellikleri için her veri kaynağı için belgelere bakın.

System Center Operations Manager aracıları için bir bağlı yönetim grubu, veri kaynağı yapılandırmalarını yönetim paketleri çevrilen ve yönetim grubuna varsayılan olarak 5 dakikada bir teslim.  Aracı gibi diğer yönetim paketi indirir ve belirtilen verileri toplar. Veri kaynağına bağlı olarak ya da Log Analytics'e veri ileten bir yönetim sunucusuna gönderilen veriler olacaktır ya da aracı verileri Log Analytics için yönetim sunucusu üzerinden geçmeden gönderir. Bkz: [veri koleksiyonu ayrıntıları Azure yönetim çözümlerine için](../azure-monitor/insights/solutions-inventory.md) Ayrıntılar için.  Operations Manager ve Log Analytics'e bağlama ve sıklığını değiştirme hakkında ayrıntılar bu yapılandırma okuyabilir, teslim [System Center Operations Manager tümleştirmesini yapılandırma](log-analytics-om-agents.md).

Aracının Log Analytics ya da Operations Manager bağlanamıyor ise, bağlantı kurduğunda teslim eder veri toplamaya devam eder.  Veri miktarı istemci için en yüksek önbellek boyutunu ulaşırsa veya aracıyı 24 saat içinde bir bağlantı kurmak mümkün değilse, verileri kaybolabilir.

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Log Analytics tarafından toplanan tüm veriler, çalışma alanında kayıt olarak depolanır.  Farklı veri kaynağı tarafından toplanan kayıtlarını kendi özellikler kümeniz ve tarafından tanımlanan kendi **türü** özelliği.  Her bir kayıt türü, her veri kaynağı için belgeler ve Ayrıntılar için çözüm bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [çözümleri](../azure-monitor/insights/solutions.md) Log Analytics'e işlev eklemek ve ayrıca çalışma alanına veri toplayın.
* Hakkında bilgi edinin [günlük aramaları](log-analytics-queries.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.  
* Yapılandırma [uyarılar](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktif olarak size, veri kaynakları ve çözümlerinden toplanan kritik veri bildirmek için.
