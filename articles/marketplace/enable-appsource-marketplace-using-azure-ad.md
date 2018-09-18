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
ms.openlocfilehash: d7fd09928c0a687755d216e7f10f7eac23677c63
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986428"
---
# <a name="enable-a-microsoft-appsource-and-azure-marketplace-listing-by-using-azure-active-directory"></a>Azure Active Directory kullanarak Microsoft AppSource ve Azure Marketi'nde listesini etkinleştir

Microsoft Azure Active Directory (Azure AD), endüstri standardı çerçeveleri kullanarak bir Microsoft hesabıyla kimlik doğrulaması sağlayan bir bulut kimlik hizmetidir.  Azure AD hakkında daha fazla bilgi için bkz. [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="benefits-of-using-azure-active-directory"></a>Azure Active Directory kullanmanın avantajları

Microsoft AppSource ve Azure Market müşterileri, bunları ürün için oturum açmanız gerekir listesi kataloglarında aramak için ürün deneyimlerini kullanın.  Uygulamanızı Azure AD ile tümleştirdiğinizde, engagement hızlandırmak ve müşteri deneyimini iyileştirin. Azure AD:

- Milyonlarca Kurumsal kullanıcı için oturum açma (SSO) etkinleştirir tek.
- Farklı iş ortakları tarafından yayımlanan uygulamalar arasında tutarlı bir kullanıcı oturum açma deneyimi sağlar.
- Mobil için ölçeklenebilir, platformlar arası kimlik doğrulaması ve bulut sağlayan uygulamalar.

Ayrıntılı olarak aşağıdaki bölümde belirli teklifleri Market'te yayımlamak için Azure AD uygulamak için gereklidir.

## <a name="azure-active-directory-requirements"></a>Azure Active Directory gereksinimleri

Vardır farklı [listeleme seçenekleri ve tür](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) Microsoft AppSource ve Azure Market.  Bu listeleme seçenekleri ve teklif türleri için Azure AD gereksinimleri aşağıda verilmiştir:

| **Teklif türü**    | **AAD SSO gerekli mi?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Benimle iletişime geçin | Deneme | Test Sürüşü | İşlem |
| Sanal Makine | Yok | Hayır | Hayır | Hayır |
| Azure uygulamaları (Çözüm şablonu)  | Yok | Yok | Yok | Yok |
| Yönetilen uygulamalar  | Yok | Yok | Yok | Hayır |
| SaaS  | Hayır | Evet | Evet | Evet |
| Kapsayıcılar  | Yok | Yok | Yok | Hayır |
| Danışmanlık Hizmetleri  | Hayır | Yok | Yok | Yok |

SaaS teknik gereksinimleri hakkında daha fazla bilgi için bkz. [SaaS uygulamaları yayımlama Kılavuzu teklif](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="integration-with-azure-active-directory"></a>Azure Active Directory ile tümleştirme

SSO'yu etkinleştirmek için Azure AD ile tümleştirme hakkında daha fazla bilgi için ziyaret https://aka.ms/aaddev.

Azure AD SSO hakkında daha fazla bilgi için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)?

## <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Azure Active Directory kullanarak listeleyen bir deneme sürümünü etkinleştirin

Bir müşteri Marketi'nde listeleme denemenizi seçtikten sonra müşterinizin deneme ortamınıza yönlendirilir. Deneme ortamınızda oturum açma ek adımlar gerek kalmadan doğrudan müşterinizi ayarlayabilirsiniz. Uygulamanızı veya teklif, Azure ad kimlik doğrulaması sırasında bir belirteç alır. Belirteci, bir kullanıcı hesabı, uygulama veya teklifi oluşturmak için kullanılan değerli kullanıcı bilgilerini içerir. Müşteri Kurulum otomatikleştirin ve dönüştürme olasılığını artırın.

Azure ad kimlik doğrulaması sırasında gönderilen belirtecin hakkında daha fazla bilgi için bkz: [örnek belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Uygulama veya deneme tek tıklamayla kimlik doğrulamasını etkinleştirmek için Azure AD'yi kullanın. Azure AD, aşağıdaki avantajları sunar: 
*   Deneme sürümü için marketten müşteri deneyiminizi kolaylaştırın.
*   Hatta kullanıcı marketten etki alanı ya da deneme ortamınıza yeniden yönlendirildiğinde bir ürün içi deneyim yapısını korur.
*   Oturum açma ek adımlar olduğundan yeniden yönlendirme, bağlı abandonment olasılığını azaltır.
*   Azure AD kullanıcılarının büyük nüfusunu dağıtım önündeki engelleri azaltın.

### <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Market'te Azure AD tümleştirmenizi doğrulayın: çok Kiracılı uygulamalar
Çözümünüz için aşağıdaki seçeneklerden desteklemek için Azure AD'yi kullanın:
*   Market'te vitrinler uygulamanızı kaydedin.
*   Tek tıklamayla çalışan bir deneme sürümü deneyimi almak için Azure AD'de çoklu müşteri mimarisi desteği özelliği etkinleştirin.

Uygulama kaydı hakkında daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

Yeni başladıysanız kullanarak Azure AD Federasyon çoklu oturum açma (SSO)'yı, aşağıdaki adımları tamamlayın:
1.  Market'te uygulamanızı kaydedin. 
2.  OAuth 2.0 veya Openıd Connect kullanarak Azure AD ile SSO geliştirin.
    *   OAuth 2.0 hakkında daha fazla bilgi için bkz: [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
    *   Open ID Connect hakkında daha fazla bilgi için bkz: [Openıd Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
3.  Tek tıklamayla çalışan bir deneme sürümü deneyimi sağlamak için Azure AD'de çoklu müşteri mimarisi desteği özelliği etkinleştirin.
    
    AppSource sertifikası hakkında daha fazla bilgi için bkz: [AppSource sertifikasyonu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

### <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Market'te Azure AD tümleştirmenizi doğrulayın: tek kiracılı uygulamalar
Tek kiracılı çözümünüz için aşağıdaki seçeneklerden birini desteklemesi için Azure AD'yi kullanın: 
*   Kullanıcılar Azure Active Directory B2B kullanarak dizininize Konuk kullanıcıları ekleme (Azure AD B2B). Azure AD B2B hakkında daha fazla bilgi için bkz: [Azure AD B2B işbirliği nedir](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   El ile denemeler müşteriler için kişi kullanarak bana yayımlama seçeneğini ayarlayın.
*   Bir müşteri başına test sürüşü geliştirin.
*   SSO kullanan çok kiracılı örnek Tanıtım uygulaması oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Zaten yapmadıysanız, 
- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te.

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,
- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com/) oluşturmak veya teklifiniz tamamlayın.

