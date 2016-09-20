<properties
    pageTitle="Azure App Service’te Node.js web uygulamalarını kullanmaya başlama"
    description="Azure App Service’te Node.js uygulamasını bir web uygulamasına dağıtmayı öğrenin."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# Azure App Service’te Node.js web uygulamalarını kullanmaya başlama

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğreticide [Azure App Service]’te cmd.exe veya bash gibi bir komut satırı ortamında nasıl basit bir [Node.js][NODEJS] uygulaması oluşturulacağı ve bir [web uygulamasına] dağıtılacağı gösterilmektedir. Bu öğreticideki yönergeler Node.js çalıştırabilen tüm işletim sistemlerinde izlenebilir.

<a name="prereq"></a>
## Ön koşullar

- **Node.js** ([Yüklemek için buraya tıklayın][NODEJS])
- **Bower** ([Yüklemek için buraya tıklayın][BOWER])
- **Yeoman** ([Yüklemek için buraya tıklayın][YEOMAN])
- **Git** ([Yüklemek için buraya tıklayın][GIT])
- **Azure CLI** ([Yüklemek için buraya tıklayın][Azure CLI])
- Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir] veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.]

## Basit bir Node.js web uygulamasına oluşturma ve dağıtma

1. Seçtiğiniz komut satırı terminalini açın ve [Yeoman için Hızlı oluşturucu]’yu yükleyin.

        npm install -g generator-express

2. `CD` edin (çalışan bir dizine) ve aşağıdaki söz dizimini kullanarak hızlı bir uygulama oluşturun:

        yo express
        
    İstendiğinde aşağıdaki seçenekleri belirleyin:  

    `? Would you like to create a new directory for your project?` **Yes**  
    `? Enter directory name` **{uygulama adı}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **None**  
    `? Select a database to use:` **None**  
    `? Select a build tool to use:` **Grunt**

3. `CD` edin (yeni uygulamanızın kök dizinine) ve geliştirme ortamınızda çalıştığından emin olmak için başlatın:

        npm start

    Tarayıcınızda, Express giriş sayfasını görebildiğinizden emin olmak için <http://localhost:3000> adresine gidin. Uygulamanızın düzgün çalıştığını doğruladıktan sonra, durdurmak için `Ctrl-C` kullanın.
    
