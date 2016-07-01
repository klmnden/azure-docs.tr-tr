<properties
    pageTitle="Azure Marketi’nde bir web uygulaması oluşturma | Microsoft Azure"
    description="Azure Portal'ı kullanarak Azure Marketi'nde yeni bir WordPress web uygulaması oluşturmayı öğrenin."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/10/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# Azure Marketi’nde bir web uygulaması oluşturma

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketi, Microsoft, üçüncü taraf şirketler ve açık kaynak yazılım girişimleri tarafından geliştirilen çok çeşitli popüler web uygulamalarını kullanıma sunar. Örneğin, WordPress, Umbraco CMS, Drupal vb. Bu web uygulamaları birçok popüler uygulamayı temel alarak oluşturulur, bu WordPress örneğinde [PHP], [.NET], [Node.js], [Java] ve [Python] bunlardan yalnızca birkaçıdır. Azure Marketi'nde bir web uygulaması oluşturmak için ihtiyacınız olan tek yazılım [Azure Portal] için kullandığınız tarayıcıdır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

* Azure Marketi'nde bir uygulama şablonunu bulma.
* Azure App Service’te şablon temelli bir web uygulaması oluşturma.
* Yeni web uygulaması ve veritabanı için Azure App Service ayarlarını yapılandırma.

Bu öğreticinin amacı doğrultusunda, Azure Marketi'nden bir WordPress blog sitesi dağıtacaksınız. Bu öğreticideki adımları tamamladığınızda, bulutta bulunan ve çalışan kendi WordPress sitenize sahip olacaksınız.

![Örnek WordPress web uygulaması panosu][WordPressDashboard]

Bu öğreticide dağıtacağınız WordPress sitesi veritabanı için MySQL kullanır. Bunun yerine veritabanı için SQL Database’i kullanmak istiyorsanız, yine Azure Marketi’nde bulunan [Proje Nami]’ye bakın.

> [AZURE.NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, şunları yapabilirsiniz [Visual Studio abone avantajlarınızı etkinleştirebilir][etkinleştirme] veya [ücretsiz deneme için kaydolabilirsiniz][ücretsiz deneme sürümül].
>
> Bir Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin]’e gidin. Burada, App Service’te hemen bir kısa süreli başlangıç web uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur

## WordPress’i seçme ve Azure App Service için yapılandırma

1. [Azure Portal]’da oturum açın.

1. **Yeni**’ye tıklayın.
    
    ![Yeni bir Azure kaynağı oluşturma][MarketplaceStart]
    
1. **WordPress**’i arayın ve ardından **WordPress**’e tıklayın. (MySQL yerine SQL Database kullanmak isterseniz, **Proje Nami** için arama yapın.)

    ![Market’te WordPress’i arama][MarketplaceSearch]
    
1. WordPress uygulaması açıklamasını okuduktan sonra, **Oluştur**’a tıklayın.

    ![WordPress web uygulaması oluşturma][MarketplaceCreate]

1. Aşağıdaki adımları tamamlamak için kullanacağını, WordPress ayarları dikey penceresi görüntülenir:

    ![WordPress web uygulaması ayarlarını yapılandırma][ConfigStart]

1. Web uygulaması için **Web uygulaması** kutusuna bir ad girin.

    Web uygulamasının URL’si *{ad}*.azurewebsites.net şeklinde olacağından, bu ad azurewebsites.net etki alanında benzersiz olmalıdır. Girdiğiniz ad benzersiz değilse, metin kutusunda kırmızı bir ünlem işareti görüntülenir.

    ![WordPress web uygulaması adını yapılandırma][ConfigAppName]

1. Birden fazla aboneliğiniz varsa, kullanmak istediğinizi seçin. 

    ![Web uygulaması için aboneliği yapılandırma][ConfigSubscription]

1. Bir **Kaynak Grubu** seçin veya yeni bir tane oluşturun.

    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Azure Portal’ı kullanma][ResourceGroups].

    ![Web uygulaması için kaynak grubunu yapılandırma][ConfigResourceGroup]

