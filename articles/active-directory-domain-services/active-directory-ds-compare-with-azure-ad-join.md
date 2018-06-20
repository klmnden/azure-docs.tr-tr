---
title: Azure AD birleştirme ve Azure Active Directory etki alanı Hizmetleri karşılaştırma | Microsoft Docs
description: Azure AD birleştirme ve Azure AD etki alanı hizmetleri arasında seçim yapma
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 31a71d36-58c1-4839-b958-80da0c6a77eb
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: maheshu
ms.openlocfilehash: 8bfc62f978b85399a64da32636627efc7ae234da
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212562"
---
# <a name="choose-between-azure-active-directory-join-and-azure-active-directory-domain-services"></a>Azure Active Directory birleştirme ve Azure Active Directory etki alanı hizmetleri arasında seçim yapma
Bu makalede, Azure Active Directory (AD) birleştirme ve Azure AD etki alanı Hizmetleri ve kullanım örnekleri üzerinde temel seçmenize yardım eden arasındaki farklar açıklanmaktadır.

## <a name="azure-ad-registered-and-azure-ad-joined-devices"></a>Azure AD kayıtlı ve Azure AD alanına katılmış aygıtlar
Azure AD, bu cihazlardan şirket kaynaklarına kuruluş ve Denetim erişim tarafından kullanılan aygıtların kimliğini yönetmenizi sağlar. Kullanıcılar bir kimlikle cihaz sağlayan Azure AD ile (Getir kendi) kişisel cihazlarını kaydedebilir. Sonuç olarak, bir kullanıcı için Azure AD oturum açtığında ve aygıtın güvenlikli kaynaklara erişmek için kullandığı Azure AD cihaz doğrulayabilir. Ayrıca, mobil cihaz Yönetimi (MDM) yazılımı Microsoft Intune gibi kullanarak cihazı yönetebilir. Bu özellik, yönetilen ve ilke uyumlu cihazlardan hassas kaynaklara erişimi kısıtlamak sağlar.

Ayrıca ait cihazlar Azure AD'ye organizasyon birleştirebilirsiniz. Bu mekanizma aynı Azure AD ile kişisel bir cihazı kaydetme avantajları sunar. Ayrıca, kullanıcıların cihazda şirket kimlik bilgilerini kullanarak oturum açabilir. Azure AD alanına katılmış aygıtlar aşağıdaki avantajları sağlar:
* Çoklu oturum açma (SSO) Azure AD tarafından güvenliği sağlanan uygulamalar
* Kuruluş İlkesi uyumlu kullanıcı ayarlarını cihazlar arasında Dolaşım.
* Şirket kimlik bilgilerinizi kullanarak iş için Windows mağazası erişimi.
* İş için Windows Hello
* Kısıtlı erişim uygulamalarına ve kaynaklarına şirket ilkesiyle uyumlu cihazlardan.

| **Aygıt türü** | **Cihaz platformları** | **Mekanizması** |
|:---| --- | --- |
| Kişisel cihazları | Windows 10, iOS, Android, Mac OS | Azure AD kayıtlı |
| Aygıt için katılmamış ait kuruluşun şirket içi AD | Windows 10 | Azure AD alanına katılmış |
| Kuruluş bir şirket içi birleşik aygıt ait AD | Windows 10 | Karma Azure AD alanına katılmış |

Üzerinde Azure AD alanına veya modern tabanlı OAuth/Openıd Connect protokolleri kullanarak kayıtlı cihaz, kullanıcı kimlik doğrulaması gerçekleşir. Bu protokollerin Internet üzerinden çalışmak üzere tasarlanmıştır ve burada kullanıcıların Kurumsal kaynaklara her yerden erişim mobil senaryoları için mükemmeldir.


## <a name="domain-join-to-azure-ad-domain-services-managed-domains"></a>Azure AD etki alanı Hizmetleri yönetilen etki alanı için etki alanına katılma
Azure AD etki alanı Hizmetleri yönetilen bir AD etki alanı bir Azure sanal ağında sağlar. Makine geleneksel etki alanına katılma mekanizmalarını kullanarak bu yönetilen etki alanına katılamaz. Windows İstemcisi (Windows 7, Windows 10) ve Windows Server makinelerini yönetilen etki alanına katılmış. Ayrıca, Linux ve Mac OS makine ayrıca yönetilen etki alanına katılamaz. Bir makine için bir AD etki alanına katıldığında, kullanıcıların şirket kimlik bilgilerini kullanarak makine oturum açabilirsiniz. Bu makineler, bu nedenle, kuruluşunuzun güvenlik ilkeleriyle uyumluluğunu zorlamayı Grup İlkesi kullanılarak yönetilebilir.

