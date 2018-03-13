---
title: "Visual Studio'da bağlı hizmetler kullanarak bir Azure Active Directory'ye ekleme | Microsoft Docs"
description: "Visual Studio bağlı Hizmetleri Ekle iletişim kutusunu kullanarak bir Azure Active Directory'ye ekleme"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: a767c93fb271f3aa33d9556c69c511bcac7cb0d5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Visual Studio'da bağlı hizmetler kullanarak bir Azure Active Directory'ye ekleme
Azure Active Directory (Azure AD) kullanarak, çoklu oturum açma (SSO) ASP.NET MVC web uygulamaları veya Active Directory kimlik doğrulaması için Web API Hizmetleri'ni destekler. Azure Active Directory kimlik doğrulaması ile kullanıcılarınız, web uygulamalarınızı bağlanmak için Azure Active Directory hesaplarını kullanın. Bir web uygulamasından bir API gösterme işlemlerinde, Azure Active Directory kimlik doğrulaması Web API ile avantajları gelişmiş veri güvenliği kullanır. Azure AD ile ayrı kimlik doğrulama sistemi kendi hesabı ve kullanıcı yönetimi ile yönetmek zorunda değildir.

## <a name="prerequisites"></a>Ön koşullar
- Azure hesabı - bir Azure hesabınız yoksa şunları yapabilirsiniz [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Azure Active bağlı Hizmetleri iletişim kutusu kullanılarak dizinine bağlanma
1. Visual Studio'da oluşturun veya bir ASP.NET MVC projesini ya da bir ASP.NET Web API projesini açın.

1. Çözüm Gezgini'nden sağ **bağlantılı Hizmetler** düğümünü ve bağlam menüsünden seçin **bağlı Hizmetleri Ekle**.

1. Üzerinde **bağlantılı Hizmetler** sayfasında, **Azure Active Directory ile kimlik doğrulaması**.
   
    ![Bağlı hizmetler sayfası](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Üzerinde **giriş** sayfasında **yapılandırma Azure AD kimlik doğrulama** seçin **sonraki**.
   
    ![Giriş sayfası](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. Üzerinde **tek oturum açma** sayfasında **yapılandırma Azure AD kimlik doğrulama** Sihirbazı, bir etki alanından seçin **etki alanı** aşağı açılan liste. Etki alanlarının listesi, hesap ayarları iletişim kutusunda listelenen hesapları tarafından erişilebilir tüm etki alanlarını içerir. Alternatif olarak, aradığınız, gibi bir bulamazsanız, bir etki alanı adını girebilirsiniz `mydomain.onmicrosoft.com`. Bir Azure Active Directory uygulama oluşturmak veya mevcut bir Azure Active Directory Uygulama ayarlarından kullanmak için seçeneğini seçebilirsiniz. Seçin **sonraki** bittiğinde.
   
    ![Çoklu oturum açma sayfası](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. Üzerinde **dizin erişimi** sayfasında **yapılandırma Azure AD kimlik doğrulama** Sihirbazı'nı emin olun **dizin verilerini okuma** seçeneği denetlenir. 
   
    ![Dizin erişim sayfası](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Seçin **son** gerekli yapılandırma kodu ve projenizi Azure AD kimlik doğrulaması için etkinleştirmek için başvurular eklemek için. Active Directory etki alanı görebilirsiniz [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Visual Studio görüntüler bir [ne](#how-your-project-is-modified) projenizi nasıl değiştirildiği göstermek için makale. Her şeyin çalıştığı denetleyin, değiştirilen yapılandırma dosyalarından birini açın ve doğrulamak istiyorsanız makalede değinilen ayarları vardır. 

## <a name="how-your-project-is-modified"></a>Projenizi nasıl değiştirilir
Sihirbazı'nı çalıştırdığınızda, Visual Studio Azure Active Directory ve ilişkili başvuruları projenize ekler. Azure AD için destek eklemek için yapılandırma dosyalarını ve kod dosyaları projenizdeki değiştirilir. Visual Studio yapar belirli değişiklikleri proje türüne bağlıdır. ASP.NET MVC projeleri nasıl değiştirilir hakkında ayrıntılı bilgi için bkz: [hangi happened – MVC projeleri](http://go.microsoft.com/fwlink/p/?LinkID=513809). Web API projeleri için bkz: [ne – Web API projeleri](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory için MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Azure Active Directory belgeleri](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blog postası: Azure Active Directory giriş](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

