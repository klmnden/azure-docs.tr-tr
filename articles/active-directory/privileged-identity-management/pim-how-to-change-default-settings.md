---
title: PIM - Azure Active Directory Azure AD rol ayarlarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure AD rol ayarlarını yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3cb8944578caf00fac5ca430e411f044a875f6af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60288739"
---
# <a name="configure-azure-ad-role-settings-in-pim"></a>PIM'de Azure AD rol ayarlarını yapılandırma

Ayrıcalıklı rol yöneticisi uygun rol atamasını etkinleştirme bir kullanıcı deneyimi değiştirme dahil olmak üzere, bir kuruluş içindeki Azure Active Directory (Azure AD) Privileged Identity Management (PIM) özelleştirebilirsiniz.

## <a name="open-role-settings"></a>Rol ayarlarını Aç

Azure AD rolüne ayarlarını açmak için şu adımları izleyin.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **ayarları**.

    ![Azure AD rolleri - ayarlar](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Tıklayın **rolleri**.

1. Ayarları yapılandırmak istediğiniz rolüne tıklayın.

    ![Azure AD rolleri - ayarları roller](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Her rol için Ayarlar sayfasında, yapılandırabileceğiniz birkaç ayar vardır. Bu ayarlar yalnızca kullanıcılar etkiler **uygun** atamaları değil **kalıcı** atamaları.

## <a name="activations"></a>Etkinleştirmeler

Kullanım **etkinleştirmeleri** en uzun süreyi saat cinsinden süresi dolmadan önce rol etkin kaldığından ayarlamak için kaydırıcıyı. Bu değer 1 ile 72 saat arasında olabilir.

## <a name="notifications"></a>Bildirimler

Kullanım **bildirimleri** rolleri etkinleştirildiğinde Yöneticiler e-posta bildirimleri alıp almayacaklarını belirtmek için anahtar. Bu, yetkisiz veya aykırı etkinleştirmeleri algılamak için yararlı olabilir.

Ayarlandığında **etkinleştirme**, bildirimler gönderilir:

- Ayrıcalıklı Rol Yöneticisi
- Güvenlik Yöneticisi
- Genel Yönetici

Daha fazla bilgi için [e-posta bildirimleri PIM](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Olay/İstek anahtarı

Kullanım **olay/istek anahtarı** uygun yöneticilerin bir bilet numarası yerine getirecekleri etkinleştirdikleri işlemlerinde gerekip gerekmediğini belirlemek için anahtarı. Rol erişim denetimleri gerçekleştirdiğinizde bu yararlı olabilir.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Kullanım **multi-Factor Authentication** rollerinin etkinleştirilebilmesi MFA ile kullanıcıların kimliğini doğrulamak kullanıcıların gerekip gerekmediğini belirlemek için anahtarı. Bunlar yalnızca bu kez her bir rolü etkinleştirmemek her zaman oturum doğrulamanız gerekir. Mfa'yı etkinleştirdiğinizde akılda tutulması gereken iki ipuçları şunlardır:

* Microsoft hesapları için e-posta adreslerini sahip kullanıcılar (genellikle @outlook.com, ama her zaman kullanılmaz) Azure MFA için kaydedilemiyor. Microsoft hesabı olan kullanıcılar için rol atamak istiyorsanız, bunları kalıcı yönetici yapmak veya o rol için mfa'yı devre dışı bırakmak gerekir.
* MFA yüksek ayrıcalıklı rolleri için Azure AD için devre dışı bırakamazsınız ve Office365. Bu, bu rolleri dikkatli bir şekilde korunması için bir güvenlik özelliğidir:  
  
  * Faturalama Yöneticisi
  * Bulut Uygulaması Yöneticisi
  * Uyumluluk Yöneticisi
  * Koşullu Erişim Yöneticisi
  * CRM Hizmet Yöneticisi
  * Müşteri Kasası Erişimi Onaylayıcısı
  * Dizin Yazıcıları
  * Exchange Yöneticisi
  * Genel Yönetici
  * Information Protection Yöneticisi
  * Intune Hizmet Yöneticisi
  * Power BI Hizmet Yöneticisi
  * Ayrıcalıklı Rol Yöneticisi
  * Güvenlik Yöneticisi
  * SharePoint Hizmet Yöneticisi
  * Skype Kurumsal Yöneticisi
  * Kullanıcı Yöneticisi

Daha fazla bilgi için [multi factor authentication (MFA) ve PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Onay iste

Bir rolü etkinleştirmek için onay gerektir istiyorsanız, aşağıdaki adımları izleyin.

1. Ayarlama **onayı iste** geçin **etkin**. Onaylayanları seçin seçeneklerle bölmesini genişletir.

    ![Azure AD rolleri - Settings - onayı iste](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Varsa, **yok** tüm onaylayanlar belirtin, varsayılan onaylayanlar ayrıcalıklı rol yöneticileri olur. Ayrıcalıklı rol yöneticileri gereken onaylanacak **tüm** bu rol için etkinleştirme istekleri.

1. Onaylayanlar belirtmek için **onaylayanları seçin**.

    ![Azure AD rolleri - Settings - onayı iste](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Bir veya daha fazla onaylayanları seçin ve ardından **seçin**. Kullanıcılar veya gruplar seçebilirsiniz. En az 2 onaylayan önerilir. Kendini onaylama izin verilmez.

    Seçimlerinizi seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirledikten sonra tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD PIM Rolleri Ata](pim-how-to-add-role-to-user.md)
- [PIM'de Azure AD rolleri güvenlik uyarılarını yapılandırma](pim-how-to-configure-security-alerts.md)
