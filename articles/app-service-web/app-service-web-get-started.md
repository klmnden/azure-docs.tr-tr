<properties 
    pageTitle="İlk web uygulamanızı Azure’a beş dakikada dağıtın | Microsoft Azure" 
    description="Örnek bir uygulama dağıtarak web uygulamalarını App Service’te çalıştırmanın ne kadar kolay olduğunu öğrenin. Gerçek geliştirmeler yapmaya hızlıca başlayın ve sonuçlarını anında görün." 
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
    ms.date="09/09/2016" 
    ms.author="cephalin"
/>
    
# İlk web uygulamanızı Azure’a beş dakikada dağıtın

Bu öğretici, ilk web uygulamanızı [App Service](../app-service/app-service-value-prop-what-is.md)’e dağıtmanıza yardımcı olur.
App Service’i web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API uygulamaları](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için kullanabilirsiniz.

Bu öğreticide yapacaklarınız: 

- Azure App Service’te web uygulaması oluşturma.
- Örnek kod (ASP.NET, PHP, Node.js, Java, veya Python arasından seçim yapın) dağıtın.
- Kodunuzu üretim ortamında çalışırken görme.
- [Git yürütmelerini gönderdiğiniz](https://git-scm.com/docs/git-push) şekilde web uygulamanızı güncelleştirme.

## Önkoşullar

- [Git’i yükleyin](http://www.git-scm.com/downloads). Yeni bir Windows komut isteminden, PowerShell penceresinden, Linux kabuğundan veya OS X terminalinden `git --version` çalıştırarak yüklemenizin başarılı olduğunu doğrulayın.
- Bir Microsoft Azure hesabı edinin. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abonelik avantajlarınızı etkinleştirebilirsiniz](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Azure hesabınız olmadan [App Service'i deneyebilirsiniz](http://go.microsoft.com/fwlink/?LinkId=523751). Başlangıç uygulaması oluşturup en fazla bir saat süreyle üzerinde çalışabilirsiniz; kredi kartı veya taahhüt gerekmez.

<a name="create"></a>
## Web uygulaması oluşturma

1. Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

2. Sol taraftaki menüden **Yeni** > **Web + Mobil** > **Web Uygulaması**'na tıklayın.

    ![Azure’da ilk web uygulamanızı oluşturmaya başlayın](./media/app-service-web-get-started/create-web-app-portal.png)

3. Uygulama oluşturma dikey penceresinde yeni uygulamanız için şu ayarları kullanın:

    - **Uygulama adı**: Benzersiz bir ad yazın.
    - **Kaynak grubu**: **Yeni Oluştur**'u seçin ve kaynak grubuna bir ad verin.
    - **App Service planı/Konumu**: Yapılandırmak için üzerine tıklayın, ardından App Service planının adını, konumunu ve fiyatlandırma katmanını ayarlamak için **Yeni Oluştur**'a tıklayın. **Ücretsiz** fiyatlandırma katmanını kullanabilirsiniz.

    İşlemleri tamamladığınızda uygulama oluşturma dikey pencereniz şu şekilde görünür:

    ![Azure’da ilk web uygulamanızı yapılandırın](./media/app-service-web-get-started/create-web-app-settings.png)

3. Alttaki **Oluştur** düğmesine tıklayın. İlerlemeyi görmek için üstteki **Bildirim** simgesine tıklayabilirsiniz.

    ![Azure’daki ilk web uygulamanız için uygulama oluşturma bildirimi](./media/app-service-web-get-started/create-web-app-started.png)

4. Dağıtım tamamlandığında bu bildirim iletisini görürsünüz. Dağıtımınızın dikey penceresini açmak için iletiye tıklayın.

    ![Azure’daki ilk web uygulamanız için dağıtım tamamlandı iletisi](./media/app-service-web-get-started/create-web-app-finished.png)

5. **Dağıtım başarılı oldu** dikey penceresinde, yeni web uygulamanızın dikey penceresini açmak için **Kaynak** bağlantısına tıklayın.

    ![Azure’daki ilk web uygulamanız için kaynak bağlantısı](./media/app-service-web-get-started/create-web-app-resource.png)

## Kodu web uygulamanıza dağıtma

Şimdi de Git kullanarak Azure'a kod dağıtalım.

5. Web uygulaması dikey penceresinde, aşağı kaydırarak veya aratarak **Dağıtım seçeneklerini** bulun ve üzerine tıklayın. 

    ![Azure’daki ilk web uygulamanız için dağıtım seçenekleri](./media/app-service-web-get-started/deploy-web-app-deployment-options.png)

6. **Kaynak Seç** > **Yerel Git Deposu** > **Tamam**'a tıklayın.

7. Web uygulaması dikey penceresine döndüğünüzde **Dağıtım kimlik bilgileri** seçeneğine tıklayın.

8. Dağıtım kimlik bilgilerinizi ayarlayın ve **Kaydet**'e tıklayın.

7. Web uygulaması dikey penceresine döndüğünüzde, aşağı kaydırarak veya aratarak **Özellikler**'i bulun ve üzerine tıklayın. **Git URL'sinin** yanındaki **Kopyala** düğmesine tıklayın.

    ![Azure’daki ilk web uygulamanız için özellikler dikey penceresi](./media/app-service-web-get-started/deploy-web-app-properties.png)

    Artık, kodunuzu Git ile dağıtmaya hazırsınız.

1. Komut satırı terminalinizde, bir çalışma dizinine (`CD`) geçip örnek uygulamayı şu şekilde kopyalayın:

        git clone <github_sample_url>

    ![Azure’daki ilk web uygulamanız için uygulama örnek kodunu kopyalama](./media/app-service-web-get-started/html-git-clone.png)

    *&lt;github_sample_url>* için, tercih ettiğiniz çerçeveye bağlı olarak aşağıdaki URL’lerden birini kullanın:

    - HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git)
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. Örnek uygulamanızın deposuna geçin. Örneğin: 

        cd app-service-web-html-get-started

3. Birkaç adım önce portaldan kopyaladığınız Git URL'sini kullanarak, Azure uygulamanız için Git uzak komutunu yapılandırın.

        git remote add azure <giturlfromportal>

4. Git ile herhangi bir kodu gönderdiğiniz şekilde, örnek kodunuzu Azure uygulamanıza dağıtın:

        git push azure master

    ![Azure’daki ilk web uygulamanıza kod gönderme](./media/app-service-web-get-started/html-git-push.png)    

    Dil çerçevelerinden birini kullandıysanız farklı bir çıktı görürsünüz. Bunun nedeni, `git push` komutunun Azure’a kod yerleştirmeye ek olarak dağıtım çerçevesinde dağıtım görevleri tetiklemesidir. Proje (depo) kökünde package.json (Node.js) veya requirements.txt (Python) dosyaları bulunuyorsa ya da ASP.NET projenizde packages.config dosyası bulunuyorsa, dağıtım betiği sizin için gerekli paketleri geri yükler. Ayrıca, PHP uygulamanızda composer.json dosyalarını otomatik olarak işlemek için [Composer uzantısını etkinleştirebilirsiniz](web-sites-php-mysql-deploy-use-git.md#composer).

İşte bu kadar! Kodunuz artık Azure'da çalışıyor. Kodunuzun nasıl çalıştığını görmek için tarayıcınızdan http://*&lt;uygulamaadı >*.azurewebsites.net sayfasına gidin. 

## Uygulamanızda güncelleştirmeler yapma

Artık, projenizin (depo) kökünden gönderim için Git kullanarak canlı sitede güncelleştirme yapabilirsiniz. Kodunuzu ilk kez dağıtırken de bu yolu izlemeniz gerekir. Örneğin, yerel olarak test ettiğiniz yeni değişiklikleri göndermek istediğinizde tek yapmanız gereken, projenizin (depo) kökünden aşağıdaki komutları çalıştırmaktır:

    git add .
    git commit -m "<your_message>"
    git push azure master

## Sonraki adımlar

Dil çerçeveniz için tercih edilen geliştirme ve dağıtım adımlarını bulun:

> [AZURE.SELECTOR]
- [.NET](web-sites-dotnet-get-started.md)
- [PHP](app-service-web-php-get-started.md)
- [Node.js](app-service-web-nodejs-get-started.md)
- [Python](web-sites-python-ptvs-django-mysql.md)
- [Java](web-sites-java-get-started.md)

Veya ilk web uygulamanızla daha fazlasını yapın. Örneğin:

- [Kodunuzu Azure'a dağıtmanın diğer yollarını](../app-service-web/web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
- Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Uygulamanızı isteğe bağlı olarak ölçeklendirin. Performans uyarıları ayarlayın. Hepsini yalnızca birkaç tıklamayla yapabilirsiniz. Bkz. [İlk web uygulamanıza işlev ekleme](app-service-web-get-started-2.md).




<!----HONumber=Sep16_HO4-->


