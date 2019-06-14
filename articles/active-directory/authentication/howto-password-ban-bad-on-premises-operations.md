---
title: Azure AD parola koruması işlemler ve raporlama - Azure Active Directory
description: Azure AD parola koruması dağıtım sonrası işlemleri ve Raporlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca85007bb016cc98d1be61ce08865945e699ad4a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60358195"
---
# <a name="azure-ad-password-protection-operational-procedures"></a>Azure AD parola koruması işletimsel yordamları

Tamamladıktan sonra [Azure AD parola koruması yüklenmesini](howto-password-ban-bad-on-premises-deploy.md) şirket içi, Azure portalında yapılandırılması gereken birkaç nokta vardır.

## <a name="configure-the-custom-banned-password-list"></a>Özel yasaklı parola listesi yapılandırma

Bu makaledeki yönergeleri [özel yasaklı parola listesi yapılandırma](howto-password-ban-bad-configure.md) yasaklı parola listesi kuruluşunuz için özelleştirmek adımlar.

## <a name="enable-password-protection"></a>Parola korumasını etkinleştir

1. Oturum [Azure portalında](https://portal.azure.com) ve **Azure Active Directory**, **kimlik doğrulama yöntemleri**, ardından **parola koruması**.
1. Ayarlama **etkinleştirme Windows Server Active Directory parola koruması** için **Evet**
1. Belirtildiği gibi [Dağıtım Kılavuzu](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy), başlangıçta ayarlamak için önerilen **modu** için **denetim**
   * Özelliğiyle memnun kaldıktan sonra geçebilirsiniz **modu** için **zorlanan**
1. **Kaydet**'e tıklayın.

![Azure portalında Azure AD parola koruması bileşenleri etkinleştirme](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Denetim modu

Denetim modu yazılım "what IF" modda çalıştırmak için bir yol olarak tasarlanmıştır. Her DC Aracısı gelen bir parola etkin ilkesine göre değerlendirir. Geçerli ilke Denetim modunda olacak şekilde yapılandırılmışsa, "yanlış" parolalar olay günlüğü iletilerine neden ancak kabul edilir. Denetim ve zorla modu arasındaki tek fark budur; diğer tüm işlemler aynı çalıştırın.

> [!NOTE]
> Microsoft, ilk dağıtım ve test her zaman denetim modunda başlar, önerir. Olay günlüğündeki olaylar, ardından zorla modu etkinleştirildikten sonra herhangi bir mevcut işlem süreçlerini dağıtılmış olup olmadığını tahmin denemek için izlenmelidir.

## <a name="enforce-mode"></a>Zorlama modunda

Zorlama modu son yapılandırma tasarlanmıştır. Yukarıdaki denetim modunda olduğu gibi her DC Aracısı gelen parolaları etkin ilkesine göre değerlendirir. Zorlama modu etkin ancak bir parola ilkesine göre güvenli olarak kabul edilir reddedilir.

Bir parola zorla modunda Azure AD parola koruması DC aracısı tarafından reddedildiğinde bir son kullanıcı tarafından görülen görünür etkisini bunların parolalarını geleneksel şirket içi parola karmaşıklık zorlama tarafından reddedildi, gördükleri için aynıdır. Örneğin, bir kullanıcı Windows logon\change parola ekranında aşağıdaki geleneksel hata iletisini görebilirsiniz:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Bu ileti olası sonuçlardan yalnızca bir örnektir. Belirli bir hata iletisi, gerçek yazılım veya güvenli olmayan bir parola ayarlamayı deniyor senaryoya bağlı olarak değişebilir.

Etkilenen son kullanıcılar, güvenli parolalar daha seçebilir ve yeni gereksinimleri anlamak için BT personeli ile çalışmak gerekebilir.

## <a name="enable-mode"></a>Modunu etkinleştir

Bu ayar varsayılan (Evet) etkin durumda normalde bırakılmalıdır. Burada tüm parolalar kabul edildiği olarak gerçekleştirilmeye moduna gitmek tüm dağıtılan Azure AD parola koruması DC aracıları (Hayır) devre dışı. Bu ayarın yapılandırılması neden olacak-olan ve hiçbir doğrulama etkinliklerini (örneğin, denetim olayları bile vermemektedir yürütülen olacaktır yayılan).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola koruması için izleme](howto-password-ban-bad-on-premises-monitor.md)
