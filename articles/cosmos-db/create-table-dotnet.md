---
title: "Hızlı Başlangıç: .NET ile Tablo API’si - Azure Cosmos DB | Microsoft Docs"
description: "Bu hızlı başlangıçta Azure portalı ve .NET ile uygulama oluşturmak için Azure Cosmos DB Tablo API’sinin nasıl kullanılacağı gösterilmektedir"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: 44637049dd5d6cfe353afe98427d843a0d4e403a
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Hızlı Başlangıç: .NET ve Azure Cosmos DB ile Tablo API’si uygulaması oluşturma 

Bu hızlı başlangıçta GitHub’dan bir örneği kopyalayarak bir uygulama oluşturmak için .NET ve Azure Cosmos DB [Tablo API’sini](table-introduction.md) nasıl kullanacağınız gösterilmektedir. Bu hızlı başlangıçta ayrıca Azure Cosmos DB hesabı oluşturma ve web tabanlı Azure portalında tablo ve varlıklar oluşturmak için Veri Gezgini’ni kullanma da gösterilmektedir.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

> [!IMPORTANT] 
> Genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için yeni bir Tablo API’si hesabı oluşturmanız gerekir. Önizleme sırasında oluşturulan Tablo API’si hesapları genel olarak kullanılabilir SDK’lar tarafından desteklenmez.
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tablo ekleme

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni tablonuza veri ekleyebilirsiniz.

1. Veri Gezgini'nde **sample-table** seçeneğini genişletin, **Varlıklar**'a ve ardından **Varlık Ekle**'ye tıklayın.

   ![Azure portalındaki Veri Gezgini'nde yeni varlık oluşturma](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Şimdi PartitionKey değer kutusu ile RowKey değer kutularına verileri ekleyin ve **Varlık Ekle**’ye tıklayın.

   ![Yeni bir varlık için Bölüm Anahtarını ve Satır Anahtarını ayarlama](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Artık tablonuza daha fazla varlık ekleyebilir, varlıklarınızı düzenleyebilir veya Veri Gezgini’nde verilerinizi sorgulayabilirsiniz. Veri Gezgini ayrıca aktarım hızınızı ölçeklendirebileceğiniz ve tablonuza depolanmış yordamlar, kullanıcı tarafından tanımlanmış işlevler ve tetikleyiciler ekleyebileceğiniz yerdir.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Tablo uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere bir klasör olarak değiştirmek için `cd` komutunu kullanın. 

    ```bash
    cd "C:\git-samples"
    ```

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Sonra Visual Studio'daki TableStorage çözüm dosyasını açın. 

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. [Azure portalında](http://portal.azure.com/) **Bağlantı Dizesi**’ne tıklayın. 

    Ekranın sağ tarafındaki kopyala düğmesini kullanarak PRIMARY CONNECTION STRING’i kopyalayın.

    ![Bağlantı Dizesi bölmesindeki PRIMARY CONNECTION STRING’i görüntüleyin ve kopyalayın](./media/create-table-dotnet/connection-string.png)

2. Visual Studio'da App.config dosyasını açın. 

3. Bu öğretici Depolama Öykünücüsü kullanmadığından 8. satırdaki StorageConnectionString öğesini açıklama durumundan çıkarın ve 7. satırdaki StorageConnectionString öğesini açıklama yapın. 7. ve 8. satır artık şöyle görünmelidir:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Portaldan PRIMARY CONNECTION STRING öğesini 8. satırdaki StorageConnectionString değerine yapıştırın. Dizeyi tırnak işareti içinde yapıştırın. 

    > [!IMPORTANT]
    > Uç Noktanız documents.azure.com kullanıyorsa, bir önizleme hesabınız var demektir ve genel olarak kullanılabilir Tablo API’si SDK’ları ile çalışmak için [yeni bir Tablo API’si hesabı](#create-a-database-account) oluşturmanız gerekir. 
    > 

    8. Satır şuna benzer şekilde görünmelidir:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. App.config dosyasını kaydedin.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="build-and-deploy-the-app"></a>Uygulama derleme ve dağıtma

1. Visual Studio'nun **Çözüm Gezgini**’nde **TableStorage** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet **Gözat** kutusuna *Microsoft.Azure.CosmosDB.Table* yazın.

3. Sonuçlardan **Microsoft.Azure.CosmosDB.Table** kitaplığını yükleyin. Bunu yaptığınızda Azure Cosmos DB Tablo API paketi ve tüm bağımlılıklar yüklenir.

4. BasicSamples.cs dosyasını açın ve 30. satır ile 52. satıra bir kesme noktası ekleyin.

5. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın.

    Konsol penceresi, Azure Cosmos DB içinde yeni tablo veritabanına eklenen tablo verilerini görüntüler. 
    
    Bağımlılıklarla ilgili bir hata alırsanız bkz. [Sorun giderme](table-sdk-dotnet.md#troubleshooting).

    İlk kesme noktasına ulaştığınızda Azure portalında Veri Gezgini’ne dönün ve demo* tablosunu genişleterek **Varlıklar**’a tıklayın. Sağdaki **Varlıklar** sekmesi eklenen yeni varlığı gösterir; kullancıı için telefon numarasının 425-555-0101 olduğuna dikkat edin.
    
6. Veri Gezgini’nde Varlıklar sekmesini kapatın.
    
7. Uygulamayı bir sonraki kesme noktasına kadar çalıştırmaya devam edin.

    Kesme noktasına ulaştığınızda portala dönün, yeniden Varlıklar’a tıklayarak Varlıklar sekmesini açın ve telefon numarasının 425-555-0105 olarak güncelleştirildiğine dikkat edin.

8. Konsol penceresine dönüp CTRL + C tuşlarına basarak uygulamanın yürütülmesini sona erdirin. 

    Artık Veri Gezgini’ne dönerek varlıkları ekleyebilir ya da dönüştürebilir ve verileri sorgulayabilirsiniz.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak tablo oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz.  Şimdi Tablo API'sini kullanarak verilerinizi sorgulayabilirsiniz.  

> [!div class="nextstepaction"]
> [Tablo verilerini Tablo API’sine aktarma](table-import.md)

