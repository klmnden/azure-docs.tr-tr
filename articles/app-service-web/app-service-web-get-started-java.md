<properties 
    pageTitle="İlk Java web uygulamanızı beş dakikada Azure'da dağıtma | Microsoft Azure" 
    description="Örnek bir uygulama dağıtarak App Service'te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin. Hızlı bir şekilde gerçek geliştirmeler yapmaya başlayın ve sonuçlarını anında görün." 
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
    

# İlk Java web uygulamanızı beş dakikada Azure'da dağıtın

Bu öğretici, [Azure Uygulama Hizmeti](../app-service/app-service-value-prop-what-is.md)’nde basit bir Java web uygulaması dağıtmanıza yardımcı olur.
Web uygulamaları, [mobil uygulama arka uçları](/documentation/learning-paths/appservice-mobileapps/) ve [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) oluşturmak için App Service kullanabilirsiniz.

Yapacaklarınız: 

- Azure Uygulama Hizmeti'nde bir web uygulaması oluşturun.
- Örnek bir Java uygulaması dağıtın.
- Kodunuzun üretim ortamında dinamik bir şekilde çalıştığını görün.

## Önkoşullar

- [FileZilla](https://filezilla-project.org/) gibi bir FTP/FTPS istemcisi edinin.
- Bir Microsoft Azure hesabı edinin. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)

>[AZURE.NOTE] Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](http://go.microsoft.com/fwlink/?LinkId=523751). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.

<a name="create"></a>
## Web uygulaması oluşturma

1. Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

2. Sol menüden **Yeni** > **Web + Mobil** > **Web Uygulaması**'na tıklayın.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. Uygulama oluşturma dikey penceresinde yeni uygulamanız için şu ayarları kullanın:

    - **Uygulama adı**: Benzersiz bir ad yazın.
    - **Kaynak grubu**: **Yeni Oluştur**'u seçin ve kaynak grubuna bir ad verin.
    - **App Service planı/Konumu**: Yapılandırmak için üzerine tıklayın, ardından App Service planının adını, konumunu ve fiyatlandırma katmanını ayarlamak için **Yeni Oluştur**'a tıklayın. **Ücretsiz** fiyatlandırma katmanını dilediğiniz gibi kullanın.

    İşlemleri tamamladığınızda uygulama oluşturma dikey pencereniz şu şekilde görünür:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Alttaki **Oluştur** düğmesine tıklayın. İlerlemeyi görmek için üstteki **Bildirim** simgesine tıklayabilirsiniz.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Dağıtım tamamlandığında bu bildirim iletisini görürsünüz. Dağıtımınızın dikey penceresini açmak için iletiye tıklayın.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. Yeni web uygulamanızın dikey penceresini açmak için, **Dağıtım başarılı oldu** dikey penceresindeki **Kaynak** bağlantısına tıklayın.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## Bir Java uygulamasını web uygulamanızda dağıtma

Şimdi, FTPS kullanarak bir Java uygulamasını Azure’da dağıtalım.

5. Web uygulaması dikey penceresinde aşağı kaydırarak veya arayarak **Uygulama ayarlarını** bulun ve üzerine tıklayın. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Java sürümü**’nde **Java 8**’i seçin ve **Kaydet**’e tıklayın.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    **Web uygulaması ayarları başarıyla güncelleştirildi** bildirimini aldığınızda, http://*&lt;uygulamaadı>*.azurewebsites.net adresine giderek varsayılan JSP servlet’ini çalışırken görün.

7. Web uygulaması dikey penceresine dönüp, aşağı kaydırarak veya arayarak **Dağıtım kimlik bilgilerini** bulun ve üzerine tıklayın.

8. Dağıtım kimlik bilgilerinizi ayarlayın ve **Kaydet**'e tıklayın.

7. Web uygulaması dikey penceresinde, **Genel Bakış**’a tıklayın. **FTP/Dağıtım kullanıcı adı** ve **FTPS ana bilgisayar adı**’nın yanındaki **Kopyala** düğmesine tıklayarak bu değerleri kopyalayın.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Artık Java uygulamanızı FTPS ile dağıtmaya hazırsınız.

8. FTP/FTPS istemcinizde, bir önceki adımda kopyaladığınız değerleri kullanarak Azure web uygulamanızın FTP sunucusunda oturum açın. Daha önce oluşturduğunuz dağıtım parolasını kullanın.

    Aşağıdaki ekran görüntüsünde, FileZilla ile oturum açma işlemi gösterilmiştir.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Azure’da tanınmayan SSL sertifikaları için güvenlik uyarılarıyla karşılaşabilirsiniz. Devam edin.

9. [Bu bağlantıya](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) tıklayarak WAR dosyasını yerel makinenize indirin.

9. FTP/FTPS istemcinizde, uzak sitede **/site/wwwroot/webapps** konumuna gidin ve yerel makinenizdeki indirilmiş WAR dosyasını bu uzak dizine sürükleyin.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    **Tamam**’a tıklayarak dosyayı Azure’da geçersiz kılın.

    >[AZURE.NOTE] Tomcat’in varsayılan davranışına uygun olarak, /site/wwwroot/webapps içindeki **ROOT.war** dosya adı, kök web uygulamasını (http://*&lt;uygulamaadı>*.azurewebsites.net); ***&lt;ad>*.war** dosya adı ise adlandırılmış bir web uygulamasını (http://*&lt;uygulamaadı>*.azurewebsites.net/*&lt;ad>*) belirtir.

İşte bu kadar! Java uygulamanız artık Azure'da çalışıyor. Kodunuzun nasıl çalıştığını görmek için tarayıcınızda http://*&lt;uygulamaadı >*.azurewebsites.net sayfasına gidin. 

## Uygulamanızda güncelleştirmeler yapma

Bir güncelleştirme yapmanız gerektiğinde, yeni WAR dosyasını FTP/FTPS istemcinizle aynı uzak dizine yüklemeniz yeterlidir.

## Sonraki adımlar

[Azure Market’te şablondan Java web uygulaması oluşturma](web-sites-java-get-started.md#marketplace). Tamamen özelleştirilebilen kendi Tomcat kapsayıcınıza sahip olabilir ve alışık olduğunuz Yönetici Kullanıcı Arabiriminden yararlanabilirsiniz. 

Doğrudan [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) veya [Eclipse](app-service-web-debug-java-web-app-in-eclipse.md) içinden Azure web uygulamanızda hata ayıklayın.

Veya ilk web uygulamanızla daha fazlasını yapın. Örnek:

- [Kodunuzu Azure'a dağıtmanın diğer yollarını](../app-service-web/web-sites-deploy.md) deneyin. Örneğin, GitHub depolarınızın birinden dağıtım yapmak için **Dağıtım seçenekleri**'nde **Yerel Git Deposu** yerine **GitHub**'ı seçmeniz yeterlidir.
- Azure uygulamanızı bir sonraki seviyeye taşıyın. Kullanıcılarınızın kimliklerini doğrulayın. Talebe göre ölçeklendirin. Performans uyarıları ayarlayın. Tümünü birkaç tıklamayla gerçekleştirin. Bkz. [İlk web uygulamanıza işlevsellik ekleme](app-service-web-get-started-2.md).




<!--HONumber=Oct16_HO1-->


