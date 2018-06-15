---
title: Linux üzerinde Azure App Service’te bir Node.js ve MongoDB web uygulaması derleme | Microsoft Docs
description: Linux üzerinde Azure App Service'te MongoDB bağlantı dizesine sahip Cosmos DB veritabanı bağlantısıyla bir Node.js uygulamasının nasıl çalıştırılacağı hakkında bilgi edinin.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: syntaxc4
editor: ''
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ee7f37f83d6b3503df1af61509f6f85ca19bc13e
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
ms.locfileid: "29932754"
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service’te bir Node.js ve MongoDB web uygulaması derleme

> [!NOTE]
> Bu makalede bir uygulamanın Linux üzerinde App Service'e dağıtımı yapılır. _Windows_'da App Service dağıtmak için bkz. [Azure'da Node.js ve MongoDB web uygulaması derleme](../app-service-web-tutorial-nodejs-mongodb-app.md).
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu öğreticide Node.js web uygulaması oluşturma, bu uygulamayı yerel olarak MongoDB veritabanına bağlama, ardından MongoDB API'sini kullanarak CosmosDB veritabanı bağlantısıyla Azure'a dağıtma işlemleri gösterilir. İşiniz bittiğinde, Linux üzerinde App Service’te çalışan bir MEAN uygulamanız (MongoDB, Express, AngularJS ve Node.js) olacaktır. Kolaylık olması için örnek uygulama [MEAN.js web çerçevesi](http://meanjs.org/)’ni kullanır.

![Azure App Service’te çalışan MEAN.js uygulaması](./media/tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Azure'da MongoDB API'sini kullanarak CosmosDB veritabanı oluşturma
> * Node.js uygulamasını MongoDB’ye bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki tanılama günlüklerinin akışını sağlama
> * Uygulamayı Azure portalında yönetme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Node.js v6.0 veya üstünü ve NPM'yi yükleyin](https://nodejs.org/)
1. [Gulp.js'yi yükleyin](http://gulpjs.com/) ([MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started) için gerekir)
1. [MongoDB Community Edition’ı yükleyin ve çalıştırın](https://docs.mongodb.com/manual/administration/install-community/)

## <a name="test-local-mongodb"></a>Yerel MongoDB’yi test etme

Terminal penceresini açın ve MongoDB yüklemenizin `bin` dizinine `cd` yazın. Bu öğreticideki tüm komutları çalıştırmak için bu terminal penceresini kullanabilirsiniz.

Yerel MongoDB sunucunuza bağlanmak için terminalde `mongo` komutunu çalıştırın.

```bash
mongo
```

Bağlantınız başarılı olursa, MongoDB veritabanınız zaten çalışıyor demektir. Aksi takdirde, yerel MongoDB veritabanınızın [MongoDB Community Edition’ı yükleme](https://docs.mongodb.com/manual/administration/install-community/) konusundaki adımları izleyerek başlatıldığından emin olun. Bazen MongoDB yüklenmiş olsa da, `mongod` komutunu çalıştırarak başlatmak gerekir.

MongoDB veritabanınızı test etmeyi bitirdiğinizde, terminale `Ctrl+C` yazın.

## <a name="create-local-nodejs-app"></a>Yerel Node.js uygulaması oluşturma

Bu adımda, yerel Node.js projesini ayarlarsınız.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresinde, `cd` ile bir çalışma dizinine gidin.

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Bu örnek depo, [MEAN.js deposu](https://github.com/meanjs/mean)’nun bir kopyasını içerir. App Service’te çalıştırılması için değiştirilmiştir (daha fazla bilgi için MEAN.js deposu [BENİOKU dosyasını](https://github.com/Azure-Samples/meanjs/blob/master/README.md) görüntüleyin).

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Gerekli paketleri yüklemek ve uygulamayı başlatmak için aşağıdaki komutları çalıştırın.

```bash
cd meanjs
npm install
npm start
```

Config.domain uyarısını yoksayın. Uygulama tam olarak yüklendiğinde, şu iletiye benzer bir şey görürsünüz:

```txt
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

Bir tarayıcıda `http://localhost:3000` sayfasına gidin. Üst menüde **Kaydol**’a tıklayın ve bir test kullanıcısı oluşturun. 

MEAN.js örnek uygulaması, kullanıcı verilerini veritabanında depolar. Kullanıcı oluşturma ve oturum açmada başarılı olursanız, uygulamanız yerel MongoDB veritabanına veri yazıyor demektir.

![MEAN.js, MongoDB’ye başarıyla bağlanır](./media/tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Birkaç makale eklemek için **Yönetici > Makaleleri Yönet**’i seçin.

Node.js’yi dilediğiniz zaman durdurmak için, terminalde `Ctrl+C` tuşlarına basın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-mongodb"></a>Üretim MongoDB’si oluşturma

Bu adımda, Azure’da bir MongoDB veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

MongoDB için bu öğreticide [Azure Cosmos DB](/azure/documentdb/) kullanılır. Cosmos DB, MongoDB istemci bağlantılarını destekler.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma

Cloud Shell'de, [`az cosmosdb create`](/cli/azure/cosmosdb?view=azure-cli-latest#az_cosmosdb_create) komutuyla bir Cosmos DB hesabı oluşturun.

Aşağıdaki komutta, *\<cosmosdb_name>* yer tutucusunu benzersiz bir Cosmos DB adıyla değiştirin. Bu ad, Cosmos DB uç noktasının bir parçası olan `https://<cosmosdb_name>.documents.azure.com/` olarak kullanıldığından, adın Azure’daki tüm Cosmos DB hesaplarında benzersiz olması gerekir. Ad yalnızca küçük harf, rakam ve tire (-) karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır.

```azurecli-interactive
az cosmosdb create --name <cosmosdb_name> --resource-group myResourceGroup --kind MongoDB
```

*--kind MongoDB* parametresi MongoDB istemci bağlantılarını etkinleştirir.

Cosmos DB hesabı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies":
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-to-production-mongodb"></a>Üretim MongoDB’sine uygulama bağlama

Bu adımda, MEAN.js örnek uygulamanızı, MongoDB bağlantı dizesi kullanarak yeni oluşturduğunuz Cosmos DB veritabanına bağlayacaksınız.

### <a name="retrieve-the-database-key"></a>Veritabanı anahtarını alma

Cosmos DB veritabanına bağlanmak için veritabanı anahtarı gerekir. Cloud Shell'de, birincil anahtarı almak için [`az cosmosdb list-keys`](/cli/azure/cosmosdb?view=azure-cli-latest#az_cosmosdb_list_keys) komutunu kullanın.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Azure CLI aşağıdaki örneğe benzer bilgiler görüntüler:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

`primaryMasterKey` değerini kopyalayın. Bu bilgiler sonraki adımda gerekli olacaktır.

<a name="devconfig"></a>

### <a name="configure-the-connection-string-in-your-nodejs-application"></a>Node.js uygulamanızda bağlantı dizesini yapılandırma

Yerel MEAN.js deponuzda, _config/env/_ klasöründe _local-production.js_ adlı bir dosya oluşturun. Bu dosyayı deponun dışında tutmak için _.gitignore_ yapılandırılır.

Aşağıdaki kodu dosyanın içine kopyalayın. İki *\<cosmosdb_name>* yer tutucusunu Cosmos DB veritabanı adınız ile, *\<primary_master_key>* yer tutucusunu da önceki adımda kopyaladığınız anahtar ile değiştirdiğinizden emin olun.

```javascript
module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false'
  }
};
```

[Cosmos DB, SSL gerektirdiğinden](../../cosmos-db/connect-mongodb-account.md#connection-string-requirements) `ssl=true` seçeneği gereklidir.

Yaptığınız değişiklikleri kaydedin.

### <a name="test-the-application-in-production-mode"></a>Uygulamayı üretim modunda test etme

Yerel terminal penceresinde, üretim ortamı için şu komutu çalıştırarak betikleri küçültün ve paketleyin. Bu işlem, üretim ortamı tarafından gerekli olan dosyaları oluşturur.

```bash
gulp prod
```

Yerel terminal penceresinde, _config/env/local-production.js_ konumunda yapılandırdığınız bağlantı dizesini kullanmak için şu komutu çalıştırın. Sertifika hatasını ve config.domain uyarısını yoksayın.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production`, Node.js’ye üretim ortamında çalışmasını söyleyen ortam değişkenini ayarlar.  `node server.js`, Node.js sunucusunu `server.js` ile depo kökünüzde başlatır. Node.js uygulamanız Azure’a bu şekilde yüklenir.

Uygulama yüklendiğinde, üretim ortamında çalıştığından emin olmak için kontrol edin:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Bir tarayıcıda `http://localhost:8443` sayfasına gidin. Üst menüde **Kaydol**’a tıklayın ve bir test kullanıcısı oluşturun. Kullanıcı oluşturma ve oturum açmada başarılı olursanız, uygulamanız Azure’da Cosmos DB veritabanına veri yazıyor demektir.

Terminalde `Ctrl+C` yazarak Node.js’yi durdurun.

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu adımda, MongoDB’ye bağlı Node.js uygulamanızı Azure App Service’e dağıtacaksınız.

### <a name="configure-local-git-deployment"></a>Yerel git dağıtımını yapılandırma

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

<a name="create"></a>

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-nodejs-linux-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Ortam değişkeni yapılandırma

Varsayılan olarak, MEAN.js projesi _config/env/local-production.js_ öğesini Git deposu dışında tutar. Azure web uygulamanız için, uygulama ayarlarını kullanarak MongoDB bağlantı dizenizi tanımlayın.

Uygulama ayarlarını belirlemek için Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanın.

Aşağıdaki örnekte, Azure web uygulamanız için bir `MONGODB_URI` uygulama ayarı yapılandırılmıştır. *\<app_name>*, *\<cosmosdb_name>* ve *\<primary_master_key>* yer tutucularını değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

Node.js kodunda, her ortam değişkenine eriştiğiniz gibi bu uygulama ayarına da `process.env.MONGODB_URI` ile erişirsiniz. 

Yerel MEAN.js deponuzda, üretim ortamına özel yapılandırma içeren _config/env/production.js_ (_config/env/local-production.js_ değil) dosyasını açın. Varsayılan MEAN.js uygulaması, zaten `MONGODB_URI` ortam değişkenini kullanmak üzere yapılandırılmıştır.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

Dağıtım işleminin `npm install` komutundan sonra [Gulp](http://gulpjs.com/)’ı çalıştırdığını fark etmişsinizdir. App Service, dağıtım sırasında Gulp veya Grunt görevlerini çalıştırmadığı için bu örnek depoyu etkinleştirmek üzere kök dizinde iki ek dosya bulunur:

- _.deployment_ - Bu dosya, App Service’ten özel dağıtım betiği olarak `bash deploy.sh` komutunu çalıştırmasını ister.
- _deploy.sh_ - Özel dağıtım betiği. Dosyayı gözden geçirirseniz, `npm install` ve `bower install` komutundan sonra `gulp prod` çalıştırdığını görürsünüz.

Git tabanlı dağıtımınıza herhangi bir adım eklemek için bu yaklaşımı kullanabilirsiniz. Azure web uygulamanızı herhangi bir noktada yeniden başlatırsanız, App Service bu otomasyon görevlerini yeniden çalıştırmaz.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

Web tarayıcınızı kullanarak dağıtılan web uygulamasına göz atın.

```bash
http://<app_name>.azurewebsites.net
```

Üst menüde **Kaydol**’a tıklayın ve bir işlevsiz kullanıcı oluşturun.

Başarılı olursanız ve uygulama otomatik olarak oluşturulan kullanıcıda oturum açarsa, Azure'daki MEAN.js uygulamanızın Cosmos DB veritabanı MongoDB API'sine bağlantısı vardır.

![Azure App Service’te çalışan MEAN.js uygulaması](./media/tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Birkaç makale eklemek için **Yönetici > Makaleleri Yönet**’i seçin.

**Tebrikler!** Linux üzerinde Azure App Service’te veri temelli bir Node.js uygulaması çalıştırıyorsunuz.

## <a name="update-data-model-and-redeploy"></a>Veri modelini güncelleştirme ve yeniden dağıtma

Bu adımda, `article` veri modelini değiştiriyorsunuz ve değişikliklerinizi Azure'da yayımlıyorsunuz.

### <a name="update-the-data-model"></a>Veri modelini güncelleştirme

Yerel MEAN.js deponuzda, _modules/articles/server/models/article.server.model.js_ dosyasını açın.

`ArticleSchema` içinde, `comment` adlı bir `String` türü ekleyin. İşiniz bittiğinde, şema kodunuz şu şekilde görünür:

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-the-articles-code"></a>Makaleler kodunu güncelleştirme

`comment` kullanmak için `articles` kodunuzun kalanını güncelleştirin.

Değiştirmeniz gereken beş dosya vardır: sunucu denetleyici ve dört istemci görünümü. 

_modules/articles/server/controllers/articles.server.controller.js_ dosyasını açın.

`update` işlevinde, `article.comment` için bir atama ekleyin. Şu kod tamamlanmış `update` işlevini gösterir:

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

_modules/articles/client/views/view-article.client.view.html_ dosyasını açın.

`</section>` kapanış etiketinin hemen üzerinde, `comment` öğesini makale verilerinin geri kalanı ile birlikte görüntülemek için şu satırı ekleyin:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

_modules/articles/client/views/list-articles.client.view.html_ dosyasını açın.

`</a>` kapanış etiketinin hemen üzerinde, `comment` öğesini makale verilerinin geri kalanı ile birlikte görüntülemek için şu satırı ekleyin:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

_modules/articles/client/views/admin/list-articles.client.view.html_ dosyasını açın.

`<div class="list-group">` öğesinin içinde ve `</a>` kapanış etiketinin hemen üzerinde, `comment` öğesini makale verilerinin geri kalanıyla birlikte görüntülemek için şu satırı ekleyin:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

_modules/articles/client/views/admin/form-article.client.view.html_ öğesini açın.

Gönder düğmesini içeren ve şuna benzeyen `<div class="form-group">` öğesini bulun:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Bu etiketin hemen üzerinde, kişilerin `comment` alanını düzenleyebilmesini sağlayan başka bir `<div class="form-group">` öğesi ekleyin. Yeni öğeniz şöyle görünmelidir:

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a>Değişikliklerinizi yerel olarak test etme

Yaptığınız tüm değişiklikleri kaydedin.

Yerel terminal penceresinde, değişikliklerinizi üretim modunda yeniden test edin.

```bash
gulp prod
NODE_ENV=production node server.js
```

Bir tarayıcıda `http://localhost:8443` konumuna gidin ve oturum açtığınızdan emin olun.

**Yönetici > Makaleleri Yönet**’i seçin, ardından **+** düğmesini seçerek bir makale ekleyin.

Artık yeni `Comment` metin kutusunu görebilirsiniz.

![Makalelere eklenen açıklama alanı](./media/tutorial-nodejs-mongodb-app/added-comment-field.png)

Terminalde `Ctrl+C` yazarak Node.js’yi durdurun.

### <a name="publish-changes-to-azure"></a>Değişiklikleri Azure’da yayımlama

Değişikliklerinizi git’e işleyin, ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "added article comment"
git push azure master
```

`git push` tamamlandığında, Azure web uygulamanıza gidin ve yeni işlevleri deneyin.

![Azure’da yayımlanan model ve veritabanı değişiklikleri](media/tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Önceden makale eklediyseniz, bunları yine de görüntüleyebilirsiniz. Cosmos DB’nizdeki mevcut veriler kaybolmaz. Ayrıca, veri şemasına yaptığınız güncelleştirmeler de kaybolmadığı gibi, mevcut verileriniz olduğu gibi bırakılır.

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını görmek için [Azure portalına](https://portal.azure.com) gidin.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/tutorial-nodejs-mongodb-app/access-portal.png)

Portal, varsayılan olarak web uygulamanızın **Genel Bakış** sayfasında görünür. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Azure'da MongoDB API'sini kullanarak CosmosDB veritabanı oluşturma
> * Node.js uygulamasını MongoDB’ye bağlama
> * Uygulamayı Azure’da dağıtma
> * Veri modelini güncelleştirme ve uygulamayı yeniden dağıtma
> * Azure’daki günlüklerin terminalinize akışını sağlama
> * Uygulamayı Azure portalında yönetme

Web uygulamanıza özel bir DNS adı eşlemeyle ilgili bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service-web-tutorial-custom-domain.md)
