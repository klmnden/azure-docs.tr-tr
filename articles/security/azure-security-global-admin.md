---
title: Tüm Azure yöneticileri için MFA'yı etkinleştirin
description: Genel yönetici etkinleştirmek için yönergeler
ms.service: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: a247f5afbca491dc9c31c74453860961188411c9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Abonelik yöneticileri için çok faktörlü kimlik doğrulaması (MFA) zorunlu

Genel Yönetici hesabınızla dahil olmak üzere yöneticilerinizle oluşturduğunuzda, çok güçlü kimlik doğrulama yöntemleri kullanmanızı gereklidir.

Özel yönetici rolleri atama tarafından günlük yönetim gerçekleştirebilirsiniz — Exchange Yöneticisi veya parola Yöneticisi gibi — gerektiğinde BT personeli, kullanıcı hesaplarına.
Ayrıca, etkinleştirme [Azure çok faktörlü kimlik doğrulama (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) , Yöneticiler için kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı ekler. Ayrıca, Azure MFA yardımcı BT güvenliği aşılmış bir kimlik bilgisi kuruluşunuzun veri erişimi olmasını olasılığını azaltır.

Örneğin: kullanıcılarınız için Azure MFA zorlamak ve bir telefon araması veya kısa mesaj doğrulama kullanacak şekilde yapılandırın. Kullanıcının kimlik bilgilerinin güvenliği aşıldığında saldırgan kendisinin kullanıcının telefonuna erişemeyecektir beri herhangi bir kaynağa erişim mümkün olmayacaktır. Ek kimlik koruma katmanları eklemeyin kuruluşlar veri güvenliğinin aşılmasına neden kimlik bilgisi hırsızlığı saldırısına daha açıktır.

Tüm kimlik doğrulama denetimi içi tutmak istediğiniz kuruluşlar için bir alternatif kullanmaktır [Azure çok faktörlü kimlik doğrulama sunucusu](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), "MFA şirket içi" olarak da bilinir. Bu yöntemi kullanarak, çok faktörlü kimlik doğrulaması, MFA sunucusu şirket içi korurken zorlayabilir devam edersiniz.

Kimin kuruluşunuzda denetlemek için aşağıdaki Microsoft Azure AD V2 PowerShell komutunu kullanarak doğrulayabilirsiniz yönetim ayrıcalıklarına sahiptir:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>MFA etkinleştirme

Gözden geçirme nasıl [MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-whats-next) devam etmeden önce çalışır.

Kullanıcıların Azure Multi-Factor Authentication içeren lisansları olduğu sürece, Azure MFA’yı etkinleştirmek için hiçbir işlem yapmanız gerekmez. Her kullanıcı için iki aşamalı doğrulama istemeye başlayabilirsiniz. Azure MFA'yı etkinleştiren lisanslar şunlardır:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Kullanıcılar için iki aşamalı doğrulamayı açma

Listelenen yordamlardan birini kullanın [iki aşamalı doğrulama gerektirecek şekilde nasıl](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) Azure MFA kullanmaya başlamak için bir kullanıcı veya grup için. Tüm oturum açma işlemleri için iki aşamalı doğrulama uygulamayı seçebilir ya da yalnızca önem verdiğiniz durumlarda iki aşamalı kimlik doğrulaması uygulamak için koşullu erişim ilkeleri oluşturabilirsiniz.

