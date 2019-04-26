---
title: Azure AD'ye katılım'ı ve Azure Active Directory etki alanı Hizmetleri karşılaştırın | Microsoft Docs
description: Azure AD'ye katılım'ı ve Azure AD etki alanı hizmetleri arasında seçim yapma
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 31a71d36-58c1-4839-b958-80da0c6a77eb
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2017
ms.author: ergreenl
ms.openlocfilehash: d4f50ea89f2623d387fb77acb09e609def547468
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60359441"
---
# <a name="choose-between-azure-active-directory-join-and-azure-active-directory-domain-services"></a>Azure Active Directory join ile Azure Active Directory etki alanı hizmetleri arasında seçim yapma
Bu makalede, Azure Active Directory (AD) birleştirme, Azure AD Domain Services ve kullanım örnekleri üzerinde temel seçmenize yardım eden arasındaki farklar açıklanmaktadır.

## <a name="azure-ad-registered-and-azure-ad-joined-devices"></a>Azure AD'ye kayıtlı ve Azure AD'ye katılmış cihazlar
Azure AD, kuruluş ve Denetim erişim sağlamak, bu cihazlardan şirket kaynaklarına kullanılan cihazların kimliğini yönetmenizi sağlar. Kullanıcıların kişisel cihazlarını (Getir kendi), temsili bir kimlikle cihaz sağlayan Azure AD ile kaydedebilirsiniz. Sonuç olarak, bir kullanıcı oturum açtığında Azure AD'ye ve aygıtın güvenlikli kaynaklara erişmek için kullandığı Azure AD cihaz kimlik doğrulaması yapabilir. Ayrıca, mobil cihaz Yönetimi (MDM) yazılımı Intune gibi kullanarak cihazı yönetebilir. Bu özellik, yönetilen ve uyumlu cihazlardan hassas kaynaklara erişimi sınırlamak sağlar.

Ayrıca, kuruluşa ait cihazlar Azure AD'ye katılmasını sağlayabilirsiniz. Bu mekanizma, kişisel bir cihazı Azure AD'ye kaydetme aynı avantajları sunar. Ayrıca, kullanıcılar cihazda şirket kimlik bilgilerini kullanarak oturum açabilir. Azure AD'ye katılmış cihazları aşağıdaki avantajları sağlar:
* Çoklu oturum açma (SSO) Azure AD tarafından güvenliği sağlanan uygulamalar
* Kuruluş İlkesi uyumlu kullanıcı ayarlarını cihazlar arasında Dolaşım.
* Kurumsal kimlik bilgilerinizi kullanarak iş için Windows Store erişim.
* İş İçin Windows Hello
* Uygulamalar ve kaynaklar için Kurumsal ilkeyle uyumlu cihazlardan kısıtlı erişim.

| **Cihaz türü** | **Cihaz platformları** | **Mekanizması** |
|:---| --- | --- |
| Kişisel cihazları | Windows 10, iOS, Android, Mac OS | Azure AD kayıtlı |
| Cihaz için katılmamış ait Kuruluş şirket içi AD | Windows 10 | Azure AD'ye katıldı |
| Kuruluşa ait cihazı alanına katılmış bir şirket içi AD | Windows 10 | Hibrit Azure AD'ye katıldı |

Bir Azure AD alanına katılmış veya modern tabanlı OAuth/Openıd Connect protokolleri kullanarak kayıtlı cihaz, kullanıcı kimlik doğrulaması gerçekleşir. Bu protokollerin internet üzerinden çalışacak şekilde tasarlanmıştır ve kullanıcıların şirket kaynaklarına her yerden eriştiği mobil senaryolar için idealdir.


## <a name="domain-join-to-azure-ad-domain-services-managed-domains"></a>Azure AD Domain Services yönetilen etki alanlarına etki alanına katılma
Azure AD Domain Services, Azure sanal ağında yönetilen bir AD etki alanı sağlar. Makineleri geleneksel etki alanına katılma mekanizmalarını kullanarak bu yönetilen etki alanına katılmasını sağlayabilirsiniz. Windows istemci (Windows 7, Windows 10) ve Windows Server makineleri yönetilen etki alanına katılabilir. Ayrıca, Linux ve Mac OS makineleri yönetilen etki de katılabilirsiniz. Bir makine için bir AD etki alanına katıldığında, kullanıcılar şirket kimlik bilgilerini kullanarak makineye oturum açabilir. Bu makineler, bu nedenle, kuruluşunuzun güvenlik ilkeleriyle uyumluluğu zorlama, Grup İlkesi kullanarak yönetilebilir.

