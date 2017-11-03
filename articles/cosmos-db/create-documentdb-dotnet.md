---
title: "Azure Cosmos DB: .NET ve DocumentDB API'si ile bir web uygulaması derleme | Microsoft Docs"
description: "Azure Cosmos DB DocumentDB API'sine bağlanmak ve sorgu göndermek için kullanabileceğiniz bir .NET kodu örneği sunar"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 295d3b8983484b33c69ebb5d0d68c451211102a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-the-azure-portal"></a>Azure Cosmos DB: .NET ve Azure portalı ile bir DocumentDB API web uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak bir Azure Cosmos DB hesabını, belge veritabanını ve koleksiyonunu nasıl oluşturacağınız anlatılmıştır. Bu işlemlerin ardından aşağıdaki ekran görüntüsünde gösterilen şekilde [DocumentDB .NET API'si](documentdb-sdk-dotnet.md) üzerinde bir yapılacaklar listesi web uygulaması derleyecek ve dağıtacaksınız. 

![Yapılacaklar listesi uygulaması ve örnek veriler](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a>Koleksiyon ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni koleksiyonunuza veri ekleyebilirsiniz.

1. Yeni veritabanı, Veri Gezgini'nin Koleksiyonlar bölmesinde görüntülenir. **Görevler** veritabanını genişletin, **Öğeler** koleksiyonunu genişletin, **Belgeler**'e ve ardından **Yeni Belge**'ye tıklayın. 

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
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

    ![Azure portalında JSON verilerini kopyalayın ve Veri Gezgini'ne kaydedin](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  `id` özelliği için benzersiz bir değer eklediğiniz yerde bir veya daha fazla belge oluşturun ve kaydedin ve diğer özellikleri uygun şekilde değiştirin. Azure Cosmos DB, verilerinizin bir şemaya uygun olmasını şart koşmadığı için yeni belgelerinizin yapısını istediğiniz şekilde oluşturabilirsiniz.

     Şimdi verilerinizi almak için Veri Gezgini'ndeki sorguları kullanabilirsiniz. Veri Gezgini koleksiyondaki tüm belgeleri almak için varsayılan olarak `SELECT * FROM c` komutunu kullanır ancak bunu `SELECT * FROM c ORDER BY c._ts DESC` gibi farklı bir [SQL sorgusuyla](documentdb-sql-query.md) değiştirerek tüm belgelerin zaman damgasına göre azalan sırada döndürülmesini sağlayabilirsiniz.
 
     Veri Gezgini'ni kullanarak ayrıca saklı yordamlar, UDF'ler ve tetikleyiciler oluşturabilir, bu sayede sunucu tarafı iş mantığını gerçekleştirebilir ve aktarım hızını ölçeklendirebilirsiniz. Veri Gezgini, API'lerdeki tüm yerleşik programlı veri erişimini açığa çıkarır ancak Azure portalındaki verilerinize kolayca erişmenizi sağlar.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. GitHub'dan bir DocumentDB API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve `CD` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Ardından Visual Studio'daki TODO çözüm dosyasını açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. DocumentDBRepository.cs dosyasını açtığınızda Azure Cosmos DB kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

* 73 satırda DocumentClient başlatılır.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* 88 satırda yeni bir veritabanı oluşturulur.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* 107 satırda yeni bir koleksiyon oluşturulur.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Anahtarlar**'a ve ardından **Okuma/Yazma Anahtarları**'na tıklayın. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda web.config dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-documentdb-dotnet/keys.png)

2. Visual Studio 2017'de web.config dosyasını açın. 

3. Portaldaki URI değerinizi kopyalayın (kopyalama düğmesini kullanarak) ve web.config dosyasına uç nokta değeri olarak yapıştırın. 

    `<add key="endpoint" value="FILLME" />`

4. Ardından portaldaki BİRİNCİL ANAHTAR değerinizi kopyalayıp web.config dosyasına authKey değeri olarak yapıştırın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma
1. Visual Studio'nun **Çözüm Gezgini** bölümünde projeye sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet **Gözat** kutusuna *DocumentDB* yazın.

3. Sonuçlardan **Microsoft.Azure.DocumentDB** kitaplığını yükleyin. Bunu yaptığınızda Microsoft.Azure.DocumentDB paketi ve tüm bağımlılıklar yüklenir.

4. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın. Uygulamanız tarayıcınızda görüntülenir. 

5. Tarayıcıda **Create New** (Yeni Oluştur) düğmesine tıklayın ve yapılacaklar listesi uygulamanızda birkaç yeni görev oluşturun.

   ![Yapılacaklar listesi uygulaması ve örnek veriler](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bir web uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)


