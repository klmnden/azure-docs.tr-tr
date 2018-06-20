---
title: Azure Active Directory koşullu erişim ilkesi geçişi nedir? | Microsoft Docs
description: Azure portalında Klasik ilkelerine bilmeniz gerekenleri öğrenin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: femila
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 4a9b3df66567c4170ba861d3e597261e37271bf1
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232540"
---
# <a name="what-is-a-policy-migration-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim ilkesi geçişi nedir? 


[Koşullu erişim](active-directory-conditional-access-azure-portal.md) olduğunu denetlemek için nasıl sağlayan Azure Active directory (Azure AD) yeteneğini yetkili kullanıcılara erişimi, bulut uygulamalarınızı. Amacı hala aynı olsa da, yeni Azure portalına sürümü koşullu erişimin nasıl çalıştığı için önemli geliştirmeler anlatılmıştır.

Nedeniyle Azure portalında oluşturmadınız ilkeleri geçirme dikkate almanız gerekir:

- Şimdi önce işleyemedi senaryolarını ele alabilir.

- Bunları birleştirerek yönetmek zorunda ilkeleri sayısını azaltabilirsiniz.   

- Tek bir merkezi konumda tüm koşullu erişim ilkelerini yönetebilirsiniz.

- Klasik Azure portalı kullanımdan kaldırılacaktır.   

Bu makalede, var olan koşullu erişim ilkelerini yeni framework geçirmek için bilmeniz gerekenler açıklanmaktadır.
 
## <a name="classic-policies"></a>Klasik ilkeler

İçinde [Azure portal](https://portal.azure.com), [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasıdır giriş noktanızdır koşullu erişim ilkeleri. Ancak, ortamınızda bu sayfayı kullanarak oluşturmadınız koşullu erişim ilkeleri de olabilir. Bu ilkeler olarak da bilinir *Klasik ilkeleri*. Klasik ilkelerini koşullu erişim ilkeleri, oluşturduğunuz:

- Klasik Azure portalı
- Intune Klasik portal
- Intune uygulama koruma portalı


Üzerinde **koşullu erişim** sayfa, Klasik ilkelerinizi tıklatarak erişebilirsiniz [ **Klasik ilkeleri (Önizleme)** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) içinde **Yönet** bölüm. 


![Azure Active Directory](./media/active-directory-conditional-access-migration/71.png)


**Klasik ilkeleri** görünümü için bir seçenek ile sağlar:

- Klasik ilkelerinizi filtreleyin.
 
    ![Azure Active Directory](./media/active-directory-conditional-access-migration/72.png)

- Klasik ilkeleri devre dışı bırakın.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/73.png)
   
- Klasik ilkelerin (ve devre dışı bırakmak) ayarlarını gözden geçirin.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/74.png)


Klasik bir ilke devre dışı bırakılırsa, bu adımı artık geri alınamaz. Klasik İlkesi kullanarak grup üyeliğini değiştirebilirsiniz bu yüzden **ayrıntıları** görünümü. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/75.png)

Seçilen grupları değiştirerek veya belirli grupların hariç tarafından eklenen tüm kullanıcılar ve gruplar için ilke devre dışı bırakmadan önce birkaç test kullanıcılar için devre dışı bırakılmış bir Klasik ilkesinin etkisini test edebilirsiniz. 



## <a name="azure-ad-conditional-access-policies"></a>Azure AD koşullu erişim ilkeleri

Azure portalında koşullu erişim ile tek bir merkezi konumda tüm ilkelerinizi yönetebilirsiniz. Koşullu erişim uygulama önemli ölçüde değiştiğinden, Klasik ilkelerinizi geçirmeden önce şu temel kavramları kendiniz alışmanız gerekir.

Bkz:

