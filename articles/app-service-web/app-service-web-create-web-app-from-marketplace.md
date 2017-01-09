---
title: "Azure Marketi’nde bir web uygulaması oluşturma | Microsoft Belgeleri"
description: "Azure Portal&quot;ı kullanarak Azure Marketi&quot;nde yeni bir WordPress web uygulaması oluşturmayı öğrenin."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: 972a296d-f927-470b-8534-0f2cb9eac223
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 00c4336bd5cef4ddc0b92127d0945d39291b9c7f


---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Azure Marketi’nde bir web uygulaması oluşturma
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketi, Microsoft, üçüncü taraf şirketler ve açık kaynak yazılım girişimleri tarafından geliştirilen çok çeşitli popüler web uygulamalarını kullanıma sunar. Örneğin, WordPress, Umbraco CMS, Drupal vb. Bu web uygulamaları birçok popüler uygulamayı temel alarak oluşturulur, bu WordPress örneğinde [PHP], [.NET], [Node.js], [Java] ve [Python] bunlardan yalnızca birkaçıdır. Azure Marketi'nde bir web uygulaması oluşturmak için ihtiyacınız olan tek yazılım [Azure Portal] için kullandığınız tarayıcıdır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Azure Uygulama Hizmeti’nde Azure Market şablonunu temel alan bir web uygulaması bulup oluşturun.
* Yeni web uygulaması için Azure Uygulama Hizmeti ayarlarını yapılandırın.
* Web uygulamanızı başlatın ve yönetin.

Bu öğreticinin amacı doğrultusunda, Azure Marketi'nden bir WordPress blog sitesi dağıtacaksınız. Bu öğreticideki adımları tamamladığınızda, bulutta bulunan ve çalışan kendi WordPress sitenize sahip olacaksınız.

![Örnek WordPress web uygulaması panosu][WordPressDashboard1]

Bu öğreticide dağıtacağınız WordPress sitesi veritabanı için MySQL kullanır. Bunun yerine veritabanı için SQL Database’i kullanmak istiyorsanız, yine Azure Marketi’nde bulunan [Proje Nami]’ye bakın.

> [!NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, [Visual Studio abone avantajlarınızı etkinleştirebilir][activate] veya [ücretsiz deneme için kaydolabilirsiniz][free trial].
> 
> Bir Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin]’e gidin. Burada, App Service’te hemen bir kısa süreli başlangıç web uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur
> 
> 

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde Web Uygulaması Bulma ve Oluşturma
1. [Azure Portal]’da oturum açın.
2. **Yeni**’ye tıklayın.
   
    ![Yeni bir Azure kaynağı oluşturma][MarketplaceStart]
3. **WordPress**’i arayın ve ardından **WordPress**’e tıklayın. (MySQL yerine SQL Database kullanmak isterseniz, **Proje Nami** için arama yapın.)
   
    ![Market’te WordPress’i arama][MarketplaceSearch]
4. WordPress uygulaması açıklamasını okuduktan sonra, **Oluştur**’a tıklayın.
   
    ![WordPress web uygulaması oluşturma][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Yeni Web Uygulaması için Azure Uygulama Hizmeti Ayarlarını Yapılandırma
1. Yeni bir web uygulaması oluşturduktan sonra aşağıdaki adımları tamamlamak için kullanacağınız WordPress ayarları dikey penceresi görüntülenir:
   
    ![WordPress web uygulaması ayarlarını yapılandırma][ConfigStart]
2. Web uygulaması için **Web uygulaması** kutusuna bir ad girin.
   
    Web uygulamasının URL’si *{ad}*.azurewebsites.net şeklinde olacağından, bu ad azurewebsites.net etki alanında benzersiz olmalıdır. Girdiğiniz ad benzersiz değilse, metin kutusunda kırmızı bir ünlem işareti görüntülenir.
   
    ![WordPress web uygulaması adını yapılandırma][ConfigAppName]
3. Birden fazla aboneliğiniz varsa, kullanmak istediğinizi seçin.
   
    ![Web uygulaması için aboneliği yapılandırma][ConfigSubscription]
4. Bir **Kaynak Grubu** seçin veya yeni bir tane oluşturun.
   
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış][ResourceGroups].
   
    ![Web uygulaması için kaynak grubunu yapılandırma][ConfigResourceGroup]
