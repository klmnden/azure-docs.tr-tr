---
title: "İlk ASP.NET web uygulamanızı Azure’a beş dakikada dağıtma | Microsoft Belgeleri"
description: "Örnek ASP.NET uygulamasını dağıtarak, App Service’te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin. Hızlı bir şekilde gerçek geliştirmeler yapmaya başlayın ve sonuçlarını anında görün."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 0458cc3b-b630-4632-8cd0-76e2ed96498b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 05e61d2fc751c4239aef4b10ad897765c59fe928
ms.openlocfilehash: 3fe44c37bee330f06b3e41b7bdb89138372c0c7a


---
# <a name="deploy-your-first-aspnet-web-app-to-azure-in-five-minutes"></a>İlk ASP.NET web uygulamanızı Azure’a beş dakikada dağıtma

> [!div class="op_single_selector"]
> * [İlk HTML sitesi](app-service-web-get-started-html-cli-nodejs.md)
> * [İlk .NET uygulaması](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [İlk PHP uygulaması](app-service-web-get-started-php-cli-nodejs.md)
> * [İlk Node.js uygulaması](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [İlk Python uygulaması](app-service-web-get-started-python-cli-nodejs.md)
> * [İlk Java uygulaması](app-service-web-get-started-java.md)
> 
> 

Bu öğretici, [Azure App Service](../app-service/app-service-value-prop-what-is.md)’e basit bir ASP.NET web uygulaması dağıtmanıza yardımcı olur.
Web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için App Service kullanabilirsiniz.

Yapacaklarınız: 

* Azure Uygulama Hizmeti'nde bir web uygulaması oluşturun.
* Örnek ASP.NET kodunu dağıtın.
* Kodunuzun üretim ortamında dinamik bir şekilde çalıştığını görün.
* Web uygulamanızı [Git yürütmelerini gönderdiğiniz](https://git-scm.com/docs/git-push) şekilde güncelleştirin.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](app-service-web-get-started-dotnet-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız
- [Azure CLI 2.0 (Önizleme)](app-service-web-get-started-dotnet.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI’mız

## <a name="prerequisites"></a>Ön koşullar
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI](../xplat-cli-install.md).
* Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](http://go.microsoft.com/fwlink/?LinkId=523751). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 

## <a name="deploy-an-aspnet-web-app"></a>ASP.NET web uygulaması dağıtma
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

4. Çalışma dizinine (`CD`) geçin ve örnek ASP.NET uygulamasını kopyalayın.
   
        git clone https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git

5. Örnek uygulamanızın deposuna geçin. 
   
        cd app-service-web-dotnet-get-started

6. Azure’da Uygulama Hizmeti uygulama kaynağını benzersiz uygulama adıyla ve daha önce yapılandırdığınız dağıtım kullanıcısıyla oluşturun. Sorulduğunda tercih edilen bölgenin numarasını belirtin.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![Azure’da ilk web uygulamanız için Azure kaynağını oluşturma](./media/app-service-web-get-started-languages/dotnet-site-create.png)
   
    Uygulamanız artık Azure’da oluşturulmuştur. Ayrıca geçerli dizininiz Git ile başlatılmıştır ve yeni App Service uygulamasına Git remote olarak bağlanmıştır.
    Varsayılan HTML sayfasını görmek için uygulama URL’sine göz atabilirsiniz (http://&lt;uygulama_adı>.azurewebsites.net), ancak şimdi kodlarınızı buraya yerleştirmeye bakalım.

7. Örnek kodunuzu, Git ile herhangi bir kodu gönderir gibi Azure uygulamanıza dağıtın. İstendiğinde önceden yapılandırdığınız parolayı kullanın.
   
        git push azure master
   
    ![Azure’da ilk web uygulamanıza kod gönderme](./media/app-service-web-get-started-languages/dotnet-git-push.png)
   
    `git push`Azure'a kodu yerleştirmenin yanı sıra gerekli paketleri geri yükler ve ASP.NET ikili değerlerini oluşturur. 

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
Visual Studio'da .NET web uygulamaları oluşturma, geliştirme ve bu uygulamaları doğrudan Azure'da dağıtma hakkında daha fazla bilgi edinmek için bkz. [Visual Studio kullanarak Azure Uygulama Hizmeti'nde ASP.NET web uygulaması dağıtma](web-sites-dotnet-get-started.md).

Veya ilk web uygulamanızla daha fazlasını yapın. Örnek:

* [Kodunuzu Azure'a dağıtmanın diğer yollarını](web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
* Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin. Bkz. [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md).




<!--HONumber=Jan17_HO1-->


