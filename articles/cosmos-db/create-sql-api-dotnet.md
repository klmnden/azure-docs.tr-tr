---
title: SQL API'sini kullanarak Azure Cosmos DB ile .NET web uygulaması oluşturma
description: Bu hızlı başlangıçta, .NET web uygulaması oluşturmak için Azure Cosmos DB SQL API’sini ve Azure portalını kullanacaksınız
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/15/2019
ms.openlocfilehash: 1ef414b2de2acbf5b92661c8b5f1e249549b14df
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259151"
---
# <a name="quickstart-build-a-net-web-app-using-azure-cosmos-db-sql-api-account"></a>Hızlı Başlangıç: Azure Cosmos DB SQL API hesabı kullanarak bir .NET web uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](create-sql-api-dotnet.md)
> * [.NET (Önizleme)](create-sql-api-dotnet-preview.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)
>  
> 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıçta bir Azure Cosmos DB oluşturma işlemini gösterir [SQL API](sql-api-introduction.md) hesabını, belge veritabanı, koleksiyonu ve örnek veriler Azure portalını kullanarak koleksiyona ekleyin. Daha sonra yapı ve kullanılarak oluşturulan bir Yapılacaklar listesi web uygulaması dağıtma [SQL .NET SDK'sı](sql-api-sdk-dotnet.md), daha fazla veri koleksiyonundaki yönetme eklemek için. 

## <a name="prerequisites"></a>Önkoşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-an-account"></a>Hesap oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection-database"></a>
## <a name="add-a-database-and-a-collection"></a>Bir veritabanı ve koleksiyon Ekle

Şimdi bir veritabanı ve koleksiyon oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. **Veri Gezgini** > **Yeni Koleksiyon**’a tıklayın. 
    
    **Koleksiyon Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, koleksiyon Ekle bölmesi](./media/create-sql-api-dotnet/azure-cosmosdb-data-explorer-dotnet.png)

2. **Koleksiyon Ekle** sayfasında, yeni koleksiyon için ayarları girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    **Veritabanı kimliği**|ToDoList|Girin *ToDoList* yeni bir veritabanı adı olarak. Veritabanı adı 1 ila 255 karakter içermeli ve içeremezler `/, \\, #, ?`, veya bir boşluk.
    **Koleksiyon kimliği**|Öğeler|Yeni koleksiyonunuzun adı olarak *Öğeler* girin. Koleksiyon kimlikleri aynı karakter gereksinimleri veritabanı adlarına sahip.
    **Bölüm anahtarı**| `<your_partition_key>`| Bir bölüm anahtarı girin. Bu makalede açıklanan örnek kullanır */Category* bölüm anahtarı olarak.
    **Aktarım hızı**|400 RU|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak değiştirin. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. 
    
    Önceki ayarlara ek olarak, isterseniz koleksiyon için **Benzersiz anahtarlar** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Koleksiyon oluştururken benzersiz anahtar ilkesi oluşturulduğunda, bölüm anahtarı başına bir veya birden çok değerin benzersiz olduğundan emin olursunuz. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](unique-keys.md) makalesine bakın.
    
    **Tamam** düğmesine tıklayın.

    Veri Gezgini, yeni veritabanını ve koleksiyonu görüntüler.

    ![Yeni veritabanını ve koleksiyonu gösteren Azure portalı Veri Gezgini](./media/create-sql-api-dotnet/azure-cosmos-db-new-collection.png)

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni koleksiyonunuza veri ekleyebilirsiniz.

1. Yeni veritabanı, Veri Gezgini'nin Koleksiyonlar bölmesinde görüntülenir. **Görevler** veritabanını genişletin, **Öğeler** koleksiyonunu genişletin, **Belgeler**'e ve ardından **Yeni Belge**'ye tıklayın. 

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-sql-api-dotnet/azure-cosmosdb-new-document.png)
  
