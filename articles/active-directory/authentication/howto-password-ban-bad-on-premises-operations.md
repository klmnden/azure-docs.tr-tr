---
title: Azure AD parola koruması Önizleme işlemleri ve Raporlama
description: Azure AD parola koruması Önizleme dağıtım sonrası işlemleri ve Raporlama
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 14aa52b6d424423f4863efa63f3e2e66b84dac70
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39164137"
---
# <a name="preview-azure-ad-password-protection-post-deployment"></a>Önizleme: Azure AD parola koruması dağıtım sonrası

|     |
| --- |
| Azure AD parola koruması ve özel yasaklı parola listesi Azure Active Directory genel Önizleme özellikleri şunlardır. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Tamamladıktan sonra [Azure AD parola koruması yüklenmesini](howto-password-ban-bad-on-premises.md) şirket içi, Azure portalında yapılandırılması gereken birkaç nokta vardır.

## <a name="configure-the-custom-banned-password-list"></a>Özel yasaklı parola listesi yapılandırma

Bu makaledeki yönergeleri [özel yasaklı parola listesi yapılandırma](howto-password-ban-bad.md) yasaklı parola listesi kuruluşunuz için özelleştirmek adımlar.

## <a name="enable-password-protection"></a>Parola korumasını etkinleştir

1. Oturum [Azure portalında](https://portal.azure.com) ve **Azure Active Directory**, **kimlik doğrulama yöntemleri**, ardından **parola koruması (Önizleme)**.
1. Ayarlama **etkinleştirme Windows Server Active Directory parola koruması** için **Evet**
1. Belirtildiği gibi [Dağıtım Kılavuzu](howto-password-ban-bad-on-premises.md#deployment-strategy), başlangıçta ayarlamak için önerilen **modu** için **denetim**
   * Özelliğiyle memnun kaldıktan sonra geçebilirsiniz **modu** için **zorlanan**
1. **Kaydet**’e tıklayın

![Azure portalında Azure AD parola koruması bileşenleri etkinleştirme](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Denetim modu

Denetim modu yazılım "what IF" modda çalıştırmak için bir yol olarak tasarlanmıştır. Her DC Aracısı gelen bir parola etkin ilkesine göre değerlendirir. Geçerli ilke Denetim modunda olacak şekilde yapılandırılmışsa, "yanlış" parolalar olay günlüğü iletilerine neden ancak kabul edilir. Denetim ve zorla modu arasındaki tek fark budur; diğer tüm işlemler aynı çalıştırın.

> [!NOTE]
> Microsoft, ilk dağıtım ve test her zaman denetim modunda başlar, önerir. Olay günlüğündeki olaylar, ardından zorla modu etkinleştirildikten sonra herhangi bir mevcut işlem süreçlerini dağıtılmış olup olmadığını tahmin denemek için izlenmelidir.

## <a name="enforce-mode"></a>Zorlama modunda

Zorlama modu son yapılandırma tasarlanmıştır. Yukarıdaki denetim modunda olduğu gibi her DC Aracısı gelen parolaları etkin ilkesine göre değerlendirir. Zorlama modu etkin ancak bir parola ilkesine göre güvenli olarak kabul edilir reddedilir.

Bir parola zorla modunda Azure AD parola koruması aracı DC tarafından reddedildiğinde bir son kullanıcı tarafından görülen görünür etkisini bunların parolalarını geleneksel şirket içi parola karmaşıklık zorlama tarafından reddedildi, gördükleri için aynıdır. Örneğin, bir kullanıcı logon\change parola ekranında aşağıdaki geleneksel hata iletisini görebilirsiniz:

"Parola güncelleştirilemedi. Yeni parola için sağlanan değer uzunluğu, karmaşıklığı ya da etki alanının geçmişi gereksinimlerini karşılamıyor."

Bu ileti olası sonuçlardan yalnızca bir örnektir. Belirli bir hata iletisi, gerçek yazılım veya güvenli olmayan bir parola ayarlamayı deniyor senaryoya bağlı olarak değişebilir.

Etkilenen son kullanıcılar, güvenli parolalar daha seçebilir ve yeni gereksinimleri anlamak için BT personeli ile çalışmak gerekebilir.

## <a name="usage-reporting"></a>Kullanım raporlama

`Get-AzureADPasswordProtectionSummaryReport` Cmdlet'i, etkinliğin Özet görünümünü oluşturmak için kullanılabilir. Bu cmdlet'in bir örnek çıktısı aşağıdaki gibidir:

```
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

Cmdlet'in raporlama kapsamı kullanarak etkilenebilir orman, - etki alanı veya – DomainController parametre. Bir parametre belirtmeden gelir – orman.

> [!NOTE]
> Bu cmdlet her DC aracı hizmetinin yönetici olay günlüğünü uzaktan sorgulayarak çalışır. Olay günlükleri, olaylar çok sayıda içeriyorsa, cmdlet tamamlanması uzun zaman alabilir. Ayrıca, toplu ağ sorguları büyük veri kümeleri, etki alanı denetleyicisi performansını etkileyebilir. Bu nedenle, bu cmdlet üretim ortamlarında dikkatli kullanılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Sorun giderme ve Azure AD parola koruması için günlük bilgilerini](howto-password-ban-bad-on-premises-troubleshoot.md)
