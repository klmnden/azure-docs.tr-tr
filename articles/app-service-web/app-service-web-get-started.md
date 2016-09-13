<properties
    pageTitle="Beş dakika içinde Azure’a ilk web uygulamanızı dağıtın | Microsoft Azure"
    description="Yalnızca birkaç adımda örnek bir uygulama dağıtarak App Service’te web uygulamalarını çalıştırmanın ne kadar kolay olduğunu öğrenin. Beş dakikada gerçek bir geliştirme yapmaya başlayın ve sonuçları hemen görün."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# Beş dakika içinde Azure’a ilk web uygulamanızı dağıtın

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğretici ilk web uygulamanızı [App Service](../app-service/app-service-value-prop-what-is.md)’e dağıtmanıza yardımcı olur.
Web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için App Service kullanabilirsiniz.

Biraz çaba göstererek:

- Örnek bir web uygulaması dağıtacaksınız (ASP.NET, PHP, Node.js, Java, veya Python arasından seçin).
- Uygulamanızın saniyeler içinde çalıştığını göreceksiniz.
- Web uygulamanızı [Git yürütmelerini gönderdiğiniz](https://git-scm.com/docs/git-push) şekilde güncelleştirin.

Bununla birlikte [Azure Portal](https://portal.azure.com)’a göz atacak ve yer alan özellikleri inceleyeceksiniz.

## Ön koşullar

- [Git’i yükleyin](http://www.git-scm.com/downloads).
- [Azure CLI’yı yükleyin](../xplat-cli-install.md).
- Bir Microsoft Azure hesabı edinin. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

>[AZURE.NOTE] Bir web uygulamasını çalışırken görün. [App Service’i hemen deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) ve kısa ömürlü bir başlangıç uygulaması oluşturun; kredi kartı veya bir taahhüt gerekli değildir.

## Bir web uygulaması dağıtma

Azure App Service’e bir web uygulaması dağıtalım.

1. Yeni bir Windows komut istemi, PowerShell penceresi, Linux kabuğu veya OS X terminali açın. Makinenizde Git ve Azure CLI’nın yüklü olduğunu doğrulamak için `git --version` ve `azure --version` çalıştırın.

    ![Azure’da ilk web uygulamanız için CLI araçlarının test yüklemesi](./media/app-service-web-get-started/1-test-tools.png)

    Araçları yüklemediyseniz, indirme bağlantıları için bkz. [Ön koşullar](#Prerequisites).

1. Çalışan bir dizine (`CD`) geçin ve örnek uygulamayı şu şekilde kopyalayın:

        git clone <github_sample_url>

    ![Azure’da ilk web uygulamanız için uygulama örnek kodunu kopyalama](./media/app-service-web-get-started/2-clone-sample.png)

    *&lt;github_sample_url>* için, tercih ettiğiniz altyapıya bağlı olarak aşağıdaki URL’lerden birini kullanın:

    - HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. Örnek uygulamanızın deposuna geçin. Örneğin:

        cd app-service-web-html-get-started

3. Azure’da aşağıdaki gibi oturum açın:

        azure login

    Oturum açma işlemine devam etmek için yardım iletisini izleyin.

    ![İlk web uygulamanızı oluşturmak için Azure’da oturum açma](./media/app-service-web-get-started/3-azure-login.png)

4. Sonraki komutu kullanarak benzersiz bir uygulama adıyla Azure’da App Service uygulama kaynağını oluşturun. Sorulduğunda tercih edilen bölgenin numarasını belirtin.

        azure site create --git <app_name>

    ![Azure’da ilk web uygulamanız için Azure kaynağını oluşturma](./media/app-service-web-get-started/4-create-site.png)

    >[AZURE.NOTE] Azure aboneliğiniz için daha önce dağıtım kimlik bilgileri oluşturmadıysanız, bunları oluşturmanız istenir. App Service, Azure hesabı kimlik bilgileri yerine bu kimlik bilgilerini yalnızca Git dağıtımları ve FTP oturum açma işlemleri için kullanır.

    Uygulamanız artık Azure’da oluşturulmuştur. Ayrıca geçerli dizininiz Git ile başlatılmıştır ve yeni App Service uygulamasına Git remote olarak bağlanmıştır.
    Varsayılan HTML sayfasını görmek için uygulama URL’sine göz atabilirsiniz (http://&lt;app_name>.azurewebsites.net), ancak şimdi kodlarınızı buraya yerleştirmeye bakalım.

4. Git ile kod gönderdiğiniz gibi örnek kodunuzu yeni App Service’e dağıtın:

        git push azure master

    ![Azure’da ilk web uygulamanıza kod gönderme](./media/app-service-web-get-started/5-push-code.png)    

    Dil altyapılarından birini kullandıysanız farklı bir çıktı göreceksiniz. Bunun nedeni `git push` Azure’a kod yerleştirmekle kalmaz, aynı zamanda dağıtım altyapısında dağıtım görevlerini tetikler. Proje (depo) kökünde package.json (Node.js) veya requirements.txt (Python) dosyaları bulunuyorsa ya da ASP.NET projenizde packages.config dosyası bulunuyorsa, dağıtım betiği sizin için gerekli paketleri geri yükler. Bununla birlikte PHP uygulamanızda composer.json dosyalarını otomatik olarak işlemek için [Composer uzantısını etkinleştirebilirsiniz](web-sites-php-mysql-deploy-use-git.md#composer).

Tebrikler, Azure App Service’ize uygulamanızı dağıttınız.

## Uygulamanızı çalışırken görme

Azure’da uygulamanızı çalışırken görmek için deponuzun herhangi bir dizininden bu komutu çalıştırın:

    azure site browse

## Uygulamanızda güncelleştirmeler yapma

Artık canlı sitede bir güncelleştirme yapmak için projenizin (depo) kökünden gönderme yapmak üzere Git’i kullanabilirsiniz. Bunu, uygulamanızı Azure’a ilk kez dağıtırken yaptığınız gibi yapacaksınız. Örneğin yerel olarak test ettiğiniz yeni bir değişikliği her göndermek istediğinizde projenizin (depo) kökünden aşağıdaki komutları çalıştırın:

    git add .
    git commit -m "<your_message>"
    git push azure master

## Uygulamanızı Azure portalında görme

Şimdi oluşturduğunuzu görmek için Azure Portal’a gidelim:

1. Azure aboneliğiniz olan bir Microsoft hesabıyla [Azure portalında](https://portal.azure.com) oturum açın.

2. Sol çubukta **App Services**’a tıklayın.

3. Oluşturduğunuz uygulamaya tıklayarak uygulamanın sayfasını portalda açın ([dikey pencere](../azure-portal-overview.md) olarak adlandırılır). Size kolaylık sağlamak üzere **Ayarlar** dikey penceresi de varsayılan olarak açılır.

    ![Azure’da ilk web uygulamanızın portal görünümü](./media/app-service-web-get-started/portal-view.png)

App Service uygulamanızın portal dikey penceresinde uygulamanızı yapılandırma, izleme, koruma ve sorunlarını giderme işlemlerini gerçekleştirmek için zengin bir ayar ve araç seti yer alır. Bazı basit görevleri gerçekleştirerek bu arayüzü tanımaya çalışın. (Görevin numarası ekrandaki numaraya karşılık gelir.)

1. Uygulamayı durdurun.
2. Uygulamayı yeniden başlatın.
3. Kaynak grubunda dağıtılan kaynakların tümünü görmek için **Kaynak Grubu** bağlantısına tıklayın.
4. Uygulamanız ile ilgili diğer bilgileri görmek için **Ayarlar** > **Özellikler**’e tıklayın.
5. İzleme ve sorun gidermeyle ilgili kullanışlı araçlara erişmek için **Araçlar**’a tıklayın.  

## Sonraki adımlar

- Azure uygulamanızı bir sonraki seviyeye taşıyın. Kimlik doğrulama ile güvenli hale getirin. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin. Bkz. [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md).
- Git ve Azure CLI dışında Azure’a web uygulaması dağıtmanın farklı yöntemleri vardır. Bkz. [Uygulamanızı Azure App Service’e dağıtma](../app-service-web/web-sites-deploy.md).
Makalenin üst kısmından altyapınızı seçerek dil altyapınız için tercih edilen geliştirme ve geliştirme adımlarını görün.



<!--HONumber=sep16_HO1-->