1. Bir **App Service planı/Konumu** seçin veya yeni bir tane oluşturun.

    App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına genel bakış][AzureAppServicePlans]. 

    ![Web uygulaması için hizmet planını yapılandırma][ConfigServicePlan]

1. **Veritabanı**’na tıklayın ve ardından **Yeni MySQL Database** dikey penceresinde, MySQL Database’inizi yapılandırmak için gereken değerleri belirtin.

    a. Yeni bir ad girin veya varsayılan adı bırakın.

    b. **Veritabanı Türü**’nü **Paylaşılan** olarak bırakın.

    c. Web uygulaması için seçtiğinizle aynı konumu seçin.

    d. Bir fiyatlandırma katmanı seçin. Minimum bağlantı ve disk alanıyla ücretsiz olan **Mercury** bu öğretici için uygundur.

    e. **Yeni MySQL Database** dikey penceresinde, yasal koşulları kabul edin ve ardından **Tamam**’a tıklayın. 

    ![Web uygulaması için veritabanı ayarlarını yapılandırma][ConfigDatabase]

1. **WordPress** dikey penceresinde, yasal koşulları kabul edin ve ardından **Oluştur**’a tıklayın. 

    ![Web uygulaması ayarlarını tamamlayın ve Tamam'a tıklayın][ConfigFinished]

    Azure App Service web uygulamasını, genellikle bir dakikadan az süre içinde oluşturur. Portal sayfasının üst kısmındaki zil simgesine tıklayarak ilerleme durumunu izleyebilirsiniz.

    ![İlerleme göstergesi][ConfigProgress]

## WordPress web uygulamanızı başlatma ve yönetme
    
1. Web uygulaması oluşturma tamamlandığında, Azure Portal’da uygulamayı oluşturduğunuz kaynak grubuna gidin, burada web uygulamasını ve veritabanını görebilirsiniz.

    Ampul simgeli ek kaynak, web hizmetiniz için izleme hizmetleri sağlayan [Application Insights][ApplicationInsights]’dır.

1. **Kaynak grubu** dikey penceresinde, web uygulaması satırına tıklayın.

    ![WordPress web uygulamanızı seçin][WordPressSelect]

1. Web uygulaması dikey penceresinde, **Gözat**’a tıklayın.

    ![WordPress web app uygulamanıza gözatma][WordPressBrowse]

1. WordPress blogunuz için dili seçmeniz istenirse, istediğiniz dili seçin ve ardından **Devam**’a tıklayın.

    ![WordPress web uygulamanız için dili yapılandırma][WordPressLanguage]

1. WordPress **Karşılama** sayfasında, WordPress için gereken yapılandırma bilgilerini girin ve ardından **WordPress’i Yükle**’ye tıklayın.

    ![WordPress web uygulamanız için ayarları yapılandırma][WordPressConfigure]

1. **Karşılama** sayfasında oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.  

1. Site Pano sayfanız açılır ve, verdiğiniz bilgiler görüntülenir.    

    ![WordPress panonuzu görüntüleme][WordPressDashboard]

## Sonraki adımlar

Bu öğreticide, Azure Marketi'nde örnek bir web uygulaması oluşturmayı dağıtmayı gördünüz.

App Service Web Apps ile çalışma hakkında daha fazla bilgi için sayfanın sol tarafındaki (geniş tarayıcı pencereleri için) veya sayfanın üst kısmındaki (dar tarayıcı pencereleri için) bağlantılara bakın.

Azure’da WordPress web uygulamaları geliştirme hakkında daha fazla bilgi için bkz. [Azure App Service’te WordPress Geliştirme ][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[etkinleştirme]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[ücretsiz deneme sürümü]: https://azure.microsoft.com/pricing/free-trial/
[App Service’i Deneyin]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../azure-portal/resource-group-portal.md
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
[WordPressDashboard]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png



<!--HONumber=Jun16_HO2-->


