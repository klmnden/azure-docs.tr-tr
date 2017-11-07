---
title: "Tablo API'sini kullanarak Azure Cosmos DB .NET uygulaması derleme | Microsoft Docs"
description: ".NET kullanarak Azure Cosmos DB'nin Tablo API’si ile çalışmaya başlama"
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
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: 9b1d41fe185f4c3d5fdce13ab8f0136bc961f013
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Tablo API'sini kullanarak bir .NET uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak bir Azure Cosmos DB hesabını oluşturma ve bu hesap içinde tablo oluşturma işlemi anlatılmıştır. Varlıkları eklemek, güncelleştirmek ve silmek için kod yazabilecek ve NuGet'in yeni [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (önizleme) paketini kullanarak bazı sorguları çalıştırabileceksiniz. Bu kitaplık genel [Windows Azure Depolama SDK'sı](https://www.nuget.org/packages/WindowsAzure.Storage) ile aynı sınıf ve yöntem imzalarına sahip olmasının yanı sıra [Tablo API'sini](table-introduction.md) (önizleme) kullanarak Azure Cosmos DB hesaplarına da bağlanabilir. 

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni tablonuza veri ekleyebilirsiniz.

1. Veri Gezgini'nde **sample-table** seçeneğini genişletin, **Varlıklar**'a ve ardından **Varlık Ekle**'ye tıklayın.

   ![Azure portalındaki Veri Gezgini'nde yeni varlık oluşturma](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Şimdi PartitionKey değer kutusu ile RowKey değer kutusuna verileri ekleyin ve **Varlık Ekle**’ye tıklayın.

   ![Yeni bir varlık için Bölüm Anahtarını ve Satır Anahtarını ayarlama](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Artık tablonuza daha fazla varlık ekleyebilir, varlıklarınızı düzenleyebilir veya Veri Gezgini’nde verilerinizi sorgulayabilirsiniz. Veri Gezgini ayrıca aktarım hızınızı ölçeklendirebileceğiniz ve tablonuza depolanmış yordamlar, kullanıcı tarafından tanımlanmış işlevler ve tetikleyiciler ekleyebileceğiniz yerdir.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. Program.cs dosyasını açtığınızda Azure Cosmos DB kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

* CloudTableClient başlatılır.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Henüz yoksa yeni bir tablo oluşturulur.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Bir dizi adımı tablosunu kullanarak yürütülme `TableOperation` sınıfı.

   ```csharp
   TableOperation insertOperation = TableOperation.Insert(item);
   table.Execute(insertOperation);
   ```
   
   ```csharp
   TableOperation retrieveOperation = TableOperation.Retrieve<T>(items[i].PartitionKey, items[i].RowKey);
   table.Execute(retrieveOperation);
   ```
   
   ```csharp
   TableOperation deleteOperation = TableOperation.Delete(items[i]);
   table.Execute(deleteOperation);
   ```


## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Şimdi bağlantı dizesi bilgilerini güncelleştireceğiz. Böylece uygulamanız Azure Cosmos DB ile iletişim kurabilecek. 

1. Visual Studio'da app.config dosyasını açın. 

2. [Azure portalının](http://portal.azure.com/) solunda bulunan Azure Cosmos DB gezinti menüsünde **Bağlantı Dizesi**'ne tıklayın. Ardından yeni bölmede bağlantı dizesine ilişkin Kopyala düğmesine tıklayın. 

    ![Bağlantı Dizesi bölmesindeki Uç Nokta ve Hesap Anahtarını görüntüleyin ve kopyalayın](./media/create-table-dotnet/keys.png)

3. Değeri app.config dosyasına PremiumStorageConnectionString değeri olarak yapıştırın. 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    StandardStorageConnectionString değerini olduğu gibi bırakabilirsiniz.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

1. Visual Studio'da **Çözüm Gezgini**'ndeki **PremiumTableGetStarted** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet'teki **Gözat** kutusuna *WindowsAzure.Storage PremiumTable* yazın.

3. **Ön sürümü dahil et** kutusunu işaretleyin. 

4. Sonuçlar arasında yer alan **WindowsAzure.Storage-PremiumTable** kitaplığını yükleyin. Bunu yaptığınızda Azure Cosmos DB Tablo API paketinin önizlemesi ve tüm bağımlılıklar yüklenir. Bu paketin Azure Tablo depolaması tarafından kullanılan Windows Azure Depolama paketinden farklı bir NuGet paketi olduğunu unutmayın. 

5. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın.

    Konsol penceresi; eklenen, alınan, sorgulanan, değiştirilen ve tablodan silinen verileri görüntüler. Betik tamamlandığında herhangi bir tuşa basarak konsol penceresini kapatın. 
    
    ![Hızlı başlangıç konsol çıktısı](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. Yeni varlıkları Veri Gezgini'nde görmek istiyorsanız için program.cs'de 188-208 numaralı satırları açıklama satırı yapmanız (silinmemeleri için) ve örneği tekrar çalıştırmanız yeterlidir. 

    Şimdi Veri Gezgini'ne geri dönüp **Yenile** düğmesine tıklayın, **people** tablosunu genişletin ve **Varlıklar**'a tıklayın. Artık yeni verilerle çalışabilirsiniz. 

    ![Veri Gezgini'ndeki yeni varlıklar](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo API’si kullanarak sorgulama](tutorial-query-table.md)

