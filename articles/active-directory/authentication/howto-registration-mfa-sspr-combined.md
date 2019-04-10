---
title: Azure AD SSPR ve multi-Factor Authentication (Önizleme) - Azure Active Directory için birleşik kaydına Başlarken
description: Azure AD multi-Factor Authentication etkin birleştirilir ve Self Servis parola sıfırlama kaydı (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3baf2690ae07b87bb4d5dba30fcd20f62a1a4506
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280579"
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

## <a name="next-steps"></a>Sonraki adımlar

[Çok faktörlü kimlik doğrulaması ve SSPR için kullanılabilen yöntemler](concept-authentication-methods.md)

[Self servis parola sıfırlamasını yapılandırma](howto-sspr-deployment.md)

[Azure Multi-Factor Authentication’ı yapılandırma](howto-mfa-getstarted.md)

[Güvenlik bilgileri kayıt birleştirilmiş sorunlarını giderme](howto-registration-mfa-sspr-combined-troubleshoot.md)