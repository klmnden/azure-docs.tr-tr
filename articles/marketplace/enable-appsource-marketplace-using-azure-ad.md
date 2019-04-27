---
title: Azure Active Directory kullanarak Microsoft AppSource ve Azure Marketi'nde listeleme etkinleştirme | Azure
description: Liste türü, uygulama ve hizmet yayımcılar için Azure Active Directory'de Azure Market ve AppSource kullanarak etkinleştirin.
services: Azure, AppSource, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/12/2018
ms.author: qianw211
ms.openlocfilehash: 247a45a38d732ace0455c6ca2ebbd5c44c384004
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734261"
---
# <a name="enable-an-appsource-and-marketplace-listing-by-using-azure-active-directory"></a>Azure Active Directory kullanarak AppSource ve Market listesini etkinleştirme

 Azure Active Directory (Azure AD), bir Microsoft hesabıyla kimlik doğrulaması sağlayan bir bulut kimlik hizmetidir. Azure AD, endüstri standardı çerçeveleri kullanır. [Azure Active Directory hakkında daha fazla bilgi](https://azure.microsoft.com/services/active-directory).

## <a name="azure-ad-benefits"></a>Azure AD'nin avantajlarını

Microsoft AppSource ve Azure Market müşterileri ürün içi deneyimleri listesi kataloglarında aramak için kullanın. Bu Eylemler, ürüne oturum açmak müşterilerin gerektirir. Azure AD tümleştirmesi aşağıdaki avantajları sağlar:

- Daha hızlı engagement ve iyileştirilmiş Müşteri Deneyimi
- Çoklu oturum açma (SSO) milyonlarca Kurumsal kullanıcı için
- Farklı iş ortakları tarafından yayımlanan uygulamalar arasında tutarlı, oturum açma deneyimi
- Mobil için ölçeklenebilir, platformlar arası kimlik doğrulaması ve bulut uygulamaları

## <a name="offers-that-require-azure-ad"></a>Azure AD gerektiren teklifler

Çeşitli [listeleme seçenekleri ve tür](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) AppSource ve Azure marketi, Azure AD uygulaması için farklı gereksinimlere sahip. Ayrıntılar için aşağıdaki tabloya bakın:

| **Teklif türü**    | **Azure AD SSO gerekli mi?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Benimle iletişime geçin | Deneme | Test Sürüşü | İşlem |
| Sanal Makine | Yok | Hayır | Hayır | Hayır |
| Azure uygulamaları (Çözüm şablonu)  | Yok | Yok | Yok | Yok |
| Yönetilen uygulamalar  | Yok | Yok | Yok | Hayır |
| SaaS  | Hayır | Evet | Evet | Evet |
| Kapsayıcılar  | Yok | Yok | Yok | Hayır |
| Danışmanlık Hizmetleri  | Hayır | Yok | Yok | Yok |

SaaS teknik gereksinimleri hakkında daha fazla bilgi için bkz. [SaaS uygulamaları yayımlama Kılavuzu teklif](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="azure-ad-integration"></a>Azure AD tümleştirmesi

- Azure AD uygulamasına listenizi tümleştirerek çoklu oturum açmayı etkinleştirme hakkında daha fazla bilgi için bkz [geliştiriciler için Azure Active Directory]( https://aka.ms/aaddev).
- Azure AD çoklu oturum açma hakkında bilgi edinmek için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="enable-a-trial-listing"></a>Deneme listesini etkinleştir

Otomatik müşteri Kurulum dönüşümü olasılığını artırabilir. Müşterinizin deneme listenizi seçer ve deneme ortamınıza yeniden yönlendirildiğinde, ek oturum açma adımlar gerek kalmadan doğrudan müşteri ayarlayabilirsiniz.

Kimlik doğrulaması sırasında Azure AD uygulamanızı veya teklif için bir belirteç gönderir. Belirteç tarafından sağlanan kullanıcı bilgileri, bir kullanıcı hesabı uygulama veya teklif oluşturulmasını sağlar. Daha fazla bilgi için bkz. [örnek belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

Uygulama veya deneme listesi, tek tıklamayla kimlik doğrulamasını etkinleştirmek için Azure AD kullandığınızda:

- Marketten deneme listenizi müşteri deneyimine kolaylaştırın.
- Kullanıcı etki alanı ya da deneme ortamınıza marketten yönlendirildiğinde bir ürün içi deneyim yapısını bile korur.
- Kullanıcıların oturum açma ek adımlar olduğundan yeniden yönlendirilmesi gerektiğinde abandonment olasılığını azaltır.
- Azure AD kullanıcılarının büyük nüfusunu dağıtım önündeki engelleri azaltın.

## <a name="verify-azure-ad-integration"></a>Azure AD tümleştirmesi doğrulayın

### <a name="multitenant-solutions"></a>Çok kiracılı çözümleri

Aşağıdaki eylemleri desteklemek için Azure AD'yi kullanın:

- Uygulamanızı Market vitrininin birleşiminden birinde kaydedin. Görünüm [uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) veya [AppSource sertifikasyonu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified) daha fazla bilgi için.
- Tek tıklamayla çalışan bir deneme sürümü deneyimi almak için Azure AD'de çoklu müşteri mimarisi desteği özelliği etkinleştirin.

Azure AD Federasyon çoklu oturum açma kullanmaya yeni başladıysanız, aşağıdaki adımları gerçekleştirin:

1. Market'te uygulamanızı kaydedin.
1. Azure ad SSO kullanarak geliştirme [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) veya [Openıd Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
1. Tek tıklamayla çalışan bir deneme sürümü deneyimi sağlamak için Azure AD'de çoklu müşteri mimarisi desteği özelliği etkinleştirin.

### <a name="single-tenant-solutions"></a>Tek kiracılı çözümleri

Azure AD'ye destek aşağıdaki eylemlerden birini kullanın:

- Kullanarak dizininize Konuk kullanıcıları eklemek [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- El ile müşteriler için deneme kullanarak ayarlamak **benimle iletişim kurun** seçeneği yayımlama.
- Bir müşteri başına test sürüşü geliştirin.
- SSO kullanan çok kiracılı örnek Tanıtım uygulaması oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

- Seçtiğiniz emin [Azure Marketi'nde kayıtlı](https://azuremarketplace.microsoft.com/sell).
- Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/) oluşturmak veya teklifiniz tamamlayın.
