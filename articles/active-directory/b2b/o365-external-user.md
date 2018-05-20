---
title: Office 365 dış paylaşım ve Azure Active Directory B2B işbirliği | Microsoft Docs
description: O365 ve Azure Active Directory B2B işbirliği kullanarak dış iş ortakları ile kaynakların paylaşımı açıklanır.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/24/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: d55d587022b763a6890c098dd0a1b9bef2a3b7fb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Office 365 dış paylaşım ve Azure Active Directory B2B işbirliği

Dış paylaşım Office 365 (OneDrive, SharePoint Online, birleşik grupları, vb.) ve Azure Active Directory (Azure AD) B2B işbirliği teknik olarak aynı şeydir. Tüm dış (OneDrive/SharePoint Online dışında) paylaşımı, Office 365 gruplarında konuklar dahil olmak üzere, paylaşım için Azure AD B2B İşbirliği davetini API'leri zaten kullanıyor.

OneDrive/SharePoint Online ayrı davet Yöneticisi sahiptir. Azure AD desteğini geliştirilen önce dış OneDrive/SharePoint Online'da paylaşım desteği başlatıldı. Zaman içerisinde, OneDrive/SharePoint Online dış paylaşım çeşitli özellikler tahakkuk ve ürün kullanan kullanıcılar ve milyonlarca-düzeni paylaşımı yerleşik. Ancak, OneDrive/SharePoint Online dış paylaşım nasıl çalıştığını ve Azure AD B2B işbirliği nasıl çalıştığını arasındaki bazı farklar vardır:

- Kullanıcılar kendi davetleri kullanılan sonra OneDrive/SharePoint Online dizine kullanıcılar ekler. Bu nedenle, kullanım önce kullanıcının Azure AD portalında görmüyorum. Bir kullanıcı bu arada başka bir siteye başvurulmasını ise yeni bir davet oluşturulur. Azure AD B2B işbirliği kullandığınızda, böylece her yerde göster ancak, kullanıcılar hemen davette eklenir.

- OneDrive/SharePoint Online'daki kullanım deneyimi Azure AD B2B işbirliği deneyimi farklı arar. Bir kullanıcıya bir davet redeems sonra deneyimleri benzer.

- Azure AD B2B işbirliği davet kullanıcıların OneDrive/SharePoint Online'a iletişim kutuları paylaşımı çekilebilir. OneDrive/SharePoint davet Çevrimiçi kullanıcılar ayrıca kendi davetleri kullanmak sonra Azure AD içinde gösterilir.

- Dış OneDrive/SharePoint Online ile Azure AD B2B işbirliği paylaşım yönetmek için OneDrive/SharePoint Online dış ayarına paylaşımı ayarlayın **yalnızca dış kullanıcılarla zaten dizinde paylaşımına izin ver**. Kullanıcılar, harici olarak paylaşılan sitelere gidin ve yönetici ekledi dış ortak çalışanlarla seçin. B2B İşbirliği davetini API'leri aracılığıyla dış ortak yönetici ekleyebilirsiniz.

![OneDrive/SharePoint Online paylaşımı ayarı dış](media/o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Bir role B2B işbirliği kullanıcı ekleme](add-guest-to-role.md)
* [B2B işbirliği davetleri temsilci seçme](delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](use-dynamic-groups.md)