---
title: Azure Active Directory B2B işbirliği sorunlarını giderme | Microsoft Docs
description: Azure Active Directory B2B işbirliği ile karşılaşılan sorunlara yönelik çözümler
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 29ebf91801478ab3d4790fafa0dfb099201e1595
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431036"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B işbirliği sorunlarını giderme

Azure Active Directory (Azure AD) B2B işbirliği ile karşılaşılan sorunlara yönelik bazı çözümler aşağıda verilmiştir.


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Bir dış kullanıcının ekledik ancak bunların benim Genel Adres Defteri'nde veya kişi seçici görmez

Burada dış kullanıcılar listede doldurulmaz durumlarda, nesne çoğaltmak için birkaç dakika sürebilir.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>B2B Konuk kullanıcı SharePoint Online/OneDrive Kişi Seçici gösterilmiyor 
 
SharePoint Online (SPO) Kişi Seçici var olan konuk kullanıcılar için arama özelliğini eski davranışı eşleştirmek için varsayılan olarak kapalı'dır.

'ShowPeoplePickerSuggestionsForGuestUsers' ayarı Kiracı ve site koleksiyonu düzeyinde kullanarak bu özelliği etkinleştirebilirsiniz. Üyelerinin dizindeki tüm mevcut Konuk kullanıcıları aramak Set-SPOTenant ve Set-SPOSite cmdlet'lerini kullanarak özelliğini ayarlayabilirsiniz. Değişiklikler Kiracı kapsamında zaten sağlanan SPO site etkilemez.

## <a name="invitations-have-been-disabled-for-directory"></a>Davet dizini devre dışı bırakıldı

Kullanıcıları davet için izinlere sahip değilsiniz bildirilir, kullanıcı hesabınızın kullanıcı ayarları altında dış kullanıcıları davet yetkisi olduğunu doğrulayın:

![](media/troubleshoot/external-user-settings.png)

Size kısa süre önce bu ayarları değiştiren veya konuk davet edici rolüne atanmış bir kullanıcıya, değişiklikler etkili olmadan önce 15-60 dakika gecikme olabilir.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Davet ettim ve kullanıcı alma sırasında bir hata mesajı

Sık karşılaşılan hatalar şunlardır:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Davetli'nın yönetici kendilerine ait kiracıda oluşturulan EmailVerified kullanıcıların izin verilmeyen

Kullanıcılar, kuruluşunuzun Azure Active Directory kullanıyor, ancak burada belirli bir kullanıcının hesabı yok davet ettiğinizde (örneğin, kullanıcının Azure AD contoso.com yok). Contoso.com yönetici, kullanıcıların oluşturulmasını önleyen bir yerde bir ilkesi olabilir. Kullanıcı, dış kullanıcılar izin verilmediğini belirlemek için yönetici ile iade etmeniz gerekir. Dış kullanıcının yönetici e-posta ile doğrulanmış kullanıcıların kendi etki alanlarındaki gerekebilir (bkz. Bu [makale](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) e-posta doğrulanmış kullanıcıların üzerinde).

![](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Dış kullanıcı zaten bir Federasyon etki alanında yok.

Kullanıcı zaten Azure Active Directory'de mevcut değil ve Federasyon kimlik doğrulaması kullanıyorsanız, kullanıcı davet edilemiyor.

Bu sorunu çözmek için dış kullanıcının yönetici kullanıcının hesabı Azure Active Directory'ye eşitlemeniz gerekir.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Nasıl mu\#', hangi değil genellikle geçerli bir karakter, Azure AD ile eşitleme?

"\#" UPN, Azure AD B2B işbirliği veya dış kullanıcılar için ayrılmış bir karakter olduğundan davet edilen hesabı user@contoso.com user_contoso.com#EXT# olur@fabrikam.onmicrosoft.com. Bu nedenle, \# şirket içinden gelen UPN, Azure portalında oturum açmak için izin verilmez. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Dış kullanıcılar eşitlenmiş bir gruba eklerken bir hata alıyorum

Dış kullanıcılar "atandı" veya "Güvenlik" grupları ve ana kopyalı şirket içinde gruplarına eklenebilir.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>My dış kullanıcı kullanmak için e-posta almadı.

Davetli aşağıdaki adresi izin verildiğinden emin olmak için ISS ya da istenmeyen posta Filtresi ile denetlemeniz gerekir: Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Özel ileti bazen davet iletileri ile birlikte almaz olduğunu fark ederim

Gizlilik yasalarıyla uyum sağlamak için Apı'lerimizi özel iletiler posta içermez olduğunda:

- Davet eden davet eden kiracının bir e-posta adresi yok
- Appservice sorumlusu davet gönderdiğinde

Bu senaryo sizin için önemli ise, API davet e-postamıza gösterme ve tercih ettiğiniz e-posta mekanizması gönderin. Bu şekilde da gizlilik yasalarına uyumlu gönderdiğiniz herhangi bir e-emin olmak için kuruluşunuzun yasal Konseyi başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği için destek alma](get-support.md)