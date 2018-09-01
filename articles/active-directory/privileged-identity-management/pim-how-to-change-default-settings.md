---
title: PIM'de Azure AD dizini rol ayarlarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure AD dizini rol ayarlarını yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8af304d28be9b0884a50d60911d901818b514752
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338916"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>PIM'de Azure AD dizini rol ayarlarını yapılandırma

Ayrıcalıklı Rol Yöneticisi, Azure AD Privileged Identity Management (PIM) uygun rol atamasını etkinleştirme bir kullanıcı deneyimi değiştirme gibi kuruluşlarındaki özelleştirebilirsiniz.

## <a name="open-role-settings"></a>Rol ayarlarını Aç

Bir Azure AD dizin rolü ayarlarını açmak için şu adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **ayarları**.

    ![Azure AD Dizin rolleri - ayarlar](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Tıklayın **rolleri**.

1. Ayarları yapılandırmak istediğiniz rolüne tıklayın.

    ![Azure AD Dizin rolleri - ayarları rolleri](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Her rol için Ayarlar sayfasında, yapılandırabileceğiniz birkaç ayar vardır. Bu ayarlar yalnızca kullanıcılar etkiler **uygun** atamaları değil **kalıcı** atamaları.

## <a name="activations"></a>Etkinleştirmeler

Kullanım **etkinleştirmeleri** en uzun süreyi saat cinsinden süresi dolmadan önce rol etkin kaldığından ayarlamak için kaydırıcıyı. Bu değer 1 ile 72 saat arasında olabilir.

## <a name="notifications"></a>Bildirimler

Kullanım **bildirimleri** sistem rol etkinleştirmiş onaylama yöneticileri için e-postaları gönderen olup olmadığını belirlemek için anahtarı. Bu, yetkisiz veya aykırı etkinleştirmeleri algılamak için yararlı olabilir.

## <a name="incidentrequest-ticket"></a>Olay/İstek anahtarı

Kullanım **olay/istek anahtarı** uygun yöneticilerin bir bilet numarası yerine getirecekleri etkinleştirdikleri işlemlerinde gerekip gerekmediğini belirlemek için anahtarı. Rol erişim denetimleri gerçekleştirdiğinizde bu yararlı olabilir.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Kullanım **multi-Factor Authentication** rollerinin etkinleştirilebilmesi MFA ile kullanıcıların kimliğini doğrulamak kullanıcıların gerekip gerekmediğini belirlemek için anahtarı. Bunlar yalnızca bu kez her bir rolü etkinleştirmemek her zaman oturum doğrulamanız gerekir. Mfa'yı etkinleştirdiğinizde akılda tutulması gereken iki ipuçları şunlardır:

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

MFA PIM ile kullanma hakkında daha fazla bilgi için bkz. [Azure AD dizin rollerini PIM için çok faktörlü kimlik doğrulaması gerektiren](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Onay iste

Bir rolü etkinleştirmek için onay gerektir istiyorsanız, aşağıdaki adımları izleyin.

1. Ayarlama **onayı iste** geçin **etkin**. Onaylayanları seçin seçeneklerle bölmesini genişletir.

    ![Azure AD Dizin rolleri - Settings - onayı iste](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Varsa, **yok** tüm onaylayanlar belirtin, varsayılan onaylayanlar ayrıcalıklı rol yöneticileri olur. Ayrıcalıklı rol yöneticileri gereken onaylanacak **tüm** bu rol için etkinleştirme istekleri.

1. Onaylayanlar belirtmek için **onaylayanları seçin**.

    ![Azure AD Dizin rolleri - Settings - onayı iste](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Bir veya daha fazla onaylayanları seçin ve ardından **seçin**. Kullanıcılar veya gruplar seçebilirsiniz. En az 2 onaylayan önerilir. Kendini onaylama izin verilmez.

    Seçimlerinizi seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirledikten sonra tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD dizin rollerini PIM atayın](pim-how-to-add-role-to-user.md)
- [Azure AD dizin rollerini PIM için çok faktörlü kimlik doğrulaması gerektir](pim-how-to-require-mfa.md)
- [Azure AD Dizin rolleri için güvenlik uyarıları PIM'de yapılandırın](pim-how-to-configure-security-alerts.md)
