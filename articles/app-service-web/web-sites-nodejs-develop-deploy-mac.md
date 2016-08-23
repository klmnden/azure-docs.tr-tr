<properties
    pageTitle="Azure App Service’te bir Node.js web uygulaması oluşturma | Microsoft Azure"
    description="Azure App Service'te Node.js uygulamasını bir web uygulamasına dağıtmayı öğrenin."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="06/24/2016"
    ms.author="robmcm"/>

# Azure App Service’te bir Node.js web uygulaması oluşturma

> [AZURE.SELECTOR]
- [.NET](web-sites-dotnet-get-started.md)
- [Node.js](web-sites-nodejs-develop-deploy-mac.md)
- [Java](web-sites-java-get-started.md)
- [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
- [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
- [Python](web-sites-python-ptvs-django-mysql.md)

Bu öğreticide [Azure App Service](../app-service/app-service-value-prop-what-is.md)’te [Git](http://git-scm.com) kullanarak nasıl basit bir [Node.js](http://nodejs.org) uygulaması oluşturulacağı ve bir [web uygulamasına](app-service-web-overview.md) dağıtılacağı gösterilmektedir. Bu öğreticideki yönergeler Node.js çalıştırabilen tüm işletim sistemlerinde izlenebilir.

Şunları öğreneceksiniz:

* Azure Portal kullanarak Azure App Service'te web uygulaması oluşturma
* Web uygulamanızın Git deposuna ileterek bir Node.js uygulamasını web uygulamasına dağıtma.

Tamamlanan uygulama tarayıcıya kısa bir “hello world” dizesi yazar.

![“Hello World” iletisini gösteren bir tarayıcı.][helloworld-completed]

Öğreticiler ve daha karmaşık Node.js uygulamaları içeren örnek kod veya Azure'da Node.js kullanma hakkındaki diğer konular için bkz. [Node.js Geliştirici Merkezi](/develop/nodejs/).

> [AZURE.NOTE]
> Bu öğreticiyi tamamlamak için Microsoft Azure hesabınızın olması gerekir. Bir hesabınız yoksa, [Visual Studio abone avantajlarınızı etkinleştirebilir ](/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) veya [ücretsiz deneme için kaydolabilirsiniz.](/en-us/pricing/free-trial/?WT.mc_id=A261C142F)
>
> Bir Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751)’e gidin. Burada, App Service'te hemen bir kısa süreli başlangıç web uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur.

## Bir web uygulaması oluşturma ve Git yayımlamayı etkinleştirme

Azure App Service’te bir web uygulaması oluşturmak ve Git yayımlamayı etkinleştirmek için bu adımları izleyin. 

[Git](http://git-scm.com/) Azure Web sitenizi dağıtmak için kullanabileceğiniz bir dağıtılmış sürüm denetim sistemidir. Web uygulamanız için yazdığınız kodu yerel bir Git deposunda depolayacak ve kodunuzu uzak depoya ileterek Azure'a dağıtacaksınız. Bu dağıtım yöntemi, bir App Service Web Apps özelliğidir.  

1. [Azure Portal](https://portal.azure.com)’da oturum açın.

2. Azure Portal’ın sol üst kısmında **+ YENİ** simgesine tıklayın.

3. **Web + Mobil**’e tıklayın ve ardından **Web uygulaması**’na tıklayın

    ![][portal-quick-create]

4. Web uygulaması için **Web uygulaması** kutusuna bir ad girin.

    Web uygulamasının URL’si {ad}.azurewebsites.net şeklinde olacağından, bu ad azurewebsites.net etki alanında benzersiz olmalıdır. Girdiğiniz ad benzersiz değilse, metin kutusunda kırmızı bir ünlem işareti gösterilir.

5. Bir **Abonelik** seçin.

6. Bir **Kaynak Grubu** seçin veya yeni bir tane oluşturun.

    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../resource-group-overview.md).

7. Bir **App Service planı/Konum** seçin veya yeni bir tane oluşturun.

    App Service planları hakkında daha fazla bilgi için bkz. [Azure App Service planlarına genel bakış](../azure-web-sites-web-hosting-plans-in-depth-overview.md)

8. **Oluştur**’a tıklayın.
   
    ![][portal-quick-create2]

    Kısa bir süre içinde, normalde bir dakika içinde, Azure yeni web uygulamasını oluşturmayı tamamlar.

9. **Web uygulamaları > {yeni web uygulamanız}**’a tıklayın.

    ![](./media/web-sites-nodejs-develop-deploy-mac/gotowebapp.png)

10. **Web uygulaması** dikey penceresinde **Dağıtım** bölümüne tıklayın.

    ![][deployment-part]

11. **Sürekli Dağıtım** dikey penceresinde **Kaynağı Seç**’e tıklayın.

12. **Yerel Git Deposu**’na ve ardından **Tamam**’a tıklayın.

    ![][setup-git-publishing]

13. Henüz yapmadıysanız, dağıtım kimlik bilgilerini ayarlayın.

    a. Web uygulaması dikey penceresinde, **Ayarlar > Dağıtım kimlik bilgileri**’ne tıklayın.

    ![][deployment-credentials]
 
    b. Bir kullanıcı adı ve parola oluşturun. 
    
    ![](./media/web-sites-nodejs-develop-deploy-mac/setdeploycreds.png)

14. Web uygulaması dikey penceresinde, **Ayarlar**’a ve ardından **Özellikler**’e tıklayın.
 
    Yayımlamak için, uzak bir Git deposuna ileteceksiniz. Depo URL'si,**GIT URL'si** altında listelenir. Bu URL'yi öğreticide daha sonra kullanacaksınız.

    ![][git-url]

## Uygulamanızı yerel olarak oluşturma ve test etme

Bu bölümde, [nodejs.org] içindeki “Hello World” örneğinin biraz değiştirilmiş bir sürümünü içeren **server.js** dosyası oluşturacaksınız. Kod, bir Azure web uygulamasında çalıştığında dinlenecek bağlantı noktası olarak process.env.PORT ekler.

1. *helloworld* adlı bir dizin oluşturun.

2. *helloworld* dizininde **server.js** adlı yeni bir dosya oluşturmak için metin düzenleyicisini kullanın.

2. Aşağıdaki kodu **server.js** dosyasına kopyalayın bulun ve dosyayı kaydedin:

        var http = require('http')
        var port = process.env.PORT || 1337;
        http.createServer(function(req, res) {
          res.writeHead(200, { 'Content-Type': 'text/plain' });
          res.end('Hello World\n');
        }).listen(port);

3. Komut satırını açın ve web uygulamasını yerel olarak başlatmak için aşağıdaki komutu kullanın.

        node server.js

4. Web tarayıcınızı açın ve http://localhost:1337 adresine gidin. 

    Aşağıdaki ekran görüntüsünde gösterildiği gibi, "Hello World" iletisini gösteren bir web sayfası görünür.

    ![“Hello World” iletisini gösteren bir tarayıcı.][helloworld-localhost]

## Uygulamanızı yayımlama

1. Henüz yapmadıysanız Git’i yükleyin.

    Platformunuza ilişkin yükleme yönergeleri için bkz. [Git indirme sayfası](http://git-scm.com/download).

1. Komut satırında, dizinleri **helloworld** diziniyle değiştirin ve yerel bir Git deposunu başlatmak için aşağıdaki komutu girin.

        git init


2. Depoya dosyaları eklemek için aşağıdaki komutları kullanın:

        git add .
        git commit -m "initial commit"

3. Aşağıdaki komutu kullanarak, önceden oluşturduğunuz web uygulamasına güncelleştirmeleri iletmek için Git remote ekleyin:

        git remote add azure [URL for remote repository]

4. Aşağıdaki komutu kullanarak değişikliklerinizi Azure’a iletin:

        git push azure master

    Daha önce oluşturduğunuz parola istenir. Çıktı aşağıdaki örneğe benzerdir.

        Counting objects: 3, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (3/3), 374 bytes, done.
        Total 3 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '5ebbe250c9'.
        remote: Preparing files for deployment.
        remote: Deploying Web.config to enable Node.js activation.
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
         * [new branch]      master -> master

5. Uygulamanızı görüntülemek için, Azure portaldaki **Web Uygulaması** bölümünde **Gözat**’a tıklayın.

    ![Gözat düğmesi](./media/web-sites-nodejs-develop-deploy-mac/browsebutton.png)

    ![Azure'da Hello world](./media/web-sites-nodejs-develop-deploy-mac/helloworldazure.png)

## Uygulamanızdaki değişiklikleri yayımlama

1. **server.js** dosyasını bir metin düzenleyicisinde açın ve 'Hello World\n ifadesini 'Hello Azure\n' ile değiştirin. 

2. Dosyayı kaydedin.

2. Komut satırında, dizinleri **helloworld** diziniyle değiştirin ve aşağıdaki komutları çalıştırın.

        git add .
        git commit -m "changing to hello azure"
        git push azure master

    Parolanız yeniden istenir.

3. Web uygulamanızın URL'sine gittiğiniz tarayıcı penceresini yenileyin.

    !['Hello Azure' iletisini gösteren bir web sayfası][helloworld-completed]

## Bir dağıtımı geri alma

**Dağıtımlar** dikey penceresinde dağıtım geçmişini görmek için **Web uygulaması** dikey penceresinde **Ayarlar > Sürekli Dağıtım**’a tıklayabilirsiniz. Önceki bir dağıtıma geri almanız gerekiyorsa, bunu seçebilir ve sonra **Dağıtım Ayrıntıları** dikey penceresinde **Yeniden dağıt**’a tıklayabilirsiniz.

## Sonraki adımlar

Azure App Service'te Node.js uygulamasını bir web uygulamasına dağıttınız. App Service web uygulamalarının Node.js uygulamalarını nasıl çalıştırdığı hakkında daha fazla bilgi için bkz. [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx) ve [Bir Azure uygulamasında Node.js sürümünü belirtme](../nodejs-specify-node-version-azure-apps.md).

Node.js, uygulamalarınız tarafından kullanılabilecek zengin bir modül ekosistemi sağlar. Web Apps’in modüllerle nasıl çalıştığını öğrenmek için bkz. [Azure uygulamalarıyla Node.js modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)

Azure’a dağıtılan uygulamanızda sorunlarla karşılaşırsanız, sorunu tanılama hakkında bilgi için bkz. [Azure App Service’teki bir Node.js uygulamasında hata ayıklama](web-sites-nodejs-debug.md).

Bu makalede bir web uygulaması oluşturmak için Azure Portal kullanılmıştır. Aynı işlemleri gerçekleştirmek için [Azure Komut Satırı Arabirimi](../xplat-cli-install.md) veya [Azure PowerShell](../powershell-install-configure.md)’i kullanabilirsiniz.

Azure’da Node.js uygulamaları geliştirme hakkında daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/develop/nodejs/).

[helloworld-completed]: ./media/web-sites-nodejs-develop-deploy-mac/helloazure.png
[helloworld-localhost]: ./media/web-sites-nodejs-develop-deploy-mac/helloworldlocal.png
[portal-quick-create]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website.png
[portal-quick-create2]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website2.png
[setup-git-publishing]: ./media/web-sites-nodejs-develop-deploy-mac/setup_git_publishing.png
[go-to-dashboard]: ./media/web-sites-nodejs-develop-deploy-mac/go_to_dashboard.png
[deployment-part]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-part.png
[deployment-credentials]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-credentials.png
[git-url]: ./media/web-sites-nodejs-develop-deploy-mac/git-url.png



<!--HONumber=Aug16_HO1-->


