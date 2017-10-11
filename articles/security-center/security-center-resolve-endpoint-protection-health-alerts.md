---
title: "Azure Güvenlik Merkezi'nde Endpoint protection durumu uyarıları gidermek | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir ** çözmek Endpoint Protection durumu uyarıları **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4050f453-98fc-4314-8438-d476469757fb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: 5e6b136d6bd3b11fb82126d104fd0cb149255118
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde uç nokta koruma durumu uyarıları Çözümle
Azure Güvenlik Merkezi, algılanan endpoint protection durumu uyarıları gidermek önerir.  Güvenlik Merkezi, endpoint protection hatalarını ve kaç tane hataları hangi sanal makineleri (VM'ler) sahip görmenizi sağlar.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.
> 
> 

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **öneriler dikey**seçin **gidermek Endpoint Protection durumu uyarıları**.
   ![Endpoint Protection sistem durumu uyarılarını çözümleme][1]
2. Bu dikey pencere açılır **Endpoint Protection hata** listelerinin VM'ler hataları ve hata sayısı için her bir VM. VM listeden seçin.
   ![Uç nokta koruma hatası][2]
3. A **hataları listesi** hatalarının listesini görüntüleyen seçili VM için dikey pencere açılır. Bir hata daha fazla bilgi için listeden seçin. Seçili hata hakkında bilgi içeren bir dikey pencere açılır.
   ![Hataları listesi][3]
   ![hatası olayı][4]

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
