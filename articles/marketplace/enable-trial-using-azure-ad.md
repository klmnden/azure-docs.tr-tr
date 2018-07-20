---
title: Azure Active Directory kullanarak Azure Marketi'nde deneme sürümünü etkinleştirme | Azure
description: Liste türü uygulama ve hizmet yayımcılar için Azure Active Directory'de Azure Market ve AppSource kullanarak bir deneme sürümünü etkinleştirin.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/04/2018
ms.author: ellacroi
ms.openlocfilehash: c5b7b4967c1acef733d366e651d50706db42aace
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160476"
---
# <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Azure Active Directory kullanarak listeleyen bir deneme sürümünü etkinleştirin

Azure Active Directory (Azure AD) kimlik doğrulamasına olanak tanıyan bir bulut kimlik hizmeti, Microsoft ile iş veya Okul hesabı endüstri standardı çerçeveleri kullanarak. Azure AD, OAuth ve Openıd Connect kimlik doğrulamasını destekler. [Azure Marketi](https://azuremarketplace.microsoft.com) sizin ve müşterilerinizin kimliğini doğrulamak için Azure AD kullanır.

Azure AD hakkında daha fazla bilgi için bkz. [Azure Active Directory](https://azure.microsoft.com/services/active-directory).

Bir müşteri Marketi'nde listeleme denemenizi seçtikten sonra müşterinizin deneme ortamınıza yönlendirilir. Deneme ortamınızda oturum açma ek adımlar gerek kalmadan doğrudan müşterinizi ayarlayabilirsiniz. Uygulamanızı veya teklif, Azure ad kimlik doğrulaması sırasında bir belirteç alır. Belirteci, bir kullanıcı hesabı, uygulama veya teklifi oluşturmak için kullanılan değerli kullanıcı bilgilerini içerir. Müşteri Kurulum otomatikleştirin ve dönüştürme olasılığını artırın.

Azure ad kimlik doğrulaması sırasında gönderilen belirtecin hakkında daha fazla bilgi için bkz: [örnek belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Uygulama veya deneme tek tıklamayla kimlik doğrulamasını etkinleştirmek için Azure AD'yi kullanın. Azure AD, aşağıdaki avantajları sunar: 
*   Deneme sürümü için marketten müşteri deneyiminizi kolaylaştırın.
*   Hatta kullanıcı marketten etki alanı ya da deneme ortamınıza yeniden yönlendirildiğinde bir ürün içi deneyim yapısını korur.
*   Oturum açma ek adımlar olduğundan yeniden yönlendirme, bağlı abandonment olasılığını azaltır.
*   Azure AD kullanıcılarının büyük nüfusunu dağıtım önündeki engelleri azaltın.

## <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Market'te Azure AD tümleştirmenizi doğrulayın: çok Kiracılı uygulamalar
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

## <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Market'te Azure AD tümleştirmenizi doğrulayın: tek kiracılı uygulamalar
Tek kiracılı çözümünüz için aşağıdaki seçeneklerden birini desteklemesi için Azure AD'yi kullanın: 
*   Kullanıcılar Azure Active Directory B2B kullanarak dizininize Konuk kullanıcıları ekleme (Azure AD B2B).
    
    Azure AD B2B hakkında daha fazla bilgi için bkz: [Azure AD B2B işbirliği nedir](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   El ile denemeler müşteriler için kişi kullanarak bana yayımlama seçeneğini ayarlayın.
*   Bir müşteri başına test sürüşü geliştirin.
*   SSO kullanan çok kiracılı bir örnek Tanıtım uygulaması oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
*   Gözden geçirme [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](./marketplace-publishers-guide.md).
