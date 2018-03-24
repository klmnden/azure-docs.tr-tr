---
title: Azure'da - genel bakış OMS uyarılar genişletmek | Microsoft Docs
description: Uyarıları genişletme sorunları OMS Azure uyarıları, ortak müşteri ayrıntılarla içine işlemine genel bakış.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: 045a7f97d9c4d380e83325c04c209a6afcc761a7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="extend-alerts-from-oms-into-azure"></a>Uyarıları OMS Azure'da genişletir.
Yeni uyarılar deneyimi çeşitli Hizmetleri ve Microsoft Azure bölümleri arasında uyarı deneyimi şimdi tümleşiktir. Kullanılabilir olarak yeni deneyime **uyarıları** portalındaki Azure monitöründe birlikte ortak bir yerde - etkinlik günlüğü uyarıları, ölçüm uyarıları ve Application Insights yanı sıra günlük analizi günlük uyarılar getirdi. 

Ancak bazı kullanıcılar için günlük analizi ve Uyarıları gibi allied işlevsellik kullanımı aracılığıyla açıldı [Microsoft işlemi Yönetim Paketi (OMS)](../operations-management-suite/operations-management-suite-overview.md). Ve bu nedenle izin vermek üzere kimliklerini kolaylıkla günlük analizi - kullanımını yanı sıra diğer Azure kaynaklarını yönetmek sistematik olarak Microsoft OMS yeteneklerini de Azure portalında kullanılabilir olduğundan emin. Şekilde, Azure uyarıları zaten tanır için günlük analizi, daha fazla bilgi için bkz: Sorgu tabanlı uyarıları yönetmek kullanıcılar, [oturum Azure uyarıları uyarılar](monitor-alerts-unified-log.md). İçindeki uyarıları Azure İzleyicisi altında OMS içinde oluşturulan uyarıların zaten uygun günlük analizi çalışma alanı altında listelenir. Ancak herhangi bir düzenleme veya OMS içinde oluşturulan bu tür uyarıların değiştirmek, Azure bırakın ve OMS kullanmak kullanıcının gerektiren; başka bir hizmet yönetmek gerekirse Azure'a geri dönün. Bu güçlük azaltmak için Microsoft şimdi uyarılarını OMS Azure'da genişletmek kullanıcıların imkan verir.

## <a name="benefits-of-extending-your-alerts"></a>Uyarılarınızı kullanmanın yararları
Azure portal dışında gidin zorunluluğunu tahakkuk avantajı dışında diğer belirgin yararları vardır uyarıları OMS Azure'da genişletme

- Aksine, burada yalnızca 250 uyarıları oluşturulabilir ve görüntülenebilir; eklemek için Azure Uyarıları'nda bu sınırlamaya mevcut değil
- Azure uyarıları, tüm sizi uyarır türleri, numaralandırılmış, görüntülenebilir ve yönetilebilir; yalnızca günlük analizi uyarılarını OMS durumuyla olarak
- Azure uyarıları kullanan [Eylem grupları](monitoring-action-groups.md), hangi izin sahip her biri için uyarı SMS, sesli arama, Otomasyon Runbook'u, Web kancası, ITSM bağlayıcı ve benzeri birden çok eylem. Burada OMS uyarıları hem sayı olarak Eylemler olası türünü sınırlıdır

## <a name="process-of-extending-your-alerts"></a>Uyarılarınızı genişletme işlemi
Uyarıları OMS Azure genişletme işlemi mu **değil** uyarı tanımı, sorgu veya herhangi bir şekilde yapılandırma değiştirilmektedir. Gerekli yalnızca Azure, e-posta bildirimi gibi tüm eylemler Otomasyon runbook'u çalıştıran veya ITSM Aracı'na bağlanma Web kancası çağrı yapılır, eylem grubu değişikliktir. Uyarınız ile - uygun eylemi Grup ilişkiliyse bu nedenle bunlar Azure'da genişletilmiş hale.

Genişletme işlemi dönüşlü ve değil interruptive olduğundan, Microsoft Operations Management Suite (OMS) Azure uyarıları otomatik olarak oluşturulan - başlayarak uyarıları uzatır **23 Nisan 2018**. Bu tarihten itibaren Microsoft Azure'da uyarıları genişletme zamanlamak başlar ve kademeli olarak eklemek için Azure portaldan yönetilebilir mevcut tüm uyarıları olun. 

Azure'da genişletmek için OMS çalışma alanında uyarılar zamanlandığında, bunlar çalışma ve devam eder **değil** izlemenizi herhangi bir şekilde tehlikeye. Zamanlanan uyarılarınızı geçici olarak değiştirilmesi/düzenleme için kullanılamıyor olabilir; Ancak bu kısa süre içinde oluşturulacak yeni Azure uyarılar devam edebilirsiniz. Herhangi bir düzenleme veya uyarının oluşturulmasını OMS yapıldığında bu kısa süre içinde kullanıcıların Azure Log Analytics veya Azure uyarıları devam seçeneğiniz vardır.

 ![Zamanlanmış süresi boyunca, kullanıcı eylemi Azure'a yeniden yönlendirilen uyarılar hakkında](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Uyarıları Operations Management Suite (OMS) Azure'a genişletme ücret ve kullanım Azure günlük analizi uyarılar değil faturalandırılacaksınız, belirtilen koşullar ve sınırları içinde kullanıldığında için sorgu tabanlı uyarılar [Azure fiyatlandırma ilkesi İzleyicisi](https://azure.microsoft.com/en-us/pricing/details/monitor/)  

Kullanıcılar bu tarihten önce uyarıları genişletmenin avantajlarından; Gönüllü uyarılarını Azure yönetilebilir hale getirmek amacıyla kullanmama tarafından.

### <a name="how-to-voluntarily-extending-your-alerts"></a>Uyarılarınızı gönüllü genişletme nasıl
Azure uyarıları kolay bir geçişinde OMS kullanıcılar etkinleştirmek için Microsoft uyarıları genişletme araç oluşturdu. Microsoft Operations Management Suite (OMS) müşteriler uyarılarını Azure OMS portalında (veya) yeni bir API kullanarak programlı bir yaklaşım sihirbazından genişletebilirsiniz. Daha fazla bilgi için bkz: [uyarıları OMS portalı ve API kullanarak Azure genişletme](monitoring-alerts-extend-tool.md).


## <a name="usage-after-extending-your-alerts"></a>Uyarılarınızı genişlettikten sonra kullanım
Belirtildiği gibi Microsoft Operasyon Management Suite içinde oluşturulan uyarıların Azure Uyarıları ' uzatılır; sonra bunları Azure'dan yönetebilirsiniz. Uyarıları OMS portalında - uyarı ayarı bölümünde listelenen devam eder. Aşağıda gösterildiği gibi:

 ![OMS portalı Azure'da genişletilmiş sonra uyarıları listeleme](./media/monitor-alerts-extend/PostExtendList.png)

Düzenleme veya OMS portalında bitti oluşturma gibi uyarılar hakkında herhangi bir işlem için kullanıcıların saydam Azure uyarıları yönlendirilir. Oluşturma varolandan devam edecek uyarı [günlük analizi API](../log-analytics/log-analytics-api-alerts.md) olarak önceki, yalnızca küçük bir değişiklik olmasına, uyarıları Azure'da - genişletilmiş sonra Eylem grupları zamanlamada ilişkilendirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanılacak araçların öğrenin [başlatma uyarıları OMS Azure'da genişletme](monitoring-alerts-extend-tool.md)
* Yeni hakkında daha fazla bilgi [Azure uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
