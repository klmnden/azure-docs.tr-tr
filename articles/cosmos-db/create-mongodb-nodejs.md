---
title: "Node.js kullanarak Bir MongoDB uygulamasını Azure Cosmos DB’ye bağlama | Microsoft Docs"
description: "Var olan bir Node.js MongoDB uygulamasını Azure Cosmos DB’ye bağlama hakkında bilgi edinin"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/19/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: 0265503689e189a3e2e30c2ae9fff39641647d0c
ms.contentlocale: tr-tr
ms.lasthandoff: 06/20/2017


---
<a id="azure-cosmos-db-migrate-an-existing-nodejs-mongodb-web-app" class="xliff"></a>

# Azure Cosmos DB: Var olan bir Node.js MongoDB web uygulamasını geçirme 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıçta, Node.js’de yazılmış mevcut bir [MongoDB](mongodb-introduction.md) uygulamasını kullanma ve MongoDB istemci bağlantılarını destekleyen Azure Cosmos DB veritabanınıza bağlama işlemi gösterilmektedir. Diğer bir deyişle, Node.js uygulamanız yalnızca MongoDB API’lerini kullanarak bir veritabanına bağlandığını bilir. Verilerin Azure Cosmos DB'de depolandığı uygulamaya açıkça gösterilir.

İşiniz bittiğinde, [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) üzerinde çalışan bir MEAN uygulamanız (MongoDB, Express, AngularJS ve Node.js) olacaktır. 

