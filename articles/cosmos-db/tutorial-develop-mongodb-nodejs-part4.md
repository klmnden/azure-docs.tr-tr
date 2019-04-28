---
title: Angular bir uygulamayı Azure Cosmos DB API'si ile MongoDB için oluşturma - Cosmos hesabı oluşturma
titleSuffix: Azure Cosmos DB
description: Azure Cosmos DB üzerinde Angular ve Node ile MongoDB için kullandığınız aynı API'leri kullanarak bir MongoDB uygulaması oluşturma öğreticisi dizisinin 4. bölümü
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
ms.openlocfilehash: 8320204f75e583dae0449f83e7c38f6638371c2a
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130407"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---create-a-cosmos-account"></a>Angular bir uygulamayı Azure Cosmos DB API'si ile MongoDB için oluşturma - Cosmos hesabı oluşturma

Bu çok bölümlü öğretici, Express ve Angular ile Node.js kullanılarak yazılmış yeni bir uygulama oluşturun ve buna bağlanmak gösterilmektedir, [MongoDB için Cosmos DB API'si ile yapılandırılan Cosmos hesabı](mongodb-introduction.md).

Öğreticinin 4. bölümünde [3. bölümdeki](tutorial-develop-mongodb-nodejs-part3.md) konular genişletilir ve aşağıdaki görevler yer alır:

> [!div class="checklist"]
> * Azure CLI kullanarak bir Azure kaynak grubu oluşturma
> * Azure CLI kullanarak Cosmos hesabı oluşturma

## <a name="video-walkthrough"></a>Görüntülü kılavuz

> [!VIDEO https://www.youtube.com/embed/hfUM-AbOh94]

## <a name="prerequisites"></a>Önkoşullar

Öğreticinin bu bölümüne başlamadan önce öğreticinin [3. bölümündeki](tutorial-develop-mongodb-nodejs-part3.md) adımları tamamladığınızdan emin olun. 

Bu öğretici bölümünde Azure Cloud Shell’i (İnternet tarayıcınızda) veya yerel olarak yüklenen [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)’yi kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

> [!TIP]
> Bu öğretici, uygulamanızı adım adım oluşturmanızı sağlayacak adımlarla size yol gösterir. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

[`az cosmosdb create`](/cli/azure/cosmosdb#az-cosmosdb-create) komutuyla bir Azure Cosmos DB hesabı oluşturun.

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

* `<cosmosdb-name>` için kendi benzersiz Azure Cosmos DB hesap adınızı kullanın. Bu adın Azure içinde tüm Azure Cosmos DB hesabı adları arasında benzersiz olması gerekir.
* `--kind MongoDB` ayarı, Azure Cosmos DB’nin MongoDB istemci bağlantıları olmasını sağlar.

Komutun tamamlanması bir veya iki dakika kadar sürebilir. Tamamlandığında, terminal penceresi yeni veritabanı hakkındaki bilgileri görüntüler. 

Azure Cosmos DB hesabı oluşturulduktan sonra:
1. Yeni bir tarayıcı penceresi açın ve [https://portal.azure.com](https://portal.azure.com) adresine gidin
1. Sol taraftaki çubukta Azure Cosmos DB logosuna ![Azure portalında Azure Cosmos DB simgesi](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-icon.png) tıklayın. Sahip olduğunuz tüm Azure Cosmos DB’ler gösterilir.
1. Yeni oluşturduğunuz Azure Cosmos DB hesabına tıklayın, **Genel bakış** sekmesini seçin ve veritabanın bulunduğu haritayı bulun. 

    ![Azure Portal’daki yeni Azure Cosmos DB hesabı](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-angular-portal.png)

4. Sol gezinti bölmesinde aşağı kaydırın ve **Verileri küresel olarak çoğalt** sekmesine tıklayın, çoğaltma hedefi olarak kullanabileceğiniz farklı alanları gösteren bir harita görüntülenir. Örneğin, Avustralya Güneydoğu veya Avustralya Doğu’ya tıklayın ve verileriniz Avustralya için çoğaltılır. Genel çoğaltma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB ile verileri küresel olarak dağıtma](distribute-data-globally.md). Şimdilik yalnızca bir örneğimiz olsun. Çoğaltmak istediğimiz zaman nasıl yapacağımızı biliyoruz.

    ![Azure Portal’daki yeni Azure Cosmos DB hesabı](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-replicate-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure CLI kullanarak bir Azure kaynak grubu oluşturdunuz
> * Azure CLI’yı kullanarak Azure Cosmos DB hesabı oluşturdunuz

Mongoose kullanarak uygulamanızı Azure Cosmos DB’ye bağlanmak için öğreticinin sonraki bölümüne geçebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye bağlanmak için Mongoose kullanma](tutorial-develop-mongodb-nodejs-part5.md)
