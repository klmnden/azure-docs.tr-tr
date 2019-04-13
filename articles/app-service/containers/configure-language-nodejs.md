---
title: Node.js uygulamaları - Azure App Service'ı yapılandırma | Microsoft Docs
description: Node.js uygulamalarını Azure App Service'te çalışacak şekilde yapılandırma hakkında bilgi edinin
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: 43dc76e6d1e1ec2a6167f1d3e3cc7b8780f843db
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551330"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Bir Linux Node.js uygulamasını Azure App Service için yapılandırma

Node.js uygulamaları ile tüm gerekli NPM bağımlılıkları dağıtılması gerekir. App Service dağıtım Altyapısı'nı (Kudu) otomatik olarak çalıştırır `npm install --production` dağıttığınızda sizin için bir [Git deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), veya bir [Zip paketini](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) ile açık yapı işlemleri. Kullanarak dosyalarınızı dağıtırsanız [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), ancak gerekli paketleri el ile karşıya yüklemeniz gerekir.

Bu kılavuzu temel kavramları ve yerleşik bir Linux kapsayıcı kullanan App Service'te Node.js geliştiricileri için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izleyin [Node.js Hızlı Başlangıç](quickstart-nodejs.md) ve [öğreticide MongoDB ile Node.js](tutorial-nodejs-mongodb-app.md) ilk.

## <a name="show-nodejs-version"></a>Node.js sürümü göster

Geçerli Node.js sürümü göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Tüm desteklenen Node.js sürümler göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Node.js sürümünü ayarlama

