---
title: Azure Active Directory B2B işbirliği nedir? | Microsoft Docs
description: Azure Active Directory B2B işbirliği iş ortaklarının Kurumsal uygulamalarınıza erişmesini olanaklı kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 51a969ae215583a0be8d75ff1de11173e0696a22
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651016"
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği nedir?

Azure Active Directory (Azure AD) işletmeler arası (B2B) işbirliği özelliklerinden herhangi diğer kuruluşun kullanıcıları, küçük veya büyük güvenli bir şekilde ve güvenli bir şekilde çalışmak için Azure AD kullanarak herhangi bir kuruluştaki etkinleştirin. Kuruluşlar ile veya olmadan Azure AD olabilir ve hatta bir BT departmanının ihtiyacınız yoktur.

Azure AD kullanan kuruluşlar, kendi şirket verileri üzerinde tam denetimini elde tutarak belgelerin, kaynakları ve bunların iş ortakları için uygulamalar için erişim sağlayabilir. Geliştiriciler, Azure AD-işletmeler kullanabileceğiniz iki kuruluş daha güvenli bir şekilde bir araya uygulamalar yazmak üzere API'ler. Ayrıca, son kullanıcıların gitmek kolaydır.

Aşağıdaki video, kullanışlı bir genel bakış sağlar.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği önemli avantajları

### <a name="work-with-any-user-from-any-partner"></a>Herhangi bir kullanıcının herhangi bir iş ortağı ile çalışma

- İş ortakları kendi kimlik bilgilerini kullan
- Hiçbir gereksinim iş ortakları, Azure AD kullanmak için
- Herhangi bir dış dizinlere veya karmaşık Kurulum gerekli

### <a name="simple-and-secure-collaboration"></a>Basit ve güvenli işbirliği

- Gelişmiş, Azure AD tarafından sağlanan Yetkilendirme İlkeleri uygulanırken herhangi bir şirket uygulamasını veya veri erişim sağlar.
- Kullanıcılar daha kolay
- Uygulamalar ve veriler için kurumsal düzeyde güvenlik

### <a name="no-management-overhead"></a>Hiçbir yönetim yükü

- Dış bir hesap veya parola yönetimi
- Hiçbir eşitleme veya el ile hesap yaşam döngüsü yönetimi
- Dış yönetim zahmetine

## <a name="easily-add-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları kolayca ekleyin

Bir yönetici olarak kolayca B2B işbirliği (konuk) kullanıcıları kuruluşunuza ekleyebileceğiniz [Azure portalında](https://portal.azure.com).

![Konuk kullanıcıları ekleme](media/what-is-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Ortak Çalışanlar kendi kimlik getirmeyi etkinleştir

B2B ortak çalışanlar istedikleri kimlik bilgilerinizle oturum açabilirsiniz. Kullanıcının bir Microsoft hesabı veya bir Azure AD hesabı – yoksa biri kendileri için sorunsuz bir şekilde teklif kullanım için zaman oluşturulur.

### <a name="delegate-to-application-and-group-owners"></a>Uygulama ve Grup sahipleri için temsilci seçme

Bir Microsoft uygulaması olup olmadığına bakılmaksızın bir uygulama veya grup sahibi B2B kullanıcıları doğrudan, önem verdiğiniz her uygulama ekleyebilirsiniz. Yöneticileri, yönetici olmayanlar için B2B kullanıcıları ekleme izni atayabilirler. Yönetici olmayan kullanıcılar kullanabilir [Azure AD uygulama erişim panelinde](https://myapps.microsoft.com) B2B işbirliği kullanıcıları uygulamaları veya gruplara eklemek için.

![Erişim paneli](media/what-is-b2b/access-panel.png)

![Üye Ekle](media/what-is-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Yetkilendirme İlkeleri Kurumsal içeriğinizi koruyun

Çok faktörlü kimlik doğrulaması gibi koşullu erişim ilkeleri uygulanabilir:
- Kiracı düzeyinde
- Uygulama düzeyinde
- Belirli kullanıcılar için şirket uygulamalarını ve verilerini koruma

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>API'leri kullanan ve örnek kod, dahili uygulamalara kolayca oluşturmak için

Karttaki dış iş ortaklarınızı kuruluşunuzun ihtiyaçlarına göre özelleştirilmiş şekilde taşıyın.

Kullanım [B2B işbirliği davet API'leri](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) ekleme özelleştirmek için Self Servis kaydolma Portal oluşturma gibi karşılaşır. Bir Self Servis portalı için örnek kodu sağlarız [github'da](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![Kayıt portalı](media/what-is-b2b/sign-up-portal.png)

Azure AD B2B işbirliğiyle iş ortağı ilişkilerinizi son kullanıcılar kolay ve sezgisel bulabileceğiniz bir şekilde korumak için Azure AD'ye tam gücünü alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-information-worker.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [Azure AD B2B işbirliği lisanslama](licensing-guidance.md)
- Ve ürün ekibine geri bildirim, tartışmalar ve öneriler aracılığıyla her zaman olduğu gibi bağlanmak bizim [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).