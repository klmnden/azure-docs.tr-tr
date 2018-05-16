---
title: Azure'da - genel bakış OMS Portalı'ndan (kopya) uyarıları genişletmek | Microsoft Docs
description: Azure uyarıları, ortak müşteri sorunları ayrıntılarla OMS portalında kopyalama uyarıları işlemine genel bakış.
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
ms.date: 05/14/2018
ms.author: vinagara
ms.openlocfilehash: 25dcbad8607a651a7dd4b79f4f418cc473a2bf0e
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="extend-copy-alerts-from-oms-portal-into-azure"></a>Azure'da OMS Portalı'ndan (kopya) uyarıları genişletme
Operations Management Suite (OMS) portal yalnızca günlük analizi uyarıları gösterir.  Yeni uyarılar deneyimi çeşitli Hizmetleri ve Microsoft Azure bölümleri arasında uyarı deneyimi şimdi tümleşiktir. Kullanılabilir olarak yeni deneyime **uyarıları** altında Azure İzleyicisi Azure portal etkinlik günlüğü uyarıları, ölçüm uyarıları ve günlük uyarıları günlük analizi ve Application Insights için içerir. 


Ancak bazı kullanıcılar için günlük analizi ve Uyarıları gibi allied işlevsellik kullanımı aracılığıyla açıldı [Microsoft işlemi Yönetim Paketi (OMS) portal](../operations-management-suite/operations-management-suite-overview.md). Yapın ve bu nedenle izin vermek üzere kimliklerini kolaylıkla günlük analizi - kullanımını yanı sıra diğer Azure kaynaklarını yönetmek sistematik olarak Microsoft OMS portalı yeteneklerini de Azure portalında kullanılabilir olduğundan emin. Şekilde, Azure uyarıları zaten tanır için günlük analizi, daha fazla bilgi için bkz: Sorgu tabanlı uyarıları yönetmek kullanıcılar, [oturum Azure uyarıları uyarılar](monitor-alerts-unified-log.md). İçindeki uyarıları Azure İzleyicisi altında OMS portalında oluşturulan uyarıların zaten uygun günlük analizi çalışma alanı altında listelenir. Ancak herhangi bir düzenleme veya OMS portalında oluşturulan bu tür uyarılar değiştirmek, Azure bırakın ve OMS portalı kullanmak kullanıcının gerektiren; başka bir hizmet yönetmek gerekirse Azure'a geri dönün. Bu güçlük azaltmak için Microsoft şimdi uyarılarını OMS Portalı'ndan Azure'da genişletmek kullanıcıların imkan verir.

## <a name="benefits-of-extending-your-alerts"></a>Uyarılarınızı kullanmanın yararları
Azure portal dışında gidin zorunluluğunu tahakkuk avantajı dışında diğer belirgin yararları vardır uyarıları OMS Portalı'ndan Azure'da genişletme

- Aksine OMS portalında burada yalnızca 250 uyarıları oluşturulabilir ve görüntülenebilir; Azure Uyarıları'nda bu sınırlamaya mevcut değil
- Azure uyarıları, tüm uyarı türleri, numaralandırılmış, görüntülenebilir ve yönetilebilir; OMS portalı ile olduğu gibi yalnızca günlük analizi uyarıları
- Kullanarak yalnızca izleme ve uyarı, kullanıcıların erişimi denetlemenize [Azure İzleyici rolü](monitoring-roles-permissions-security.md)
- Azure uyarıları kullanma [Eylem grupları](monitoring-action-groups.md), SMS, sesli arama, Otomasyon Runbook'u, Web kancası, ITSM bağlayıcı ve benzeri her uyarı için birden fazla eylem sahip sağlar. 

## <a name="process-of-extending-your-alerts"></a>Uyarılarınızı genişletme işlemi
Uyarıları OMS Portalı'ndan Azure genişletme işlemi mu **değil** uyarı tanımı, sorgu veya herhangi bir şekilde yapılandırma değiştirilmektedir. Gerekli yalnızca Azure, e-posta bildirimi gibi tüm eylemler Otomasyon runbook'u çalıştıran veya ITSM Aracı'na bağlanma Web kancası çağrı yapılır, eylem grubu değişikliktir. Uyarınız ile - uygun eylemi Grup ilişkiliyse bu nedenle bunlar Azure'da genişletilmiş hale.

