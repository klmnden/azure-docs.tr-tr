---
title: MongoDB için .NET ve Azure Cosmos DB'nin API'sini bir Xamarin.Forms uygulaması oluşturma
description: Bağlanmak ve MongoDB için Azure Cosmos DB API'si ile sorgulamak için kullanabileceğiniz bir Xamarin kod örneği sunar
author: codemillmatt
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: masoucou
ms.openlocfilehash: c7a80ae704d13934a5d51bc5538bc28693b49e5b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60890636"
---
# <a name="quickstart-build-a-xamarinforms-app-with-net-sdk-and-azure-cosmos-dbs-api-for-mongodb"></a>Hızlı Başlangıç: MongoDB için .NET SDK'sını ve Azure Cosmos DB'nin API'sini bir Xamarin.Forms uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

Bu hızlı başlangıçta nasıl oluşturulacağını gösterir. bir [MongoDB için Azure Cosmos DB API'si ile yapılandırılan Cosmos hesabı](mongodb-introduction.md), belge veritabanını ve Azure portalını kullanarak bir koleksiyon. Ardından, kullanarak bir todo uygulaması Xamarin.Forms uygulaması oluşturacaksınız [MongoDB .NET sürücüsü](https://docs.mongodb.com/ecosystem/drivers/csharp/).

## <a name="prerequisites-to-run-the-sample-app"></a>Örnek uygulamayı çalıştırmak için önkoşullar

Örneği çalıştırmak için [Visual Studio](https://www.visualstudio.com/downloads/) veya [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/) ve geçerli bir Azure CosmosDB hesabı olması gerekir.

Visual Studio yoksa [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirin ve **.NET ile mobil geliştirme** iş yükünü kurulumla birlikte yükleyin.

Mac'te çalışmayı tercih ediyorsanız [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)'yu indirin ve kurulumu çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

Bu makalede açıklanan örnek, MongoDB.Driver 2.6.1 sürümü ile uyumludur.

## <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

İlk olarak, örnek uygulamasını Github'dan indirin. Bu, MongoDB’nin belge depolama modeliyle bir yapılacak işler uygulamasını uygular.

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-xamarin-getting-started.git
    ```

Git kullanmak istemiyorsanız [projeyi ZIP dosyası olarak da indirebilirsiniz](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-xamarin-getting-started/archive/master.zip)

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz.

Aşağıdaki kod parçacıklarının tümü alınmıştır `MongoService` sınıfı, şu yolda bulunamadı: src/TaskList.Core/Services/MongoService.cs.

* Mongo İstemcisini başlatın.
    ```cs
    MongoClientSettings settings = MongoClientSettings.FromUrl(
        new MongoUrl(APIKeys.ConnectionString)
    );

    settings.SslSettings =
        new SslSettings() { EnabledSslProtocols = SslProtocols.Tls12 };

    MongoClient mongoClient = new MongoClient(settings);
    ```

* Veritabanı ve koleksiyona bir başvuru alın. Mevcut değilse MongoDB .NET SDK'sı otomatik olarak veritabanını ve koleksiyonu oluşturur.
    ```cs
    string dbName = "MyTasks";
    string collectionName = "TaskList";

    var db = mongoClient.GetDatabase(dbName);

    var collectionSettings = new MongoCollectionSettings {
        ReadPreference = ReadPreference.Nearest
    };

    tasksCollection = db.GetCollection<MyTask>(collectionName, collectionSettings);
    ```
* Tüm belgeleri Liste olarak alın.
    ```cs
    var allTasks = await TasksCollection
                    .Find(new BsonDocument())
                    .ToListAsync();
    ```

* Belirli belgeleri sorgulayın.
    ```cs
    public async Task<List<MyTask>> GetIncompleteTasksDueBefore(DateTime date)
    {
        var tasks = await TasksCollection
                        .AsQueryable()
                        .Where(t => t.Complete == false)
                        .Where(t => t.DueDate < date)
                        .ToListAsync();

        return tasks;
    }
    ```

* Bir görev oluşturun ve koleksiyona ekleyin.
    ```cs
    public async Task CreateTask(MyTask task)
    {
        await TasksCollection.InsertOneAsync(task);
    }
    ```

* Bir koleksiyondaki bir görevi güncelleştirin.
    ```cs
    public async Task UpdateTask(MyTask task)
    {
        await TasksCollection.ReplaceOneAsync(t => t.Id.Equals(task.Id), task);
    }
    ```

* Bir görev bir koleksiyonundan silin.
    ```cs
    public async Task DeleteTask(MyTask task)
    {
        await TasksCollection.DeleteOneAsync(t => t.Id.Equals(task.Id));
    }
    ```

<a id="update-your-connection-string"></a>

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](https://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Bağlantı Dizesi**'ne ve ardından **Okuma-Yazma Anahtarları**'na tıklayın. Ekranın sağ tarafındaki kopyala düğmesini kullanarak sonraki adımlarda bulunan Birincil Bağlantı Dizesini kopyalayın.

2. **TaskList.Core** projesinin **Helpers** dizinindeki **APIKeys.cs** dosyasını açın.

3. Portaldan **birincil bağlantı dizesi** değerinizi kopyalayın (kopyala düğmesini kullanarak) ve **APIKeys.cs** dosyanızdaki **ConnectionString** alanının değeri olarak kullanın.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Visual Studio'nun **Çözüm Gezgini** bölümünde her bir projeye sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
2. **Tüm NuGet paketlerini geri yükle**'ye tıklayın.
3. **TaskList.Android**'e sağ tıklayın ve **Başlangıç projesi olarak ayarla**'yı seçin.
4. Uygulamada hata ayıklamaya başlamak için F5'e basın.
5. iOS üzerinde çalıştırmak istiyorsanız ilk olarak makinenizin bir Mac'e bağlı olduğundan ([talimatlara](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio) buradan ulaşabilirsiniz) emin olun.
6. **TaskList.iOS** projesine sağ tıklayın ve **Başlangıç projesi olarak ayarla**'yı seçin.
7. Uygulamada hata ayıklamaya başlamak için F5'e basın.

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1. Platform açılan listesinde çalıştırmak istediğiniz platforma bağlı olarak TaskList.iOS veya TaskList.Android girişini seçin.
2. Uygulamada hata ayıklamaya başlamak için Cmd+Enter tuşlarına basın.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı ve MongoDB API’sini kullanarak bir Xamarin.Forms uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB MongoDB için Azure Cosmos DB API'si ile yapılandırılan veri içeri aktarma](mongodb-migrate.md)