2. Şimdi koleksiyona aşağıdaki yapıya sahip bir belge ekleyin.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. JSON öğesini **Belgeler** sekmesine ekledikten sonra **Kaydet**'e tıklayın.

    ![Azure portalında JSON verilerini kopyalayın ve Veri Gezgini'ne kaydedin](./media/create-sql-api-dotnet/azure-cosmosdb-save-document.png)

4. `id` özelliği için benzersiz bir değer eklediğiniz yerde bir veya daha fazla belge oluşturun ve kaydedin ve diğer özellikleri uygun şekilde değiştirin. Azure Cosmos DB, verilerinizin bir şemaya uygun olmasını şart koşmadığı için yeni belgelerinizin yapısını istediğiniz şekilde oluşturabilirsiniz.

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. [GitHub'dan bir SQL API'si uygulaması](https://github.com/Azure-Samples/documentdb-dotnet-todo-app) kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

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
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

4. Ardından Visual Studio'daki TODO çözüm dosyasını açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. Bu hızlı başlangıçta, Azure portalını kullanarak bir veritabanı ve koleksiyonu oluşturma ve bir .NET örneği kullanarak örnek veriler ekleyin. Ancak, ayrıca veritabanı ve koleksiyonu .NET örneği kullanarak oluşturabilirsiniz. 

Aşağıdaki kod parçacıklarının tamamı, DocumentDBRepository.cs dosyasından alınır.

* Aşağıdaki kodda gösterildiği gibi DocumentClient başlatılır:

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
    ```

* Yeni bir veritabanı kullanılarak oluşturulan `CreateDatabaseAsync` aşağıdaki kodda gösterildiği gibi yöntemi:

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Yeni bir koleksiyon kullanılarak oluşturulan `CreateDocumentCollectionAsync` aşağıdaki kodda gösterildiği gibi:

    ```csharp
    private static async Task CreateCollectionIfNotExistsAsync()
    {
        try
        {
           await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
        }
        catch (DocumentClientException e)
        {
           if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
           {
              await client.CreateDocumentCollectionAsync(
              UriFactory.CreateDatabaseUri(DatabaseId),
              new DocumentCollection
              {
                  Id = CollectionId
              },
              new RequestOptions { OfferThroughput = 400 });
           }
           else
           {
             throw;
           }
        }
    }
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalda](https://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Anahtarlar**'ı ve ardından **Okuma/Yazma Anahtarları**'nı seçin. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda web.config dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. Visual Studio 2017'de web.config dosyasını açın. 

3. Portaldaki URI değerinizi kopyalayın (kopyalama düğmesini kullanarak) ve web.config dosyasına uç nokta değeri olarak yapıştırın. 

    `<add key="endpoint" value="FILLME" />`

4. Ardından portaldaki BİRİNCİL ANAHTAR değerinizi kopyalayıp web.config dosyasına authKey değeri olarak yapıştırın. 

    `<add key="authKey" value="FILLME" />`
    
5. Ardından, veritabanı ve koleksiyon değerleri daha önce oluşturduğunuz veritabanının adını eşleşecek şekilde güncelleştirin. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

   ```csharp
   <add key="database" value="ToDoList"/>
   <add key="collection" value="Items"/>
   ```
 
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma
1. Visual Studio'nun **Çözüm Gezgini** bölümünde projeye sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'i seçin. 

2. NuGet **Gözat** kutusuna *DocumentDB* yazın.

3. Sonuçlardan **Microsoft.Azure.DocumentDB** kitaplığını yükleyin. Bunu yaptığınızda Microsoft.Azure.DocumentDB paketi ve tüm bağımlılıklar yüklenir.

4. Uygulamayı çalıştırmak için CTRL+F5 tuşlarını seçin. Uygulamanız tarayıcınızda görüntülenir. 

5. Tarayıcıda **Create New** (Yeni Oluştur) düğmesini seçin ve yapılacaklar listesi uygulamanızda birkaç yeni görev oluşturun.

   ![Yapılacaklar listesi uygulaması ve örnek veriler](./media/create-sql-api-dotnet/azure-comosdb-todo-app-list.png)

Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bir web uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)


