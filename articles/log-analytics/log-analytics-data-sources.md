---
title: Azure günlük analizi veri kaynakları yapılandırma | Microsoft Docs
description: Veri kaynakları günlük analizi toplar aracıları ve diğer kaynakları bağlı verileri tanımlayın.  Bu makalede kavramı nasıl günlük analizi veri kaynakları kullanır, bunların nasıl yapılandırılacağı ayrıntılarını açıklar ve kullanılabilen değişik veri kaynakları özetini sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: e0366e6a2403f7cea83efde35c7b3b42565a1cf2
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062788"
---
# <a name="data-sources-in-log-analytics"></a>Günlük analizi veri kaynaklarında
Günlük analizi bağlı kaynaklarınızdan verilerini toplar ve günlük analizi çalışma alanınızda depolar.  Her birinden toplanan verileri yapılandırdığınız veri kaynakları tarafından tanımlanır.  Günlük analizi veri bir kayıt kümesi depolanır.  Her veri kaynağı kendi özellikler kümesini sahip her türüyle belirli bir türdeki kayıtları oluşturur.

![Günlük analizi veri toplama](./media/log-analytics-data-sources/overview.png)

Veri kaynakları farklı [yönetim çözümleri](log-analytics-add-solutions.md), ayrıca bağlı kaynaklardan veri toplamak ve günlük analizi kayıtları oluşturun.  Veri toplama yanı sıra çözümler genellikle günlük arar ve belirli bir uygulama veya hizmet işlemi analiz etmenize yardımcı olacak görünümler içerir.


## <a name="summary-of-data-sources"></a>Veri kaynakları özeti
Aşağıdaki tabloda günlük analizi şu anda kullanılabilir veri kaynaklarını listeler.  Her ayrıntı için bu veri kaynağı sağlayarak ayrı bir makale için bir bağlantı vardır.   Ayrıca, yöntemi ve günlük analizi veri koleksiyonuna sıklığını hakkında bilgiler sağlar.  Kullanılabilir farklı çözümler tanımlamak ve farklı yönetim çözümleri için veri akışı ve bağlantı gereksinimleri anlamak için bu makaledeki bilgileri kullanabilirsiniz. Sütunları açıklamalar için bkz [Azure yönetim çözümleri için veri toplama ayrıntılarına](../monitoring/monitoring-solutions-inventory.md).


| Veri kaynağı | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Özel günlükler](log-analytics-data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | geldiğinde |
| [Özel günlükler](log-analytics-data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | geldiğinde |
| [IIS günlükleri](log-analytics-data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |Günlük dosyası aktarma ayarına bağlıdır |
| [Performans sayaçları](log-analytics-data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Performans sayaçları](log-analytics-data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |zamanlandığı gibi en az 10 saniye |
| [Syslog](log-analytics-data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Azure depolama biriminden: 10 dakika; aracısından: işle |
| [Windows olay günlükleri](log-analytics-data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | geldiğinde |


## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Veri kaynaklarından yapılandırma **veri** günlük analizi menüde **Gelişmiş ayarları**.  Herhangi bir yapılandırma çalışma alanınızdaki tüm bağlı kaynakları teslim edilir.  Şu anda bu yapılandırmasından tüm aracıları dışarıda bırakılamaz.

![Windows olayları yapılandırın](./media/log-analytics-data-sources/configure-events.png)

1. Azure portalında seçin **günlük analizi** > çalışma alanınızı > **Gelişmiş ayarları**.
2. Seçin **veri**.
3. Yapılandırmak istediğiniz veri kaynağında'ı tıklatın.
4. Yapılandırmalarını hakkında ayrıntılar için yukarıdaki tabloda her bir veri kaynağı için belgelerine bağlantıyı izleyin.


## <a name="data-collection"></a>Veri toplama
Veri kaynağı yapılandırmaları günlük analizi için birkaç dakika içinde doğrudan bağlı olan aracılara teslim edilir.  Belirtilen veri Aracıdan toplanır ve her bir veri kaynağı için belirli aralıklarla doğrudan günlük analizi için teslim.  Bu özellikleri için her bir veri kaynağı belgelerine bakın.

Bağlı Yönetim grubundaki System Center Operations Manager aracıları için veri kaynağı yapılandırmaları yönetim paketleri çevrilen ve yönetim grubuna varsayılan olarak 5 dakikada bir teslim.  Aracı gibi diğer yönetim paketi indirir ve belirtilen verileri toplar. Veri kaynağı bağlı olarak veri ya da günlük analizi veri ileten bir yönetim sunucusuna gönderilen olur veya aracı veri günlük analizi için yönetim sunucusu üzerinden geçmeden gönderir. Başvurmak [veri koleksiyonu ayrıntıları](log-analytics-add-solutions.md#data-collection-details) Ayrıntılar için.  Bu yapılandırma, Operations Manager'ı ve günlük analizi bağlanma ve sıklığını değiştirme ayrıntılarını okuyabilir, teslim [System Center Operations Manager tümleştirmesini yapılandırma](log-analytics-om-agents.md).

Aracı günlük analizi veya Operations Manager bağlanamıyor ise, bağlantı kurduğunda teslim eder veri toplamaya devam eder.  Veriler veri miktarı istemci için en büyük önbellek boyutu ulaşırsa veya aracı 24 saat içinde bir bağlantı kurup kuramadığını değilse kaybolmuş olabilir.

## <a name="log-analytics-records"></a>Log Analytics kayıtları
Günlük analizi tarafından toplanan tüm veriler, çalışma alanında kayıt olarak depolanır.  Farklı veri kaynakları tarafından toplanan kayıtları kendi özellikler kümesini sahip olur ve tarafından tanımlanan kendi **türü** özelliği.  Her veri kaynağı için belgelere ve çözüm ayrıntıları için her bir kayıt türü bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [çözümleri](log-analytics-add-solutions.md) işlevselliği için günlük analizi eklemek ve ayrıca çalışma alanına verilerini topla.
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümleri toplanan verileri çözümlemek için.  
* Yapılandırma [uyarıları](log-analytics-alerts.md) proaktif olarak, toplanan veri kaynakları ve çözümlerinin kritik verilerin bildirmek için.