Uygulamanızı ayarlamak için bir [Node.js sürümünü desteklenen](#show-nodejs-version), aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Bu ayarı kullanmak için çalışma zamanında ve Kudu otomatik paket geri yükleme sırasında hem de Node.js sürümünü belirtir.

> [!NOTE]
> Node.js sürümü projenizin ayarlamalısınız `package.json`. Dağıtım altyapısı, tüm desteklenen Node.js sürümler içeren ayrı bir kapsayıcıda çalışır.

## <a name="configure-nodejs-server"></a>Node.js sunucusunu yapılandırma

Node.js kapsayıcılar ile gelir [PM2](http://pm2.keymetrics.io/), üretim işlem yöneticisi. Uygulamanızı PM2, veya NPM veya özel bir komut başlatmak için yapılandırabilirsiniz.

- [Özel bir komut çalıştırın](#run-custom-command)
- [Npm start çalıştırın](#run-npm-start)
- [PM2 ile çalıştırma](#run-with-pm2)

### <a name="run-custom-command"></a>Özel bir komut çalıştırın

App Service, özel bir komut kullanarak uygulamanızı başlatabilir, ister bir yürütülebilir dosya gibi *run.sh*. Örneğin, çalıştırılacak `npm run start:prod`, aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Npm start çalıştırın

Uygulamayı kullanmaya başlamak üzere `npm start`, yalnızca emin bir `start` betiğidir içinde *package.json* dosya. Örneğin:

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Özel bir kullanılacak *package.json* projenizde, aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>PM2 ile çalıştırma

Sık kullanılan Node.js dosyalardan biri projenizde bulunduğunda kapsayıcı uygulamanızı otomatik olarak PM2 ile başlar:

- *bin/www*
- *Server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Aşağıdakilerden birini [PM2 dosyaları](http://pm2.keymetrics.io/docs/usage/application-declaration/#process-file): *process.json* ve *ecosystem.config.js*

Özel başlangıç dosyası aşağıdaki uzantılar da yapılandırabilirsiniz:

- A *.js* dosyası
- A [PM2 dosya](http://pm2.keymetrics.io/docs/usage/application-declaration/#process-file) uzantılı *.json*, *. config.js*, *.yaml*, veya *.yml*

Özel başlangıç dosyası eklemek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Uzaktan hata ayıklama

> [!NOTE]
> Uzaktan hata ayıklama, şu anda Önizleme aşamasındadır.

Node.js uygulamanızı uzaktan da hatalarını ayıklayabilir [Visual Studio Code](https://code.visualstudio.com/) için yapılandırırsanız [PM2 ile çalıştırma](#run-with-pm2), kullanarak çalıştırdığınızda dışında bir *. config.js, *.yml, veya *.yaml*.

Çoğu durumda, herhangi bir fazladan yapılandırma, uygulamanız için gereklidir. Uygulamanızı çalıştırılırsa bir *process.json* dosyası (varsayılan veya özel) olmalıdır bir `script` JSON kök özelliği. Örneğin:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Uzaktan hata ayıklama için Visual Studio kodu ayarlamak için yükleme [App Service uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Uzantı sayfasındaki yönergeleri izleyin ve Visual Studio Code Azure'da oturum açın.

Azure Gezgini içinde hata ayıklama, sağ tıklayın ve istediğiniz uygulamayı bulun **uzaktan hata ayıklamayı Başlat**. Tıklayın **Evet** uygulamanız için etkinleştirin. App Service, sizin için bir tünel proxy başlar ve hata ayıklayıcı ekler. Ardından, uygulamaya isteklerde ve hata ayıklayıcı kesme noktalarında duraklatma bakın.

Hata ayıklama ile işleminizi tamamladıktan sonra seçerek hata ayıklayıcıyı **Bağlantıyı Kes**. İstendiğinde, tıklatmalısınız **Evet** uzaktan hata ayıklama devre dışı bırakmak için. Daha sonra devre dışı bırakmak için uygulamanızı Azure Gezgini'nde sağ tıklayıp **uzaktan hata ayıklama devre dışı**.

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişme

Uygulama hizmetinde [uygulama ayarlarını belirlemek](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) , uygulama kodunuz dışında. Daha sonra bunları standart Node.js deseni kullanarak erişebilirsiniz. Örneğin, bir uygulama ayarı erişmeye adlı `NODE_ENV`, aşağıdaki kodu kullanın:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Grunt/Bower/Gulp çalıştırın

Varsayılan olarak, Kudu çalışır `npm install --production` zaman tanıdığı bir Node.js uygulaması dağıtılmıştır. Grunt, Bower veya Gulp, gibi popüler Otomasyon araçlardan herhangi birini uygulamanızı gerektiriyorsa, sağlamanız gereken bir [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) çalıştırmak için.

Bu araçları çalıştırmanın deponuzu etkinleştirmek için bunları bağımlılıklara eklemeniz gerekir *package.json.* Örneğin:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Yerel terminal penceresinde, depo kökünüzde dizini değiştirin ve aşağıdaki komutları çalıştırın:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Depo kökünüzde şimdi iki ek dosyalar var: *.deployment* ve *deploy.sh*.

Açık *deploy.sh* ve bulma `Deployment` bölümünde, şuna benzeyen:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Bu bölümde çalışan ile sona erer `npm install --production`. Gerekli aracı çalıştırmak için gereken kod bölümünde eklemek *sonunda* , `Deployment` bölümü:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Bkz: bir [MEAN.js örnek örnekte](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), dağıtım betiğini de özel çalıştığı `npm install` komutu.

### <a name="bower"></a>Bower

Bu kod parçacığı çalıştıran `bower install`.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Bu kod parçacığı çalıştıran `gulp imagemin`.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Bu kod parçacığı çalıştıran `grunt`.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>HTTPS oturumu algılayın

App Service'te [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) tüm HTTPS isteklerini, şifrelenmemiş HTTP istekleri olarak uygulamanızı ulaşmak için Ağ Yük Dengeleyiciler, olur. Uygulama mantığı ihtiyaçlarınızı veya değil, kullanıcı isteklerini şifreli olup olmadığı denetlenecek incelemek, `X-Forwarded-Proto` başlığı.

Popüler web çerçeveleri erişmenizi `X-Forwarded-*` bilgileri, standart uygulama deseni. İçinde [Express](https://expressjs.com/), kullanabileceğiniz [güven proxy'leri](http://expressjs.com/guide/behind-proxies.html). Örneğin:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturum aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Node.js uygulamanız App Service'te farklı davranır ya da hatalı aşağıdakileri deneyin:

- [Günlük akışı erişim](#access-diagnostic-logs).
- Uygulamayı yerel olarak üretim modunda test edin. App Service, Node.js uygulamaları üretim modunda çalışır ve böylece projeniz yerel olarak üretim modunda beklendiği gibi çalıştığından emin olmanız gerekir. Örneğin:
    - Yapılandırmanıza bağlı olarak, *package.json*, farklı paketleri üretim modu için yüklü (`dependencies` karşılaştırması `devDependencies`).
    - Bazı web çerçeveleri, statik dosyalar farklı üretim modunda dağıtabilirsiniz.
    - Bazı web çerçeveleri, üretim modunda çalışırken özel başlatma komut dosyaları kullanabilirsiniz.
- Uygulamanızı App Service'te geliştirme modunda çalıştırın. Örneğin, [MEAN.js](http://meanjs.org/), çalışma zamanı tarafından geliştirme modunda için uygulamanızı ayarlayın [ayarı `NODE_ENV` uygulama ayarı](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MongoDB ile node.js uygulaması](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)