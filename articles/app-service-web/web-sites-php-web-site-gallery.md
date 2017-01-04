---
title: "Azure App Service’te WordPress web uygulaması oluşturma | Microsoft Belgeleri"
description: "Azure Portal&quot;ı kullanarak WordPress blogu için yeni bir Azure web uygulaması oluşturmayı öğrenin."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: hero-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f48aab02e0229c440848dc8e4a3d26d0d43d96aa


---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a>Azure App Service’te bir WordPress web uygulaması oluşturma
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğreticide Azure Market’ten bir WordPress blog sitesinin nasıl dağıtılacağı gösterilmiştir.

Bu öğreticiyi tamamladığınızda, bulutta bulunan ve çalışan kendi WordPress blog sitenize sahip olacaksınız.

![WordPress sitesi](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Şunları öğreneceksiniz:

* Azure Marketi'nde bir uygulama şablonunu bulma.
* Azure App Service'te şablon temelli bir web uygulaması oluşturma.
* Yeni web uygulaması ve veritabanı için Azure App Service ayarlarını yapılandırma.

Azure Marketi, Microsoft, üçüncü taraf şirketler ve açık kaynak yazılım girişimleri tarafından geliştirilen çok çeşitli popüler web uygulamalarını kullanıma sunar. Web uygulamaları birçok popüler uygulamayı temel alarak oluşturulur, bu WordPress örneğinde [PHP](/develop/nodejs/), [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/) ve [Python](/develop/python/) bunlardan yalnızca birkaçıdır. Azure Marketi'nde bir web uygulaması oluşturmak için ihtiyacınız olan tek yazılım [Azure Portal](https://portal.azure.com/) için kullandığınız tarayıcıdır. 

Bu öğreticide dağıttığınız WordPress sitesi veritabanı için MySQL kullanır. Bunun yerine veritabanı için SQL Database’i kullanmak istiyorsanız, bkz: [Project Nami](http://projectnami.org/). **Project Nami** Market üzerinden de kullanılabilir.

> [!NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, [Visual Studio abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Bir Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751)’e gidin. Burada, App Service'te hemen bir kısa süreli başlangıç web uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur.
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a>WordPress’i seçme ve Azure App Service için yapılandırma
1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. **Yeni**’ye tıklayın.
   
    ![Yeni Oluştur][5]
3. **WordPress**’i arayın ve ardından **WordPress**’e tıklayın. (MySQL yerine SQL Database kullanmak isterseniz, **Project Nami** için arama yapın.)
   
    ![Listede WordPress][7]
4. WordPress uygulaması açıklamasını okuduktan sonra, **Oluştur**’a tıklayın.
   
    ![Oluştur](./media/web-sites-php-web-site-gallery/create.png)
5. Web uygulaması için **Web uygulaması** kutusuna bir ad girin.
   
    Web uygulamasının URL’si {ad}.azurewebsites.net şeklinde olacağından, bu ad azurewebsites.net etki alanında benzersiz olmalıdır. Girdiğiniz ad benzersiz değilse, metin kutusunda kırmızı bir ünlem işareti gösterilir.
6. Birden fazla aboneliğiniz varsa, kullanmak istediğinizi seçin. 
7. Bir **Kaynak Grubu** seçin veya yeni bir tane oluşturun.
   
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).
8. Bir **App Service planı/Konum** seçin veya yeni bir tane oluşturun.
   
    App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)    
9. **Veritabanı**’na tıklayın ve ardından **Yeni MySQL Veritabanı** dikey penceresinde, MySQL veritabanınızı yapılandırmak için gereken değerleri belirtin.
   
    a. Yeni bir ad girin veya varsayılan adı bırakın.
   
    b. **Veritabanı Türü**’nü **Paylaşılan** olarak bırakın.
   
    c. Web uygulaması için seçtiğinizle aynı konumu seçin.
   
    d. Bir fiyatlandırma katmanı seçin. Minimum izin verilen bağlantı ve disk alanıyla ücretsiz olan Mercury bu öğretici için uygundur.
10. **Yeni MySQL veritabanı** dikey penceresinde **Tamam**’a tıklayın. 
11. **WordPress** dikey penceresinde, yasal koşulları kabul edin ve ardından **Oluştur**’a tıklayın. 
    
     ![Web uygulaması yapılandırma](./media/web-sites-php-web-site-gallery/configure.png)
    
     Azure App Service web uygulamasını, genellikle bir dakikadan az süre içinde oluşturur. Portal sayfasının üst kısmındaki zil simgesine tıklayarak ilerleme durumunu izleyebilirsiniz.
    
     ![İlerleme göstergesi](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a>WordPress web uygulamanızı başlatma ve yönetme
1. Web uygulaması oluşturma tamamlandığında, Azure Portal’da uygulamayı oluşturduğunuz kaynak grubuna gidin, burada web uygulamasını ve veritabanını görebilirsiniz.
   
    Ampul simgeli ek kaynak, web hizmetiniz için izleme hizmetleri sağlayan [Application Insights](/services/application-insights/)’dır.
2. **Kaynak grubu** dikey penceresinde, web uygulaması satırına tıklayın.
   
    ![Web uygulaması yapılandırma](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. Web uygulaması dikey penceresinde, **Gözat**’a tıklayın.
   
    ![site URL][browse]
4. WordPress **Karşılama** sayfasında, WordPress için gereken yapılandırma bilgilerini girin ve ardından **WordPress’i Yükle**’ye tıklayın.
   
    ![WordPress’i yapılandırma](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. **Karşılama** sayfasında oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.  
6. Sitenizin Pano sayfası açılır.    
   
    ![WordPress sitesi](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a>Sonraki adımlar
Galeriden bir PHP web uygulamasını nasıl oluşturup dağıtacağınızı gördünüz. Azure’da PHP kullanma hakkında daha fazla bilgi için bkz. [PHP Geliştirici Merkezi](/develop/php/).

App Service Web Apps ile çalışma hakkında daha fazla bilgi için sayfanın sol tarafındaki (geniş tarayıcı pencereleri için) veya sayfanın üst kısmındaki (dar tarayıcı pencereleri için) bağlantılara bakın. 

## <a name="whats-changed"></a>Yapılan değişiklikler
* Web Sitelerinden App Service’e değiştirme kılavuzu için bkz. [Azure App Service ve mevcut Azure Hizmetlerine etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png



<!--HONumber=Jan17_HO1-->


