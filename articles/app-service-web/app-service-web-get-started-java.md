<properties 
    pageTitle="İlk Java web uygulamanızı Azure’a beş dakikada dağıtın | Microsoft Azure" 
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
    
# İlk Java web uygulamanızı Azure’a beş dakikada dağıtın

Bu öğretici, [Azure App Service](../app-service/app-service-value-prop-what-is.md)’e basit bir Java web uygulaması dağıtmanıza yardımcı olur.
App Service’i web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API uygulamaları](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için kullanabilirsiniz.

Bu öğreticide yapacaklarınız: 

- Azure App Service’te web uygulaması oluşturma.
- Örnek bir Java uygulaması oluşturma.
- Kodunuzu üretim ortamında çalışırken görme.

## Önkoşullar

- [FileZilla](https://filezilla-project.org/) gibi bir FTP/FTPS istemcisi edinin.
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

## Bir Java uygulamasını web uygulamanıza dağıtma

Şimdi, FTPS kullanarak bir Java uygulamasını Azure’a dağıtalım.

5. Web uygulaması dikey penceresinde aşağı kaydırarak veya aratarak **Uygulama ayarlarını** bulun ve üzerine tıklayın. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java sürümü**’nde **Java 8**’i seçin ve **Kaydet**’e tıklayın.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    **Web uygulaması ayarları başarıyla güncelleştirildi** bildirimini aldığınızda, http://*&lt;uygulamaadı>*.azurewebsites.net adresine giderek varsayılan JSP servlet’ini çalışırken görün.

7. Web uygulaması dikey penceresine dönüp, aşağı kaydırarak veya aratarak **Dağıtım kimlik bilgilerini** bulun ve üzerine tıklayın.

8. Dağıtım kimlik bilgilerinizi ayarlayın ve **Kaydet**'e tıklayın.

7. Web uygulaması dikey penceresine döndüğünüzde **Gözat**’a tıklayın. **FTP/Dağıtım kullanıcı adı** ve **FTPS ana bilgisayar adı**’nın yanındaki **Kopyala** düğmesine tıklayarak bu değerleri kopyalayın.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Artık Java uygulamanızı FTPS ile dağıtmaya hazırsınız.

8. FTP/FTPS istemcinizde, bir önceki adımda kopyaladığınız değerleri kullanarak Azure web uygulamanızın FTP sunucusunda oturum açın. Daha önce oluşturduğunuz dağıtım parolasını kullanın.

    Aşağıdaki ekran görüntüsünde, FileZilla kullanarak oturum açma işlemi gösterilmiştir.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Azure’daki tanınmayan sertifika türleri için güvenlik uyarılarıyla karşılaşabilirsiniz. Devam edin.

9. [Bu bağlantıya](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) tıklayarak WAR dosyasını yerel makinenize indirin.

9. FTP/FTPS istemcinizde, uzak sitede **/site/wwwroot/webapps** konumuna gidin ve yerel makinenizdeki indirilmiş WAR dosyasını bu uzak dizine sürükleyin.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    **Tamam**’a tıklayarak dosyayı Azure’da geçersiz kılın.

    >[AZURE.NOTE] Tomcat’in varsayılan davranışına uygun olarak, /site/wwwroot/webapps içindeki **ROOT.war** dosya adı, kök web uygulamasını (http://*&lt;uygulamaadı>*.azurewebsites.net); ***&lt;ad>*.war** ise adlandırılmış bir web uygulaması (http://*&lt;uygulamaadı>*.azurewebsites.net/*&lt;ad>*) verir.

İşte bu kadar! Java uygulamanız artık Azure'da çalışıyor. Kodunuzun nasıl çalıştığını görmek için tarayıcınızdan http://*&lt;uygulamaadı >*.azurewebsites.net sayfasına gidin. 

## Uygulamanızda güncelleştirmeler yapma

Bir güncelleştirme yapmanız gerektiğinde, yeni WAR dosyasını FTP/FTPS istemcinizle aynı uzak dizine yükleyin.

## Sonraki adımlar

[Azure Market’te şablondan Java web uygulaması oluşturma](app-service-web-java-get-started.md#marketplace). Tamamen özelleştirilebilen kendi Tomcat kapsayıcınızı edinebilir ve tanıdık Yönetici Kullanıcı Arabiriminden yararlanabilirsiniz. 

Azure web uygulamanızda [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) veya [Eclipse](app-service-web-debug-java-web-app-in-eclipse.md) ile doğrudan hata ayıklayın.

Veya ilk web uygulamanızla daha fazlasını yapın. Örneğin:

- [Kodunuzu Azure'a dağıtmanın diğer yollarını](../app-service-web/web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
- Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Uygulamanızı isteğe bağlı olarak ölçeklendirin. Performans uyarıları ayarlayın. Hepsini yalnızca birkaç tıklamayla yapabilirsiniz. Bkz. [İlk web uygulamanıza işlev ekleme](app-service-web-get-started-2.md).




<!---HONumber=Sep16_HO4-->


