<properties 
    pageTitle="İlk Node.js web uygulamanızı Azure’a beş dakikada dağıtın | Microsoft Azure" 
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
    ms.date="09/16/2016" 
    ms.author="cephalin"
/>
    
# İlk Node.js web uygulamanızı Azure’a beş dakikada dağıtın

Bu öğretici, ilk Node.js web uygulamanızı [App Service](../app-service/app-service-value-prop-what-is.md)’e dağıtmanıza yardımcı olur.
App Service’i web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API uygulamaları](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için kullanabilirsiniz.

Bu öğreticide yapacaklarınız: 

- Azure App Service’te web uygulaması oluşturma.
- Örnek Node.js kodu dağıtma.
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

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Uygulama oluşturma dikey penceresinde yeni uygulamanız için şu ayarları kullanın:

    - **Uygulama adı**: Benzersiz bir ad yazın.
    - **Kaynak grubu**: **Yeni Oluştur**'u seçin ve kaynak grubuna bir ad verin.
    - **App Service planı/Konumu**: Yapılandırmak için üzerine tıklayın, ardından App Service planının adını, konumunu ve fiyatlandırma katmanını ayarlamak için **Yeni Oluştur**'a tıklayın. **Ücretsiz** fiyatlandırma katmanını kullanabilirsiniz.

    İşlemleri tamamladığınızda uygulama oluşturma dikey pencereniz şu şekilde görünür:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Alttaki **Oluştur** düğmesine tıklayın. İlerlemeyi görmek için üstteki **Bildirim** simgesine tıklayabilirsiniz.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Dağıtım tamamlandığında bu bildirim iletisini görürsünüz. Dağıtımınızın dikey penceresini açmak için iletiye tıklayın.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. **Dağıtım başarılı oldu** dikey penceresinde, yeni web uygulamanızın dikey penceresini açmak için **Kaynak** bağlantısına tıklayın.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## Kodu web uygulamanıza dağıtma

Şimdi de Git kullanarak Azure'a kod dağıtalım.

5. Web uygulaması dikey penceresinde, aşağı kaydırarak veya aratarak **Dağıtım seçeneklerini** bulun ve üzerine tıklayın. 

    ![](./media/app-service-web-get-started-languages/deploy-web-app-deployment-options.png)

6. **Kaynak Seç** > **Yerel Git Deposu** > **Tamam**'a tıklayın.

7. Web uygulaması dikey penceresine döndüğünüzde **Dağıtım kimlik bilgileri** seçeneğine tıklayın.

8. Dağıtım kimlik bilgilerinizi ayarlayın ve **Kaydet**'e tıklayın.

7. Web uygulaması dikey penceresine döndüğünüzde, aşağı kaydırarak veya aratarak **Özellikler**'i bulun ve üzerine tıklayın. **Git URL'sinin** yanındaki **Kopyala** düğmesine tıklayın.

    ![](./media/app-service-web-get-started-languages/deploy-web-app-properties.png)

    Artık, kodunuzu Git ile dağıtmaya hazırsınız.

1. Komut satırı terminalinizde, bir çalışma dizinine (`CD`) geçip örnek uygulamayı şu şekilde kopyalayın:

        git clone https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git

    ![Azure’daki ilk web uygulamanız için uygulama örnek kodunu kopyalama](./media/app-service-web-get-started-languages/node-git-clone.png)

    *&lt;github_sample_url>* için, tercih ettiğiniz çerçeveye bağlı olarak aşağıdaki URL’lerden birini kullanın:

2. Örnek uygulamanızın deposuna geçin. Örneğin: 

        cd app-service-web-nodejs-get-started

3. Birkaç adım önce portaldan kopyaladığınız Git URL'sini kullanarak, Azure uygulamanız için Git uzak komutunu yapılandırın.

        git remote add azure <giturlfromportal>

4. Git ile herhangi bir kodu gönderdiğiniz şekilde, örnek kodunuzu Azure uygulamanıza dağıtın:

        git push azure master

    ![Azure’daki ilk web uygulamanıza kod gönderme](./media/app-service-web-get-started-languages/node-git-push.png)    

    Dil çerçevelerinden birini kullandıysanız farklı bir çıktı görürsünüz. Bunun nedeni, `git push` komutunun Azure’a kod yerleştirmeye ek olarak dağıtım çerçevesinde dağıtım görevleri tetiklemesidir. Proje (depo) kökünüzde package.json varsa, dağıtım betiği gerekli paketleri sizin için geri yükler. 

İşte bu kadar! Kodunuz artık Azure'da çalışıyor. Kodunuzun nasıl çalıştığını görmek için tarayıcınızdan http://*&lt;uygulamaadı >*.azurewebsites.net sayfasına gidin. 

## Uygulamanızda güncelleştirmeler yapma

Artık, projenizin (depo) kökünden gönderim için Git kullanarak canlı sitede güncelleştirme yapabilirsiniz. Kodunuzu ilk kez dağıtırken de bu yolu izlemeniz gerekir. Örneğin, yerel olarak test ettiğiniz yeni değişiklikleri göndermek istediğinizde tek yapmanız gereken, projenizin (depo) kökünden aşağıdaki komutları çalıştırmaktır:

    git add .
    git commit -m "<your_message>"
    git push azure master

## Sonraki adımlar

[Node.js Express web uygulaması oluşturma, yapılandırma ve Azure’a dağıtma](app-service-web-nodejs-get-started.md). Bu öğreticiyi izleyerek, Azure’da Node.js web uygulaması çalıştırmak için ihtiyacınız olan temel becerileri öğrenebilirsiniz:

- Azure’da PowerShell/Bash uygulamaları oluşturma ve yapılandırma.
- Node.js sürümünü ayarlama.
- Kök uygulama dizininde olmayan bir başlangıç dosyası kullanma.
- NPM ile otomatikleştirme.
- Hata ve çıktı günlükleri alma.

Veya ilk web uygulamanızla daha fazlasını yapın. Örneğin:

- [Kodunuzu Azure'a dağıtmanın diğer yollarını](../app-service-web/web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
- Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Uygulamanızı isteğe bağlı olarak ölçeklendirin. Performans uyarıları ayarlayın. Hepsini yalnızca birkaç tıklamayla yapabilirsiniz. Bkz. [İlk web uygulamanıza işlev ekleme](app-service-web-get-started-2.md).




<!---HONumber=Sep16_HO4-->


