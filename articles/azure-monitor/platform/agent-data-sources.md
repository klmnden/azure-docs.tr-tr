---
title: Azure İzleyici'de aracı veri kaynaklarını yapılandıracaksınız | Microsoft Docs
description: Veri kaynakları, Azure İzleyici toplar aracıları ve diğer kaynakları bağlı günlük verileri tanımlar.  Bu makale, nasıl Azure İzleyici veri kaynaklarını kullanmaktadır, nasıl yapılandırılacakları ayrıntılarını açıklayan ve farklı veri kaynaklarının bir özetini sunar kavramını açıklar.
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
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: d7d4aa89c4dcf2ac9cc0c393e0481cae1f3aeaf2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776093"
---
# <a name="agent-data-sources-in-azure-monitor"></a>Azure İzleyici aracı veri kaynakları
Azure İzleyici aracılardan topladığı veriler, yapılandırdığınız veri kaynakları tarafından tanımlanır.  Aracılardan gelen veri olarak depolanır [günlük verilerini](data-platform-logs.md) bir kayıt kümesi ile.  Her veri kaynağı kendi özellikler kümesini sahip her türüyle belli bir türdeki kayıtları oluşturur.

![Günlük veri toplama](media/agent-data-sources/overview.png)

## <a name="summary-of-data-sources"></a>Veri kaynakları özeti
Aşağıdaki tablo, Azure İzleyici'de şu anda kullanılabilir aracı veri kaynaklarını listeler.  Bu veri kaynağı için ayrıntı sağlayan ayrı bir makale için bir bağlantı vardır.   Ayrıca kendi yöntemi ve toplama sıklığı bilgi sağlar. 


| Veri kaynağı | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Özel günlükler](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | geldiğinde |
| [Özel günlükler](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | geldiğinde |
| [IIS günlükleri](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |Günlük dosyası aktarma ayarına bağlıdır |
| [Performans sayaçları](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Performans sayaçları](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Azure depolama biriminden: 10 dakika; Aracısı'ndan: işle |
| [Windows olay günlükleri](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | geldiğinde |


## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Veri kaynaklarından yapılandırma **veri** menüde **Gelişmiş ayarlar** çalışma alanı için.  Herhangi bir yapılandırma çalışma alanınızdaki tüm bağlı kaynaklar teslim edilir.  Herhangi bir aracı şu anda bu yapılandırmayı dışarıda tutulamaz.

![Windows olayları Yapılandır](media/agent-data-sources/configure-events.png)

1. Azure portalında **Log Analytics çalışma alanları** > çalışma alanınızı > **Gelişmiş ayarlar**.
2. Seçin **veri**.
3. Yapılandırmak istediğiniz veri kaynağında'a tıklayın.
4. Kendi yapılandırma hakkında ayrıntılar için yukarıdaki tabloda her bir veri kaynağı için belgelere bağlantıyı izleyin.


## <a name="data-collection"></a>Veri toplama
Veri kaynağı yapılandırmalarını, Azure İzleyici için birkaç dakika içinde doğrudan bağlanan aracılara teslim edilir.  Belirtilen verileri Aracıdan toplanan ve her bir veri kaynağı için belirli aralıklarla doğrudan Azure İzleyici teslim.  Bu özellikleri için her veri kaynağı için belgelere bakın.

System Center Operations Manager aracıları için bir bağlı yönetim grubu, veri kaynağı yapılandırmalarını yönetim paketleri çevrilen ve yönetim grubuna varsayılan olarak 5 dakikada bir teslim.  Aracı gibi diğer yönetim paketi indirir ve belirtilen verileri toplar. Veri kaynağına bağlı olarak ya da Azure İzleyici için veri ileten bir yönetim sunucusuna gönderilen veriler olacaktır ya da aracı verileri için Azure İzleyici yönetim sunucusu üzerinden geçmeden gönderir. Bkz: [çözümlerini azure'da izleme için veri koleksiyonu ayrıntıları](../insights/solutions-inventory.md) Ayrıntılar için.  Operations Manager ve Azure İzleyici bağlanma ve sıklığını değiştirme hakkında ayrıntılar bu yapılandırma okuyabilir, teslim [System Center Operations Manager tümleştirmesini yapılandırma](om-agents.md).

Aracıyı Azure İzleyici ya da Operations Manager'a bağlanamıyor ise, bağlantı kurduğunda teslim eder veri toplamaya devam eder.  Veri miktarı istemci için en yüksek önbellek boyutunu ulaşırsa veya aracıyı 24 saat içinde bir bağlantı kurmak mümkün değilse, verileri kaybolabilir.

## <a name="log-records"></a>Günlük kayıtları
Azure İzleyici tarafından toplanan tüm günlük verileri, çalışma alanında kayıt olarak depolanır.  Farklı veri kaynağı tarafından toplanan kayıtlarını kendi özellikler kümeniz ve tarafından tanımlanan kendi **türü** özelliği.  Her bir kayıt türü, her veri kaynağı için belgeler ve Ayrıntılar için çözüm bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [izleme çözümleri](../insights/solutions.md) işlevselliği eklemek için Azure İzleyici ve ayrıca çalışma alanına veri toplayın.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) izleme çözümleri ve veri kaynaklarından toplanan verileri çözümlemek için.  
* Yapılandırma [uyarılar](alerts-overview.md) proaktif olarak size veri kaynaklarından toplanan ve izleme çözümlerinin kritik verilerin bildirmek için.