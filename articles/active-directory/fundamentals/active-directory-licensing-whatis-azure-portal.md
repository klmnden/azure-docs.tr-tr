---
title: Hangi Grup tabanlı lisanslama - Azure Active Directory | Microsoft Docs
description: Nasıl çalıştığını ve en iyi uygulamalar da dahil olmak üzere Azure Active Directory grup tabanlı lisanslama hakkında bilgi edinin.
services: active-directory
keywords: Azure AD lisanslama
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.openlocfilehash: c83eeb78f041b86032f1ebb28949cd035487b81e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086360"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Grup tabanlı Azure Active Directory lisansı nedir?

Microsoft'un ücretli bulut hizmetleri Office 365, Enterprise Mobility + Security, Dynamics 365 ve diğer benzer ürünler lisans gerektirir. Bu lisanslar, bu hizmetlere erişmesi gereken her kullanıcıya atanır. Lisansları yönetmek için, yöneticiler yönetim portallarından birini (Office veya Azure) ve PowerShell cmdlet'lerini kullanır. Azure Active Directory (Azure AD), tüm Microsoft bulut hizmetleri için kimlik yönetimini destekleyen temel altyapıdır. Azure AD kullanıcıların lisans atama durumları hakkındaki bilgileri depolar.

Şimdiye kadar, lisanslar yalnızca tek tek kullanıcı düzeyinde atanabiliyordu ve bu da geniş ölçekli yönetimi zorlaştırabiliyordu. Örneğin, kuruluşta veya bölümde yeni katılan veya ayrılan kullanıcılar gibi kurumsal değişikliklere göre kullanıcı lisansları eklemek veya kaldırmak için, yöneticinin çoğunlukla karmaşık bir PowerShell betiği yazması gerekir. Bu betik bulut hizmetine tek tek çağrılar yapar.

Bu zorlukları gidermek için, Azure AD artık grup tabanlı lisanslama özelliğini içermektedir. Gruba bir veya birden çok ürün lisansı atayabilirsiniz. Azure AD lisansların tüm grup üyelerine atanmasını güvence altına alır. Gruba katılan tüm yeni üyelere uygun lisanslar atanır. Kullanıcılar gruptan ayrıldığında, söz konusu lisanslar kaldırılır. Bu, PowerShell yoluyla lisans yönetimini kuruluşta ve bölüm yapısında tek tek kullanıcı değişikliklerini yansıtacak şekilde otomatik hale getirme gereksinimini ortadan kaldırır.


## <a name="features"></a>Özellikler

Grup tabanlı lisanslamanın başlıca özellikleri şunlardır:

- Lisanslar Azure AD'de her güvenlik grubuna atanabilir. Güvenlik grupları, Azure AD Connect kullanarak şirket içi, eşitlenebilir. Ayrıca, güvenlik gruplarını doğrudan Azure AD'de (yalnızca bulut grupları olarak da adlandırılır) veya Azure AD dinamik grup özelliği yoluyla otomatik olarak oluşturabilirsiniz.

- Gruba bir ürün lisansı atandığında, yönetici üründe bir veya birden çok hizmet planını devre dışı bırakabilir. Normalde bu işlem, kuruluş ürüne eklenmiş olan hizmeti kullanmaya henüz hazır olmadığında yapılır. Örneğin, yönetici bir bölüme Office 365'i atayabilir ama Yammer hizmetini geçici olarak devre dışı bırakabilir.

- Kullanıcı düzeyi lisanslama gerektiren tüm Microsoft bulut hizmetleri desteklenir. Bunlar tüm Office 365 ürünlerini, Enterprise Mobility + Security'yi ve Dynamics 365'i içerir.

- Grup tabanlı lisanslama şu anda yalnızca [Azure portal](https://portal.azure.com) üzerinden sağlanır. Kullanıcı ve grup yönetimi için öncelikli olarak Office 365 portalı gibi diğer yönetim portallarını kullanıyorsanız, bunu yapmaya devam edebilirsiniz. Ancak, lisansları grup düzeyinde yönetmek için Azure portalını kullanmalısınız.

- Azure AD, grup üyeliği değişikliklerinden kaynaklanan lisans değişikliklerini otomatik olarak yönetir. Normalde, lisans değişiklikleri üyelik değişikliğini izleyen birkaç dakika içinde geçerlilik kazanır.

- Kullanıcı, lisans ilkeleri belirtilmiş olan birden çok gruba üye olabilir. Ayrıca kullanıcının, herhangi bir grubun dışında doğrudan atanmış bazı lisansları da olabilir. Sonuçta elde edilen kullanıcı durumu tüm atanan ürün ve hizmet lisanslarının bir bileşimidir. Birden fazla kaynaktan atanmış bir kullanıcı aynı lisans, lisans yalnızca bir kez tüketilir.

- Bazı durumlarda, bir kullanıcıya lisans atanamayabilir. Örneğin, kiracıda yeterli kullanılabilir lisans olmayabilir veya aynı anda çakışan hizmetler atanmış olabilir. Yöneticiler, Azure AD'nin grup lisanslarını tam olarak işleyemediği kullanıcılar hakkındaki bilgilere erişebilir. Bu bilgiler temelinde düzeltme eylemi gerçekleştirebilir.

- Ücretli veya deneme aboneliği Azure AD temel veya bir ücretli veya deneme Office 365 Kurumsal E3, Office 365 A3 ve üzeri sürümleri, kiracıda, Grup tabanlı lisans yönetimi kullanmak için gereklidir. Bu özellik, lisans atanmış olan grupların üyesi olan her benzersiz kullanıcı lisansı gerektirir. Lisans atanan gruplarının üyeleri olarak bunlar için kullanıcılara lisans atama gerekmez ancak tüm kullanıcıları kapsayacak kiracıda lisansları en az sayıda olmalıdır. Örneğin, lisans atanmış kiracınızda içindeki tüm gruplar 1.000 benzersiz kullanıcıların toplam olsaydı, lisans gereksinimi karşılamak için en az 1000 lisansları gerekir.

## <a name="your-feedback-is-welcome"></a>Geri bildiriminiz bizim için çok önemlidir!

Geri bildirimleriniz veya özellik istekleriniz varsa, lütfen [Azure AD yönetici forumunu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510) kullanarak bunları bizimle paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisanslama aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için bkz:

* [Azure Active Directory'de gruba lisans atama](../users-groups-roles/licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](../users-groups-roles/licensing-groups-migrate-users.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](../users-groups-roles/licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../users-groups-roles/licensing-group-advanced.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](../users-groups-roles/licensing-ps-examples.md)
