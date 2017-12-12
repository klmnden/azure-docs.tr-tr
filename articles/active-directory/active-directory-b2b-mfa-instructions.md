---
title: "Azure Active Directory B2B işbirliği kullanıcılar için koşullu erişim | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği, şirket uygulamalarınıza seçmeli erişim için çok faktörlü kimlik doğrulaması (MFA) destekler."
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 09/11/2017
ms.author: sasubram
ms.openlocfilehash: 2f2cfc351d372d665aac054d52d6e1520e1ffe48
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcılar için koşullu erişim

## <a name="multi-factor-authentication-for-b2b-users"></a>B2B kullanıcılar için çok faktörlü kimlik doğrulaması
Azure AD B2B işbirliği ile kuruluşlar B2B kullanıcılar için çok faktörlü kimlik doğrulaması (MFA) ilkeleri uygulayabilirsiniz. Bu ilkeler Kiracı, uygulama veya bireysel kullanıcı düzeyinde tam zamanlı çalışanlar ve kuruluşun üyeleri için etkinleştirilen aynı şekilde zorunlu tutulabilir. Kaynak kuruluşta MFA ilkeleri uygulanır.

Örnek:
1. Yönetici veya bilgi çalışanı şirketindeki başvurulmasını Şirket B kullanıcıdan uygulamaya *Foo* A. şirketteki
2. Uygulama *Foo* şirkette A erişimi MFA gerektirecek şekilde yapılandırılmış.
3. Şirket B kullanıcıdan uygulamaya erişmeye çalıştığında *Foo* şirket bir kiracı, bunlar bir MFA testini tamamlamanız istenir.
4. Kullanıcı kendi MFA Şirket A ile ayarlayabilirsiniz ve bunların MFA seçeneğini seçer.
5. Bu senaryo için herhangi bir kimlik çalışır (Azure AD veya örneğin, kullanıcıların şirket b sosyal kimliği kullanarak kimlik doğrulaması MSA)
6. Şirket A MFA desteği yeterli Azure AD Premium lisansı olması gerekir. Bu lisans A. şirketten Şirket B kullanıcıdan kullanır

İş ortağı kuruluşun MFA yetenekleri olsa bile davet kiralama her zaman MFA için iş ortağı kuruluştan kullanıcılar sorumludur.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcılar için MFA'yı ayarlama
B2B işbirliği kullanıcılar için MFA'yı ayarlama ne kadar kolay olduğunu öğrenmek için bkz aşağıdaki videoda nasıl:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>Kullanım B2B kullanıcılar için MFA deneyimi sunar
Kullanım görmek için aşağıdaki animasyonu kullanıma alın deneyimi:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>MFA B2B işbirliği kullanıcılar için Sıfırla
Şu anda, yöneticinin B2B işbirliği kanıt kullanıcılara yukarı yeniden aşağıdaki PowerShell cmdlet'lerini kullanarak yalnızca gerektirebilir:

1. Azure AD'ye Bağlanma

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Tüm kullanıcılara sağlama yöntemleri ile Al

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Örnek aşağıda verilmiştir:

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Güçlü yöntemlerini yeniden ayarlama B2B işbirliği kullanıcının gerektiren belirli bir kullanıcı için MFA yöntemi sıfırlayın. Örnek:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>Neden şu kaynak kiralama MFA gerçekleştiriyorsunuz?

Geçerli sürümde MFA kaynak Kiracı öngörülebilirlik nedenleri için her zaman kullanılıyor. Örneğin, Contoso kullanıcı (değiştirmemesi) Fabrikam için davet ettiğiniz ve Fabrikam B2B kullanıcılar için MFA etkinleştirilmiş varsayalım.

Contoso etkin App1 ancak değil App2 için MFA ilkesi varsa, biz Contoso MFA talep belirteci bakarsanız sonra biz aşağıdaki sorunları görebilirsiniz:

* 1. güne: Bir kullanıcının Contoso ağında MFA varsa ve App1 sonra hiçbir ek MFA erişme istemi Fabrikam ' gösterilir.

* 2 gün: Kullanıcı, Fabrikam erişirken artık Contoso, uygulama 2 eriştiğini, var. MFA için kaydolması gerekir.

Bu işlem kafa karıştırıcı olabilir ve oturum açma tamamlamalar içinde bırakmasına neden olabilir.

Contoso MFA yeteneği olsa bile, ayrıca, her zaman çalışması Fabrikam güven Contoso MFA İlkesi değildir.

Son olarak, kaynak Kiracı MFA ayarladığınız MFA olmayan iş ortağı kuruluşu ve Msa'lar ve sosyal kimlikleri için çalışır.

Bu nedenle, mfa B2B kullanıcılar için her zaman davet kiracısında MFA gerektirecek şekilde önerilir. Bu gereksinim bazı durumlarda çift MFA neden olabilir, ancak davet Kiracı erişimi olduğunda, son kullanıcıların deneyimini tahmin edilebilir: değiştirmemesi MFA için davet Kiracı ile kaydetmeniz gerekir.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>B2B kullanıcılar için cihaz, konum ve risk tabanlı koşullu erişim

Contoso şirket verilerini için cihaz temelli koşullu erişim ilkeleri etkinleştirdiğinde, Contoso tarafından yönetilen ve Contoso cihaz ilkeleriyle uyumlu olmayan cihazlar üzerinden erişimi engelledi.

B2B kullanıcının cihaz tarafından Contoso yönetilen değil, bu ilkeleri zorunlu herhangi bir bağlamı içinde iş ortağı kuruluşlardan B2B kullanıcıların erişim engellendi. Ancak, Contoso cihaz temelli koşullu erişim ilkesinden hariç tutulacak belirli iş ortağı kullanıcıları içeren bir dışlama listesi oluşturabilirsiniz.

#### <a name="location-based-conditional-access-for-b2b"></a>Konum temelli B2B için koşullu erişim

Davet kuruluşun kendi ortak kuruluşlar tanımlayan güvenilir bir IP adresi aralığı oluşturmak mümkün ise B2B kullanıcılar için konum temelli koşullu erişim ilkeleri uygulanabilir.

#### <a name="risk-based-conditional-access-for-b2b"></a>Risk tabanlı B2B için koşullu erişim

Şu anda, risk değerlendirmesine B2B kullanıcının ev kuruluştan yapıldığından risk tabanlı oturum açma ilkeleri B2B kullanıcılara uygulanamaz.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B işbirliği API ve özelleştirme](active-directory-b2b-api.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure Active Directory'de uygulama yönetimi için makale dizini](active-directory-apps-index.md)
