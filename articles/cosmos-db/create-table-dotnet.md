---
title: "Tablo API&quot;sini kullanarak Azure Cosmos DB .NET uygulaması derleme | Microsoft Docs"
description: ".NET kullanarak Azure Cosmos DB&quot;nin Tablo API’si ile çalışmaya başlama"
services: cosmosdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: cba0b278d84e25876a8b73cedb7e35f84500fc5e
ms.contentlocale: tr-tr
ms.lasthandoff: 05/11/2017


---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Tablo API'sini kullanarak bir .NET uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak bir Azure Cosmos DB hesabını oluşturma ve bu hesap içinde tablo oluşturma işlemi anlatılmıştır. Bundan sonra varlık eklemek, güncelleştirmek ve silmek üzere kod yazıp bazı sorgular çalıştıracaksınız. Genel [Azure depolama SDK’sı](https://www.nuget.org/packages/WindowsAzure.Storage) ile aynı sınıf ve yöntem imzalarına sahip olmasının yanında [Tablo API’sini](table-introduction.md) (önizleme) kullanarak Azure Cosmos DB hesaplarına bağlanabilen [Azure Depolama Önizleme SDK’sını](https://aka.ms/premiumtablenuget) NuGet’ten indirebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmosdb-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmosdb-create-table](../../includes/cosmosdb-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni tablonuza veri ekleyebilirsiniz.

1. Veri Gezgini’nde **sample-database**, **sample-table** seçeneğini genişletin, **Varlıklar** ve ardından **Varlık Ekle**’ye tıklayın.
2. Şimdi PartitionKey değer kutusu ile RowKey değer kutusuna verileri ekleyin ve **Varlık Ekle**’ye tıklayın.

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
    Artık tablonuza daha fazla varlık ekleyebilir, varlıklarınızı düzenleyebilir veya Veri Gezgini’nde verilerinizi sorgulayabilirsiniz. Veri Gezgini ayrıca aktarım hızınızı ölçeklendirebileceğiniz ve tablonuza depolanmış yordamlar, kullanıcı tarafından tanımlanmış işlevler ve tetikleyiciler ekleyebileceğiniz yerdir.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir DocumentDB API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. Program.cs dosyasını açtığınızda Azure Cosmos DB kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

* DocumentClient başlatılır.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Henüz yoksa yeni bir tablo oluşturulur.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Yeni bir Tablo kapsayıcısı oluşturulur. Bu kodun normal Azure Tablo depolama SDK'sına çok benzer olduğunu fark edeceksiniz 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Anahtarlar**'a ve ardından **Okuma/Yazma Anahtarları**'na tıklayın. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda app.config dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-documentdb-dotnet-core/keys.png)

2. Visual Studio'da app.config dosyasını açın. 

3. Azure Cosmos DB hesap adınızı portaldan kopyalayın ve app.config dosyasındaki PremiumStorageConnection dize değerinde AccountName değerine yapıştırın. Yukarıdaki ekran görüntüsünde, hesap adı cosmos-db-quickstart şeklindedir. Hesap adınız portalın üst kısmında gösterilir.

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`

4. Ardından portaldaki BİRİNCİL ANAHTAR değerinizi kopyalayıp PremiumStorageConnectionString dosyasına accountKey değeri olarak yapıştırın. 

    `AccountKey=AUTHKEY`

5. Son olarak, URI değerinizi portalın Anahtarlar sayfasından kopyalayın (kopyala düğmesini kullanarak) ve PremiumStorageConnectionString dosyasına TableEndpoint değeri olarak yapıştırın.

    `TableEndpoint=https://COSMOSDB.documents.azure.com`

    StandardStorageConnectionString değerini olduğu gibi bırakabilirsiniz.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Visual Studio'nun **Çözüm Gezgini** bölümünde projeye sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet **Gözat** kutusuna *WindowsAzure.Storage* yazın ve **Yayın öncesini ekle** kutusunu işaretleyin. 

3. Sonuçlardan **WindowsAzure.Storage** kitaplığını yükleyin. Bunu yaptığınızda Azure Cosmos DB Tablo API paketinin önizlemesi ve tüm bağımlılıklar yüklenir.

4. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın.

    Konsol penceresinde tabloya eklenmekte olan veriler gösterilir. Betik tamamlandığında konsol penceresini kapatın. 

Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo API’si kullanarak sorgulama](tutorial-query-table.md)


