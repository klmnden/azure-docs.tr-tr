---
title: "Azure App Service’te Node.js web uygulamalarını kullanmaya başlama | Microsoft Belgeleri"
description: "Azure App Service’te Node.js uygulamasını bir web uygulamasına dağıtmayı öğrenin."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: fb2b90c8-02b6-4700-929b-5de9a35d67cc
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 9482315aba089f3f00e114b835963c247a4f5007
ms.lasthandoff: 02/28/2017


---
# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Azure App Service’te Node.js web uygulamalarını kullanmaya başlama
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğreticide cmd.exe veya bash gibi bir komut satırı ortamında basit bir [Node.js] uygulaması oluşturma ve bir [Azure App Service]’e dağıtma işlemi gösterilmektedir. Bu öğreticideki yönergeler Node.js çalıştırabilen tüm işletim sistemlerinde izlenebilir.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

<a name="prereq"></a>

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri

Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](app-service-web-nodejs-get-started-cli-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız
- [Azure CLI 2.0](app-service-web-nodejs-get-started.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="prerequisites"></a>Ön koşullar
* [Node.js]
* [Bower]
* [Yeoman]
* [Git]
* [Azure CLI]
* Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir] veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.]

> [!NOTE]
> Azure hesabınız olmadan [App Service'i Deneyebilirsiniz](https://azure.microsoft.com/try/app-service/). Başlangıç uygulaması oluşturun ve bir saate kadar üzerinde çalışın; kredi kartı veya taahhüt gerekmez.
> 
> 

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Basit bir Node.js web uygulamasına oluşturma ve dağıtma
1. Seçtiğiniz komut satırı terminalini açın ve [Yeoman için Hızlı oluşturucu]’yu yükleyin.
   
        npm install -g generator-express
2. `CD` gerçekleştirin (çalışan bir dizine) ve aşağıdaki söz dizimini kullanarak hızlı bir uygulama oluşturun:
   
        yo express
   
    İstendiğinde aşağıdaki seçenekleri belirleyin:  
   
    `? Would you like to create a new directory for your project?` **Evet**  
    `? Enter directory name` **{uygulama adı}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **Hiçbiri**  
    `? Select a database to use:` **Hiçbiri**  
    `? Select a build tool to use:` **Grunt**
3. `CD` gerçekleştirin (yeni uygulamanızın kök dizinine) ve geliştirme ortamınızda çalıştığından emin olmak için başlatın:
   
        npm start
   
    Tarayıcınızda, Express giriş sayfasını görebildiğinizden emin olmak için <http://localhost:3000> adresine gidin. Uygulamanızın düzgün çalıştığını doğruladıktan sonra, durdurmak için `Ctrl-C` kullanın.
4. ASM moduna geçiş yapın ve Azure’da oturum açın ([Azure CLI](#prereq) gerekir):
   
        azure config mode asm
        azure login
   
    Azure aboneliğinizin bulunduğu bir Microsoft hesabıyla tarayıcıda oturum açmaya devam etmek için istemi izleyin.


3. App Service için dağıtım kullanıcısını ayarlayın. Kodu daha sonra bu kimlik bilgilerini kullanarak dağıtacaksınız.
   
        azure site deployment user set --username <username> --pass <password>

5. Hala uygulamanızın kök dizininde olduğunuzdan emin olun, sonra Azure’da sonraki komutla benzersiz bir uygulama adına sahip App Service uygulama kaynağı oluşturun. Örneğin: http://{appname}.azurewebsites.net
   
        azure site create --git {appname}
   
    Dağıtım yapılacak Azure bölgesini seçmek için istemi izleyin. 
6. Uygulamanızın kökünde./config/config.js dosyasını açın ve üretim bağlantı noktasını `process.env.port` olarak değiştirin; `config` nesnesi içindeki `production` özelliği aşağıdaki örnekteki gibi görünmelidir:
   
        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }
   
    > [!NOTE] 
    > Azure App Service, Node.js uygulamalarını varsayılan olarak `production` ortam değişkenleriyle (`process.env.NODE_ENV="production"`) çalıştırır.
    > Bu nedenle buradaki yapılandırmanız Azure'daki Node.js uygulamanızın, iisnode'un dinlediği varsayılan bağlantı noktasında web isteklerine yanıt vermesini sağlar.
    >
    >

7. ./package.json dosyasını açın ve [istediğiniz Node.js sürümünü belirtmek üzere](#version) `engines` özelliğini ekleyin.
   
        "engines": {
            "node": "6.9.1"
        }, 
8. Yaptığınız değişiklikleri kaydedin, uygulamanızı Azure'a dağıtmak için git'i kullanın. İstendiğinde önceden oluşturduğunuz kullanıcı kimlik bilgilerini kullanın.
   
        git add .
        git add -f config
        git commit -m "{your commit message}"
        git push azure master
   
    Hızlı oluşturucu zaten bir .gitignore dosyası sağlar; bu nedenle `git push` node_modules/ dizinine yüklemeye çalışırken bant genişliği kullanmaz.
9. Son olarak, dinamik Azure uygulamanızı tarayıcıda başlatın:
   
        azure site browse
   
    Artık, Node.js web uygulamanızın Azure App Service’te çalıştığını görmelisiniz.
   
    ![Dağıtılan bir uygulama için gözatma örneği.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Node.js web uygulamanızı güncelleştirme
App Service’te çalışan Node.js web uygulamasına güncelleştirmeler yapmak için web uygulamanızı ilk dağıttığınızda yaptığınız gibi `git add`, `git commit` ve `git push` çalıştırmanız yeterlidir.

## <a name="how-app-service-deploys-your-nodejs-app"></a>App Service Node.js uygulamanızı nasıl dağıtır?
Azure App Service Node.js uygulamalarını çalıştırmak için [iisnode] kullanır. Komut satırında Node.js uygulamaları geliştirdiğinizde ve dağıttığınızda size kolaylaştırılmış bir deneyim sunmak için, Azure CLI ve Kudu altyapısı (Git dağıtımı) birlikte çalışır. 

* `azure site create --git`, server.js ya da app.js’nin ortak Node.js düzenini tanır ve kök dizininizde bir iisnode.yml oluşturur. Bu dosyayı iisnode’u özelleştirmek için kullanabilirsiniz.
* `git push azure master` komutunda, Kudu aşağıdaki dağıtım görevlerini otomatik hale getirir:
  
  * Package.json depo kök dizininde ise, `npm install --production` komutunu çalıştırın.
  * Package.json’da başlangıç komut dosyanıza işaret eden bir Web.config oluşturun (örn, server.js ya da app.js).
  * Node-Inspector ile hata ayıklamak için, Web.config’i uygulamanızı hazırlamak üzere özelleştirin.

## <a name="use-a-nodejs-framework"></a>Node.js altyapısı kullanma
Uygulama geliştirmek için [Sails.js][SAILSJS] veya [MEAN.js][MEANJS] gibi popüler bir Node.js altyapısı kullanıyorsanız, bunları App Service'e dağıtabilirsiniz. Popüler Node.js altyapıları kendi özel quirk’lerine sahiptir ve bunların paket bağımlılıkları sürekli güncelleştirilmektedir. Bununla birlikte, uygulamanızda neler olduğunu bilmeniz ve uygun değişiklikleri yapabilmeniz için, App Service stdout ve stderr günlüklerini kullanımınıza sunar. Daha fazla bilgi için bkz. [iisnode’dan stdout ve stderr günlüklerini alma](#iisnodelog).

Aşağıdaki öğreticiler App Service’te belirli bir altyapıyla nasıl çalışacağınızı gösterir:

* [Azure Uygulama Hizmeti’ne Sails.js web uygulaması dağıtma]
* [Azure Uygulama Hizmeti’nde Socket.IO ile bir Node.js sohbet uygulaması oluşturma]
* [Azure Uygulama Hizmeti Web Apps ile io.js kullanma]

<a name="version"></a>

## <a name="use-a-specific-nodejs-engine"></a>Belirli bir Node.js altyapısını kullanma
Tipik iş akışınızda, normalde package.json’da yapabildiğiniz gibi App Service’e belirli bir Node.js altyapısını kullanmasını söyleyebilirsiniz.
Örneğin:

    "engines": {
        "node": "6.9.1"
    }, 

Kudu dağıtım altyapısı aşağıdaki sırayla, hangi Node.js altyapısının kullanılacağını belirler:

* İlk olarak, `nodeProcessCommandLine` belirtilmiş olup olmadığını görmek için iisnode.yml’ye bakın. Belirtilmişse, bunu kullanın.
* Sonra, `engines` nesnesinde `"node": "..."` belirtilmiş olup olmadığını görmek için package.json’a bakın. Belirtilmişse, bunu kullanın.
* Varsayılan olarak, varsayılan bir Node.js sürümünü seçin.

App Service’te desteklenen tüm Node.js/NPM sürümlerinin güncel listesine uygulamanız için şu URL'den ulaşabilirsiniz:

    https://<app_name>.scm.azurewebsites.net/api/diagnostics/runtime

> [!NOTE]
> İstediğiniz Node.js altyapısını açıkça tanımlamanız önerilir. Varsayılan Node.js sürümü değişebilir ve varsayılan Node.js sürümü uygulamanıza uygun olmadığı için Azure web uygulamanızda hatalar alabilirsiniz.
> 
> 

<a name="iisnodelog"></a>

## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>iisnode’dan stdout ve stder günlüklerini alma
iisnode günlüklerini okumak için aşağıdaki adımları izleyin.

> [!NOTE]
> Bu adımları tamamladıktan sonra, bir hata gerçekleşene kadar günlük dosyaları oluşmayabilir.
> 
> 

1. Azure CLI’nin sağladığı iisnode.yml dosyasını açın.
2. Aşağıdaki iki parametreyi ayarlayın: 
   
        loggingEnabled: true
        logDirectory: iisnode
   
    Bunlar birlikte, App Service’te iisnode’a stdout ve stderror çıktısını D:\home\site\wwwroot\**iisnode** dizinine yerleştirmesini söyler.
3. Yaptığınız değişiklikleri kaydedin ve aşağıdaki Git komutları ile değişikliklerinizi Azure'a gönderin:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode artık yapılandırılmıştır. Sonraki adımlar, bu günlüklere nasıl erişeceğinizi gösterir.
4. Tarayıcınızda, aşağıdaki konumda bulunan, uygulamanızın Kudu hata ayıklama konsoluna erişin:
   
        https://{appname}.scm.azurewebsites.net/DebugConsole 
   
    Bu URL "*.scm.*" eklentisi nedeniyle web uygulaması URL’sinden farklıdır dikkat edin. Bunu URL’ye eklemeyi atlarsanız, 404 hatası alırsınız.
5. D:\home\site\wwwroot\iisnode konumuna gidin
   
    ![iisnode günlük dosyalarının konumuna gitme][iislog-kudu-console-find]
6. Okumak istediğiniz günlük için **Düzenle** simgesine tıklayın. İsterseniz, **İndir** veya **Sil**’e de tıklayabilirsiniz.
   
    ![iisnode günlük dosyasını açma.][iislog-kudu-console-open]
   
    Artık, App Service dağıtımınızda hata ayıklamaya yardımcı olması için günlüğü görebilirsiniz.
   
    ![iisnode günlük dosyasını inceleme][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Node-Inspector ile uygulamanızın hatalarını ayıklama
Node.js uygulamalarınızın hatalarını ayıklamak için Node-Inspector kullanıyorsanız, bunu dinamik App Service uygulamanız için kullanabilirsiniz. Node-Inspector App Service için iisnode yüklemesinde önceden yüklenir. Ve Git aracılığıyla dağıtırsanız, Kudu’da otomatik olarak oluşturulan Web.config zaten Node-Inspector’ı etkinleştirmek için gereken tüm yapılandırmaları içerir.

Node-Inspector’ı etkinleştirmek için aşağıdaki adımları izleyin:

1. Depo kökünüzde iisnode.yml’yi açın ve aşağıdaki parametreleri belirtin: 
   
        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll
2. Yaptığınız değişiklikleri kaydedin ve aşağıdaki Git komutları ile değişikliklerinizi Azure'a gönderin:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
3. Şimdi, yalnızca URL'ye eklenen /debug ile package.json’ınızdaki başlangıç betiği tarafından belirtilen, uygulamanızın başlangıç dosyasını gidin. Örneğin,
   
        http://{appname}.azurewebsites.net/server.js/debug
   
    Veya
   
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Diğer kaynaklar
* [Bir Azure uygulamasında Node.js sürümünü belirtme](../nodejs-specify-node-version-azure-apps.md)
* [Azure'da Node.js uygulamaları için en iyi yöntemler ve sorun giderme kılavuzu](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
* [Azure Uygulama Hizmeti’ndeki bir Node.js web uygulamasına hata ayıklama](web-sites-nodejs-debug.md)
* [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
* [Azure Uygulama Hizmeti Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
* [Node.js Geliştirici Merkezi](/develop/nodejs/)
* [Azure Uygulama Hizmeti’nde web uygulamalarını kullanmaya başlama](app-service-web-get-started.md)
* [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Azure Uygulama Hizmeti’nde Socket.IO ile bir Node.js sohbet uygulaması oluşturma]: ./web-sites-nodejs-chat-app-socketio.md
[Azure Uygulama Hizmeti’ne Sails.js web uygulaması dağıtma]: ./app-service-web-nodejs-sails.md
[Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Yeoman için Hızlı oluşturucu]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Azure Uygulama Hizmeti Web Apps ile io.js kullanma]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[ücretsiz deneme için kaydolabilir]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png

