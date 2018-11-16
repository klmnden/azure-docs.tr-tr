---
title: Azure uyarılarına genişletecektir - genel bakış (kopya) Log Analytics uyarıları genişletme
description: Uyarıları Log Analytics'ten OMS Portalı'nda Azure uyarılarına genişletecektir ile kopyalama işlemine genel bakış adresleme yaygın müşteri endişeler ayrıntıları.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 0f0bee419fdb119fcd99f0e72cc61ddf20f6253a
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712951"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Azure uyarıları log Analytics uyarıları genişletme
Yakın zamanda kadar Azure Log Analytics, Log Analytics verilerine dayalı koşulları proaktif olarak bildirebilirsiniz kendi uyarı işlevleri dahil. Microsoft Operations Management Suite portalında uyarı kuralları Yönetildi Yeni uyarı deneyimi artık Microsoft azure'da çeşitli hizmetlerdeki uyarı tümleştirilmiştir. Bu olarak kullanılabilir **uyarılar** Azure portalında, Azure İzleyici ve etkinlik günlükleri, ölçümler, uyarı destekler ve hem Log Analytics hem de Azure Application Insights günlüğe kaydeder. 

## <a name="benefits-of-extending-your-alerts"></a>Uyarılarınızı genişletmenin avantajları
Oluşturma ve Azure portalında, uyarılar gibi yönetme çeşitli avantajları vardır:

- Farklı Azure uyarıları burada yalnızca 250 uyarı oluşturulan görüntülenebilir ve, Operations Management Suite portalında, böyle bir kısıtlama vardır.
- Azure Uyarıları'ndan yönetebilir, listeleme ve tüm uyarı türlerinizi görüntüleyebilirsiniz. Daha önce yalnızca Log Analytics uyarılarını için bunu.
- Yalnızca izleme ve uyarı, kullanarak kullanıcıların erişimini sınırlayabilirsiniz [Azure İzleyici rol](monitoring-roles-permissions-security.md).
- Azure Uyarıları'nda kullanabileceğiniz [Eylem grupları](monitoring-action-groups.md). Bu, her uyarı için birden fazla eyleme sahip olmanızı sağlar. Yapabilecekleriniz SMS, sesli çağrı gönderme, Azure Otomasyonu runbook'u çağırma, bir Web kancası çağırma ve BT Hizmet Yönetimi (ITSM) bağlayıcısını yapılandırın. 

## <a name="process-of-extending-your-alerts"></a>Uyarılarınızı genişletme işlemi
Uyarıları Log Analytics'ten Azure uyarılarına genişletecektir taşıma işlemi, uyarı tanımınızı, sorgunuzu veya yapılandırmanızı herhangi bir şekilde değiştirmeyi gerektirmez. Gereken tek değişiklik, Azure'da, tüm eylemleri bir eylem grubu kullanarak gerçekleştirmenizi ' dir. Eylem grupları Uyarınız ile zaten ilişkilendirilmiş, bunlar Azure'a genişletilmiş dahil edilir.

> [!NOTE]
> Microsoft, tamamlanana kadar yinelenen bir dizide 14 Mayıs 2018'de başlayarak Azure uyarıları Log Analytics'e genel bulut örneklerinde oluşturulan uyarıların otomatik olarak genişletilir. Oluşturma sorunları varsa [Eylem grupları](monitoring-action-groups.md), kullanın [düzeltme adımları](monitoring-alerts-extend-tool.md#troubleshooting) otomatik olarak oluşturulan eylem grupları alınamıyor. 5 Temmuz 2018'e kadar bu adımları kullanabilirsiniz. *Azure kamu ve Log Analytics bağımsız bulut kullanıcıları için uygulanamaz*. 
> 

Azure'a genişletilecek bir Log Analytics çalışma alanı Uyarılardaki zamanladığınızda çalışmaya ve buna herhangi şekilde yapılandırmanızı tehlikeye sağlamadığı devam eder. Zamanlanan uyarılarınızı geçici olarak değiştirilmesi için kullanılamıyor olabilir, ancak bu süre boyunca yeni Azure uyarıları oluşturmaya devam edebilirsiniz. Operations Management Suite portalında uyarıları oluşturma veya düzenleme çalışırsanız, bunları Log Analytics çalışma alanınızdan oluşturmaya devam etmek için seçeneğiniz vardır. Bunları Azure portalında Azure Uyarıları'ndan oluşturulacağını seçebilirsiniz.

 ![Log Analytics veya Azure uyarıları uyarılar oluşturmak için seçeneğinin ekran görüntüsü](media/monitoring-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Uyarıları Log Analytics'ten Azure'a genişletme hesabınıza ücrete tabi değildir. Sorgu tabanlı Log Analytics uyarılar Azure uyarıları kullanarak değil faturalandırılır sınırlar içinde kullanıldığında ve cinsinden ifade edilen koşullar [Azure İlkesi fiyatlandırması İzleyici](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Uyarılarınızı gönüllü genişletme
Uyarılarınızı Azure Uyarıları'na genişletmek için Operations Management Suite Portalı'nda kullanılabilir olan bir sihirbaz kullanabilirsiniz veya bunu programlı bir API kullanarak bunu yapabilirsiniz. Daha fazla bilgi için [uyarıları Operations Management Suite portalını ve API'yi kullanarak Azure'a genişletme](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Uyarılarınızı genişlettikten sonra deneyimi
Azure Uyarıları'na uyarılarınızı genişletilmektedir sonra Hayır daha önce farklı yönetim için Operations Management Suite portalında kullanılabilir olması devam.

![Operations Management Suite ekran portalıyla listelenen uyarıları](media/monitoring-alerts-extend/PostExtendList.png)

Var olan bir uyarı düzenleme veya yeni bir uyarı Operations Management Suite portalında denediğinizde, Azure Uyarıları'na otomatik olarak yönlendirilir.  

> [!NOTE]
> Uyarıları eklemek veya düzenlemek için gereken kişiler için atanan izinler Azure'da düzgün bir şekilde atandığından emin olun. Hangi izinleri vermek için gereksinim duyduğunuz anlamak için bkz: [Azure İzleyici ve uyarı kullanarak izinlerini](monitoring-roles-permissions-security.md).  
> 

Uyarıları oluşturmaya devam edebilirsiniz [Log Analytics API](../log-analytics/log-analytics-api-alerts.md) ve [Log Analytics kaynak şablonu](../azure-monitor/insights/solutions-resources-searches-alerts.md). Bunu yaptığınızda Eylem grupları içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

* Araçlar hakkında bilgi edinin [başlatma uyarıları Log Analytics'ten Azure'a genişletme](monitoring-alerts-extend-tool.md).
* Daha fazla bilgi edinin [Azure Uyarıları'deneyimi](monitoring-overview-alerts.md).
* Oluşturmayı [uyarılar Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