Genişletme işlemi dönüşlü ve değil interruptive olduğundan, Microsoft Azure uyarılar OMS portalında otomatik olarak oluşturulan - başlayarak uyarıları uzatır **14 Mayıs 2018**. Bu tarihten itibaren Microsoft Azure'da uyarıları genişletme zamanlamak ve tüm uyarıları OMS portalında, Azure Portalı'ndan da yönetilebilir mevcut kademeli olarak yapmak başlar. 

> [!NOTE]
> 14 Mayıs 2018 - başlangıç Microsoft Azure için uyarıları otomatik olarak genişletme işlemi başlayacak. Tüm çalışma alanları ve Uyarıları bu günde uzatılır; Bunun yerine Microsoft tranches otomatik olarak uyarıları gelecek haftalarda genişletmek başlar. Bu nedenle uyarılarınızı OMS portalında otomatik-Azure'da hemen 14 Mayıs 2018 üzerinde uzatır değil ve kullanıcının hala [el ile uyarılarını genişletmek](monitoring-alerts-extend-tool.md) bu süre boyunca.

Azure'da genişletmek için günlük analizi çalışma alanındaki uyarılar zamanlandığında, bunlar çalışma ve devam edecek **değil** izlemenizi herhangi bir şekilde tehlikeye. Zamanlanan uyarılarınızı geçici olarak değiştirilmesi/düzenleme için kullanılamıyor olabilir; Ancak bu kısa süre içinde oluşturulacak yeni Azure uyarılar devam edebilirsiniz. Herhangi bir düzenleme veya uyarının oluşturulmasını OMS Portalı'ndan yapıldığında bu kısa süre içinde kullanıcıların Azure Log Analytics veya Azure uyarıları devam seçeneğiniz vardır.

 ![Zamanlanmış süresi boyunca, kullanıcı eylemi Azure'a yeniden yönlendirilen uyarılar hakkında](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Uyarılar için Azure OMS Portalı'ndan genişletme ücret ve kullanım Azure günlük analizi uyarılar değil faturalandırılacaksınız, belirtilen koşullar ve sınırları içinde kullanıldığında için sorgu tabanlı uyarılar [Azure fiyatlandırma ilkesi İzleyicisi](https://azure.microsoft.com/pricing/details/monitor/)  

Kullanıcılar bu tarihten önce uyarıları genişletmenin avantajlarından; Gönüllü uyarılarını Azure yönetilebilir hale getirmek amacıyla kullanmama tarafından.

### <a name="how-to-voluntarily-extending-your-alerts"></a>Uyarılarınızı gönüllü genişletme nasıl
Azure uyarıları kolay bir geçişinde OMS kullanıcılar etkinleştirmek için Microsoft uyarıları genişletme araç oluşturdu. Microsoft OMS portalı müşteriler uyarılarını Azure OMS portalında (veya) yeni bir API kullanarak programlı bir yaklaşım sihirbazından genişletebilirsiniz. Daha fazla bilgi için bkz: [uyarıları OMS portalı ve API kullanarak Azure genişletme](monitoring-alerts-extend-tool.md).


## <a name="usage-after-extending-your-alerts"></a>Uyarılarınızı genişlettikten sonra kullanım
Belirtildiği gibi Microsoft Operasyon Management Suite içinde oluşturulan uyarıların Azure Uyarıları ' uzatılır; sonra bunları Azure'dan yönetebilirsiniz. Uyarıları OMS portalında - uyarı ayarı bölümünde listelenen devam eder. Aşağıda gösterildiği gibi:

 ![OMS portalı Azure'da genişletilmiş sonra uyarıları listeleme](./media/monitor-alerts-extend/PostExtendList.png)

Düzenleme veya OMS portalında bitti oluşturma gibi uyarılar hakkında herhangi bir işlem için kullanıcıların saydam Azure uyarıları yönlendirilir. 

> [!NOTE]
> Kullanıcılar, Azure üzerinde herhangi bir ek saydam gerçekleştirilecek veya OMS - bir uyarıda eylem Düzenle olarak kullanıcılar düzgün uygun eşlendi emin [Azure İzleyici ve uyarı kullanmak için izinler](monitoring-roles-permissions-security.md)

Oluşturma varolandan devam edecek uyarı [günlük analizi API](../log-analytics/log-analytics-api-alerts.md) ve [günlük analizi kaynak şablonu](../monitoring/monitoring-solutions-resources-searches-alerts.md) uyarıları Azure - Eylem grupları genişletilmiş sonra edilen yalnızca küçük değişiklikle olarak önceki zamanlama ilişkilendirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanılacak araçların öğrenin [başlatma uyarıları OMS Azure'da genişletme](monitoring-alerts-extend-tool.md)
* Yeni hakkında daha fazla bilgi [Azure uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
