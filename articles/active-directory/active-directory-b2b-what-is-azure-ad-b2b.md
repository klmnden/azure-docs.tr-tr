---
title: Azure Active Directory B2B işbirliği nedir? | Microsoft Docs
description: Azure Active Directory B2B işbirliği Kurumsal uygulamalarınıza seçmeli olarak erişmek için iş ortaklarıyla etkinleştirerek, şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 1eff04cf43260a12a50a08b9145e1975b51af0cc
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği nedir?

Azure Active Directory (Azure AD) işletmeden işletmeye (B2B) işbirliği özelliklerinden herhangi diğer kuruluştan, küçük veya büyük kullanıcılarla güvenle ve güvenli bir şekilde çalışması için Azure AD kullanan herhangi bir kuruluş etkinleştirin. Bu kuruluşlardan ile veya olmadan Azure AD olabilir ve hatta bir BT departmanının olması gerekmez.

Azure AD kullanarak kuruluşlar kendi şirket verileri üzerinde tam denetimi korurken belgeleri, kaynakları ve ortakları, uygulamalara erişim sağlayabilir. Geliştiriciler, Azure AD-işletmeler kullanabileceğiniz iki kuruluş birlikte daha güvenli bir şekilde Getir uygulamalar yazmak üzere API'ler. Ayrıca, son kullanıcıların gitmek kolaydır.

Aşağıdaki video yararlı bir genel bakış sağlar.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği kilit yararları

### <a name="work-with-any-user-from-any-partner"></a>Herhangi bir ortaktan herhangi bir kullanıcı ile çalışma

- İş ortakları kendi kimlik bilgilerini kullan
- Azure AD kullanmak iş ortakları için gereksinimi yoktur
- Hiçbir dış dizinleri veya karmaşık Kurulum gerekli

### <a name="simple-and-secure-collaboration"></a>Basit ve güvenli işbirliği

- Gelişmiş, Azure AD destekli Yetkilendirme İlkeleri uygulanırken herhangi bir şirket uygulamasını veya veri erişim sağlamak
- Kullanıcılar için kolay
- Uygulamalar ve veriler için kurumsal düzeyde güvenlik

### <a name="no-management-overhead"></a>Yönetim yükü

- Dış bir hesap veya parola yönetimi
- Hiçbir eşitleme veya el ile hesap yaşam döngüsü yönetimi
- Hiçbir dış yönetim yükü

## <a name="easily-add-b2b-collaboration-users"></a>B2B işbirliği kullanıcılar ekleyin

Yönetici olarak, kolayca B2B işbirliği (konuk) kullanıcıların, kuruluşunuzda ekleyebileceğiniz [Azure portal](https://portal.azure.com).

![Konuk kullanıcı ekleme](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Kendi kimliğini getirmek, ortak etkinleştir

B2B Ortak Çalışanlar kendi seçtikleri kimlik bilgilerinizle oturum açabilirsiniz. Kullanıcının bir Microsoft hesabı veya bir Azure AD hesabı – yoksa, bunlar için sorunsuz bir şekilde teklif kullanım için anda oluşturulur.

### <a name="delegate-to-application-and-group-owners"></a>Uygulama ve Grup sahipleri için temsilci seçme

Bir Microsoft uygulaması olup olmadığını belirten bir uygulama veya grup sahibi, B2B kullanıcıların doğrudan, ilgilendiğiniz her uygulama ekleyebilirsiniz. Yöneticiler, yönetici olmayanların için B2B kullanıcılar ekleme izni devredebilirsiniz. Yönetici olmayanlar kullanabileceğiniz [Azure AD uygulama erişim Paneli'ne](https://myapps.microsoft.com) B2B işbirliği kullanıcılar uygulamaları veya grupları eklemek için.

![erişim paneli](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![üye ekleme](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Yetkilendirme İlkeleri, şirket içeriğini koruyun

Koşullu erişim ilkeleri, çok faktörlü kimlik doğrulaması gibi uygulanabilir:
- Kiracı düzeyinde
- Uygulama düzeyinde
- Belirli kullanıcıların şirket uygulamaları ve verileri koruma

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>API'leri kullanan ve kolayca yerleşik uygulamaları oluşturmak için kod örneği

Dış ortaklarınız karttaki kuruluşunuzun gereksinimlerine göre özelleştirilmiş yolları duruma getirin.

Kullanım [B2B İşbirliği davetini API'leri](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) , onboarding özelleştirmek için Self Servis kaydolma portalları oluşturma dahil karşılaştığında. Bir Self Servis portalı için örnek kod sağladığımız [github'da](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![Kayıt portalı](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

Azure AD B2B işbirliği ile iş ortağı ilişkileri, son kullanıcıların kolay ve sezgisel Bul şekilde korumak için Azure ad tam güç elde edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
- [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
- [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
- [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
- Ve her zaman olduğu gibi tüm geri bildirim, tartışmalara ve üzerinden öneriler için ürün ekibi bağlamak bizim [Microsoft teknik topluluk](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).