<properties 
    pageTitle="5 dakikada Azure’a ilk web uygulamanızı dağıtın" 
    description="Yalnızca birkaç adımda örnek bir uygulama dağıtarak App Service’te web uygulamalarını çalıştırmanın ne kadar kolay olduğunu öğrenin. 5 dakikada gerçekten geliştirme yapmaya başlayın ve sonuçları hemen görün." 
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
    
# 5 dakikada Azure’a ilk web uygulamanızı dağıtın

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğretici ilk web uygulamanızı [App Service](../app-service/app-service-value-prop-what-is.md)’e dağıtmanıza yardımcı olur. App Service; web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmanıza olanak tanır.

Biraz çaba göstererek: 

- Örnek bir web uygulaması dağıtacaksınız (ASP.NET, PHP, Node.js, Java, veya Python arasından seçin).
- Uygulamanızın saniyeler içinde çalıştığını göreceksiniz.
- Web uygulamanızı [Git yürütmelerini gönderdiğiniz](https://git-scm.com/docs/git-push) şekilde güncelleştirin.

Bununla birlikte [Azure Portal](https://portal.azure.com)’a göz atacak ve yer alan özellikleri inceleyeceksiniz. 

## Ön koşullar

- [Git’i yükleyin](http://www.git-scm.com/downloads). 
- [Azure CLI’yı yükleyin](../xplat-cli-install.md). 
- Bir Microsoft Azure hesabı edinin. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

>[AZURE.NOTE] Bir web uygulamasını çalışırken görün. [App Service’i](http://go.microsoft.com/fwlink/?LinkId=523751) hemen deneyin ve kısa ömürlü bir başlangıç uygulaması oluşturun; kredi kartı veya bir taahhüt gerekli değildir.

## Bir web uygulaması dağıtma

Azure App Service’e bir web uygulaması dağıtalım. 

1. Yeni bir Windows komut istemi, PowerShell penceresi, Linux kabuğu veya OS X terminali açın. Makinenizde Git ve Azure CLI’nın yüklü olduğunu doğrulamak için `git --version` ve `azure --version` çalıştırın. 

    ![Azure’da ilk web uygulamanız için CLI araçlarının test yüklemesi](./media/app-service-web-get-started/1-test-tools.png)

    Araçları yüklemediyseniz, indirme bağlantıları için bkz. [Ön koşullar](#Prerequisites).

1. `CD` komutunu kullanarak çalışma dizinine geçin ve örnek uygulamayı şu şekilde kopyalayın:

        git clone <github_sample_url>

    ![Azure’da ilk web uygulamanız için uygulama örnek kodunu kopyalama](./media/app-service-web-get-started/2-clone-sample.png)

    *&lt;github_sample_url>* için, tercih ettiğiniz altyapıya bağlı olarak aşağıdaki URL’lerden birini kullanın: 

    - HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git) 
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. `CD` komutunu kullanarak örnek uygulama deposunun bulunduğu dizine geçin. Örneğin, 

        cd app-service-web-html-get-started

3. Şuna benzer şekilde Azure’da oturum açın:

        azure login
    
    Oturum açma işlemine devam etmek için yardım iletisini izleyin.
    
    ![İlk web uygulamanızı oluşturmak için Azure’da oturum açma](./media/app-service-web-get-started/3-azure-login.png)

4. İleri komutu ile benzersiz bir uygulama adıyla Azure’da App Service uygulama kaynağını oluşturun. İstendiğinde, tercih edilen bölgenin numarasını belirleyin.

        azure site create --git <app_name>
    
    ![Azure’da ilk web uygulamanız için Azure kaynağını oluşturma](./media/app-service-web-get-started/4-create-site.png)
    
    >[AZURE.NOTE] Azure aboneliğiniz için daha önce dağıtım kimlik bilgileri oluşturmadıysanız, bunları oluşturmanız istenir. Azure hesap bilgilerinden farklı olan bu kimlik bilgileri App Service tarafından yalnızca Git dağıtımları ve FTP oturum açma işlemleri için kullanılır. 
    
    Uygulamanız artık Azure’da oluşturulmuştur. Ayrıca geçerli dizininiz Git ile başlatılmıştır ve yeni App Service uygulamasına Git remote olarak bağlanmıştır.
    Varsayılan HTML sayfasını görmek için uygulama URL’sini görmek üzere gezinebilirsiniz (http://&lt;app_name>.azurewebsites.net), ancak şimdi kodlarınızı buraya yerleştirmeye bakalım.

4. Şimdi, Git ile kod gönderdiğiniz gibi örnek kodunuzu yeni App Service’e dağıtın:

        git push azure master 

    ![Azure’da ilk web uygulamanıza kod gönderme](./media/app-service-web-get-started/5-push-code.png)    
    
    Dil altyapılarından birini kullandıysanız, yukarıda gösterilenden farklı bir çıkış göreceksiniz. Bunun nedeni `git push` Azure’a kod yerleştirmekle kalmaz, aynı zamanda dağıtım altyapısında dağıtım görevlerini tetikler. Proje (depo) kökünde package.json (Node.js) veya requirements.txt (Python) dosyaları bulunuyorsa veya ASP.NET projenizde packages.config dosyası bulunuyorsa, dağıtım betikleri sizin için gerekli paketleri geri yükler. Bununla birlikte PHP uygulamanızda composer.json dosyalarını otomatik olarak işlemek için [Composer uzantısını etkinleştirebilirsiniz](web-sites-php-mysql-deploy-use-git.md#composer).

Tebrikler, Azure App Service’ize uygulamanızı dağıttınız. 

## Uygulamanızı çalışırken görme

Azure’da uygulamanızı çalışırken görmek için deponuzun herhangi bir dizininden bu komutu çalıştırın:

    azure site browse

## Uygulamanızda güncelleştirmeler yapma

Artık canlı sitede bir güncelleştirme yapmak için projenizin (depo) kökünden gönderme yapmak üzere Git’i kullanabilirsiniz. Bunu, uygulamanızı Azure’a ilk kez dağıtırken yaptığınız gibi yapacaksınız. Örneğin yerel olarak test ettiğiniz yeni bir değişikliği her göndermek istediğinizde tek yapmanız gereken projenizin (depo) kökünden aşağıdaki komutları çalıştırmaktır:
    
    git add .
    git commit -m "<your_message>"
    git push azure master

## Uygulamanızı Azure Portal’da görme

Şimdi oluşturduğunuzu görmek için Azure Portal’a gidelim:

1. Azure aboneliğiniz olan bir Microsoft hesabıyla [Azure Portal](https://portal.azure.com)’da oturum açın.

2. Sol çubukta **App Services**’a tıklayın.

3. Oluşturduğunuz uygulamaya tıklayarak uygulamanın sayfasını portalda açın ([dikey pencere](../azure-portal-overview.md) olarak adlandırılır). Size kolaylık sağlamak üzere **Ayarlar** dikey penceresi de varsayılan olarak açılır.

    ![Azure’da ilk web uygulamanızın portal görünümü](./media/app-service-web-get-started/portal-view.png) 

App Service uygulamanızın portal dikey penceresinde uygulamanızı yapılandırma, izleme, koruma ve uygulama sorunlarını giderme işlemlerini gerçekleştirmek için zengin bir ayar ve araç seti yer alır. Bazı basit görevleri gerçekleştirerek arabirimi tanımak için zaman ayırın (görev sayısı, ekran görüntüsündeki sayıyla uyuşur):

1. Uygulamayı durdurun
2. Uygulamayı yeniden başlatın
3. Kaynak grubunda dağıtılan kaynakların tümünü görmek için **Kaynak Grubu** bağlantısına tıklayın.
4. Uygulamanız ile ilgili diğer bilgileri görmek için **Ayarlar** > **Özellikler**’e tıklayın
5. İzleme ve sorun gidermeyle ilgili kullanışlı araçlara erişmek için **Araçlar**’a tıklayın.  

## Sonraki adımlar

- Azure uygulamanızı bir sonraki seviyeye taşıyın. Kimlik doğrulama ile güvenli hale getirin. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin. Bkz. [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md).
- Git ve Azure CLI dışında Azure’a web uygulaması dağıtmanın farklı yöntemleri vardır (bkz. [Uygulamanızı Azure App Service’e dağıtma](../app-service-web/web-sites-deploy.md)).
Makalenin üst kısmından altyapınızı seçerek dil altyapınız için tercih edilen geliştirme ve geliştirme adımlarını görün.



<!--HONumber=Jun16_HO2-->