Etki alanına katılmış bir makinede, NTLM veya Kerberos kimlik doğrulama protokolleri kullanarak kullanıcı kimlik doğrulaması gerçekleşir. Etki alanına katılmış makinede görebilmesi için sırayla çalışmak kullanıcı kimlik doğrulaması için yönetilen etki alanının etki alanı denetleyicileri gerekir. Bu nedenle, yönetilen etki alanı ile aynı sanal ağda olması için makine gereksinimlerine etki alanına katıldı. Alternatif olarak, yönetilen etki alanına eşlenmiş bir sanal ağ veya siteden siteye VPN/ExpressRoute bağlantısı üzerinden bağlanılmak makine gereksinimlerine etki alanına katıldı. Bu nedenle, bu mekanizma, kurumsal ağ dışından bağlanmak veya mobil cihazlar için çok uygun değildir.

> [!NOTE]
> Teknik olarak, bir şirket içi istemci iş istasyonu, siteden siteye VPN veya ExpressRoute bağlantısı üzerinden yönetilen etki alanına mümkündür. Ancak, için son kullanıcı cihazlarında kullanmanız önerilir (Kişisel cihazlar) Azure AD'ye cihaz kaydetme veya cihazı Azure AD'ye (şirket aygıtları) katılın. Bu mekanizma, internet üzerinden daha iyi çalışır ve son kullanıcıların yerden çalışmalarına olanak tanır. Azure AD etki alanı Hizmetleri Windows veya Linux sunucusu üzerinde dağıtılan uygulamalarınızı Azure sanal ağlarınızda, dağıtılan sanal makineler için idealdir.


## <a name="summary---key-differences"></a>Özet - temel farklılıklar
| **En boy** | **Azure AD Join** | **Azure AD etki alanı Hizmetleri** |
|:---| --- | --- |
| Denetlenen cihaz | Azure AD | Azure AD Domain Services yönetilen etki alanı |
| Dizini gösteriminde | Cihaz nesneleri, Azure AD dizini. | AAD-DS yönetilen etki alanında bilgisayar nesneleri. |
| Kimlik Doğrulaması | Tabanlı OAuth/Openıd Connect protokolleri | Kerberos, NTLM protokolleri |
| Yönetim | Intune gibi mobil cihaz Yönetimi (MDM) yazılımı | Grup İlkesi |
| Ağ | İnternet üzerinden çalışır | Makineleri yönetilen etki alanı ile aynı sanal ağda olmasını gerektirir.|
| Harika... | Son kullanıcı, mobil veya Masaüstü cihazları | Server sanal makineleri, Azure'a dağıtılmış |


## <a name="next-steps"></a>Sonraki adımlar
### <a name="learn-more-about-azure-ad-domain-services"></a>Azure AD Domain Services hakkında daha fazla bilgi edinin
* [Azure AD Domain Services genel bakış](active-directory-ds-overview.md)
* [Özellikler](active-directory-ds-features.md)
* [Dağıtım senaryoları](active-directory-ds-scenarios.md)
* [Azure AD Domain Services, kullanım örneklerinize uygun öğrenin](active-directory-ds-comparison.md)
* [Azure AD Domain Services ile Azure AD dizininizi nasıl eşitleneceğini anlama](active-directory-ds-synchronization.md)

### <a name="learn-more-about-azure-ad-join"></a>Azure AD'ye katılımı hakkında daha fazla bilgi edinin
* [Azure Active Directory'de cihaz yönetimine giriş](../active-directory/device-management-introduction.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri’ni kullanmaya başlama
* [Azure portalını kullanarak Azure AD Domain Services'ı etkinleştir](active-directory-ds-getting-started.md)