5. Bir **App Service planı/Konum** seçin veya yeni bir tane oluşturun.
   
    App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına genel bakış][AzureAppServicePlans].
   
    ![Web uygulaması için hizmet planını yapılandırma][ConfigServicePlan]
6. **Veritabanı**’na tıklayın ve ardından **Yeni MySQL Database** dikey penceresinde, MySQL Database’inizi yapılandırmak için gereken değerleri belirtin.
   
    a. Yeni bir ad girin veya varsayılan adı bırakın.
   
    b. **Veritabanı Türü**’nü **Paylaşılan** olarak bırakın.
   
    c. Web uygulaması için seçtiğinizle aynı konumu seçin.
   
    d. Bir fiyatlandırma katmanı seçin. Minimum bağlantı ve disk alanıyla ücretsiz olan **Mercury** bu öğretici için uygundur.
   
    e. **Yeni MySQL Database** dikey penceresinde, yasal koşulları kabul edin ve ardından **Tamam**’a tıklayın.
   
    ![Web uygulaması için veritabanı ayarlarını yapılandırma][ConfigDatabase]
7. **WordPress** dikey penceresinde, yasal koşulları kabul edin ve ardından **Oluştur**’a tıklayın.
   
    ![Web uygulaması ayarlarını tamamlayın ve Tamam'a tıklayın][ConfigFinished]
   
    Azure App Service web uygulamasını, genellikle bir dakikadan az süre içinde oluşturur. Portal sayfasının üst kısmındaki zil simgesine tıklayarak ilerleme durumunu izleyebilirsiniz.
   
    ![İlerleme göstergesi][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>WordPress web uygulamanızı başlatma ve yönetme
1. Web uygulaması oluşturma tamamlandığında, Azure Portal’da uygulamayı oluşturduğunuz kaynak grubuna gidin, burada web uygulamasını ve veritabanını görebilirsiniz.
   
    Ampul simgeli ek kaynak, web hizmetiniz için izleme hizmetleri sağlayan [Application Insights][ApplicationInsights] hizmetidir.
2. **Kaynak grubu** dikey penceresinde, web uygulaması satırına tıklayın.
   
    ![WordPress web uygulamanızı seçin][WordPressSelect]
3. Web uygulaması dikey penceresinde, **Gözat**’a tıklayın.
   
    ![WordPress web app uygulamanıza gözatma][WordPressBrowse]
4. WordPress blogunuz için dili seçmeniz istenirse, istediğiniz dili seçin ve ardından **Devam**’a tıklayın.
   
    ![WordPress web uygulamanız için dili yapılandırma][WordPressLanguage]
5. WordPress **Karşılama** sayfasında, WordPress için gereken yapılandırma bilgilerini girin ve ardından **WordPress’i Yükle**’ye tıklayın.
   
    ![WordPress web uygulamanız için ayarları yapılandırma][WordPressConfigure]
6. **Karşılama** sayfasında oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.  
7. Site Pano sayfanız açılır ve, verdiğiniz bilgiler görüntülenir.    
   
    ![WordPress panonuzu görüntüleme][WordPressDashboard2]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Marketi'nde örnek bir web uygulaması oluşturmayı dağıtmayı gördünüz.

App Service Web Apps ile çalışma hakkında daha fazla bilgi için sayfanın sol tarafındaki (geniş tarayıcı pencereleri için) veya sayfanın üst kısmındaki (dar tarayıcı pencereleri için) bağlantılara bakın.

Azure'da WordPress web uygulamaları geliştirme hakkında daha fazla bilgi için bkz. [Azure App Service'te WordPress Geliştirme][WordPressOnAzure].

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[App Service’i Deneyin]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Proje Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png



<!--HONumber=Dec16_HO1-->


