---
title: "Office 365 dış paylaşım ve Azure Active Directory B2B işbirliği | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği için başvuru eşleme talepleri"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: cad0ce8f745f3d6ca14436fd714b08c60de0e459
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Office 365 dış paylaşım ve Azure Active Directory B2B işbirliği

Dış paylaşım Office 365 (OneDrive, SharePoint Online, birleşik grupları, vb.) ve Azure Active Directory (Azure AD) B2B işbirliği teknik olarak aynı şeydir. Tüm dış (OneDrive/SharePoint Online dışında) paylaşımı, Office 365 gruplarında konuklar dahil olmak üzere, paylaşım için Azure AD B2B İşbirliği davetini API'leri zaten kullanıyor.

OneDrive/SharePoint Online ayrı davet Yöneticisi sahiptir. Azure AD desteğini geliştirilen önce dış OneDrive/SharePoint Online'da paylaşım desteği başlatıldı. Zaman içerisinde, OneDrive/SharePoint Online dış paylaşım çeşitli özellikler tahakkuk ve ürün kullanan kullanıcılar ve milyonlarca-düzeni paylaşımı yerleşik. Ancak, OneDrive/SharePoint Online dış paylaşım nasıl çalıştığını ve Azure AD B2B işbirliği nasıl çalıştığını arasındaki bazı farklar vardır:

- Kullanıcılar kendi davetleri kullanılan sonra OneDrive/SharePoint Online dizine kullanıcılar ekler. Bu nedenle, kullanım önce kullanıcının Azure AD portalında görmüyorum. Bir kullanıcı bu arada başka bir siteye başvurulmasını ise yeni bir davet oluşturulur. Azure AD B2B işbirliği kullandığınızda, böylece her yerde göster ancak, kullanıcılar hemen davette eklenir.

- OneDrive/SharePoint Online'daki kullanım deneyimi Azure AD B2B işbirliği deneyimi farklı arar. Bir kullanıcıya bir davet redeems sonra deneyimleri benzer.

- Azure AD B2B işbirliği davet kullanıcıların OneDrive/SharePoint Online'a iletişim kutuları paylaşımı çekilebilir. OneDrive/SharePoint davet Çevrimiçi kullanıcılar ayrıca kendi davetleri kullanmak sonra Azure AD içinde gösterilir.

- Dış OneDrive/SharePoint Online ile Azure AD B2B işbirliği paylaşım yönetmek için OneDrive/SharePoint Online dış ayarına paylaşımı ayarlayın **yalnızca dış kullanıcılarla zaten dizinde paylaşımına izin ver**. Kullanıcılar, harici olarak paylaşılan sitelere gidin ve yönetici ekledi dış ortak çalışanlarla seçin. B2B İşbirliği davetini API'leri aracılığıyla dış ortak yönetici ekleyebilirsiniz.

![OneDrive/SharePoint Online paylaşımı ayarı dış](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [B2B işbirliği davetleri temsilci seçme](active-directory-b2b-delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](active-directory-b2b-dynamic-groups.md)
* [B2B işbirliği kodu ve PowerShell örnekleri](active-directory-b2b-code-samples.md)
* [SaaS uygulamaları B2B işbirliği için yapılandırma](active-directory-b2b-configure-saas-apps.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
* [B2B işbirliği geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
