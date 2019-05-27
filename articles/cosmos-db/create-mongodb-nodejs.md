---
title: Bir Node.js MongoDB uygulamasını Azure Cosmos DB'ye bağlama
description: Bu hızlı başlangıçta Azure Cosmos DB için node.js'de yazılmış mevcut bir MongoDB uygulamasını na nasıl bağlanacağınız gösterilmiştir.
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 05/21/2019
ms.openlocfilehash: 3f6eca30379eb8890695df946f1d7e697cb3f7d7
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65979058"
---
# <a name="quickstart-migrate-an-existing-mongodb-nodejs-web-app-to-azure-cosmos-db"></a>Hızlı Başlangıç: Mevcut bir MongoDB Node.js web uygulamasını Azure Cosmos DB'ye geçirme 

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Hızla oluşturun ve belge, anahtar/değer ve her biri genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz Cosmos DB'nin grafik veritabanlarını sorgulama. 

Bu hızlı başlangıçta, node.js'de yazılmış mevcut bir MongoDB uygulamasını kullanma ve MongoDB istemci destekleyen Cosmos veritabanına bağlanmak gösterilmektedir. Diğer bir deyişle, saydam uygulama verileri bir Cosmos veritabanında depolanır.

İşiniz bittiğinde, çalışan bir MEAN uygulamanız (MongoDB, Express, Angular ve Node.js) olacaktır üzerinde [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

![Azure App Service’te çalışan MEAN.js uygulaması](./media/create-mongodb-nodejs/meanjs-in-azure.png)


[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="prerequisites"></a>Önkoşullar 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

`npm` ve `git` komutlarını çalıştırmak için Azure CLI'nin yanı sıra yerel olarak [Node.js](https://nodejs.org/) ve [Git](https://www.git-scm.com/downloads) yüklemeniz gerekir.

Node.js çalıştırma bilgisine sahip olmanız gerekir. Bu hızlı başlangıç, genel olarak Node.js uygulamaları geliştirmenize yardımcı olmak için tasarlanmamıştır.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın. Bu örnek depo, varsayılan [MEAN.js](https://meanjs.org/) uygulamasını içerir.

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/prashanthmadi/mean
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Gereken paketleri yükleyip uygulamayı başlatın.

```bash
cd mean
npm install
npm start
```
Uygulama bir MongoDB kaynağına bağlanmayı deneyip başarısız olur, çıkış "[MongoError: connect ECONNREFUSED 127.0.0.1:27017]" döndürdüğünde uygulamadan çıkabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Yüklenen bir Azure CLI kullanıyorsanız [az login](/cli/azure/reference-index#az-login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri uygulayın. Azure Cloud Shell'i kullanıyorsanız bu adımı atlayabilirsiniz.

```azurecli
az login 
``` 
   
## <a name="add-the-azure-cosmos-db-module"></a>Azure Cosmos DB modülü ekleme

Yüklenen bir Azure CLI kullanıyorsanız `az` komutunu çalıştırarak `cosmosdb` bileşeninin zaten yüklü olup olmadığını kontrol edin. `cosmosdb`, temel komutlar listesindeyse bir sonraki komuta geçin. Azure Cloud Shell'i kullanıyorsanız bu adımı atlayabilirsiniz.

`cosmosdb`, temel komutlar listesinde değilse [Azure CLI]( /cli/azure/install-azure-cli)'yi yeniden yükleyin.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) ile bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Azure kaynak grubu; web uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnekte Batı Avrupa bölgesinde bir kaynak grubu oluşturulmaktadır. Kaynak grubu için benzersiz bir ad seçin.

Azure Cloud Shell kullanıyorsanız **Deneyin**'e tıklayın, oturum açmak için ekrandaki istemleri uygulayın, ardından komutu komut istemine kopyalayın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturun

Bir Cosmos hesabı [az cosmosdb oluşturma](/cli/azure/cosmosdb#az-cosmosdb-create) komutu.

Aşağıdaki komutta Lütfen gördüğünüz kendi benzersiz Cosmos hesap adınızı alternatif `<cosmosdb-name>` yer tutucu. Bu benzersiz ad, Cosmos DB uç noktasının bir parçası kullanılacak (`https://<cosmosdb-name>.documents.azure.com/`), adın azure'daki tüm Cosmos hesaplarında benzersiz olması gerekir. 

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

`--kind MongoDB` parametresi MongoDB istemci bağlantılarını etkinleştirir.

Azure Cosmos DB hesabı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir. 

> [!NOTE]
> Bu örnek, varsayılan Azure CLI çıktı biçimi olarak JSON kullanır. Başka bir çıktı biçimi kullanmak için bkz. [Azure CLI komutları için çıktı biçimleri](https://docs.microsoft.com/cli/azure/format-output-azure-cli).

```json
{
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb-name>.documents.azure.com:443/",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Document
DB/databaseAccounts/<cosmosdb-name>",
  "kind": "MongoDB",
  "location": "West Europe",
  "name": "<cosmosdb-name>",
  "readLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ],
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "writeLocations": [
    {
      "documentEndpoint": "https://<cosmosdb-name>-westeurope.documents.azure.com:443/",
      "failoverPriority": 0,
      "id": "<cosmosdb-name>-westeurope",
      "locationName": "West Europe",
      "provisioningState": "Succeeded"
    }
  ]
} 
```

## <a name="connect-your-nodejs-application-to-the-database"></a>Node.js uygulamanızı veritabanına bağlama

Bu adımda, MEAN.js örnek uygulamanızı, yeni oluşturduğunuz Cosmos veritabanına bağlanın. 

<a name="devconfig"></a>
## <a name="configure-the-connection-string-in-your-nodejs-application"></a>Node.js uygulamanızda bağlantı dizesini yapılandırma

MEAN.js deponuzda `config/env/local-development.js` dosyasını açın.

Bu dosyanın içeriğini aşağıdaki kodla değiştirin. Ayrıca iki değiştirdiğinizden emin olun `<cosmosdb-name>` yer tutucusunu Cosmos hesabınızın adını.

```javascript
'use strict';

module.exports = {
  db: {
    uri: 'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean-dev?ssl=true&sslverifycertificate=false'
  }
};
```

## <a name="retrieve-the-key"></a>Anahtarı alma

Bir Cosmos veritabanına bağlanmak için veritabanı anahtarı gerekir. Birincil anahtarı almak için [az cosmosdb list-keys](/cli/azure/cosmosdb#az-cosmosdb-list-keys) komutunu kullanın.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb-name> --resource-group myResourceGroup --query "primaryMasterKey"
```

Azure CLI aşağıdaki örneğe benzer bilgiler çıkarır. 

```json
"RUayjYjixJDWG5xTqIiXjC..."
```

`primaryMasterKey` değerini kopyalayın. `local-development.js` içinde bulunan `<primary_master_key>` üzerine yapıştırın.

Yaptığınız değişiklikleri kaydedin.

### <a name="run-the-application-again"></a>Uygulamayı yeniden çalıştırın.

`npm start` komutunu yeniden çalıştırın. 

```bash
npm start
```

Bir konsol iletisi, geliştirme ortamının çalışır durumda olduğunu söyleyecektir. 

Bir tarayıcıda `http://localhost:3000` sayfasına gidin. Üstteki menüden **Kaydol**’a tıklayıp iki sahte kullanıcı oluşturmaya çalışın. 

MEAN.js örnek uygulaması, kullanıcı verilerini veritabanında depolar. Başarılı olursanız ve MEAN.js oluşturulan kullanıcının oturumunu otomatik olarak açarsa, Azure Cosmos DB bağlantınız çalışıyordur. 

![MEAN.js, MongoDB’ye başarıyla bağlanır](./media/create-mongodb-nodejs/mongodb-connect-success.png)

## <a name="view-data-in-data-explorer"></a>Veri Gezgini’nde verileri görüntüleme

Bir Cosmos veritabanında depolanan verileri görüntüleyin ve Azure portalındaki sorgu kullanılabilir.

Önceki adımda oluşturulan verileri görüntülemek, sorgulamak ve üzerinde çalışmak için web tarayıcınızda [Azure portalı](https://portal.azure.com) oturumunu açın.

Arama kutusuna Azure Cosmos DB yazın. Cosmos hesabı dikey penceresi açıldığında Cosmos hesabınızı seçin. Sol gezinti bölmesinde Veri Gezgini'ne tıklayın. Koleksiyonlar bölmesinde koleksiyonunuzu genişletin; bundan sonra koleksiyondaki belgeleri görüntüleyebilir, verileri sorgulayabilir ve hatta saklı yordam, tetikleyici ve UDF’ler oluşturup çalıştırabilirsiniz. 

![Azure portalında Veri Gezgini](./media/create-mongodb-nodejs/cosmosdb-connect-mongodb-data-explorer.png)


## <a name="deploy-the-nodejs-application-to-azure"></a>Node.js uygulamasını Azure’a dağıtma

Bu adımda, Cosmos DB için Node.js uygulamanızı dağıtın.

Daha önce değiştirdiğiniz yapılandırma dosyasının geliştirme ortamına (`/config/env/local-development.js`) yönelik olduğunu fark etmiş olabilirsiniz. Uygulamanızı App Service’e dağıttığınızda, varsayılan olarak üretim ortamında çalıştırılır. O halde şimdi, ilgili yapılandırma dosyasında aynı değişikliği yapmanız gerekir.

MEAN.js deponuzda `config/env/production.js` dosyasını açın.

`db` nesnesinde `uri` değerini aşağıdaki örnekte gösterilen şekilde değiştirin. Yer tutucuları daha önceki gibi değiştirdiğinizden emin olun.

```javascript
'mongodb://<cosmosdb-name>:<primary_master_key>@<cosmosdb-name>.documents.azure.com:10255/mean?ssl=true&sslverifycertificate=false',
```

> [!NOTE] 
> `ssl=true` Seçeneği önemlidir çünkü [Cosmos DB, SSL gerektirir](connect-mongodb-account.md#connection-string-requirements). 
>
>

Terminalde tüm değişikliklerinizi Git’e uygulayın. Her iki komutu birlikte çalıştırmak üzere kopyalayabilirsiniz.

```bash
git add .
git commit -m "configured MongoDB connection string"
```
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cosmos hesabı oluşturma, bir koleksiyon oluşturun ve bir konsol uygulamasını çalıştırmayı öğrendiniz. Şimdi, Cosmos veritabanınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
