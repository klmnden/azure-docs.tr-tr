---
title: Rol etkinleştirme ayarlarını yönetme | Microsoft Docs
description: Azure Active Directory Privileged Identity Management uzantısı ile ayrıcalıklı kimlikleri için varsayılan ayarları değiştirmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4ca74c001ba379b287c0c9799d90336eb187b2c2
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619609"
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management içinde rol etkinleştirme ayarlarını yönetme
Ayrıcalıklı Rol Yöneticisi, Azure AD Privileged Identity Management (PIM) uygun rol atamasını etkinleştirme bir kullanıcı deneyimi değiştirme gibi kuruluşlarındaki özelleştirebilirsiniz.

## <a name="manage-the-role-activation-settings"></a>Rol etkinleştirme ayarlarını yönetme
1. Git [Azure portalında](https://portal.azure.com) seçip **Azure AD Privileged Identity Management** panodan uygulama.
2. Seçin **ayrıcalıklı rollerimi Yönet** > **ayarları** > **ayrıcalıklı rolleri**.
3. Rol ayarlarını seçin, yönetmek istediğiniz.

Her rol için Ayarlar sayfasında, ayarları yapılandırabileceğiniz bir dizi vardır. Bu ayarlar, yalnızca uygun Yöneticiler, kalıcı olmayan yönetici kullanıcılar etkiler.

**Etkinleştirme**: süresi dolmadan önce rol etkin kaldığından saat olarak süre. Bu, 1 ile 72 saat arasında olabilir.

**Bildirimleri**: Sistem yöneticileri rol etkinleştirmiş onaylayan e-postaları gönderen olup olmadığını seçin. Bu, yetkisiz veya aykırı etkinleştirmeleri algılamak için yararlı olabilir.

**Olay/istek anahtarı**: uygun yöneticilerinin, rollerine etkinleştirdikleri bir bilet numarası eklemek isteyip istemediğinizi seçin. Rol erişim denetimleri gerçekleştirdiğinizde bu yararlı olabilir.

**Çok faktörlü kimlik doğrulaması**: rollerinin etkinleştirilebilmesi MFA ile kullanıcıların kimlik doğrulamasını gerektir verilip verilmeyeceğini seçebilirsiniz. Bunlar yalnızca bu kez her bir rolü etkinleştirmemek her zaman oturum doğrulamanız gerekir. Mfa'yı etkinleştirdiğinizde akılda tutulması gereken iki ipuçları şunlardır:

* Microsoft hesapları için e-posta adreslerini sahip kullanıcılar (genellikle @outlook.com, ama her zaman kullanılmaz) Azure MFA için kaydedilemiyor. Microsoft hesabı olan kullanıcılar için rol atamak istiyorsanız, bunları kalıcı yönetici yapmak veya o rol için mfa'yı devre dışı bırakmak gerekir.
* MFA yüksek ayrıcalıklı rolleri için Azure AD için devre dışı bırakamazsınız ve Office365. Bu, bu rolleri dikkatli bir şekilde korunması için bir güvenlik özelliğidir:  
  
  * Uygulama yöneticisi
  * Uygulama Ara sunucusu Yöneticisi
  * Faturalama yöneticisi  
  * Uyumluluk yöneticisi  
  * CRM Hizmet Yöneticisi
  * Müşteri Kasası erişim onaylayıcı
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
  * Kullanıcı hesabı yöneticisi  

MFA PIM ile kullanma hakkında daha fazla bilgi için bkz. [gerektiren mfa'yı nasıl](pim-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

