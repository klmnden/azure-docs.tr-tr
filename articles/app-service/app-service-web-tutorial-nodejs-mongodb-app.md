---
title: "Azure'da Node.js ve MongoDB bir web uygulaması oluşturma | Microsoft Docs"
description: "MongoDB bağlantı dizesi Cosmos DB veritabanıyla bağlantı ile Azure içinde çalışma bir Node.js uygulaması alma hakkında bilgi."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: c1c18deb41e16ec57eacd8272094dc418503b0fc
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Azure'da Node.js ve MongoDB bir web uygulaması oluşturma

Azure Web Apps düzeyde ölçeklenebilir, otomatik olarak düzeltme eki uygulama web barındırma hizmeti sağlar. Bu öğretici, Azure'da Node.js web uygulaması oluşturun ve bir MongoDB veritabanına bağlanmak gösterilmiştir. İşiniz bittiğinde, ortalama uygulama (MongoDB, Express, AngularJS ve Node.js) çalışan bir gerekir, [Azure App Service](app-service-web-overview.md). Kolaylık olması için örnek uygulama kullanır [MEAN.js web çerçevesi](http://meanjs.org/).

![Azure App Service’te çalışan MEAN.js uygulaması](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * MongoDB veritabanı oluşturma
> * MongoDB için bir Node.js uygulamasını bağlama
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure Stream tanılama günlükleri
> * Azure portalında uygulama yönetme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
1. [Node.js ve NPM'yi yükleyin](https://nodejs.org/)
1. [Bower yükleme](https://bower.io/) (gerektirdiği [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Gulp.js yükleme](http://gulpjs.com/) (gerektirdiği [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Yükleyip çalıştırabilirsiniz MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="test-local-mongodb"></a>Test yerel MongoDB

Terminal penceresi açın ve `cd` için `bin` MongoDB yüklemenizin dizin. Bu öğreticide tüm komutları çalıştırmak için bu bir terminal penceresi kullanabilirsiniz.

Çalıştırma `mongo` yerel MongoDB sunucusuna bağlanmak için Terminal.

```bash
mongo
```

Bağlantı başarılı olursa, MongoDB veritabanı zaten çalışıyor. Aksi takdirde, yerel MongoDB veritabanı verilen adımları izleyerek başlatıldığından emin olun [MongoDB Community Edition yüklemek](https://docs.mongodb.com/manual/administration/install-community/). Genellikle, MongoDB yüklenir, ancak hala çalıştırarak başlatmak gereken `mongod`. 

Bitirdiğinizde, MongoDB veritabanı sınama, yazın `Ctrl+C` Terminal. 

## <a name="create-local-nodejs-app"></a>Yerel Node.js uygulaması oluşturma

Bu adımda, yerel Node.js projesi ayarlayın.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Terminal penceresinde `cd` bir çalışma dizini için.  

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Bu örnek depo bir kopyasını içeren [MEAN.js depo](https://github.com/meanjs/mean). Uygulama hizmeti çalıştırmak için değiştirdi (MEAN.js depo daha fazla bilgi için bkz [Benioku dosyasını](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Gereken paketleri yüklemek ve uygulamayı başlatmak için aşağıdaki komutları çalıştırın.

```bash
cd meanjs
npm install
npm start
```

Uygulamanın tam olarak yüklendiğinde, aşağıdaki iletiye benzer bir şey görürsünüz:

```console
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

Bir tarayıcıda `http://localhost:3000` sayfasına gidin. Tıklatın **kaydolun** üst menüsünde ve test kullanıcısı oluşturma. 

MEAN.js örnek uygulaması, kullanıcı verilerini veritabanında depolar. Kullanıcı oluşturma ve oturum açma başarılı olursa, uygulamanızı yerel MongoDB veritabanı veri yazıyor.

![MEAN.js, MongoDB’ye başarıyla bağlanır](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Seçin **yönetici > yönetmek makaleleri** bazı makaleler eklemek için.

Herhangi bir zamanda node.js durdurmak için basın `Ctrl+C` Terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-production-mongodb"></a>Üretim MongoDB oluşturma

Bu adımda, Azure MongoDB veritabanı oluşturun. Uygulamanızın Azure'a dağıtıldığında, bu bulut veritabanını kullanır.

MongoDB için Bu öğretici kullanır [Azure Cosmos DB](/azure/documentdb/). Cosmos DB MongoDB istemci bağlantılarını destekler.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma

Cosmos DB hesabıyla bulut Kabuğu'nda oluşturma [az cosmosdb oluşturma](/cli/azure/cosmosdb#create) komutu.

Aşağıdaki komutta için benzersiz bir Cosmos DB ad yerine  *\<cosmosdb_name >* yer tutucu. Bu ad Cosmos DB endpoint parçası olarak kullanılır `https://<cosmosdb_name>.documents.azure.com/`, adının Azure içindeki tüm Cosmos DB hesaplar arasında benzersiz olması gerekir. Ad yalnızca küçük harf, sayı ve tire (-) karakterini içermelidir ve 3 ila 50 karakter uzunluğunda olmalıdır.

```azurecli-interactive
az cosmosdb create --name <cosmosdb_name> --resource-group myResourceGroup --kind MongoDB
```

*--Tür MongoDB* parametresi MongoDB istemci bağlantıları sağlar.

Cosmos DB hesabı oluşturduğunuzda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

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

## <a name="connect-app-to-production-mongodb"></a>Üretim MongoDB uygulamaya Bağlan

Bu adımda, MongoDB bağlantı dizesi kullanarak oluşturduğunuz Cosmos DB veritabanı MEAN.js örnek uygulamanıza bağlayın. 

### <a name="retrieve-the-database-key"></a>Veritabanı anahtarı alma

Cosmos DB veritabanına bağlanmak için veritabanı anahtarı gerekir. Bulut Kabuğu'nda kullanmak [az cosmosdb listesi anahtarlar](/cli/azure/cosmosdb#list-keys) birincil anahtarı almak için komutu.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

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

Yerel MEAN.js deponuzun, içinde _config/env/_ klasör adında bir dosya oluşturun _yerel production.js_. Varsayılan olarak, _.gitignore_ bu dosya deposu dışında tutmak için yapılandırılır. 

Aşağıdaki kodu buraya kopyalayın. İki değiştirdiğinizden emin olun  *\<cosmosdb_name >* Cosmos DB yer tutucularını veritabanı adı ve değiştirme  *\<primary_master_key >* anahtarla yer tutucu, Önceki adımda kopyaladığınız.

```javascript
module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false'
  }
};
```

`ssl=true` Seçeneği, çünkü gereklidir [Cosmos DB SSL gerekir](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Yaptığınız değişiklikleri kaydedin.

### <a name="test-the-application-in-production-mode"></a>Üretim modunda uygulamayı test etme 

Minify ve üretim ortamı için komut dosyalarını paket için aşağıdaki komutu çalıştırın. Bu işlem, üretim ortamı tarafından gerekli olan dosyalar oluşturur.

```bash
gulp prod
```

Yapılandırdığınız bağlantı dizesi kullanmak için aşağıdaki komutu çalıştırın _config/env/local-production.js_.

```bash
# Bash
NODE_ENV=production node server.js

# Windows PowerShell
$env:NODE_ENV = "production" 
node server.js
```

`NODE_ENV=production`Üretim ortamında çalıştırmak için Node.js söyler ortam değişkenini ayarlar.  `node server.js`Node.js sunucusu ile başlar `server.js` depo kök. Azure'da Node.js uygulamanızı nasıl yüklenir budur. 

Uygulama yüklendiğinde, üretim ortamında çalıştığından emin olmak için kontrol edin:

```console
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Bir tarayıcıda `http://localhost:8443` sayfasına gidin. Tıklatın **kaydolun** üst menüsünde ve test kullanıcısı oluşturma. Başarılı bir kullanıcı oluşturma ve oturum açma, uygulamanızı Azure Cosmos DB veritabanına veri yazıyor. 

Terminale yazarak Node.js durdurun `Ctrl+C`. 

## <a name="deploy-app-to-azure"></a>Azure için uygulama dağıtma

Bu adımda, MongoDB bağlı Node.js uygulamanızı Azure App Service'e dağıtın.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-nodejs-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Bir ortam değişkeni yapılandırın

Varsayılan olarak, MEAN.js proje tutar _config/env/local-production.js_ Git deposu dışında. Azure web uygulamanız için uygulama ayarları MongoDB bağlantı dizenizi tanımlamak için kullanın.

Uygulama ayarlarını belirlemek için kullanın [az webapp config appsettings güncelleştirme](/cli/azure/webapp/config/appsettings#update) bulut Kabuğu'nda komutu. 

Aşağıdaki örnek yapılandırır bir `MONGODB_URI` , Azure web uygulaması uygulama ayarı. Değiştir  *\<app_name >*,  *\<cosmosdb_name >*, ve  *\<primary_master_key >* yer tutucuları.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

Bu uygulama ayarı ile erişim node.js kodu `process.env.MONGODB_URI`herhangi bir ortam değişkeni erişim gibi. 

Yerel MEAN.js depoda açmak _config/env/production.js_ (değil _config/env/local-production.js_), üretim ortamında özel yapılandırma içeriyor. Varsayılan MEAN.js uygulama zaten kullanmak üzere yapılandırılmış `MONGODB_URI` oluşturduğunuz ortam değişkeni.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

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

Dağıtım işlemi çalıştığını fark edebilirsiniz [Gulp](http://gulpjs.com/) sonra `npm install`. Uygulama hizmeti dağıtımı sırasında Gulp veya Grunt görevleri çalışmaz, bu örnek deposu, kök dizindeki etkinleştirmek için iki ek dosyaların nedenle: 

- _.Deployment_ -uygulama hizmeti çalıştırmak için bu dosya söyler `bash deploy.sh` özel dağıtım komut dosyası olarak.
- _Deploy.sh_ -özel dağıtım komut dosyası. Dosyayı gözden geçirirseniz, çalıştığını görürsünüz `gulp prod` sonra `npm install` ve `bower install`. 

Git tabanlı dağıtımınız herhangi bir adımı eklemek için bu yaklaşımı kullanın. Uygulama hizmeti, herhangi bir noktada, Azure web uygulamanızın yeniden başlatırsanız, bu otomasyon görevleri yeniden değil.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulaması'na göz atın 

Web tarayıcınız üzerinden dağıtılan web uygulamasının göz atın. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Tıklatın **kaydolun** üst menüsünde ve sahte bir kullanıcı oluşturun. 

Başarılı ve uygulama otomatik olarak oluşturulan kullanıcı oturum açtığında, azure'da MEAN.js uygulamanız MongoDB (Cosmos DB) veritabanı bağlantısı vardır. 

![Azure App Service’te çalışan MEAN.js uygulaması](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Seçin **yönetici > yönetmek makaleleri** bazı makaleler eklemek için. 

**Tebrikler!** Azure App Service'te veri güdümlü bir Node.js uygulaması kullanıyorsunuz.

## <a name="update-data-model-and-redeploy"></a>Güncelleştirme veri modeli ve yeniden dağıtın

Bu adımda, değiştirdiğiniz `article` veri modeli ve değişikliklerinizi Azure'a yayımlayacaksınız.

### <a name="update-the-data-model"></a>Veri modelini güncelleştir

Açık _modules/articles/server/models/article.server.model.js_.

İçinde `ArticleSchema`, ekleme bir `String` adlı türü `comment`. İşiniz bittiğinde, şema kodunuzu aşağıdaki gibi görünmelidir:

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

### <a name="update-the-articles-code"></a>Makaleler kodunu güncelleştirin

Kalan güncelleştirme, `articles` kullanmak için kodu `comment`.

Beş dosyayı değiştirmek için ihtiyacınız vardır: sunucu denetleyici ve dört istemci görünümleri. 

Açık _modules/articles/server/controllers/articles.server.controller.js_.

İçinde `update` işlev, atama için ekleme `article.comment`. Aşağıdaki kod tamamlanmış gösterir `update` işlevi:

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

Açık _modules/articles/client/views/view-article.client.view.html_.

Kapatma hemen `</section>` etiketi, görüntülemek için aşağıdaki satırı ekleyin `comment` makale verilerini geri kalanı ile birlikte:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Açık _modules/articles/client/views/list-articles.client.view.html_.

Kapatma hemen `</a>` etiketi, görüntülemek için aşağıdaki satırı ekleyin `comment` makale verilerini geri kalanı ile birlikte:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Açık _modules/articles/client/views/admin/list-articles.client.view.html_.

İçinde `<div class="list-group">` öğesi ve kapatma hemen `</a>` etiketi, görüntülemek için aşağıdaki satırı ekleyin `comment` makale verilerini geri kalanı ile birlikte:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Açık _modules/articles/client/views/admin/form-article.client.view.html_.

Bul `<div class="form-group">` bu gibi görünen Gönder düğmesi içeren öğe:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Bu etiketi yalnızca başka bir tane eklemek `<div class="form-group">` Düzenle kişiler olanak sağlayan öğe `comment` alan. Yeni öğe aşağıdaki gibi görünmelidir:

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a>Yaptığınız değişiklikler yerel olarak test etme

Yaptığınız tüm değişiklikleri kaydedin.

Yerel terminal penceresinde değişikliklerinizi üretim modunda yeniden sınayın.

```bash
# Bash
gulp prod
NODE_ENV=production node server.js

# Windows PowerShell
gulp prod
$env:NODE_ENV = "production" 
node server.js
```

Gidin `http://localhost:8443` bir tarayıcıda ve oturum açtınız olduğundan emin olun.

Seçin **yönetici > yönetmek makaleleri**, ardından bir makale seçerek eklemek  **+**  düğmesi.

Yeni gördüğünüz `Comment` şimdi metin kutusu.

![Makaleler için ek açıklama alanı](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

Terminale yazarak Node.js durdurun `Ctrl+C`. 

### <a name="publish-changes-to-azure"></a>Değişiklikler için Azure yayımlama

Yerel terminal penceresi Git yaptığınız değişiklikleri kaydetmek ve ardından kod değişiklikleri Azure'a gönderin.

```bash
git commit -am "added article comment"
git push azure master
```

Bir kez `git push` tamamlamak, Azure web uygulamasına gidin ve yeni işlevselliği deneyin.

![Azure için yayımlanan modeli ve veritabanı değişiklikleri](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Tüm makaleleri daha önce eklediyseniz, bunları yine görebilirsiniz. Cosmos veritabanında var olan veri kaybı olmadığından. Ayrıca, veri şeması yaptığınız güncelleştirmeler ve varolan verilerinizi dokunmaz.

## <a name="stream-diagnostic-logs"></a>Akış tanılama günlükleri 

Node.js uygulamanızı Azure App Service'te çalışırken, terminal yöneltilen konsol günlükleri alabilirsiniz. Böylece, uygulama hatalarını hata ayıklama yardımcı olmak için aynı tanılama iletileri alabilirsiniz.

Günlük akış başlatmak için kullanmak [az webapp günlük tail](/cli/azure/webapp/log#tail) bulut Kabuğu'nda komutu.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Günlük akış başladıktan sonra Azure web uygulamanızda bazı web trafiği almak için tarayıcıyı yenileyin. Artık, terminal yöneltilen konsol günlüklerine bakın.

Herhangi bir zamanda yazarak akış günlük durdurma `Ctrl+C`. 

## <a name="manage-your-azure-web-app"></a>Azure web uygulamanızı yönetme

Git [Azure portal](https://portal.azure.com) oluşturduğunuz web uygulaması görmek için.

Sol menüden **Uygulama Hizmetleri**’ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Varsayılan olarak, portal, web uygulamanızın gösterir **genel bakış** sayfası. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafında sekmeleri açabilir farklı yapılandırma sayfalarında gösterilir.

![Azure portalında App Service sayfası](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Sonraki adımlar

Öğrendiklerinizi:

> [!div class="checklist"]
> * MongoDB veritabanı oluşturma
> * MongoDB için bir Node.js uygulamasını bağlama
> * Uygulamayı Azure'a dağıtma
> * Veri modeli güncelleştirme ve uygulamayı yeniden dağıtın
> * Azure akış günlükleri, terminal
> * Azure portalında uygulama yönetme

Web uygulamanıza özel bir DNS adı eşleme öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"] 
> [Mevcut bir özel DNS adını Azure Web Apps ile eşleme](app-service-web-tutorial-custom-domain.md)