![Azure App Service’te çalışan MEAN.js uygulaması](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

<a id="prerequisites" class="xliff"></a>

## Ön koşullar 
`npm` ve `git` komutlarını çalıştırmak için Azure CLI'nin yanı sıra yerel olarak [Node.js](https://nodejs.org/) ve [Git](http://www.git-scm.com/downloads) yüklemeniz gerekir.

Node.js çalıştırma bilgisine sahip olmanız gerekir. Bu hızlı başlangıç, genel olarak Node.js uygulamaları geliştirmenize yardımcı olmak için tasarlanmamıştır.

<a id="clone-the-sample-application" class="xliff"></a>

## Örnek uygulamayı kopyalama

Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın. Bu örnek depo, varsayılan [MEAN.js](http://meanjs.org/) uygulamasını içerir. 

```bash
git clone https://github.com/prashanthmadi/mean
```

<a id="run-the-application" class="xliff"></a>

## Uygulamayı çalıştırma

Gereken paketleri yükleyip uygulamayı başlatın.

```bash
cd mean
npm install
npm start
```

<a id="log-in-to-azure" class="xliff"></a>

## Azure'da oturum açma

Yüklenen bir Azure CLI kullanıyorsanız [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri uygulayın. Azure Cloud Shell'i kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
az login 
``` 
   
<a id="add-the-azure-cosmos-db-module" class="xliff"></a>

## Azure Cosmos DB modülü ekleme

Yüklenen bir Azure CLI kullanıyorsanız `az` komutunu çalıştırarak `cosmosdb` bileşeninin zaten yüklü olup olmadığını kontrol edin. `cosmosdb`, temel komutlar listesindeyse bir sonraki komuta geçin. Azure Cloud Shell'i kullanıyorsanız bu adımı atlayabilirsiniz.

`cosmosdb`, temel komutlar listesinde değilse [Azure CLI 2.0]( /cli/azure/install-azure-cli)'ı yeniden yükleyin.

<a id="create-a-resource-group" class="xliff"></a>

## Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) ile bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Azure kaynak grubu; web uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnekte Batı Avrupa bölgesinde bir kaynak grubu oluşturulmaktadır. Kaynak grubu için benzersiz bir ad seçin.

Azure Cloud Shell kullanıyorsanız **Deneyin**'e tıklayın, oturum açmak için ekrandaki istemleri uygulayın, ardından komutu komut istemine kopyalayın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

<a id="create-an-azure-cosmos-db-account" class="xliff"></a>

## Azure Cosmos DB hesabı oluşturma

[az cosmosdb create](/cli/azure/cosmosdb#create) komutuyla bir Azure Cosmos DB hesabı oluşturun.

Aşağıdaki komutta lütfen benzersiz Azure Cosmos DB hesap adınızı `<cosmosdb_name>` yer tutucusunu gördüğünüz yere yerleştirin. Bu benzersiz ad, Azure Cosmos DB uç noktanızın (`https://<cosmosdb_name>.documents.azure.com/`) bir parçası olarak kullanılacağı için, adın Azure’daki tüm Azure Cosmos DB hesaplarında benzersiz olması gerekir. 

```azurecli-interactive
az cosmosdb create --name <cosmosdb_name> --resource-group myResourceGroup --kind MongoDB
```

`--kind MongoDB` parametresi MongoDB istemci bağlantılarını etkinleştirir.

Azure Cosmos DB hesabı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir. 

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb_name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb_name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb_name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb_name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb_name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb_name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

<a id="connect-your-nodejs-application-to-the-database" class="xliff"></a>

## Node.js uygulamanızı veritabanına bağlama

Bu adımda, MEAN.js örnek uygulamanızı, MongoDB bağlantı dizesi kullanarak yeni oluşturduğunuz bir Azure Cosmos DB veritabanına bağlayacaksınız. 

<a id="retrieve-the-key" class="xliff"></a>

## Anahtarı alma

Bir Azure Cosmos DB veritabanına bağlanmak için veritabanı anahtarı gerekir. Birincil anahtarı almak için [az cosmosdb list-keys](/cli/azure/cosmosdb#list-keys) komutunu kullanın.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Azure CLI aşağıdaki örneğe benzer bilgiler çıkarır. 

```json
{
  "primaryMasterKey": "RUayjYjixJDWG5xTqIiXjC...",
  "primaryReadonlyMasterKey": "...",
  "secondaryMasterKey": "...",
  "secondaryReadonlyMasterKey": "..."
}
```

`primaryMasterKey` değerini bir metin düzenleyicisine kopyalayın. Bu bilgiler sonraki adımda gerekli olacaktır.

<a name="devconfig"></a>
<a id="configure-the-connection-string-in-your-nodejs-application" class="xliff"></a>

## Node.js uygulamanızda bağlantı dizesini yapılandırma

MEAN.js deponuzda `config/env/local-development.js` dosyasını açın.

Bu dosyanın içeriğini aşağıdaki kodla değiştirin. Ayrıca iki `<cosmosdb_name>` yer tutucusunu Azure Cosmos DB hesap adı ile, `<primary_master_key>` yer tutucusunu da önceki adımda kopyaladığınız anahtar ile değiştirdiğinizden emin olun.

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

> [!NOTE] 
> [Azure Cosmos DB, SSL gerektirdiğinden](connect-mongodb-account.md#connection-string-requirements) `ssl=true` seçeneği önemlidir. 
>
>

Yaptığınız değişiklikleri kaydedin.

<a id="run-the-application-again" class="xliff"></a>

### Uygulamayı yeniden çalıştırın.

`npm start` komutunu yeniden çalıştırın. 

```bash
npm start
```

Bir konsol iletisi, geliştirme ortamının çalışır durumda olduğunu söyleyecektir. 

Bir tarayıcıda `http://localhost:3000` sayfasına gidin. Üstteki menüden **Kaydol**’a tıklayıp iki sahte kullanıcı oluşturmaya çalışın. 

MEAN.js örnek uygulaması, kullanıcı verilerini veritabanında depolar. Başarılı olursanız ve MEAN.js oluşturulan kullanıcının oturumunu otomatik olarak açarsa, Azure Cosmos DB bağlantınız çalışıyordur. 

![MEAN.js, MongoDB’ye başarıyla bağlanır](./media/create-mongodb-nodejs/mongodb-connect-success.png)

<a id="view-data-in-data-explorer" class="xliff"></a>

## Veri Gezgini’nde verileri görüntüleme

Bir Azure Cosmos DB tarafından depolanan veriler, Azure portalında görüntülenebilir, sorgulanabilir ve iş mantığında çalıştırılabilir.

Önceki adımda oluşturulan verileri görüntülemek, sorgulamak ve üzerinde çalışmak için web tarayıcınızda [Azure portalı](https://portal.azure.com) oturumunu açın.

Arama kutusuna Azure Cosmos DB yazın. Cosmos DB hesabı dikey penceresi açıldığında Cosmos DB hesabınızı seçin. Sol gezinti bölmesinde Veri Gezgini'ne tıklayın. Koleksiyonlar bölmesinde koleksiyonunuzu genişletin; bundan sonra koleksiyondaki belgeleri görüntüleyebilir, verileri sorgulayabilir ve hatta saklı yordam, tetikleyici ve UDF’ler oluşturup çalıştırabilirsiniz. 

![Azure portalında Veri Gezgini](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


<a id="deploy-the-nodejs-application-to-azure" class="xliff"></a>

## Node.js uygulamasını Azure’a dağıtma

Bu adımda, MongoDB’ye bağlı Node.js uygulamanızı Azure Cosmos DB’ye dağıtacaksınız.

Daha önce değiştirdiğiniz yapılandırma dosyasının geliştirme ortamına (`/config/env/local-development.js`) yönelik olduğunu fark etmiş olabilirsiniz. Uygulamanızı App Service’e dağıttığınızda, varsayılan olarak üretim ortamında çalıştırılır. O halde şimdi, ilgili yapılandırma dosyasında aynı değişikliği yapmanız gerekir.

MEAN.js deponuzda `config/env/production.js` dosyasını açın.

`db` nesnesinde `uri` değerini aşağıdaki örnekte gösterilen şekilde değiştirin. Yer tutucuları daha önceki gibi değiştirdiğinizden emin olun.

```javascript
'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
```

Terminalde tüm değişikliklerinizi Git’e uygulayın. Her iki komutu birlikte çalıştırmak üzere kopyalayabilirsiniz.

```bash
git add .
git commit -m "configured MongoDB connection string"
```
<a id="clean-up-resources" class="xliff"></a>

## Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir MongoDB koleksiyonu oluşturmayı öğrendiniz. Şimdi MongoDB verilerinizi Azure Cosmos DB’ye geçirebilirsiniz.  

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)

