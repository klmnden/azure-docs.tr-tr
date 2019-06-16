---
title: Tüm Azure yöneticileri için mfa'yı etkinleştirme
description: Genel yönetici etkinleştirmek için yönergeler
ms.service: security
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: 7d40b8f0ca05000a51e70d7a124e9cb143aa2dcf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67127246"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Abonelik yöneticileri için multi-Factor authentication (MFA) zorunlu

Genel yönetici hesabınız dahil olmak üzere yöneticilerinize oluşturduğunuzda, çok güçlü kimlik doğrulama yöntemleri kullanmanızı gereklidir.

Günlük yönetim özel yönetici rolleri atayarak gerçekleştirebileceğiniz — Exchange yönetici veya yönetici parolası gibi — kullanıcı hesaplarını gerektiği gibi BT personeli için.
Ayrıca, etkinleştirme [Azure multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) kullanıcı oturum açma ve işlemler için Yöneticiler için ikinci bir güvenlik katmanı ekler. Azure mfa'yı da yardımcı olan BT güvenliği aşılmış bir kimlik bilgisi kuruluş verilerine erişimi olmasını olasılığını azaltır.

Örneğin: Kullanıcılarınız için Azure MFA zorlamak ve telefon görüşmesi veya SMS mesajı doğrulaması kullanacak şekilde yapılandırın. Saldırgan, kullanıcının kimlik bilgilerinin güvenliğinin aşılması durumunda kullanıcının telefonuna erişimi sahip herhangi bir kaynağa erişmek mümkün olmayacaktır. Ek kimlik koruma katmanları eklemeyin kuruluşlar, veri güvenliğinin aşılmasına yol açabilir kimlik bilgisi hırsızlığı saldırısına için daha açıktır.

Bir alternatif için tüm denetim şirket içi kimlik doğrulaması tutmak isteyen kuruluşların kullanmaktır [Azure multi-Factor Authentication sunucusu](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), "Şirket içi MFA" olarak da bilinir. Bu yöntemi kullanarak, multi-Factor authentication sunucusu şirket içi MFA tutarken uygulayabilmektir devam edersiniz.

Kimin kuruluşunuzda denetlemek için aşağıdaki Microsoft Azure AD V2 PowerShell komutunu kullanarak doğrulayabilirsiniz yönetim ayrıcalıklarına sahiptir:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>Mfa'yı etkinleştirme

Gözden geçirme nasıl [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) devam etmeden önce çalışır.

Kullanıcıların Azure Multi-Factor Authentication içeren lisansları olduğu sürece, Azure MFA’yı etkinleştirmek için hiçbir işlem yapmanız gerekmez. Her kullanıcı için iki aşamalı doğrulama istemeye başlayabilirsiniz. Azure MFA'yı etkinleştiren lisanslar şunlardır:

- Azure Multi-Factor Authentication
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Kullanıcılar için iki aşamalı doğrulamayı açma

Bölümünde listelenen yordamlardan birini kullanın [iki aşamalı doğrulama gerektirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) Azure mfa'yı kullanmaya başlamak için bir kullanıcı veya grup için. Tüm oturum açma işlemleri için iki aşamalı doğrulamayı zorunlu tutmayı seçebilirsiniz veya yalnızca önem verdiğiniz durumlarda iki aşamalı doğrulama gerektirmek için koşullu erişim ilkeleri oluşturabilirsiniz.