Etki alanına katılmış bir makinede NTLM veya Kerberos kimlik doğrulama protokollerini kullanarak kullanıcı kimlik doğrulaması gerçekleşir. Etki alanına katılmış makinenin görüş sırayla çalışması kullanıcı kimlik doğrulaması için yönetilen etki alanının etki alanı denetleyicilerine gerekiyor. Bu nedenle, yönetilen etki alanı ile aynı sanal ağda olması için makine gereksinimlerini etki alanına katıldı. Alternatif olarak, yönetilen etki alanına eşlenen bir sanal ağ veya siteden siteye VPN/ExpressRoute bağlantısı üzerinden bağlı olması için makine gereksinimlerini etki alanına katıldı. Bu nedenle, bu mekanizma, mobil veya kurumsal ağ dışından kaynaklarına bağlanan cihazlar için harika bir uygun değil.

> [!NOTE]
> Teknik olarak, bir şirket içi istemci iş istasyonunda siteden siteye VPN veya ExpressRoute bağlantısı üzerinden yönetilen etki alanına mümkündür. Ancak, son kullanıcı aygıtlarının kullanmanız önerilir (Kişisel aygıtlar) Azure AD ile kaydedilecek ya da cihazı Azure AD (şirket aygıtları) ekleyin. Bu mekanizma daha iyi Internet üzerinden çalışır ve son kullanıcıların her yerden çalışma sağlar. Azure AD etki alanı Hizmetleri Windows veya Linux sunucusu uygulamalarınızı dağıtılmış olduğundan, Azure sanal ağlarda, dağıtılan sanal makineleri için harika bir seçenek.


## <a name="summary---key-differences"></a>Özet - temel farklılıklar
| **En boy** | **Azure AD Join** | **Azure AD etki alanı Hizmetleri** |
|:---| --- | --- |
| Cihaz tarafından denetlenen | Azure AD | Azure AD etki alanı Hizmetleri yönetilen etki alanı |
| Dizinde gösterimi | Azure AD directory içindeki aygıt nesneleri. | AAD DS yönetilen etki alanında bilgisayar nesneleri. |
| Kimlik Doğrulaması | OAuth/Openıd Connect tabanlı iletişim kuralları | Kerberos, NTLM protokolleri |
| Yönetim | Mobil cihaz Yönetimi (MDM) yazılımı Intune gibi | Grup İlkesi |
| Ağ | Internet üzerinden çalışır | Yönetilen etki alanı ile aynı sanal ağda olması için makine gerektirir.|
| Harika... | Son kullanıcı mobil veya Masaüstü aygıtları | Azure'da dağıtılan sunucusu sanal makineleri |


## <a name="next-steps"></a>Sonraki adımlar
### <a name="learn-more-about-azure-ad-domain-services"></a>Azure AD etki alanı hizmetleri hakkında daha fazla bilgi edinin
* [Azure AD Etki Alanı Hizmetleri'ne Genel Bakış](active-directory-ds-overview.md)
* [Özellikler](active-directory-ds-features.md)
* [Dağıtım senaryoları](active-directory-ds-scenarios.md)
* [Azure AD etki alanı Hizmetleri, kullanım örnekleri, uygun öğrenin](active-directory-ds-comparison.md)
* [Azure AD etki alanı Hizmetleri ile Azure AD dizinini nasıl eşitleneceğini anlama](active-directory-ds-synchronization.md)

### <a name="learn-more-about-azure-ad-join"></a>Azure AD katılım hakkında daha fazla bilgi edinin
* [Azure Active Directory'de cihaz yönetimine giriş](../active-directory/device-management-introduction.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri’ni kullanmaya başlama
* [Azure portalını kullanarak Azure AD Etki Alanı Hizmetleri'ni etkinleştirme](active-directory-ds-getting-started.md)
