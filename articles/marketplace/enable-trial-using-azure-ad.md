---
title: Azure AD kullanarak deneme etkinleştirme | Azure
description: Deneme listesi türünü Azure Active Directory (Azure AD) kullanarak Azure Marketi ve AppSource için uygulama ve hizmet yayımcılar etkinleştir
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
ms.openlocfilehash: 4140ba98c0c65c22674c61dc7266818af904e777
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825340"
---
# <a name="enable-trial-using-azure-ad"></a>Azure AD kullanarak deneme etkinleştir  
Azure Active Directory (Azure AD) olduğu kimlik doğrulamasına olanak tanıyan bir bulut kimliği hizmeti ile bir Microsoft iş veya Okul hesabı endüstri standardı çerçeveler kullanılarak: OAuth ve Openıd Connect.  
*   Azure AD hakkında daha fazla bilgi için Azure Active Directory'ı sayfasında bulunan ziyaret [azure.microsoft.com/services/active-directory](https://azure.microsoft.com/services/active-directory).  

Siz ve müşterileriniz Market'te Azure AD kullanarak kimlik doğrulaması yapılır. Market'te listeleme denemenizi müşteri seçtikten sonra müşterinizin deneme ortamınıza yönlendirilir.  Deneme ortamınızda, oturum açma ek adımlar gerek kalmadan doğrudan müşterinizi ayarlayabilir. Uygulama veya teklif, uygulama veya teklif bir kullanıcı hesabı oluşturmak için değerli kullanıcı bilgilerini içeren bir kimlik doğrulaması sırasında Azure AD'den bir belirteç isteyip alıyor. Kurulum otomatik hale getirmek ve dönüştürme olasılığını artırmak.  
*   Kimlik doğrulaması sırasında Azure AD'den gönderilen belirteç hakkında daha fazla bilgi için örnek bölümü bulunan belirteçleri ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens)

Azure AD uygulama ya da deneme tek tıklatmayla kimlik doğrulamasını etkinleştirmek için kullanın.  
*   Market müşteri deneyimine deneme kolaylaştırın.  
*   Kullanıcının etki alanı ya da deneme ortamınıza marketten bile yönlendirildiği zaman bir ürün deneyimi yapısını korur.  
*   Başka oturum açma adım olduğundan yeniden yönlendirme üzerinde abandonment olasılığını azaltır.  
*   Azure AD kullanıcıları büyük popülasyonunu dağıtım engelleri azaltın.  

## <a name="verify-your-azure-ad-integration-on-the-marketplace-multitenant-apps"></a>Bilgisayarınızı Azure AD tümleştirme Market'te doğrulayın: çok kullanıcılı uygulamalar  
Aşağıdaki seçenekler, çözümünüz için Azure AD kullanarak destekler.  
*   Market'te veriş uygulamanızı kaydedin.  
*   Çoklu müşteri mimarisi desteği özelliği, bir tek tıklamayla deneme sürümü deneyimi almak için Azure AD'de etkinleştirin.  
    *   Uygulama kaydı hakkında daha fazla bilgi için sayfa bulunan Azure Active Directory ile tümleştirme uygulamaları ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Yeni olması durumunda Azure AD kullanarak çoklu oturum açma (SSO) federe sonra aşağıdaki adımları izleyin.  
1.  Uygulamanızı Market'te kaydedin. 
2.  Azure AD ile SSO OAuth 2.0 veya Openıd Connect kullanarak geliştirin.  
    *   OAuth 2.0 hakkında daha fazla bilgi için sayfa bulunan OAuth 2.0 ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).  
    *   Open ID Connect hakkında daha fazla bilgi için sayfa bulunan Openıd Connect ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  
3.  Çoklu müşteri mimarisi desteği özelliği, bir tek tıklamayla deneme sürümü deneyimi sağlamak için Azure AD'de etkinleştirin.  
    *   AppSource sertifika hakkında daha fazla bilgi için sertifika sayfası bulunan AppSource ziyaret [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-on-the-marketplace-single-tenant-apps"></a>Bilgisayarınızı Azure AD tümleştirme Market'te doğrulayın: tek Kiracı uygulamalar  
Aşağıdaki seçeneklerden birini tek Kiracı çözümünüz için destek.  
*   Kullanıcıları Azure AD B2B kullanarak dizininize Konuk kullanıcılar olarak ekleyin.  
    *   Azure AD B2B hakkında daha fazla bilgi için Azure AD B2B işbirliği sayfasında bulunan nedir ziyaret [docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Denemeler kişi Me kullanan müşteriler için el ile ayarlama  
*   Müşteri başına sınamayı geliştirin.  
*   SSO çok müşterili örnek bir tanıtım uygulaması oluşturun.  

## <a name="next-steps"></a>Sonraki adımlar
*   Ziyaret [Azure Marketi ve AppSource yayımcı Kılavuzu](./marketplace-publishers-guide.md) sayfası.  
 
---  

