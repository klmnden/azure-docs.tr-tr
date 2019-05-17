---
title: B2B işbirliği kullanıcıları - Azure Active Directory için koşullu erişim | Microsoft Docs
description: Azure Active Directory B2B işbirliği, Kurumsal uygulamalarınıza seçmeli erişim için çok faktörlü kimlik doğrulaması (MFA) destekler.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f3bfe067b7a927f800f88958ee2ffca09711c10
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65812803"
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için koşullu erişim

## <a name="multi-factor-authentication-for-b2b-users"></a>B2B kullanıcıları için multi-Factor authentication
Azure AD B2B işbirliği, kuruluşların, B2B kullanıcıları için çok faktörlü kimlik doğrulaması (MFA) İlkeleri zorunlu kılabilir. Bu ilkeler Kiracı, uygulama veya tek tek kullanıcı düzeyinde, tam zamanlı çalışanlar ve kuruluş için etkinleştirilir aynı şekilde zorunlu tutulabilir. Kaynak kuruluşta MFA ilkeleri uygulanır.

Örnek:
1. Bir şirket yöneticisi veya bilgi çalışanı Şirket B kullanıcıdan uygulamaya davet *Foo* şirkette A.
2. Uygulama *Foo* şirketteki A erişimi MFA gerektirecek şekilde yapılandırılmış.
3. Çalıştığında kullanıcıdan Şirket B uygulamaya erişmek *Foo* şirket bir kiracı, kullanıcıdan MFA testini tamamlamanız istenir.
4. Kullanıcı kendi MFA şirketi ile ayarlayabilirsiniz ve bunların MFA seçeneğini seçer.
5. Bu senaryo için herhangi bir kimliğe çalışır (Azure AD veya MSA, örneğin, kullanıcılar şirket b sosyal Kimliğini kullanarak kimlik doğrulaması)
6. Şirketi, mfa'yı destekleyen yeterli Azure AD Premium lisansına sahip olması gerekir. A. şirketten bu lisans kullanıcının şirketten B kullanır

İş ortağı kuruluşun MFA özelliklerine sahip olsa bile davet eden Kiracı her zaman MFA için kullanıcılar iş ortağı kuruluştan sorumludur.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için mfa'yı ayarlama
B2B işbirliği kullanıcıları için mfa'yı ayarlamak için ne kadar kolay olduğunu öğrenmek için bkz: aşağıdaki videoda nasıl:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>B2B kullanıcıları MFA deneyimi için kullanım sunar.
Kullanım görmek için aşağıdaki animasyonu kullanıma alın deneyimi:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>Sıfırlama B2B işbirliği kullanıcıları için MFA
Şu anda yönetici B2B işbirliği kullanıcıları için kavram yukarı yeniden aşağıdaki PowerShell cmdlet'lerini kullanarak yalnızca gerektirebilir:

1. Azure AD'ye bağlan

   ```
   $cred = Get-Credential
   Connect-MsolService -Credential $cred
   ```
2. Tüm kullanıcılar yöntemleri'kurmak kavram alın

   ```
   Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
   ```
   Örnek aşağıda verilmiştir:

   ```
   Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
   ```

3. Kavram yukarı yöntemlerini yeniden ayarlama B2B işbirliği gerektirmek belirli bir kullanıcı için mfa'yı yöntemi sıfırlayın. Örnek:

   ```
   Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
   ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>MFA kaynak kiralama neden gerçekleştiririz?

Geçerli sürümde, MFA öngörülebilirlik nedenlerini için kaynak kiralama her zaman bulunduğu. Örneğin, Contoso kullanıcısı (Sally) için Fabrikam davet edilmesi ve Fabrikam B2B kullanıcıları için mfa'yı etkinleştirilmiş varsayalım.

Contoso App1 ancak değil App2 etkin MFA ilkesi varsa, Contoso MFA talebi belirteci baktığımızda ardından biz aşağıdaki sorunları görebilirsiniz:

* 1. günü: Kullanıcı MFA Contoso ağında sahiptir ve App1'i, ardından hiçbir ek MFA erişme istemi Fabrikam'da gösterilir.

* 2. gün: Kullanıcı, Fabrikam erişirken şimdi contoso'da, uygulama 2 eriştiğini, MFA için orada kaydetmelisiniz.

Bu işlem, kafa karıştırıcı olabilir ve oturum açma tamamlamaları drop neden olabilir.

Contoso MFA özelliği olsa bile, ayrıca, her zaman çalışması Fabrikam Contoso MFA ilkesini güven değildir.

Son olarak, kaynak Kiracı MFA Msa'lar ve sosyal kimlikleri ve mfa'yı ayarlama izniniz yok, iş ortağı kuruluşlar için çalışır.

Bu nedenle, mfa B2B kullanıcıları için mfa'yı davet eden Kiracı her zaman gerekli önerilir. Bu gereksinim bazı durumlarda, çift mfa'yı neden olabilir, ancak davet eden Kiracı erişimi olduğunda, son kullanıcıların deneyimini tahmin edilebilir: Sally MFA için davet eden kiracısı ile kaydetmeniz gerekir.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>B2B kullanıcıları için cihaz, konum ve risk tabanlı koşullu erişim

Contoso Kurumsal verileri cihaz tabanlı koşullu erişim ilkelerini etkinleştirdiğinde, Contoso tarafından yönetilen ve Contoso cihaz ilkeleriyle uyumlu olmayan cihazlar üzerinden erişimi engelledi.

B2B kullanıcı cihazına Contoso tarafından yönetilmiyorsa, bu ilkeleri zorunlu hangi bağlamda iş ortağı kuruluşlardan B2B kullanıcıları erişimi engellenir. Ancak, Contoso bunları cihaz tabanlı koşullu erişim ilkesinden hariç tutmak için belirli bir iş ortağı kullanıcıları içeren bir dışlama listesi oluşturabilirsiniz.

#### <a name="location-based-conditional-access-for-b2b"></a>Konum tabanlı koşullu erişim için B2B

Davet eden kuruluştan iş ortağı kuruluşları tanımlayan bir güvenilen IP adresi aralığı oluşturma erişebiliyorsa B2B kullanıcıları için konum tabanlı koşullu erişim ilkelerini zorunlu tutulabilir.

#### <a name="risk-based-conditional-access-for-b2b"></a>B2B için risk tabanlı koşullu erişim

Şu anda, risk değerlendirmesine B2B kullanıcının ev kuruluştan gerçekleştirildiği için oturum açma risk tabanlı ilkeler B2B kullanıcıları için uygulanamaz.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure AD B2B işbirliği lisanslaması](licensing-guidance.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)
