---
title: Visual Studio'da bağlı hizmetler kullanarak bir Azure Active Directory'ye ekleme | Microsoft Docs
description: Visual Studio bağlı Hizmetleri Ekle iletişim kutusunu kullanarak bir Azure Active Directory'ye ekleme
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: ghogen
ms.openlocfilehash: 882ba1c7ea8ef6889bc9ad20031070cd54100026
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Visual Studio'da bağlı hizmetler kullanarak bir Azure Active Directory'ye ekleme

Azure Active Directory (Azure AD) kullanarak, çoklu oturum açma (SSO) ASP.NET MVC web uygulamaları veya Active Directory kimlik doğrulaması için Web API Hizmetleri'ni destekler. Azure AD kimlik doğrulaması ile kullanıcılarınız, web uygulamalarınızı bağlanmak için Azure Active Directory hesaplarını kullanın. Bir web uygulamasından bir API gösterme işlemlerinde, Web API ile Azure AD kimlik doğrulaması avantajları gelişmiş veri güvenliği kullanır. Azure AD ile ayrı kimlik doğrulama sistemi kendi hesabı ve kullanıcı yönetimi ile yönetmek zorunda değildir.

Bu makalede ve yardımcı makalelerinden Active Directory için Visual Studio bağlı hizmeti özelliğini kullanarak ayrıntılarını sağlayın. Visual Studio 2017 ve Visual Studio 2015 özelliği kullanılabilir.

Şu anda bağlı Active Directory Hizmeti ASP.NET Core uygulamaları desteklemez.

## <a name="prerequisites"></a>Önkoşullar

- Azure hesabı: bir Azure hesabınız yoksa, şunları yapabilirsiniz [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Azure Active bağlı Hizmetleri iletişim kutusu kullanılarak dizinine bağlanma

1. Visual Studio'da oluşturun veya bir ASP.NET MVC projesini ya da bir ASP.NET Web API projesini açın. MVC, Web API, tek sayfa uygulaması, Azure API uygulaması, Azure mobil uygulama ve Azure mobil hizmet şablonları kullanabilirsiniz.

1. Seçin **Proje > bağlı hizmeti Ekle...**  menü komutu ya da çift **bağlantılı Hizmetler** düğüm Çözüm Gezgini'nde proje altında bulundu.

1. Üzerinde **bağlantılı Hizmetler** sayfasında, **Azure Active Directory ile kimlik doğrulaması**.

    ![Bağlı hizmetler sayfası](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. Üzerinde **giriş** sayfasında, **sonraki**. Bu sayfada hatalar görürseniz, başvurmak [Azure Active Directory bağlı hizmeti ile hataları tanılama](vs-active-directory-error.md).

    ![Giriş sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. Üzerinde **tek oturum açma** sayfasında, bir etki alanından seçin **etki alanı** aşağı açılan liste. Visual Studio hesap ayarları iletişim kutusunda listelenen hesapları tarafından erişilebilir tüm etki alanları listesini içerir (**Dosya > Hesap Ayarları...** ). Alternatif olarak, aradığınız, gibi bir bulamazsanız, bir etki alanı adını girebilirsiniz `mydomain.onmicrosoft.com`. Bir Azure Active Directory uygulama oluşturmak veya mevcut bir Azure Active Directory Uygulama ayarlarından kullanmak için seçeneğini seçebilirsiniz. Seçin **sonraki** bittiğinde.

    ![Çoklu oturum açma sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. Üzerinde **dizin erişimi** sayfasında, **dizin verilerini okuma** istendiği gibi seçeneği. Geliştiriciler genellikle bu seçenek içerir.

    ![Dizin erişim sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Seçin **son** projenizi Azure AD kimlik doğrulamasını etkinleştirmek için yapılan değişiklikler başlatmak için. Visual Studio bu süre boyunca ilerleme durumunu gösterir:

    ![Active Directory Hizmeti ilerleme durumunu bağlı](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. İşlemi tamamlandığında, Visual Studio Proje türü için uygun şekilde aşağıdaki makaleleri birine tarayıcınızı açar:

    - [.NET MVC projelerini kullanmaya başlama](vs-active-directory-dotnet-getting-started.md)
    - [.NET WebAPI projelerini kullanmaya başlama](vs-active-directory-webapi-getting-started.md)

1. Active Directory etki alanı üzerinde görebilirsiniz [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

## <a name="how-your-project-is-modified"></a>Projenizi nasıl değiştirilir

Bağlı hizmet sihirbaz eklediğinizde, Visual Studio Azure Active Directory ve ilişkili başvuruları projenize ekler. Azure AD için destek eklemek için yapılandırma dosyalarını ve kod dosyaları projenizdeki değiştirilir. Visual Studio yapar belirli değişiklikleri proje türüne bağlıdır. Ayrıntılar için aşağıdaki makalelere bakın:

- [.NET MVC projeme ne oldu?](vs-active-directory-dotnet-what-happened.md)
- [My Web API projesi ne oldu?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](guidedsetups/active-directory-aspnetwebapp-v1.md)
