---
title: Visual Studio bağlı Hizmetler'i kullanarak bir Azure Active Directory'ye ekleme
description: Visual Studio bağlı Hizmetleri Ekle iletişim kutusunu kullanarak bir Azure Active Directory Ekle
services: active-directory
author: ghogen
manager: douge
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bea0a870a6ef0685f4f4bce5ad3b0d1ff1f616a
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65414003"
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Visual Studio bağlı Hizmetler'i kullanarak bir Azure Active Directory'ye ekleme

Azure Active Directory (Azure AD) kullanarak, çoklu oturum açma (SSO) ASP.NET MVC web uygulamaları veya Active Directory kimlik doğrulaması için Web API Hizmetleri destekler. Azure AD kimlik doğrulaması ile kullanıcıların hesaplarını Azure Active Directory'den web uygulamalarınıza bağlanmak için kullanabilirsiniz. Web API'si ile Azure AD kimlik doğrulaması avantajları, bir web uygulamasından API gösterme, Gelişmiş veri güvenliği içerir. Azure AD ile bir kimlik doğrulama sistemi kendi hesabı ve kullanıcı yönetimi ile yönetmek zorunda değildir.

Bu makalede ve yardımcı makaleleri için Active Directory Visual Studio bağlı hizmeti özelliğini kullanarak ayrıntılarını sağlayın. Visual Studio 2015'te kullanılabilir ve sonraki özellik yöneliktir.

Şu anda Active Directory bağlı hizmetini ASP.NET Core uygulamaları desteklemez.

## <a name="prerequisites"></a>Önkoşullar

- Azure hesabı: bir Azure hesabınız yoksa, şunları yapabilirsiniz [ücretsiz deneme için kaydolun](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone Avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
- **Visual Studio 2015** veya üzeri. [Visual Studio'yu hemen indirin](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Azure bağlı hizmetler iletişim kutusunu kullanarak Active Directory için Bağlan

1. Visual Studio'da oluşturun veya bir ASP.NET MVC projesi ya da bir ASP.NET Web API projesi açın. MVC, Web API'si, tek sayfalı uygulama, Azure API uygulaması, Azure mobil uygulaması ve Azure mobil hizmet şablonları kullanabilirsiniz.

1. Seçin **Proje > bağlı hizmet Ekle...**  menü komutu ya da çift **bağlı hizmetler** düğümü Çözüm Gezgini'nde proje altında bulunamadı.

1. Üzerinde **bağlı hizmetler** sayfasında **Azure Active Directory ile kimlik doğrulaması**.

    ![Bağlı hizmetler sayfası](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. Üzerinde **giriş** sayfasında **sonraki**. Bu sayfada hatalar görürseniz, başvurmak [Azure Active Directory bağlı hizmetini ile hataları tanılama](vs-active-directory-error.md).

    ![Giriş sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. Üzerinde **tek oturum açma** sayfasında, bir etki alanından **etki alanı** aşağı açılan listesi. Visual Studio hesap ayarları iletişim kutusunda listelenen hesaplar tarafından erişilebilen tüm etki alanları listesi içeriyor (**Dosya > Hesap Ayarları...** ). Alternatif olarak, aradığınız, gibi bir bulamazsanız, bir etki alanı adı girebilirsiniz `mydomain.onmicrosoft.com`. Azure Active Directory Uygulama oluşturma veya mevcut bir Azure Active Directory Uygulama ayarlarını kullanmak için bu seçeneği seçebilirsiniz. Seçin **sonraki** işiniz bittiğinde.

    ![Çoklu oturum açma sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. Üzerinde **dizin erişimi** sayfasında **dizin verilerini okuma** istediğiniz gibi seçeneği. Geliştiriciler genellikle bu seçenek içerir.

    ![Dizin erişimi sayfası](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Seçin **son** değişiklikler Azure AD kimlik doğrulamasını etkinleştirmek için projenizi yeniden başlatılacak. Visual Studio, bu süre boyunca ilerleme durumunu gösterir:

    ![Active Directory bağlı hizmetini ilerleme durumu](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. İşlem tamamlandığında Visual Studio, proje türü için uygun şekilde aşağıdaki makalelerden birine için tarayıcınızı açar:

    - [.NET MVC projelerini kullanmaya başlama](vs-active-directory-dotnet-getting-started.md)
    - [.NET WebAPI projelerini kullanmaya başlama](vs-active-directory-webapi-getting-started.md)

1. Active Directory etki alanı üzerinde görebilirsiniz [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040).

## <a name="how-your-project-is-modified"></a>Projenizi nasıl değiştirilir

Bağlı hizmet Sihirbazı eklediğinizde, Visual Studio, Azure Active Directory ve ilişkili başvuruları projenize ekler. Azure AD için destek eklemek için yapılandırma dosyalarını ve kod dosyaları, projenizdeki değiştirilir. Visual Studio yapan belirli değişiklikleri proje türüne bağlıdır. Ayrıntılar için aşağıdaki makalelere bakın:

- [.NET MVC projeme ne oldu?](vs-active-directory-dotnet-what-happened.md)
- [Web API projeme ne oldu?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](quickstart-v1-aspnet-webapp.md)
