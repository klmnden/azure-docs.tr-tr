---
title: "Beş dakikada Azure portalında bir WordPress uygulaması dağıtma | Microsoft Docs"
description: "Bir WordPress uygulaması dağıtarak App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin. Sonuçları anında görün."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/13/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7ef5802866bf96859d3f44cdb58cbb412b08a700
ms.lasthandoff: 02/28/2017


---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a>Beş dakikada Azure portalında bir WordPress uygulaması dağıtma

Bu öğreticide [Azure App Service](../app-service/app-service-value-prop-what-is.md)'e ilk [WordPress](https://wordpress.org/) web uygulamanızı nasıl dağıtacağınız gösterilmiştir.

![WordPress sitesi](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Ön koşullar
Bir Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](https://azure.microsoft.com/try/app-service/). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 

## <a name="deploy-the-wordpress-app"></a>WordPress uygulamasını dağıtma
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress) adresini açın.

    Bu bağlantı, Azure portalında yeni bir WordPress uygulaması yapılandırma sayfasına hemen geçmenizi sağlayan bir kısayoldur.

3. **Uygulama adı** bölümünde bir web uygulaması adı girin. Girdiğiniz ad `azurewebsites.net` etki alanında benzersizse kutuda yeşil renkli bir onay işareti göreceksiniz.
   
5. **Kaynak Grubu**'nda **Yeni oluştur**'a tıklayarak yeni bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun ve ad verin.

6. **Veritabanı Sağlayıcısı**'nda **CleaDB**'yi seçin.

7. **App Service planı/Konum** > **Yeni Oluştur**'a tıklayın. [App Service planını](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) gösterilen şekilde yapılandırın:

    - **App Service planı** alanına istediğiniz adı girin.
    - **Konum** alanında planınızın barındırılacağı konumu seçin.
    - **Fiyatlandırma katmanı**'na tıklayıp **F1 Ücretsiz** veya uygun bulduğunuz başka bir katmanı seçin ve **Seç**'e tıklayın.
    - **Tamam** düğmesine tıklayın.

8. **Veritabanı** > **Yeni Oluştur**'a tıklayın. SQL Veritabanını gösterilen şekilde yapılandırın:

    - **Veritabanı Adı** alanına bir veritabanı adı yazın. 
    - **Konum** alanında App Service planıyla aynı konumu seçin.
    - **Fiyatlandırma katmanı**'na tıklayıp **Mercury** veya uygun bulduğunuz başka bir katmanı seçin ve **Seç**'e tıklayın.
    - **Yasal Koşullar**'a ve **Satın Al**'a tıklayın.
    - **Tamam** düğmesine tıklayın.

9. **Oluştur**’a tıklayın.

    Azure şimdi yapılandırmanızı temel alarak WordPress uygulamanızı oluşturur. **Dağıtım başlatıldı...** bildirimini görmeniz gerekir.

    ![Dağıtım başladı - Azure App Service’teki ilk WordPress uygulaması](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>WordPress web uygulamanızı başlatma ve yönetme

Azure uygulama dağıtımını tamamladığında başka bir bildirim daha göreceksiniz.

![Dağıtım başarılı - Azure App Service’teki ilk WordPress uygulaması](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Bildirime tıklayın. Bildirimi kaçırırsanız çan simgesine tıklayarak erişebilirsiniz (![Bildirim - Azure App Service’teki ilk WordPress](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Şimdi web uygulamasının yönetim [dikey penceresini](../azure-resource-manager/resource-group-portal.md#manage-resources) (*dikey pencere*: dikey olarak açılan portal sayfası) görmeniz gerekir.

3. Genel bakış sayfasının en üstünde yer alan **Göz at**'a tıklayın.
   
    ![Göz at - Azure App Service’teki ilk WordPress](./media/app-service-web-get-started-php-portal/browse.png)

    Şimdi WordPress **Karşılama** sayfasını görmeniz gerekir. WordPress yüklemesini yapılandırın ve kurcalamaya başlayın!

    ![WordPress yapılandırması - Azure App Service’teki ilk WordPress](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Sonraki adımlar
* [Laravel web uygulaması oluşturma, yapılandırma ve Azure'a dağıtma](app-service-web-php-get-started.md) - Azure'da PHP web uygulaması çalıştırmak için gerekli olan temel becerileri öğrenin, örneğin:

    * Azure’da PowerShell/Bash uygulamaları oluşturma ve yapılandırma.
    * PHP sürümünü ayarlama.
    * Kök uygulama dizininde olmayan bir başlangıç dosyası kullanma.
    * Composer otomasyonunu etkinleştirme.
    * Ortama özel değişkenlere erişme.
    * Sık karşılaşılan hataları giderme.

* [Kodunuzu Azure App Service’e dağıtma](web-sites-deploy.md): FTP veya kaynak denetim depolarından dağıtım yapmayı öğrenin.
* [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md): Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin.

