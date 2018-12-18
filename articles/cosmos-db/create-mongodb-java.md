---
title: MongoDB için Java ve Azure Cosmos DB API bir konsol uygulaması oluşturma
description: Bağlanmak ve MongoDB için Azure Cosmos DB API sorgulamak için kullanabileceğiniz bir Java kodu örneği sunar
services: cosmos-db
author: slyons
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: quick start connect, mvc
ms.devlang: java
ms.topic: quickstart
ms.date: 05/10/2017
ms.author: sclyon
ms.openlocfilehash: ddecb179b2ea0001ec358ad17ec76754054b0309
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53535563"
---
# <a name="build-a-console-app-with-java-and-the-azure-cosmos-db-api-for-mongodb"></a>MongoDB için Java ve Azure Cosmos DB API bir konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıçta nasıl oluşturulacağını gösterir. bir [Azure Cosmos DB MongoDB API'si](mongodb-introduction.md) hesabını, belge veritabanı ve Azure portalını kullanarak bir koleksiyon. Bu adımların ardından [MongoDB Java sürücüsünü](https://docs.mongodb.com/ecosystem/drivers/java/) kullanarak bir konsol uygulaması derleyebilir ve çalıştırabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmadan önce aşağıdaki önkoşullara sahip olmanız gerekir:
* JDK 1.7+ (JDK yoksa `apt-get install default-jdk` komutunu çalıştırın)
* Maven (Maven yoksa `apt-get install maven` komutunu çalıştırın)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

Yeni veritabanınıza **db**, yeni koleksiyonunuza da **coll** adını verin.

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)] 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi MongoDB uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı bir Azure Cosmos DB API github'dan çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. Daha sonra kodu sık kullandığınız düzenleyicinizde açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

Aşağıdaki kod parçacıklarının tamamı, Program.java dosyasından alınmıştır.

* DocumentClient başlatılır.

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* Yeni bir veritabanı ve koleksiyonu oluşturulur.

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* `MongoCollection.insertOne` kullanılarak birkaç belge eklenir

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* `MongoCollection.find` kullanılarak birkaç sorgu gerçekleştirilir

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. Hesap sayfasında **Hızlı Başlangıç**'ı ve ardından Java'yı seçip bağlantı dizesini panonuza kopyalayın

2. `Program.java` dosyasını açın, MongoClientURI oluşturucusunun bağımsız değişkenini bağlantı dizesiyle değiştirin. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 
    
## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

1. Gerekli npm modüllerini yüklemek için bir terminalde `mvn package` komutunu çalıştırın

2. Java uygulamanızı başlatmak için bir terminalde `mvn exec:java -D exec.mainClass=GetStarted.Program` komutunu çalıştırın.

Artık [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) kullanarak yeni verileri sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bir konsol uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)


