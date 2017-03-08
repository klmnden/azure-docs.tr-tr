---
title: "Beş dakikada Azure portalında bir Umbraco web uygulaması dağıtma | Microsoft Docs"
description: "Örnek ASP.NET uygulamasını dağıtarak, App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin. Sonuçları anında görün."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/10/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: fa3f31cdd708729071876ffad707bea70567da83
ms.lasthandoff: 02/28/2017


---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a>Beş dakikada Azure portalında bir Umbraco web uygulaması dağıtma

Bu öğretici, [Azure App Service](../app-service/app-service-value-prop-what-is.md)'te dakikalar içinde bir [Umbraco](https://our.umbraco.org/) web uygulaması dağıtmanıza yardımcı olur.

![Umbraco uygulaması](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Ön koşullar
Bir Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](https://azure.microsoft.com/try/app-service/). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 

## <a name="deploy-the-aspnet-app"></a>ASP.NET uygulamasını dağıtma
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS) adresini açın.

    Bu bağlantı, Azure portalında yeni bir Umbraco uygulaması yapılandırma sayfasına hemen geçmenizi sağlayan bir kısayoldur.

3. **Uygulama adı** bölümünde bir web uygulaması adı girin. Girdiğiniz ad `azurewebsites.net` etki alanında benzersizse kutuda yeşil renkli bir onay işareti göreceksiniz.
   
5. **Kaynak Grubu**'nda **Yeni oluştur**'a tıklayarak yeni bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun ve ad verin.

7. **App Service planı/Konum** > **Yeni Oluştur**'a tıklayın. [App Service planını](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) gösterilen şekilde yapılandırın:

    - **App Service planı** alanına istediğiniz adı girin.
    - **Konum** alanında planınızın barındırılacağı konumu seçin.
    - **Fiyatlandırma katmanı**'na tıklayıp **F1 Ücretsiz** veya uygun bulduğunuz başka bir katmanı seçin ve **Seç**'e tıklayın.
    - **Tamam** düğmesine tıklayın.

    Umbraco CMS yapılandırmanız aşağıdaki ekran görüntüsündeki gibi olmalıdır:

    ![Yapılandırma devam ediyor - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. **SQL Veritabanı** > **Yeni veritabanı oluştur**'a tıklayın. SQL Veritabanını gösterilen şekilde yapılandırın:

    - **Ad** alanına **myDB** gibi bir ad girin.
    - **Fiyatlandırma katmanı**'na tıklayıp **F Ücretsiz** veya uygun bulduğunuz başka bir katmanı seçin ve **Seç**'e tıklayın.
    - **Hedef sunucu** > **Yeni sunucu oluştur**'a tıklayın. Veritabanı sunucusunu gösterilen şekilde yapılandırın:

        - **Sunucu adı** alanına bir sunucu adı yazın. Girdiğiniz ad `.database.windows.net` etki alanında benzersizse kutuda yeşil renkli bir onay işareti göreceksiniz.
        - **Sunucu yöneticisi oturum açma** alanına istediğiniz yönetici kullanıcı adını girin.
        - **Parola** ve **Parola onayı** alanlarına istediğiniz parolayı girin.
        - Konum alanında web uygulaması için seçtiğiniz konumu seçin.
        - **Azure hizmetlerinin sunucuya erişmesine izin ver** seçeneğinin işaretlenmiş olduğundan emin olun.
        - **Seç**'e tıklayın.
    
    - **Seç**'e tıklayın.

13. **Web uygulaması ayarları**'na tıklayın, veritabanı kullanıcı adını ve parolasını belirtin ve **Tamam**'a tıklayın.

    Umbraco CMS yapılandırmanız aşağıdaki ekran görüntüsündeki gibi olmalıdır:

    ![Yapılandırma tamamlandı - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. **Oluştur**’a tıklayın.
    
    Azure şimdi yapılandırmanızı temel alarak Umbraco uygulamanızı oluşturur. **Dağıtım başlatıldı...** bildirimini görmeniz gerekir.

    ![Dağıtım başarılı - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Umrbaco web uygulamanızı başlatma ve yönetme

Azure uygulama dağıtımını tamamladığında başka bir bildirim daha göreceksiniz.

![Dağıtım başarılı - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Bildirime tıklayın. Bildirimi kaçırırsanız çan simgesine tıklayarak erişebilirsiniz (![Bildirim - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Şimdi web uygulamasının yönetim [dikey penceresini](../azure-resource-manager/resource-group-portal.md#manage-resources) (*dikey pencere*: dikey olarak açılan portal sayfası) görmeniz gerekir.

3. Genel bakış sayfasının en üstünde yer alan **Göz at**'a tıklayın.
   
    ![Göz at - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Şimdi Umbraco **Karşılama** sayfasını görmeniz gerekir. Umbraco yüklemesini yapılandırın ve kurcalamaya başlayın!

    ![Umbraco yapılandırması - Azure App Service’teki ilk Umbraco](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio'yu kullanarak Azure App Service’e bir ASP.NET Web uygulaması dağıtma](web-sites-dotnet-get-started.md) - Mevcut uygulama şablonlarından birini kullanarak Visual Studio'dan yeni bir Azure web uygulaması oluşturmayı öğrenin.
* [Kodunuzu Azure App Service’e dağıtma](web-sites-deploy.md): FTP veya kaynak denetim depolarından dağıtım yapmayı öğrenin.
* [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md): Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin.