1. ASM moduna geçiş yapın ve Azure’da oturum açın (bunun için [Azure CLI](#prereq) gerekir):

        azure config mode asm
        azure login

    Azure aboneliğinizin bulunduğu bir Microsoft hesabıyla tarayıcıda oturum açmaya devam etmek için istemi izleyin.

2. Hala uygulamanızın kök dizininde olduğunuzdan emin olun, sonra Azure’da sonraki komutla benzersiz bir uygulama adına sahip App Service uygulama kaynağı oluşturun; örneğin, http://{uygulama adı}.azurewebsites.net

        azure site create --git {appname}

    Dağıtım yapılacak Azure bölgesini seçmek için istemi izleyin. Azure aboneliğiniz için daha önce Git/FTP dağıtımı kimlik bilgileri oluşturmadıysanız, bunların oluşturmanız da istenir.

3. Uygulamanızın kökünde./config/config.js dosyasını açın ve üretim bağlantı noktasını `process.env.port` olarak değiştirin; `config` nesnesi içindeki `production` özelliği aşağıdaki örnekteki gibi görünmelidir.

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Bu, Node.js uygulamanızın, iisnode’un dinlediği varsayılan bağlantı noktasında web isteklerine yanıt vermesini sağlar.
    
4. Yaptığınız değişiklikleri kaydedin, uygulamanızı Azure’a dağıtmak için git’i kullanın:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Hızlı oluşturucu zaten bir .gitignore dosyası sağlar, bu nedenle, `git push` node_modules/ dizinini yüklemeye çalışırken bant genişliği tüketmezsiniz.

5. Son olarak, dinamik Azure uygulamanızı tarayıcıda başlatın:

        azure site browse

    Artık, Node.js web uygulamanızın Azure App Service’te çalıştığını görmelisiniz.
    
    ![Dağıtılan bir uygulama için gözatma örneği.][deployed-express-app]

## Node.js web uygulamanızı güncelleştirme

App Service’te çalışan Node.js web uygulamasına güncelleştirmeler yapmak için web uygulamanızı ilk dağıttığınızda yaptığınız gibi `git add`, `git commit` ve `git push` çalıştırmanız yeterlidir.
     
## App Service Node.js uygulamanızı nasıl dağıtır?

Azure App Service Node.js uygulamalarını çalıştırmak için [iisnode] kullanır. Komut satırında Node.js uygulamaları geliştirdiğinizde ve dağıttığınızda size kolaylaştırılmış bir deneyim sunmak için, Azure CLI ve Kudu altyapısı (Git dağıtımı) birlikte çalışır. 

- `azure site create --git` server.js ya da app.js’nin ortak Node.js düzenini tanır ve kök dizininizde bir iisnode.yml oluşturur. Bu dosyayı iisnode’u özelleştirmek için kullanabilirsiniz.
- `git push azure master` komutunda, Kudu aşağıdaki dağıtım görevlerini otomatik hale getirir:

    - Package.json depo kök dizininde ise, `npm install --production` komutunu çalıştırın.
    - Package.json’da başlangıç komut dosyanıza işaret eden bir Web.config oluşturun (örn, server.js ya da app.js).
    - Node-Inspector ile hata ayıklamak için, Web.config’i uygulamanızı hazırlamak üzere özelleştirin.
    
## Node.js altyapısı kullanma

Uygulama geliştirmek için [Sails.js][SAILSJS] veya [MEAN.js][MEANJS] gibi popüler bir Node.js altyapısı kullanıyorsanız, bunları App Service’e dağıtabilirsiniz. Popüler Node.js altyapıları kendi özel quirk’lerine sahiptir ve bunların paket bağımlılıkları sürekli güncelleştirilmektedir. Bununla birlikte, uygulamanızda neler olduğunu bilmeniz ve uygun değişiklikleri yapabilmeniz için, App Service stdout ve stderr günlüklerini kullanımınıza sunar. Daha fazla bilgi için bkz. [iisnode’dan stdout ve stderr günlüklerini alma](#iisnodelog).

Aşağıdaki öğreticiler App Service’te belirli bir altyapıyla nasıl çalışacağınızı gösterir:

- [Azure App Service’e Sails.js web uygulaması dağıtma]
- [Azure App Service’te Socket.IO ile bir Node.js sohbet uygulaması oluşturma]
- [Azure App Service Web Apps ile io.js kullanma]

## Belirli bir Node.js altyapısını kullanma

Tipik iş akışınızda, normalde package.json’da yapabildiğiniz gibi App Service’e belirli bir Node.js altyapısını kullanmasını söyleyebilirsiniz.
Örneğin:

    "engines": {
        "node": "5.5.0"
    }, 

Kudu dağıtım altyapısı aşağıdaki sırayla, hangi Node.js altyapısının kullanılacağını belirler:

- İlk olarak, `nodeProcessCommandLine` belirtilmiş olup olmadığını görmek için iisnode.yml’ye bakın. Belirtilmişse, bunu kullanın.
- Sonra, `engines` nesnesinde `"node": "..."` belirtilmiş olup olmadığını görmek için package.json’a bakın. Belirtilmişse, bunu kullanın.
- Varsayılan olarak, varsayılan bir Node.js sürümünü seçin.

<a name="iisnodelog"></a>
## iisnode’dan stdout ve stder günlüklerini alma

iisnode günlüklerini okumak için aşağıdaki adımları kullanın.

> [AZURE.NOTE] Bu adımları tamamladıktan sonra, bir hata gerçekleşene kadar günlük dosyaları oluşmayabilir.

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

    Bu URL’nin DNS adına "*.scm.*" eklentisiyle web uygulaması URL’sinden farklı olduğuna dikkat edin. Bunu URL’ye eklemeyi atlarsanız, 404 hatası alırsınız.

5. D:\home\site\wwwroot\iisnode konumuna gidin

    ![iisnode günlük dosyalarının konumuna gitme][iislog-kudu-console-find]

6. Okumak istediğiniz günlük için **Düzenle** simgesine tıklayın. İsterseniz, **İndir** veya **Sil**’e de tıklayabilirsiniz.

    ![iisnode günlük dosyasını açma.][iislog-kudu-console-open]

    Artık, App Service dağıtımınızda hata ayıklamaya yardımcı olması için günlüğü görebilirsiniz.
    
    ![iisnode günlük dosyasını inceleme][iislog-kudu-console-read]

## Node-Inspector ile uygulamanızın hatalarını ayıklama

Node.js uygulamalarınızın hatalarını ayıklamak için Node-Inspector kullanıyorsanız, bunu dinamik App Service uygulamanız için kullanabilirsiniz. Node-Inspector App Service için iisnode yüklemesinde önceden yüklenir. Ve Git aracılığıyla dağıtırsanız, Kudu’da otomatik olarak oluşturulan Web.config zaten Node-Inspector’ı etkinleştirmek için gereken tüm yapılandırmaları içerir.

Node-Inspector’ı etkinleştirmek için aşağıdaki adımları izleyin:

1. Depo kökünüzde iisnode.yml’yi açın ve aşağıdaki parametreleri belirtin: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Yaptığınız değişiklikleri kaydedin ve aşağıdaki Git komutları ile değişikliklerinizi Azure'a gönderin:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Şimdi, yalnızca URL'ye eklenen /debug ile package.json’ınızdaki başlangıç betiği tarafından belirtilen, uygulamanızın başlangıç dosyasını gidin. Örneğin,

        http://{appname}.azurewebsites.net/server.js/debug
    
    Veya
    
        http://{appname}.azurewebsites.net/app.js/debug

## Diğer kaynaklar

- [Bir Azure uygulamasında Node.js sürümünü belirtme](../nodejs-specify-node-version-azure-apps.md)
- [Azure'da Node.js uygulamaları için en iyi yöntemler ve sorun giderme kılavuzu](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Azure App Service’teki bir Node.js web uygulamasına hata ayıklama](web-sites-nodejs-debug.md)
- [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
- [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js Geliştirici Merkezi](/develop/nodejs/)
- [Azure App Service’te web uygulamalarını kullanmaya başlama](app-service-web-get-started.md)
- [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.]: http://go.microsoft.com/fwlink/?LinkId=623901
[BOWER]: http://bower.io/
[Azure App Service’te Socket.IO ile bir Node.js sohbet uygulaması oluşturma]: ./web-sites-nodejs-chat-app-socketio.md
[Azure App Service’e Sails.js web uygulaması dağıtma]: ./app-service-web-nodejs-sails.md
[Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Yeoman için Hızlı oluşturucu]: https://github.com/petecoop/generator-express
[GIT]: http://www.git-scm.com/downloads
[Azure App Service Web Apps ile io.js kullanma]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[NODEJS]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[ücretsiz deneme için kaydolabilir]: http://go.microsoft.com/fwlink/?LinkId=623901
[web uygulamasına]: ./app-service-web-overview.md
[YEOMAN]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png



<!--HONumber=sep16_HO2-->


