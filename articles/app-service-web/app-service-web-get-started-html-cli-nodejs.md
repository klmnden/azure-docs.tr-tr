---
title: "İlk HTML web uygulamanızı Azure’da beş dakikada dağıtma | Microsoft Docs"
description: "Örnek bir uygulama dağıtarak App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin. Hızlı bir şekilde gerçek geliştirmeler yapmaya başlayın ve sonuçlarını anında görün."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 27c50be7-421a-47c9-8279-506519e404a4
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: d24e78a826de9fb72e0eedc48877c83e867f70ca
ms.openlocfilehash: 93b9ec270ffc69fcfde56bcc56c8953024133fcd
ms.lasthandoff: 02/28/2017


---
# <a name="deploy-your-first-html-web-app-to-azure-in-five-minutes"></a>Beş dakika içinde Azure’a ilk HTML web uygulamanızı dağıtma
[!INCLUDE [app-service-web-selector-get-started-nodejs-cli](../../includes/app-service-web-selector-get-started-nodejs-cli.md)]  

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
* [Azure CLI](../xplat-cli-install.md).
* Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](https://azure.microsoft.com/try/app-service/). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 

## <a name="deploy-a-simple-html-site"></a>Basit bir HTML sitesi dağıtma
1. Yeni bir Windows komut istemi, PowerShell penceresi, Linux kabuğu veya OS X terminali açın. Makinenizde Git ve Azure CLI’nın yüklü olduğunu doğrulamak için `git --version` ve `azure --version` çalıştırın.
   
    ![Azure’da ilk web uygulamanız için CLI araçlarının test yüklemesi](./media/app-service-web-get-started/1-test-tools.png)
   
    Araçları yüklemediyseniz, indirme bağlantıları için bkz. [Ön koşullar](#Prerequisites).
2. Azure’da aşağıdaki gibi oturum açın:
   
        azure login
   
    Oturum açma işlemine devam etmek için yardım iletisini izleyin.
   
    ![İlk web uygulamanızı oluşturmak için Azure’da oturum açma](./media/app-service-web-get-started/3-azure-login.png)

3. Azure CLI’yi ASM moduna alın ve Uygulama Hizmeti için dağıtım kullanıcısını ayarlayın. Kodu daha sonra bu kimlik bilgilerini kullanarak dağıtacaksınız.
   
        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

4. Çalışan bir dizine (`CD`) geçin ve örnek HTML sitesini şu şekilde kopyalayın:
   
        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

5. Örnek uygulamanızın deposuna geçin. 
   
        cd app-service-web-html-get-started

6. Azure’da Uygulama Hizmeti uygulama kaynağını benzersiz uygulama adıyla ve daha önce yapılandırdığınız dağıtım kullanıcısıyla oluşturun. Sorulduğunda tercih edilen bölgenin numarasını belirtin.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![Azure’da ilk web uygulamanız için Azure kaynağını oluşturma](./media/app-service-web-get-started/4-create-site.png)
   
    Uygulamanız artık Azure’da oluşturulmuştur. Ayrıca geçerli dizininiz Git ile başlatılmıştır ve yeni App Service uygulamasına Git remote olarak bağlanmıştır.
    Varsayılan HTML sayfasını görmek için uygulama URL’sine göz atabilirsiniz (http://&lt;uygulama_adı>.azurewebsites.net), ancak şimdi kodlarınızı buraya yerleştirmeye bakalım.
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
* [PHP](app-service-web-php-get-started-cli-nodejs.md)
* [Node.js](app-service-web-nodejs-get-started-cli-nodejs.md)
* [Python](web-sites-python-ptvs-django-mysql.md)
* [Java](web-sites-java-get-started.md)

Veya ilk web uygulamanızla daha fazlasını yapın. Örnek:

* [Kodunuzu Azure'a dağıtmanın diğer yollarını](web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
* Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin. Bkz. [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md).


