---
title: Azure Active Directory koşullu erişim ilkesi geçiş nedir? | Microsoft Docs
description: Azure portalında Klasik ilkeleri geçirme için bilmeniz gerekenleri öğrenin.
services: active-directory
keywords: Koşullu erişim uygulamalara, Azure AD koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim ile koşullu erişim
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/24/2018
ms.author: joflore
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25161a6317392274ccce8865f7cc0071f0ec89b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112176"
---
# <a name="what-is-a-policy-migration-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim ilkesi geçiş nedir? 


[Koşullu erişim](../active-directory-conditional-access-azure-portal.md) olduğunu denetlemek için nasıl sağlayan Azure Active Directory (Azure AD) bir özellik yetkili kullanıcılara erişimi bulut uygulamalarınızda. Amacı hala aynı olsa da, yeni Azure portal'ın sürüm koşullu erişim nasıl çalışır için önemli geliştirmeler kullanıma sundu.

Azure portalında çünkü oluşturmadınız ilkeleri geçirme dikkate almanız gerekir:

- Şimdi, önce işleyemedi senaryoları karşılayabilirsiniz.

- Bunları birleştirerek yönetmek için kullandığınız ilke sayısına azaltabilir.   

- Tüm koşullu erişim ilkelerinizi tek bir merkezi konumda yönetebilir.

- Klasik Azure portalına kullanımdan kaldırılacaktır.   

Bu makalede, var olan koşullu erişim ilkelerini yeni Framework'e taşımak için bilmeniz gerekenler açıklanmaktadır.
 
## <a name="classic-policies"></a>Klasik ilkeler

