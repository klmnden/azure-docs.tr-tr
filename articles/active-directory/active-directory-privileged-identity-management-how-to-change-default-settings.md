---
title: Rol etkinleştirme ayarlarını yönetme | Microsoft Docs
description: Azure Active Directory Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için varsayılan ayarları değiştirmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 06/06/2017
ms.author: curtand
ms.custom: pim
ms.openlocfilehash: 972fd1e322e578516073307d01548132473bc52c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ın rol etkinleştirme ayarlarını yönetme
Ayrıcalıklı Rol Yöneticisi Azure AD Privileged Identity Management (PIM) uygun rol ataması etkinleştirme bir kullanıcı deneyimini değiştirme gibi kuruluşlarındaki özelleştirebilirsiniz.

## <a name="manage-the-role-activation-settings"></a>Rol etkinleştirme ayarlarını yönet
1. Git [Azure portal](https://portal.azure.com) seçip **Azure AD Privileged Identity Management** panodan uygulama.
2. Seçin **yönetmek ayrıcalıklı rolleri** > **ayarları** > **ayrıcalıklı rolleri**.
3. Rolü ayarlarını seçin, yönetmek istediğiniz.

Her rol için Ayarları sayfasında, ayarları yapılandırabileceğiniz bir dizi vardır. Bu ayarlar yalnızca uygun yönetici, değil kalıcı yönetici sahip kullanıcıların etkiler.

**Etkinleştirme**: süresi dolmadan önce bir rol etkin kalmasını saat cinsinden süre. Bu, 1 ila 72 saat arasında olabilir.

**Bildirimleri**: rol etkinleştirdikten admins onaylayan Sistem e-posta gönderir olup olmadığını seçebilirsiniz. Bu, yetkisiz veya aykırı etkinleştirmeleri algılamak için yararlı olabilir.

**Olay/istek anahtarı**: rolleri etkinleştirdiğinizde bilet numarası eklemek için uygun yöneticilerinin kullanmamayı seçebilirsiniz. Rol erişim denetimleri gerçekleştirdiğinizde bu yararlı olabilir.

**Çok faktörlü kimlik doğrulaması**: kullanıcılar kendi rolleri etkinleştirilebilmesi MFA ile kullanıcıların kimliğini doğrulamak gerekli verilip verilmeyeceğini seçebilirsiniz. Yalnızca bu kez her bir rolü etkinleştirmemek her zaman oturum doğrulamak sahiptirler. MFA etkinleştirdiğinizde göz önünde bulundurmanız iki ipuçları şunlardır:

* E-postaları için Microsoft hesabına sahip kullanıcıların (genellikle @outlook.com, ancak her zaman) için Azure MFA kaydedilemiyor. Microsoft hesabı olan kullanıcılar için rol atamak istiyorsanız, bunları kalıcı yönetici yapın veya bu rol için MFA devre dışı bırakmak gerekir.
* MFA için üst düzey ayrıcalıklı rolleri Azure AD için devre dışı bırakamazsınız ve Office365. Bu, bu rolleri dikkatle korunması için bir güvenlik özelliğidir:  
  
  * Uygulama yöneticisi
  * Uygulama Proxy sunucusu Yöneticisi
  * Faturalama yöneticisi  
  * Uyumluluk yöneticisi  
  * CRM Hizmet Yöneticisi
  * Müşteri kasa erişimi onaylayıcı
  * Directory yazıcısı  
  * Exchange yöneticisi  
  * Genel yönetici
  * Intune hizmet yöneticisi
  * Posta kutusu Yöneticisi  
  * Partner tier1 desteği  
  * Partner tier2 desteği  
  * Ayrıcalıklı rol yöneticisi   
  * Güvenlik yöneticisi  
  * SharePoint yöneticisi  
  * Skype Kurumsal yöneticisi  
  * Kullanıcı Hesap Yöneticisi  

MFA ile PIM kullanma hakkında daha fazla bilgi için bkz: [gerektiren MFA nasıl](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

