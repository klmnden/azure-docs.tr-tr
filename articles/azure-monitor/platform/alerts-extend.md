---
title: Azure kamu Bulutuna (kopya) Log Analytics uyarıları genişletme
description: Uyarıları Log Analytics'ten OMS Portalı'nda Azure uyarılarına genişletecektir ile kopyalama işlemine genel bakış adresleme yaygın müşteri endişeler ayrıntıları.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: efd2fc9d164564126b7b641ef35bbb10c4834f49
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996501"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Azure uyarıları log Analytics uyarıları genişletme

> [!NOTE]
> Microsoft Azure genel sürümleri için bu makalede açıklanan işlemi zaten tamamlandı. Ancak, yine de ABD kamu sürümleri için geçerlidir.  

Yakın zamanda kadar Azure Log Analytics, Log Analytics verilerine dayalı koşulları proaktif olarak bildirebilirsiniz kendi uyarı işlevleri dahil. Microsoft Operations Management Suite portalında uyarı kuralları Yönetildi Yeni uyarı deneyimi artık Microsoft azure'da çeşitli hizmetlerdeki uyarı tümleştirilmiştir. Bu olarak kullanılabilir **uyarılar** Azure portalında, Azure İzleyici ve etkinlik günlükleri, ölçümler, uyarı destekler ve hem Log Analytics hem de Azure Application Insights günlüğe kaydeder.

OMS portalını kullanarak azure kamu Bulutu müşterileri için [gönüllü olarak kendi uyarı kuralları Azure'a genişletme](alerts-extend-tool.md) gelen **1 Şubat 2019**. Başlangıç **1 Mart'ta 2019** - Microsoft Otomatik-genişletmek sistematik bir şekilde var olan uyarı kurallarının tümünü Azure kamu'da - OMS portalı Azure; izleme sonu veya kapalı kalma süresi olmadan. Sonra oluşturulan yeni çalışma alanı **1 Mart'ta 2019** Azure kamu bulutunda OMS portalı Azure'a otomatik olarak genişletilmiş olur.

## <a name="benefits-of-extending-your-alerts"></a>Uyarılarınızı genişletmenin avantajları
Oluşturma ve Azure portalında, uyarılar gibi yönetme çeşitli avantajları vardır:

- Farklı Azure uyarıları burada yalnızca 250 uyarı oluşturulan görüntülenebilir ve, Operations Management Suite portalında, böyle bir kısıtlama vardır.
- Azure Uyarıları'ndan yönetebilir, listeleme ve tüm uyarı türlerinizi görüntüleyebilirsiniz. Daha önce yalnızca Log Analytics uyarılarını için bunu.
- Yalnızca izleme ve uyarı, kullanarak kullanıcıların erişimini sınırlayabilirsiniz [Azure İzleyici rol](../../azure-monitor/platform/roles-permissions-security.md).
- Azure Uyarıları'nda kullanabileceğiniz [Eylem grupları](../../azure-monitor/platform/action-groups.md). Bu, her uyarı için birden fazla eyleme sahip olmanızı sağlar. Yapabilecekleriniz SMS, sesli çağrı gönderme, Azure Otomasyonu runbook'u çağırma, bir Web kancası çağırma ve BT Hizmet Yönetimi (ITSM) bağlayıcısını yapılandırın. 

## <a name="process-of-extending-your-alerts"></a>Uyarılarınızı genişletme işlemi
Uyarıları Log Analytics'ten Azure uyarılarına genişletecektir taşıma işlemi, uyarı tanımınızı, sorgunuzu veya yapılandırmanızı herhangi bir şekilde değiştirmeyi gerektirmez. Gereken tek değişiklik, Azure'da, tüm eylemleri bir eylem grubu kullanarak gerçekleştirmenizi ' dir. Eylem grupları Uyarınız ile zaten ilişkilendirilmiş, bunlar Azure'a genişletilmiş dahil edilir.

Azure'a genişletilecek bir Log Analytics çalışma alanı Uyarılardaki zamanladığınızda çalışmaya ve buna herhangi şekilde yapılandırmanızı tehlikeye sağlamadığı devam eder. Zamanlanan uyarılarınızı geçici olarak değiştirilmesi için kullanılamıyor olabilir, ancak bu süre boyunca yeni Azure uyarıları oluşturmaya devam edebilirsiniz. Operations Management Suite portalında uyarıları oluşturma veya düzenleme çalışırsanız, bunları Log Analytics çalışma alanınızdan oluşturmaya devam etmek için seçeneğiniz vardır. Bunları Azure portalında Azure Uyarıları'ndan oluşturulacağını seçebilirsiniz.

 ![Log Analytics veya Azure uyarıları uyarılar oluşturmak için seçeneğinin ekran görüntüsü](media/alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Uyarıları Log Analytics'ten Azure'a genişletme hesabınıza ücrete tabi değildir. Sorgu tabanlı Log Analytics uyarılar Azure uyarıları kullanarak değil faturalandırılır sınırlar içinde kullanıldığında ve cinsinden ifade edilen koşullar [Azure İlkesi fiyatlandırması İzleyici](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Uyarılarınızı gönüllü genişletme
Uyarılarınızı Azure Uyarıları'na genişletmek için Operations Management Suite Portalı'nda kullanılabilir olan bir sihirbaz kullanabilirsiniz veya bunu programlı bir API kullanarak bunu yapabilirsiniz. Daha fazla bilgi için [uyarıları Operations Management Suite portalını ve API'yi kullanarak Azure'a genişletme](alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Uyarılarınızı genişlettikten sonra deneyimi
Azure Uyarıları'na uyarılarınızı genişletilmektedir sonra Hayır daha önce farklı yönetim için Operations Management Suite portalında kullanılabilir olması devam.

![Operations Management Suite ekran portalıyla listelenen uyarıları](media/alerts-extend/PostExtendList.png)

Var olan bir uyarı düzenleme veya yeni bir uyarı Operations Management Suite portalında denediğinizde, Azure Uyarıları'na otomatik olarak yönlendirilir.  

> [!NOTE]
> Uyarıları eklemek veya düzenlemek için gereken kişiler için atanan izinler Azure'da düzgün bir şekilde atandığından emin olun. Hangi izinleri vermek için gereksinim duyduğunuz anlamak için bkz: [Azure İzleyici ve uyarı kullanarak izinlerini](../../azure-monitor/platform/roles-permissions-security.md).  
> 

Uyarıları oluşturmaya devam edebilirsiniz [Log Analytics API](../../azure-monitor/platform/api-alerts.md) ve [Log Analytics kaynak şablonu](../../azure-monitor/insights/solutions-resources-searches-alerts.md). Bunu yaptığınızda Eylem grupları içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

* Araçlar hakkında bilgi edinin [başlatma uyarıları Log Analytics'ten Azure'a genişletme](alerts-extend-tool.md).
* Daha fazla bilgi edinin [Azure Uyarıları'deneyimi](../../azure-monitor/platform/alerts-overview.md).
* Oluşturmayı [uyarılar Azure Uyarıları'nda oturum](alerts-unified-log.md).

