---
title: "Azure Active Directory B2B işbirliği nedir? | Microsoft Belgeleri"
description: "Azure Active Directory B2B işbirliği Kurumsal uygulamalarınıza seçmeli olarak erişmek için iş ortaklarıyla etkinleştirerek, şirketler arası ilişkilerinizi destekler."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: fbc12a52555b190d43b5e953fd4d19923a25b0ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği nedir?

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

Azure AD iş işletmeden işletmeye (B2B) işbirliği özelliklerinden herhangi diğer kuruluştan, küçük veya büyük kullanıcılarla güvenle ve güvenli bir şekilde çalışması için Azure AD kullanan herhangi bir kuruluş etkinleştirin. Bu kuruluşlardan Azure AD ile veya olmadan, hatta bir BT kuruluşu ile veya olmadan olabilir. 

Azure AD kullanarak kuruluşlar kendi şirket verileri üzerinde tam denetimi korurken belgeleri, kaynakları ve ortakları, uygulamalara erişim sağlayabilir. Geliştiriciler, Azure AD-işletmeler kullanabileceğiniz iki kuruluş birlikte daha güvenli bir şekilde Getir uygulamalar yazmak üzere API'ler. Ayrıca, son kullanıcıların gitmek oldukça kolaydır.

Azure AD B2B işbirliği için çok önemlidir, müşterilerimizin %97 olmamızı istiyordu.

![Pasta grafik](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Erken Nisan 2017 itibariyle zaten Azure AD B2B işbirliği özelliklerini kullanarak yaklaşık 3 milyon kullanıcı vardı. Ve birden fazla %23 10'dan fazla kullanıcınız Azure AD kuruluşların zaten bu özelliklerini benefiting.

## <a name="the-key-benefits-of-azure-ad-b2b-collaboration-to-your-organization"></a>Kuruluşunuz için Azure AD B2B işbirliği kilit yararları

### <a name="work-with-any-user-from-any-partner"></a>Herhangi bir ortaktan herhangi bir kullanıcı ile çalışma

* İş ortakları kendi kimlik bilgilerini kullan

* Azure AD kullanmak iş ortakları için gereksinimi yoktur

* Hiçbir dış dizinleri veya karmaşık Kurulum gerekli

### <a name="simple-and-secure-collaboration"></a>Basit ve güvenli işbirliği

* Gelişmiş, Azure AD destekli Yetkilendirme İlkeleri uygulanırken herhangi bir şirket uygulamasını veya veri erişim sağlamak

* Kullanıcılar için kolay

* Uygulamalar ve veriler için kurumsal düzeyde güvenlik

### <a name="no-management-overhead"></a>Yönetim yükü

* Dış bir hesap veya parola yönetimi

* Hiçbir eşitleme veya el ile hesap yaşam döngüsü yönetimi

* Hiçbir dış yönetim yükü

## <a name="you-can-easily-add-b2b-collaboration-users-to-your-organization"></a>B2B işbirliği kullanıcılar kuruluşunuza kolayca ekleyebilirsiniz.

Yöneticiler, B2B işbirliği (konuk) kullanıcılar ekleyebilirsiniz [Azure portal](https://portal.azure.com).

![Konuk kullanıcı ekleme](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Kendi kimliğini getirmek, ortak etkinleştir

B2B Ortak Çalışanlar kendi seçtikleri kimlik bilgilerinizle oturum açabilirsiniz. Kullanıcının bir Microsoft hesabı veya bir Azure AD hesabı – yoksa, bunlar için sorunsuz bir şekilde teklif kullanım için anda oluşturulur.

![oturum açma kimlik seçimi](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-to-application-and-group-owners"></a>Uygulama ve Grup sahipleri için temsilci seçme 
Bir Microsoft uygulaması olup olmadığına bakılmaksızın uygulama ve Grup sahipleri B2B kullanıcılar, ilgilendiğiniz doğrudan herhangi bir uygulama ekleyebilirsiniz. Yöneticiler, yönetici olmayanlar için B2B kullanıcılar ekleme izni devredebilirsiniz. Olmayan yöneticilere kullanabileceğiniz [Azure AD uygulama erişim Paneli'ne](https://myapps.microsoft.com) B2B işbirliği kullanıcılar uygulamaları veya grupları eklemek için.

![erişim paneli](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![üye ekleme](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Yetkilendirme İlkeleri, şirket içeriğini koruyun

Koşullu erişim ilkeleri, çok faktörlü kimlik doğrulaması gibi uygulanabilir:
- Kiracı düzeyinde
- Uygulama düzeyinde
- Belirli kullanıcıların şirket uygulamaları ve verileri koruma

![üye ekleme](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Bizim API'leri kullanan ve kolayca yerleşik uygulamaları oluşturmak için kod örneği
Dış ortaklarınız karttaki kuruluşunuzun gereksinimlerine göre özelleştirilmiş yolları duruma getirin.

Kullanarak [B2B İşbirliği davetini API'leri](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), Self Servis kaydolma portalları oluşturma dahil olmak üzere onboarding deneyimlerinizi özelleştirebilirsiniz. Bir Self Servis portalı için örnek kod sağladığımız [github'da](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![Kayıt portalı](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

Azure AD B2B işbirliği ile iş ortağı ilişkileri, son kullanıcıların kolay ve sezgisel Bul şekilde korumak için Azure ad tam güç elde edebilirsiniz. Bu nedenle devam edin, Azure AD B2B kendi dış işbirliği için zaten kullanan kuruluş binlerce katılın!

## <a name="next-steps"></a>Sonraki adımlar

* Yönetici deneyimleri içinde bulunan [Azure portal](https://portal.azure.com).

* Bilgi çalışanı deneyimleri kullanılabilir [erişim paneli](https://myapps.microsoft.com).

* Ve her zaman olduğu gibi tüm geri bildirim, tartışmalara ve üzerinden öneriler için ürün ekibi bağlamak bizim [Microsoft teknik topluluk](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B işbirliği API ve özelleştirme](active-directory-b2b-api.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [B2B işbirliği kullanıcı Denetim ve Raporlama](active-directory-b2b-auditing-and-reporting.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
