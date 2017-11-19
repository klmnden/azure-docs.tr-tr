---
title: "Hızlı Başlangıç: Tablo API Node.js - Azure Cosmos DB ile | Microsoft Docs"
description: "Bu hızlı başlangıç Azure Cosmos DB tablo API Node.js ve Azure portal ile bir uygulama oluşturmak için nasıl kullanılacağını gösterir"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: quickstart
ms.date: 11/16/2017
ms.author: arramac
ms.openlocfilehash: 1dcc2178b3c7017338e0097773fbf0d04c8b6a20
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-table-api-app-with-nodejs-and-azure-cosmos-db"></a>Hızlı Başlangıç: bir tablo Node.js ve Azure Cosmos DB ile API uygulaması oluşturma

Bu hızlı başlangıç Node.js ve Azure Cosmos DB nasıl kullanılacağını gösterir [tablo API](table-introduction.md) örneği github'dan kopyalanarak bir uygulama oluşturmak için. Bu hızlı başlangıç ayrıca bir Azure Cosmos DB hesabının nasıl oluşturulacağını ve Veri Gezgini tabloları ve varlıkları web tabanlı Azure portalında oluşturmak için nasıl kullanılacağı gösterilmektedir.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Hızlı bir şekilde oluşturmak ve belge, anahtar/değer, genişliğinde bir sütun ve grafik veritabanları, her biri genel dağıtım ve yatay ölçek yetenekleri Azure Cosmos DB en yararlı sorgulayabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Buna ek olarak:

* [Node.js](https://nodejs.org/en/) sürüm v0.10.29 veya üzeri
* [Git](http://git-scm.com/)

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni tablonuza veri ekleyebilirsiniz.

1. Veri Gezgini'nde **sample-table** seçeneğini genişletin, **Varlıklar**'a ve ardından **Varlık Ekle**'ye tıklayın.

   ![Azure portalındaki Veri Gezgini'nde yeni varlık oluşturma](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Şimdi veri PartitionKey değer kutusuna ve RowKey değer kutularına ekleyin ve **varlık Ekle**.

   ![Yeni bir varlık için Bölüm Anahtarını ve Satır Anahtarını ayarlama](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Artık tablonuza daha fazla varlık ekleyebilir, varlıklarınızı düzenleyebilir veya Veri Gezgini’nde verilerinizi sorgulayabilirsiniz. Veri Gezgini ayrıca aktarım hızınızı ölçeklendirebileceğiniz ve tablonuza depolanmış yordamlar, kullanıcı tarafından tanımlanmış işlevler ve tetikleyiciler ekleyebileceğiniz yerdir.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve kullanmak `cd` örnek uygulamayı yüklemek için bir klasör olarak değiştirmek için komutu. 

    ```bash
    cd "C:\git-samples"
    ```

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulaması bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-node-getting-started.git
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, barındırılan veritabanıyla iletişim kurmak uygulamanızı sağlar. 

1. İçinde [Azure portal](http://portal.azure.com/), tıklatın **bağlantı dizesi**. 

    ![Görüntüleyin ve gerekli bağlantı dizesi bilgilerini kopyalamak bağlantı dizesi bölmesinde](./media/create-table-nodejs/connection-string.png)

2. Sağ tarafta Kopyala düğmesini kullanarak birincil bağlantı DİZESİNİ kopyalayın.

3. App.config dosyasını açın ve üç satırındaki connectionString değeri yapıştırın. Bağlantı dizenizi uç noktası bölümünü documents.azure.com kullanıyorsa, bunun yerine table.cosmosdb.azure.com kullanmak için bu bölümü düzeltin.

3. App.config dosyasını kaydedin.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Git terminal penceresinde `cd` depolama-tablo-java-başlama klasörüne.

    ```
    cd "C:\git-samples\storage-table-node-getting-started"
    ```

2. [Azure], yüklemek için aşağıdaki komutu çalıştırın [düğüm-UUID], [nconf] ve package.json dosyasına bir giriş kaydetmekte konusunda da yerel olarak [async] modülleri

   ```
   npm install azure-storage node-uuid async nconf --save
   ```

2. Git düğüm uygulama çalıştırmak için aşağıdaki komutları çalıştırın, terminal penceresi başlatın.

    ```
    node ./tableSample.js 
    ```

    Konsol penceresinde Azure Cosmos veritabanı yeni tablo veritabanına eklenen tablo verileri görüntüler.

    Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo API için tablo verileri alma](table-import.md)
