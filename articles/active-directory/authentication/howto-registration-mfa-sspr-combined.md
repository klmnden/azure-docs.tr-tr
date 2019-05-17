---
title: Azure AD SSPR ve multi-Factor Authentication (Önizleme) - Azure Active Directory için birleşik kaydına Başlarken
description: Azure AD multi-Factor Authentication etkin birleştirilir ve Self Servis parola sıfırlama kaydı (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc4ff596cdafd348288187b0cd9b32f7b4c2d275
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65823385"
---
# <a name="enable-combined-security-information-registration-preview"></a>Birleştirilmiş etkinleştir güvenlik bilgileri kayıt (Önizleme)

Yeni deneyimi etkinleştirmeden önce makalesini gözden geçirin [güvenlik bilgileri kayıt (Önizleme) birleştirilmiş](concept-registration-mfa-sspr-combined.md) işlevleri ve bu özellik etkilerini anladığınızdan emin olmak için.

![Birleştirilmiş güvenlik bilgileri geliştirilmiş kayıt deneyimi](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Azure multi-Factor Authentication ve Azure Active Directory (Azure AD) Self Servis parola sıfırlama için birleşik güvenlik bilgileri kayıt bir Azure AD genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="enable-combined-registration"></a>Birleşik kaydı etkinleştirme

Birleşik kaydını etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalına genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.
2. Git **Azure Active Directory** > **kullanıcı ayarları** > **erişim paneli Önizleme özellikleri için ayarları yönetme**.
3. Altında **kullanıcılar Önizleme özellikleri kaydediliyor ve güvenlik bilgilerinizi yönetmek için - yenileme**, için etkinleştirmeyi seçerseniz bir **seçili** için kullanıcı ve grup **tüm** kullanıcılar.

   ![Tüm kullanıcılar için birleşik güvenlik bilgisi Önizleme deneyimini etkinleştirmek](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> Mart 2019 ' başlayarak, telefon araması seçenekleri çok faktörlü kimlik doğrulaması ve Azure AD ücretsiz/deneme kiracıları SSPR kullanıcıları için kullanılabilir olmayacaktır. SMS iletileri, bu değişiklikten etkilenmez. Telefon araması seçenekleri hala Ücretli Azure AD kiracılarıyla kullanıcılar için kullanılabilir.

> [!NOTE]
> Birleşik kaydını, kayıt veya telefon numarasını onaylamak kullanıcıları etkinleştirin veya bu yöntem çok faktörlü kimlik doğrulaması ve SSPR etkinleştirilip etkinleştirilmediğini yeni deneyim aracılığıyla mobil uygulama bunları çok faktörlü kimlik doğrulaması ve SSPR için kullanabilirsiniz ilkeleri. Bu deneyim devre dışı bırakırsanız, önceki SSPR kaydı için Git Kullanıcılar sayfasında `https:/aka.ms/ssprsetup` sayfa erişebilmeniz için önce çok faktörlü kimlik doğrulaması gerçekleştirmek için gerekli.

Internet Explorer'da siteyi bölgeye ataması Listesi'ni yapılandırdıysanız, aşağıdaki siteleri aynı bölgede olması gerekir:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Birleşik kayıt için koşullu erişim ilkeleri

Ne zaman ve kullanıcıların kaydolmak için Azure multi-Factor Authentication ve Self Servis parola sıfırlama artık koşullu erişim ilkesi kullanıcı eylemleri ile mümkündür nasıl güvenli hale getirme. Bu önizleme özelliğini etkinleştirdiniz kuruluşlar için kullanılabilir [kayıt Önizleme birleştirilmiş](../authentication/concept-registration-mfa-sspr-combined.md). Bu işlev, ik ekleme sırasında Azure multi-Factor Authentication ve güvenilir ağ konumu gibi merkezi bir konumdan SSPR kaydolmak için kullanıcıların istedikleri kuruluşlardaki etkinleştirilebilir. Koşullu erişim güvenilen konumları oluşturma hakkında daha fazla bilgi için bkz [konum koşulu Azure Active Directory koşullu erişim nedir?](../conditional-access/location-condition.md#named-locations)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Güvenilen bir konumdan kayıt gerektiren bir ilkeniz oluşturma

Güvenilen ağ işaretlenmiş bir konumdan bağlanıyorsanız sürece erişimi engeller ve birleşik kayıt deneyimi kullanarak kaydolmaya tüm seçili kullanıcılar, aşağıdaki ilke uygulanır.

![Güvenlik bilgileri kayıt denetlemek için bir CA ilkesi oluşturma](media/howto-registration-mfa-sspr-combined/conditional-access-register-security-info.png)

1. İçinde **Azure portalında**, Gözat **Azure Active Directory** > **koşullu erişim**
1. **Yeni ilke**'yi seçin
1. Adı, bu ilke için bir ad girin. Örneğin, **birleştirilmiş güvenlik bilgileri kayıt güvenilen ağlarda**
1. Altında **atamaları**, tıklayın **kullanıcılar ve gruplar**, kullanıcıları ve bu ilkenin uygulanmasını istediğiniz grupları seçin

   > [!WARNING]
   > Kullanıcılar için etkinleştirilmesi gerekir [kayıt Önizleme birleştirilmiş](../authentication/howto-registration-mfa-sspr-combined.md).

1. Altında **bulut uygulamaları veya Eylemler**seçin **kullanıcı eylemlerini**, kontrol **kaydetme güvenlik bilgilerini (Önizleme)**
1. Altında **koşullar** > **konumları**
   1. Yapılandırma **Evet**
   1. Dahil **herhangi bir yerde**
   1. Dışlama **tüm Güvenilen Konumlar**
   1. Tıklayın **Bitti** konumları dikey
   1. Tıklayın **Bitti** koşullar dikey
1. Altında **erişim denetimleri** > **verin**
   1. Tıklayın **erişimi engelle**
   1. Ardından **seçin**
1. Ayarlama **ilkesini etkinleştir** için **üzerinde**
1. Ardından **oluştur**

## <a name="next-steps"></a>Sonraki adımlar

[Çok faktörlü kimlik doğrulaması ve SSPR için kullanılabilen yöntemler](concept-authentication-methods.md)

[Self Servis parola sıfırlamayı yapılandırın](howto-sspr-deployment.md)

[Azure çok faktörlü kimlik doğrulamasını yapılandırma](howto-mfa-getstarted.md)

[Güvenlik bilgileri kayıt birleştirilmiş sorunlarını giderme](howto-registration-mfa-sspr-combined-troubleshoot.md)

[Konum koşulu Azure Active Directory koşullu erişim nedir?](../conditional-access/location-condition.md)