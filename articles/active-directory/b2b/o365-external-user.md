---
title: Office 365 dış paylaşım ve B2B işbirliği - Azure Active Directory | Microsoft Docs
description: O365 ve Azure Active Directory B2B işbirliğini kullanarak dış iş ortakları ile kaynakların paylaşımı açıklanır.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/24/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 388d42cd41d34a8aebed41dafc48e42006a78457
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60355211"
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Office 365 dış paylaşım ve Azure Active Directory B2B işbirliği

Dış paylaşım (OneDrive, SharePoint Online, birleşik gruplar vb.) Office 365 ve Azure Active Directory (Azure AD) B2B işbirliği teknik olarak aynı şeydir. Tüm dış (OneDrive/SharePoint Online dışında) paylaşımı, Konuklar, Office 365 grupları dahil olmak üzere, önceden paylaşmak için Azure AD B2B işbirliği davet API'leri kullanır.

## <a name="how-does-azure-ad-b2b-differ-from-external-sharing-in-sharepoint-online"></a>Azure AD B2B dış SharePoint Online'da paylaşmasını farkı nedir?

OneDrive/SharePoint Online ayrı davet manager vardır. Dış OneDrive/SharePoint Online ile paylaşma desteği, Azure AD desteğini geliştirilen önce başladı. Zaman içerisinde, OneDrive/SharePoint Online dış paylaşım çeşitli özellikler tahakkuk ettirilen ve birçok ürün kullanan kullanıcılar milyonlarca deseni paylaşımı yerleşik. Ancak, OneDrive/SharePoint Online dış paylaşım nasıl çalıştığını ve Azure AD B2B işbirliği nasıl çalıştığını arasındaki bazı farklar vardır. OneDrive/SharePoint Online harici olarak paylaşma hakkında daha fazla bilgi [dış paylaşımına genel bakış](https://docs.microsoft.com/sharepoint/external-sharing-overview). İşlem, genellikle aşağıdaki şekillerde Azure AD B2B ' farklıdır:

- Kullanıcı davetlerini yararlandınız sonra OneDrive/SharePoint Online dizine kullanıcılar ekler. Bu nedenle, kullanım önce kullanıcının Azure AD portalında görmüyorum. Başka bir siteye sırada bir kullanıcı davet yeni bir davet oluşturulur. Azure AD B2B işbirliği kullandığınız zaman, her yerde göster ancak kullanıcılar hemen davetinde eklenir.

- Azure AD B2B işbirliği deneyimi farklı kullanım deneyimi OneDrive/SharePoint Online'daki arar. Bir kullanıcı davet redeems sonra deneyimleri benzer.

- Azure AD B2B işbirliği davet edilen kullanıcıların OneDrive/SharePoint Online'dan iletişim kutuları paylaşımı çekilebilir. Davet OneDrive/SharePoint Online kullanıcıları Ayrıca bunlar davetlerini pass'inizi sonra Azure AD'de gösterilir.

- Lisanslama gereksinimleri farklılık gösterir. Her Ücretli Azure AD lisansı için en fazla 5 Konuk kullanıcı, ücretli erişim sağlayabilirsiniz. Azure AD özellikleri. Lisanslama hakkında daha fazla bilgi için bkz: [Azure AD B2B lisansı](https://docs.microsoft.com/azure/active-directory/b2b/licensing-guidance) ve ["Bir dış kullanıcının SharePoint Online'da paylaşmaya genel bakış dış nedir?"](https://docs.microsoft.com/sharepoint/external-sharing-overview#what-is-an-external-user).

Dış OneDrive/SharePoint Online ile Azure AD B2B işbirliği paylaşımını yönetmek için OneDrive/SharePoint Online dış ayarına paylaşımı ayarlama **izin vermek kuruluşunuzun zaten yalnızca dış kullanıcılarla paylaşma Dizin**. Kullanıcılar, harici olarak paylaşılan sitelerine gidin ve yönetici eklemiştir dış ortak çalışanlar ' ı seçin. Yönetici, dış ortak çalışanlar B2B işbirliği davet API'leri aracılığıyla ekleyebilirsiniz.


![OneDrive/SharePoint Online paylaşımı ayarı dış](media/o365-external-user/odsp-sharing-setting.png)

Dış paylaşım etkinleştirdikten sonra SharePoint Online (SPO) Kişi Seçici var olan konuk kullanıcılar için arama yapma, eski davranışı eşleştirmek için varsayılan olarak OFF durumda.

'ShowPeoplePickerSuggestionsForGuestUsers' ayarı Kiracı ve site koleksiyonu düzeyinde kullanarak bu özelliği etkinleştirebilirsiniz. Üyelerinin dizindeki tüm mevcut Konuk kullanıcıları aramak Set-SPOTenant ve Set-SPOSite cmdlet'lerini kullanarak özelliğini ayarlayabilirsiniz. Değişiklikler Kiracı kapsamında zaten sağlanan SPO site etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)
* [Temsilci B2B işbirliği davetleri](delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](use-dynamic-groups.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](troubleshoot.md)