İçinde [Azure portalında](https://portal.azure.com), [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasıdır giriş noktanız, koşullu erişim ilkeleri. Ancak, ortamınızda bu sayfayı kullanarak oluşturmadınız koşullu erişim ilkeleri de olabilir. Bu ilkeleri olarak da bilinir *Klasik ilkeleri*. Klasik ilkeleri koşullu erişim ilkeleri, oluşturduğunuz:

- Klasik Azure portalı
- Klasik Intune portalı
- Intune uygulama koruması portalı


Üzerinde **koşullu erişim** sayfa, Klasik ilkelerinizi tıklayarak erişebilirsiniz [ **Klasik ilkeleri (Önizleme)** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) içinde **Yönet** bölümü. 


![Azure Active Directory](./media/policy-migration/71.png)


**Klasik ilkeleri** görünümü için bir seçenek ile sağlar:

- İlkelerinizi Klasik filtreleyin.
 
    ![Azure Active Directory](./media/policy-migration/72.png)

- Klasik ilke devre dışı bırakın.

    ![Azure Active Directory](./media/policy-migration/73.png)
   
- Klasik ilke (ve devre dışı bırakmak) ayarları gözden geçirin.

    ![Azure Active Directory](./media/policy-migration/74.png)


Klasik ilke devre dışı bırakılırsa, bu adımı artık geri alınamaz. Klasik ilke kullanarak bir grup üyeliğini değiştirebilirsiniz. Bu yüzden **ayrıntıları** görünümü. 

![Azure Active Directory](./media/policy-migration/75.png)

Seçili gruplara değiştirerek veya belirli grupların hariç tarafından eklenen tüm kullanıcılar ve gruplar için ilke devre dışı bırakmadan önce birkaç test kullanıcılar için devre dışı bırakılmış bir Klasik ilkesinin etkisini test edebilirsiniz. 



## <a name="azure-ad-conditional-access-policies"></a>Azure AD koşullu erişim ilkeleri

Azure portalında koşullu erişim ile tüm ilkelerinizi tek bir yerden yönetebilirsiniz. Uygulamasının nasıl koşullu erişim önemli ölçüde değiştiğinden, ilkelerinizi Klasik geçirmeden önce şu temel kavramları kendinizi alıştırın.

Bkz.

- [Azure Active Directory'de koşullu erişim nedir](../active-directory-conditional-access-azure-portal.md) temel kavramları ve terminolojisi hakkında bilgi edinmek için.

- [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md) koşullu erişim, kuruluşunuzda dağıtma konusunda rehberlik almak için.

- [Azure Active Directory koşullu erişimiyle birlikte belirli uygulamalar için mfa'yı gerekli](app-based-mfa.md) Azure portalındaki kullanıcı arabirimi öğrenin.


 
## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

Bu makalede, Azure AD koşullu erişim ilkeleri de denir *yeni ilkeler*.
Klasik ilkelerinizi devre dışı bırakın veya silene kadar yeni ilkelerinizi yan yana çalışmaya devam etmektedir. 

Aşağıdaki durumlara İlkesi birleştirme bağlamında önemlidir:

- Klasik ilkeleri bir özel bulut uygulamasına bağlıdır, ancak yeni ilke için gerektiği kadar bulut uygulamaları seçebilirsiniz.

- Klasik ilke ve bir bulut uygulaması için yeni bir ilke denetimleri gerektiren tüm denetimleri (*ve*) yerine getirilmesi için. 


- Yeni bir ilke şunları yapabilirsiniz:
 
    - Birden çok koşulu senaryonuza göre gerekirse birleştirin. 

    - Birkaç vermek erişim gereksinimleri denetim ve bir mantıksal birleştirip seçin *veya* (Seçili denetimlerden birini gerektir) veya bir mantıksal ile *ve* (tüm seçili denetimleri gerektirir).

        ![Azure Active Directory](./media/policy-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange online

İçin Klasik ilkeleri geçirme istiyorsanız **Office 365 Exchange online** içeren **Exchange Active Sync** istemci uygulamaları koşulu olarak, yeni bir ilkesine birleştirmek mümkün olmayabilir. 

Tüm istemci uygulaması türlerini desteklemek istiyorsanız, örneğin, durum budur. Sahip yeni bir ilke **Exchange Active Sync** diğer istemci uygulamalar, istemci uygulamaları koşulu olarak seçemezsiniz.

![Azure Active Directory](./media/policy-migration/64.png)

Bir birleştirme yeni bir ilke de klasik ilkelerinizi çeşitli koşullar içeriyorsa mümkün değildir. Sahip yeni bir ilke **Exchange Active Sync** istemci uygulamaları olarak yapılandırılmış koşulu, diğer koşulları desteklemez:   

![Azure Active Directory](./media/policy-migration/08.png)

Sahip yeni bir ilke varsa **Exchange Active Sync** istemci uygulamaları olarak yapılandırılmış koşulu, gereken diğer tüm koşullar yapılandırılmamış olduğundan emin olun. 

![Azure Active Directory](./media/policy-migration/16.png)
 

[Uygulama tabanlı](technical-reference.md#approved-client-app-requirement) içeren Office 365 Exchange Online için Klasik ilkeleri **Exchange Active Sync** istemci uygulamaları koşulu olarak izin **desteklenen** ve **desteklenmiyor** [cihaz platformlarını](technical-reference.md#device-platform-condition). Bireysel cihaz platformlarını ilgili yeni ilke yapılandıramazsınız, ancak destek sınırlayabilirsiniz [desteklenen cihaz platformları](technical-reference.md#device-platform-condition) yalnızca. 

![Azure Active Directory](./media/policy-migration/65.png)

İçeren birden çok Klasik ilke birleştirebilir **Exchange Active Sync** oluşturulduysa istemci uygulamaları koşulu olarak:

- Yalnızca **Exchange Active Sync** koşulu olarak 

- Erişim verme için birkaç gereksinim

Sık karşılaşılan senaryolardan biri, birleştirme şöyledir:

- Bir cihaz tabanlı Klasik ilke Azure Klasik portalından 
- Bir uygulama tabanlı Klasik portalında Bu ilkeyi Intune uygulama koruması 
 
Bu durumda, seçilen her iki gereksinimleri olan bir yeni ilkesine Klasik ilkelerini birleştirebilir.

![Azure Active Directory](./media/policy-migration/62.png)



### <a name="device-platforms"></a>Cihaz platformları

Klasik ilkeleri ile [denetimleri uygulama tabanlı](technical-reference.md#approved-client-app-requirement) iOS ve Android ile önceden yapılandırılmış [cihaz platformu koşul](technical-reference.md#device-platform-condition). 

Yeni bir ilke seçmeniz gerekir. [cihaz platformlarını](technical-reference.md#device-platform-condition) ayrı ayrı desteklemek istediğiniz.

![Azure Active Directory](./media/policy-migration/41.png)



 
 


## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [gerektiren MFA belirli uygulamalar için Azure Active Directory koşullu erişim ile](app-based-mfa.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md). 
