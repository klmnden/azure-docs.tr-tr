---
title: "Azure Active Directory B2B işbirliği sorunlarını giderme | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği ile ortak sorunları çözümler"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 2009cfc956a2703e268c9364996aa2d0fbd8f279
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B işbirliği sorunlarını giderme

Azure Active Directory (Azure AD) B2B işbirliği ile ortak sorunları için bazı çözümler aşağıda verilmiştir.


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Bir dış kullanıcı eklediğiniz, ancak bunların benim genel adres defteri veya kişi seçici görmez

Burada dış kullanıcılar listesinde doldurulmaz durumlarda nesne çoğaltmak için birkaç dakika sürebilir.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>B2B Konuk kullanıcı SharePoint Online/OneDrive kişiler seçicide göstermiyor 
 
SharePoint Online (SPO) Kişi Seçici varolan Konuk kullanıcılar için arama özelliğini eski davranışı eşleştirmek için varsayılan olarak kapalı'dır.

Bu özellik Kiracı ve site koleksiyonu düzeyinde 'ShowPeoplePickerSuggestionsForGuestUsers' ayarı kullanarak etkinleştirebilirsiniz. Tüm mevcut Konuk kullanıcılar dizinde arama üyelerin kümesini SPOTenant ve Set-SPOSite cmdlet'leri kullanarak özelliğini ayarlayabilirsiniz. Kiracı kapsamındaki değişiklikleri zaten sağlanan SPO site etkilemez.

## <a name="invitations-have-been-disabled-for-directory"></a>Dizin için davet devre dışı bırakıldı

Kullanıcıları davet iznine sahip değil bildirilir, kullanıcı hesabınızın kullanıcı ayarları altında dış kullanıcıları davet yetkisi olduğunu doğrulayın:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

Kısa süre önce bu ayarları değiştirilebilir veya konuk davet eden rolü atanmış bir kullanıcı, değişikliklerin etkili olması 15-60 dakikalık bir gecikmeyle olabilir.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>I davet kullanıcı kullanım sırasında bir hata alıyor

Sık karşılaşılan hatalar şunlardır:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Davetlilerin yönetici kendi Kiracı oluşturulan EmailVerified kullanıcılar izin verilmiyor

Kullanıcılar, kuruluşunuzun Azure Active Directory'yi kullanarak, ancak burada belirli kullanıcının hesap yok davet olduğunda (örneğin, kullanıcının Azure AD contoso.com yok). Contoso.com yönetici kullanıcılar oluşturulmasını engelleyerek yerinde bir ilke olabilir. Kullanıcı dış kullanıcıların izinleri olup olmadığını belirlemek için yönetici olmanız gerekir. Dış kullanıcının yönetici e-posta doğrulandı kullanıcıların kendi etki alanlarındaki gerekebilir (Bu bkz [makale](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) e-posta doğrulandı kullanıcıların üzerinde).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Dış kullanıcı zaten bir Federasyon etki alanında mevcut değil

Federasyon kimlik doğrulaması kullanıyorsanız ve kullanıcının Azure Active Directory'de zaten yoksa, kullanıcı davet edilemez.

Bu sorunu çözmek için dış kullanıcının yönetici kullanıcının hesabı Azure Active Directory'ye eşitlemeniz gerekir.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Nasıl mu\#', olmayan normal olarak geçerli bir karakter, Azure AD ile eşitleme?

"\#" Azure AD B2B işbirliği veya dış kullanıcılar için ayrılmış bir karakter UPN'ler içinde olduğundan davet edilen hesap user@contoso.com user_contoso.com# haleEXT@fabrikam.onmicrosoft.com. Bu nedenle, \# şirket içinden gelen UPN'ler de Azure portalında oturum açmak için izin verilmez. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Bir hata Dış kullanıcılar eşitlenmiş bir gruba eklerken alır

Dış kullanıcılar yalnızca "atanan" veya "Güvenlik" grupları ve ana kopyalı şirket içi gruplarına eklenebilir.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>My dış kullanıcı kullanmak için bir e-posta almadı.

Davet edilene aşağıdaki adresi izin verildiğinden emin olmak için kendi ISS ya da istenmeyen posta Filtresi ile denetlemeniz gerekir:Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Özel ileti bazen davet iletileri ile birlikte almaz, ı dikkat edin

Gizlilik yasalarına uymak için bizim API'leri özel iletileri e-posta davetinde içermez zaman:

- Davet eden davet Kiracı içinde bir e-posta adresi yok
- Bir uygulama hizmeti asıl daveti gönderdiğinde

Bu senaryo için önemliyse bizim API davet e-posta bastırmak ve tercih ettiğiniz bir e-posta mekanizma aracılığıyla gönderebilirsiniz. Bu şekilde de gizlilik yasalarına uyumlu gönderdiğiniz herhangi bir e-emin olmak için kuruluşunuzun yasal Danışmanı başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B işbirliği API ve özelleştirme](active-directory-b2b-api.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
