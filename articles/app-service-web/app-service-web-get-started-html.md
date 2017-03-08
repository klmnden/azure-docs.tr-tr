---
title: "Beş dakika içinde Azure’a ilk HTML web uygulamanızı dağıtma (CLI 2.0 Önizleme) | Microsoft Docs"
description: "Örnek bir uygulama dağıtarak App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin. Hızlı bir şekilde gerçek geliştirmeler yapmaya başlayın ve sonuçlarını anında görün."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7bc52251f2d0a6aca271bd3d013690bdd0d6b752
ms.lasthandoff: 02/28/2017


---
# <a name="deploy-your-first-html-web-app-to-azure-in-five-minutes-cli-20-preview"></a>Beş dakika içinde Azure’a ilk HTML web uygulamanızı dağıtma (CLI 2.0 Önizleme)
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Bu öğretici, [Azure Uygulama Hizmeti](../app-service/app-service-value-prop-what-is.md)'nde basit bir HTML+CSS web uygulaması dağıtmanıza yardımcı olur.
Web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için App Service kullanabilirsiniz.

Yapacaklarınız: 

* Azure Uygulama Hizmeti'nde bir web uygulaması oluşturun.
* Bu web uygulamasına HTML ve CSS dağıtın.
* Sayfalarınızı üretim ortamında çalışırken görüntüleyin.
* İçeriğinizi, [Git yürütmelerini gönderdiğiniz](https://git-scm.com/docs/git-push) şekilde güncelleştirebilirsiniz.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](app-service-web-get-started-html-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız
- [Azure CLI 2.0](app-service-web-get-started-html.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="prerequisites"></a>Ön koşullar
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI 2.0 Önizleme](/cli/azure/install-az-cli2).
* Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](https://azure.microsoft.com/try/app-service/). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 

## <a name="deploy-a-simple-html-site"></a>Basit bir HTML sitesi dağıtma
1. Yeni bir Windows komut istemi, PowerShell penceresi, Linux kabuğu veya OS X terminali açın. Makinenizde Git ve Azure CLI’nın yüklü olduğunu doğrulamak için `git --version` ve `azure --version` çalıştırın.
   
    ![Azure’da ilk web uygulamanız için CLI araçlarının test yüklemesi](./media/app-service-web-get-started-languages/1-test-tools-2.0.png)
   
    Araçları yüklemediyseniz, indirme bağlantıları için bkz. [Ön koşullar](#Prerequisites).
2. Azure’da aşağıdaki gibi oturum açın:
   
        az login
   
    Oturum açma işlemine devam etmek için yardım iletisini izleyin.
   
    ![İlk web uygulamanızı oluşturmak için Azure’da oturum açma](./media/app-service-web-get-started-languages/3-azure-login-2.0.png)

3. App Service için dağıtım kullanıcısını ayarlayın. Kodu daha sonra bu kimlik bilgilerini kullanarak dağıtacaksınız.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Yeni bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Bu ilk App Service öğreticisi için bunun ne olduğunu bilmeniz gerekmez.

        az group create --location "<location>" --name my-first-app-group

    `<location>` için hangi olası değerleri kullanabileceğinizi görmek için `az appservice list-locations` CLI komutunu kullanın.

3. Yeni bir "ÜCRETSİZ" [App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oluşturun. App Service’e yönelik bu ilk öğreticide, bu plandaki web uygulamaları için ücret ödemeniz gerekmeyecektir.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. `<app_name>` içinde benzersiz bir ada sahip yeni bir web uygulaması oluşturun.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Ardından, dağıtmak istediğiniz örnek HTML sitesini alın. Çalışan bir dizine (`CD`) geçin ve örnek uygulamayı şu şekilde kopyalayın:
   
        cd <working_directory>
        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

5. Örnek uygulamanızın deposuna geçin. 
   
        cd app-service-web-html-get-started
5. Şu komutla App Service web uygulamanıza yönelik yerel Git dağıtımını yapılandırın:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    Buna benzer bir JSON çıktısı alırsınız ve bu çıktı uzak Git deposunun yapılandırıldığı anlamına gelir:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. JSON'daki URL'yi yerel deponuz (basit olması açısından `azure` diyelim) için bir uzak Git deposu olarak ekleyin.

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Örnek kodunuzu, Git ile herhangi bir kodu gönderir gibi Azure uygulamanıza dağıtın. İstendiğinde önceden yapılandırdığınız parolayı kullanın.
   
        git push azure master
   
    ![Azure’da ilk web uygulamanıza kod gönderme](./media/app-service-web-get-started/5-push-code.png)
   
    Dil altyapılarından birini kullandıysanız farklı bir çıktı göreceksiniz. Bunun nedeni `git push` Azure’a kod yerleştirmekle kalmaz, aynı zamanda dağıtım altyapısında dağıtım görevlerini tetikler. Proje (depo) kökünde package.json (Node.js) veya requirements.txt (Python) dosyaları bulunuyorsa ya da ASP.NET projenizde packages.config dosyası bulunuyorsa, dağıtım betiği sizin için gerekli paketleri geri yükler. Bununla birlikte PHP uygulamanızda composer.json dosyalarını otomatik olarak işlemek için [Composer uzantısını etkinleştirebilirsiniz](web-sites-php-mysql-deploy-use-git.md#composer).

Tebrikler, Azure App Service’ize uygulamanızı dağıttınız.

## <a name="see-your-app-running-live"></a>Uygulamanızı çalışırken görme
Azure’da uygulamanızı çalışırken görmek için deponuzun herhangi bir dizininden bu komutu çalıştırın:

    azure site browse

## <a name="make-updates-to-your-app"></a>Uygulamanızda güncelleştirmeler yapma
Artık canlı sitede bir güncelleştirme yapmak için projenizin (depo) kökünden gönderme yapmak üzere Git’i kullanabilirsiniz. Kodunuzu ilk kez dağıtırken de bu yolu izlersiniz. Örneğin yerel olarak test ettiğiniz yeni bir değişikliği her göndermek istediğinizde tek yapmanız gereken projenizin (depo) kökünden aşağıdaki komutları çalıştırmaktır:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Sonraki adımlar
Dil altyapınız için tercih edilen geliştirme ve dağıtım adımlarını bulun:

* [.NET](web-sites-dotnet-get-started.md)
* [PHP](app-service-web-php-get-started.md)
* [Node.js](app-service-web-nodejs-get-started.md)
* [Python](web-sites-python-ptvs-django-mysql.md)
* [Java](web-sites-java-get-started.md)

Veya ilk web uygulamanızla daha fazlasını yapın. Örnek:

* [Kodunuzu Azure'a dağıtmanın diğer yollarını](web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
* Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin. Bkz. [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md).