- [Azure Active Directory'de koşullu erişim nedir](active-directory-conditional-access-azure-portal.md) temel kavramları ve terminolojiyi hakkında bilgi edinmek için.

- [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md) koşullu erişim, kuruluşunuzda dağıtma konusunda bazı yönergeler almak için.

- [Azure Active Directory koşullu erişimi olan belirli uygulamalar için MFA'ya gerek](active-directory-conditional-access-app-based-mfa.md) Azure portalında kullanıcı arabirimi tanımak için.


 
## <a name="migration-considerations"></a>Geçiş konuları

Bu makalede, Azure AD koşullu erişim ilkeleri de denir *yeni ilkeler*.
Klasik ilkelerinizi devre dışı bırakmak veya silene kadar yeni ilkelerinizi yan yana çalışmaya devam eder. 

Aşağıdaki durumlar İlkesi birleştirme bağlamında önemlidir:

- Klasik ilkeler bir özel bulut uygulamasına bağlıdır, ancak yeni ilke için gereksinim duyduğunuz kadar çok bulut uygulamaları seçebilirsiniz.

- Klasik bir ilkeyi ve bir bulut uygulaması için yeni bir ilke denetimlerin gerektiren tüm denetimler (*ve*) yerine getirilmesi için. 


- Yeni bir ilke şunları yapabilirsiniz:
 
    - Birden çok koşul senaryonuz tarafından gerekiyorsa birleştirmek. 

    - Erişim gereksinimlerini denetlemek ve bir mantıksal ile birleştirip birkaç vermek seçin *veya* (Seçili denetimleri birini gerektirir) veya bir mantıksal ile *ve* (tüm seçili denetimlerin gerekir).

        ![Azure Active Directory](./media/active-directory-conditional-access-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange online

Klasik ilkelerini geçirmek istiyorsanız **Office 365 Exchange online** içeren **Exchange Active Sync** istemci uygulamaları koşulu olarak, yeni bir ilke birleştirmek kuramamış olabilir. 

Tüm istemci uygulaması türlerini desteklemek istiyorsanız, bu, örneğin, bir durumdur. Sahip yeni bir ilke **Exchange Active Sync** istemci uygulamaları koşulu olarak diğer istemci uygulamaları seçemezsiniz.

![Azure Active Directory](./media/active-directory-conditional-access-migration/64.png)

Yeni bir ilke içine bir birleştirme de klasik ilkelerinizi çeşitli koşullar içeriyorsa mümkün değildir. Sahip yeni bir ilke **Exchange Active Sync** istemci uygulamaları olarak yapılandırılmış koşul diğer koşulları desteklemez:   

![Azure Active Directory](./media/active-directory-conditional-access-migration/08.png)

Sahip yeni bir ilke varsa **Exchange Active Sync** istemci uygulamaları olarak yapılandırılmış koşul gereken diğer tüm koşulları yapılandırılmamış olduğundan emin olun. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/16.png)
 

[Uygulama tabanlı](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) içeren Office 365 Exchange Online için Klasik ilkelerini **Exchange Active Sync** istemci uygulamaları koşulu olarak izin **desteklenen** ve **desteklenmiyor** [cihaz platformları](active-directory-conditional-access-technical-reference.md#device-platform-condition). İlgili yeni ilke tek tek cihaz platformları yapılandıramazsınız olsa da, destek sınırlayabilirsiniz [desteklenen cihaz platformlarında](active-directory-conditional-access-technical-reference.md#device-platform-condition) yalnızca. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/65.png)

İçeren birden çok Klasik ilkelerini birleştirebilir **Exchange Active Sync** bunlar varsa, istemci uygulamaları koşulu olarak:

- Yalnızca **Exchange Active Sync** koşulu olarak 

- Yapılandırılan erişim izni verme için birkaç gereksinimi

Yaygın bir senaryo birleştirilmesi şöyledir:

- Bir aygıt tabanlı Klasik ilkesinden Klasik Azure portalı 
- Bir uygulama tabanlı Klasik ilke Intune app koruma portalı 
 
Bu durumda, seçilen iki gereksinimleri olan bir yeni ilkesine Klasik ilkelerinizi birleştirebilir.

![Azure Active Directory](./media/active-directory-conditional-access-migration/62.png)



### <a name="device-platforms"></a>Cihaz platformları

Klasik ilkeleriyle [uygulama tabanlı denetimler](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) iOS ve Android olarak önceden yapılandırılmış olan [cihaz platformu koşul](active-directory-conditional-access-technical-reference.md#device-platform-condition). 

Yeni bir ilke seçmeniz gerekir. [cihaz platformları](active-directory-conditional-access-technical-reference.md#device-platform-condition) ayrı ayrı desteklemek istediğiniz.

![Azure Active Directory](./media/active-directory-conditional-access-migration/41.png)



 
 


## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [GRequire MFA Azure Active Directory koşullu erişimi olan belirli uygulamalar için](active-directory-conditional-access-app-based-mfa.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
