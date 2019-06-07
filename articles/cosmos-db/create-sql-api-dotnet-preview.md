---
title: "Hızlı Başlangıç: SQL API'sini ve Azure portalını kullanarak Azure Cosmos DB ile bir ASP.NET web uygulaması derleme"
description: Bu hızlı başlangıçta, ASP.NET web uygulaması oluşturmak için Azure Cosmos DB SQL API ve Azure portalını kullanın
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 12/01/2018
ms.author: dech
ms.openlocfilehash: bf749cde85be10bae5f7acf3f1f995be1b81c30f
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754340"
---
# <a name="quickstart-build-an-aspnet-web-app-using-azure-cosmos-db-sql-api-account"></a>Hızlı Başlangıç: Azure Cosmos DB SQL API hesabı kullanarak bir ASP.NET web uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET (Önizleme)](create-sql-api-dotnet-preview.md)
> * [.NET](create-sql-api-dotnet.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)
>  
> 

Azure Cosmos DB, Microsoft'un Global olarak dağıtılmış, yüksek oranda kullanılabilir, çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıçta bir Azure Cosmos DB oluşturma işlemini gösterir [SQL API](sql-api-introduction.md) hesabı, veritabanı ve Azure portalını kullanarak kapsayıcı. Daha sonra yapı ve üzerinde derlenmiş bir ASP.NET Yapılacaklar listesi web uygulaması dağıtma [SQL .NET API](sql-api-sdk-dotnet.md)aşağıdaki ekran görüntüsünde gösterildiği gibi. 

Bu hızlı başlangıçta Azure Cosmos DB .NET SDK'sının 3.0 + sürümü kullanılır. 

![Yapılacaklar listesi uygulaması ve örnek veriler](./media/create-sql-api-dotnet/azure-comosdb-todo-app-list-preview.png)

## <a name="prerequisites"></a>Önkoşullar

Visual Studio yüklü 2019 yoksa, indirip kullanabilirsiniz **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). Etkinleştirdiğinizden emin olun **Azure geliştirme** Visual Studio Kurulumu sırasında iş yükü.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-an-azure-cosmos-account"></a>Bir Azure Cosmos hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

<a id="create-collection"></a>
## <a name="add-a-container"></a>Bir kapsayıcı ekleyin

[!INCLUDE [cosmos-db-create-collection-preview](../../includes/cosmos-db-create-collection-preview.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. [GitHub'dan bir SQL API'si uygulaması](https://github.com/Azure-Samples/cosmos-dotnet-todo-app) kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. 

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
    git clone https://github.com/Azure-Samples/cosmos-dotnet-todo-app.git
    ```

4. Açık **todo.sln** Visual Studio'daki çözüm dosyası. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

Unutmayın, .NET SDK'sının önceki sürümüyle biliyorsanız, koşulları 'collection' ve 'document' görmeye kullanılabilir Azure Cosmos DB, birden çok API modelini desteklediğinden, sürüm 3.0 + .NET SDK'sının genel Koşulları 'kapsayıcısı' ve 'öğesi' kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. [Daha fazla ilgili veritabanları, kapsayıcıları ve öğeleri öğrenin.](databases-containers-items.md)

Aşağıdaki kod parçacıklarının tümü ToDoItemService.cs dosyasından alınmıştır.

* CosmosClient 68 69 satırlarda başlatılır.

    ```csharp
    CosmosConfiguration config = new CosmosConfiguration(Endpoint, PrimaryKey);
    client = new CosmosClient(config);
    ```

* 71. satırda yeni bir veritabanı oluşturulur.

    ```csharp
    CosmosDatabase database = await client.Databases.CreateDatabaseIfNotExistsAsync(DatabaseId);
    ```

* Bölüm anahtarı ile 72 satırda yeni bir kapsayıcı oluşturulur "/ kategori."

    ```csharp
    CosmosContainer container = await database.Containers.CreateContainerIfNotExistsAsync(ContainerId, "/category");
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalda](https://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Anahtarlar**'ı ve ardından **Okuma/Yazma Anahtarları**'nı seçin. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda web.config dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. Visual Studio'da açın **web.config** dosya. 

3. (Kopyalama düğmesini kullanarak) portaldaki URI değerinizi kopyalayın ve değeri yapın ``endpoint`` web.config dosyasındaki anahtar. 

    `<add key="endpoint" value="FILLME" />`

4. Ardından portaldaki birincil anahtar değerinizi kopyalayın ve değeri yapın ``primarykey`` web.config dosyasındaki. 

    `<add key="primaryKey" value="FILLME" />`
    
5. Ardından veritabanını ve daha önce oluşturduğunuz kapsayıcının adını eşleştirmek için veritabanı ve kapsayıcı değeri güncelleştirin. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `<add key="database" value="Tasks" />`

    `<add key="container" value="Items" />`
    
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. İçinde **Çözüm Gezgini**olan Visual Studio çözümünüzün altındaki yeni konsol uygulaması projeniz üzerinde sağ tıklayın ve ardından **NuGet paketlerini Yönet...**
    
    ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/create-sql-api-dotnet/manage-nuget-package.png)
1. İçinde **NuGet** sekmesinde **Gözat**ve türü **Microsoft.Azure.Cosmos** arama kutusuna.
1. Bul sonuçları içinde **Microsoft.Azure.Cosmos** tıklatıp **yükleme**.
   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)’dır.

   ![Azure Cosmos DB istemci SDK'sını bulmak için NuGet menüsünün ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

1. Uygulamayı çalıştırmak için CTRL+F5 tuşlarını seçin. Uygulamanız tarayıcınızda görüntülenir. 

1. Tarayıcıda **Create New** (Yeni Oluştur) düğmesini seçin ve yapılacaklar listesi uygulamanızda birkaç yeni görev oluşturun. İçinde oluşturduğunuz görevlerin sayfasına da [örnek verileri Ekle](#add-sample-data)

   ![Yapılacaklar listesi uygulaması ve örnek veriler](./media/create-sql-api-dotnet/azure-comosdb-todo-app-list-preview.png)

Şimdi veri Gezgini'ne dönün ve bakın, sorgu, değiştirebilir ve bu yeni verilerle çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Cosmos DB hesabı oluşturma, bir kapsayıcı oluşturun ve Veri Gezgini'ni kullanarak öğeleri ekleyin ve bir web uygulamasını çalıştırmayı öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)


