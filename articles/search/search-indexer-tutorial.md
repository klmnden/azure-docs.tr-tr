---
title: Azure Search'te Azure SQL veritabanlarının dizinini oluşturmak için öğretici | Microsoft Docs
description: Aranabilir verileri ayıklamak ve Azure Search dizinini doldurmak için Azure SQL veritabanında gezinin.
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: na
ms.topic: tutorial
ms.date: 11/10/2017
ms.author: heidist
ms.openlocfilehash: f123b4f5d0a51a4ab5015a2a0008a76fbfa0318e
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="how-to-crawl-an-azure-sql-database-using-azure-search-indexers"></a>Azure Search dizin oluşturucuyu kullanarak Azure SQL veritabanında gezinme

Bu öğreticide, örnek bir Azure SQL veritabanında aranabilir verileri ayıklamak için dizin oluşturucunun nasıl yapılandırılacağı gösterilir. [Dizin Oluşturucular](search-indexer-overview.md), Azure Search'ün dış veri kaynaklarında gezinen ve [arama dizinini](search-what-is-an-index.md) içerikle dolduran bir bileşenidir. Tüm dizin oluşturucular arasında, en yaygın kullanılan Azure SQL veritabanının dizin oluşturucusudur. 

Dizin oluşturucu yapılandırmasında yeterlilik kazanmak yararlı olur çünkü yazmanız ve korumanız gereken kod miktarını azaltır. Şemayla uyumlu bir JSON veri kümesi hazırlayıp göndermek yerine, veri kaynağına bir dizin oluşturucu ekleyebilir, bu dizin oluşturucunun verileri ayıklamasını ve dizine eklemesini sağlayabilir ve isteğe bağlı olarak temel kaynaktaki değişiklikleri almak için dizin oluşturucuyu yinelenen bir zamanlamada çalıştırabilirsiniz.

Bu öğreticide, [Azure Search .NET istemci kitaplıklarını](https://aka.ms/search-sdk) ve .NET Core konsol uygulamasını kullanarak aşağıdaki görevleri yerine getireceksiniz:

> [!div class="checklist"]
> * Çözümü indirme ve yapılandırma
> * Uygulama ayarlarına arama hizmeti bilgilerini ekleme
> * Azure SQL veritabanında bir dış veri kümesi hazırlama 
> * Örnek kodda dizinin ve dizin oluşturucunun tanımlarını gözden geçirme
> * Dizin oluşturucu kodunu çalıştırarak verileri içeri aktarma
> * Dizinde arama yapma
> * Portalda dizin oluşturucu yapılandırmasını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure hesabı. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/free/). 

* Azure Search hizmeti. Hizmeti ayarlamaya yardımcı olması için bkz. [Arama hizmeti oluşturma](search-create-service-portal.md).

* Dizin oluşturucunun kullandığı dış veri kaynağını sağlayan bir Azure SQL veritabanı. Örnek çözümde, tablo oluşturmak için bir SQL veri dosyası sağlanır.

* Visual Studio 2017. Ücretsiz [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)'ı kullanabilirsiniz. 

> [!Note]
> Ücretsiz Azure Search hizmetini kullanıyorsanız, üç dizin, üç dizin oluşturucu ve üç veri kaynağı sınırlandırmanız vardır. Bu öğreticide hepsinden birer tane oluşturulur. Hizmetinizde yeni kaynakları kabul edecek kadar yer olduğundan emin olun.

## <a name="download-the-solution"></a>Çözümü indirme

Bu öğreticide kullanılan dizin oluşturucu çözümü, tek bir ana indirmeyle sunulan Azure Search örnekleri koleksiyonundan alınmıştır. Bu öğreticide kullanılan çözüm *DotNetHowToIndexers*'dır.

1. Azure örnekleri GitHub deposunda [**Azure-Samples/search-dotnet-getting-started**](https://github.com/Azure-Samples/search-dotnet-getting-started) bölümüne gidin.

2. **Clone or Download** > **Download ZIP** öğesine tıklayın. Varsayılan olarak, dosya İndirilenler klasörüne gider.

3. **Dosya Gezgini** > **İndirilenler**'de dosyaya sağ tıklayın ve **Tümünü ayıkla**'yı seçin.

4. Salt okunur izinlerini devre dışı bırakın. Klasör adına sağ tıklayın, **Özellikler** > **Genel**'e tıklayın, ardından geçerli klasör, alt klasörler ve dosyalar için **Salt okunur** özniteliğini temizleyin.

5. **Visual Studio 2017**'de, *DotNetHowToIndexers.sln* çözümünü açın.

6. **Çözüm Gezgini**'nde, en üst düğüm ana Çözüm'e sağ tıklayın ve **NuGet Paketlerini Geri Yükle**'ye tıklayın.

## <a name="set-up-connections"></a>Bağlantıları ayarlama
Gerekli hizmetlere bağlantı bilgileri, çözümdeki **appsettings.json** dosyasında belirtilir. 

Bu öğreticide sağlanan yönergeleri kullanarak tüm ayarları doldurabilmek için, Çözüm Gezgini'nde **appsettings.json** dosyasını açın.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
}
```

### <a name="get-the-search-service-name-and-admin-api-key"></a>Arama hizmeti adını ve yönetici API anahtarını alma

Arama hizmeti uç noktasını ve anahtarı portalda bulabilirsiniz. Anahtar, hizmet işlemlerine erişim sağlar. Yönetici anahtarları, hizmetinizde dizin ve dizin oluşturucu gibi nesneleri oluşturmak ve silmek için gereken yazma erişimini sağlar.

1. [Azure Portal](https://portal.azure.com/)'da oturum açın ve [aboneliğinize ilişkin arama hizmetlerini bulun](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

2. Hizmet sayfasını açın.

3. En üstte, ana sayfada hizmet adını bulun. Aşağıdaki ekran görüntüsünde bu ad *azs-tutorial*'dır.

   ![Hizmet adı](./media/search-indexer-tutorial/service-name.png)

4. Bu adı kopyalayın ve Visual Studio'da ilk girdi olarak **appsettings.json** dosyasına yapıştırın.

  > [!Note]
  > Hizmet adı, search.windows.net'i içeren uç noktanın bir parçasıdır. Merak ediyorsanız, tam URL'yi Genel Bakış sayfasındaki **Temel Bileşenler**'de görebilirsiniz. URL bu örnekteki gibi görünür: https://your-service-name.search.windows.net

5. Son taraftaki **Ayarlar** > **Anahtarlar**'da, yönetici anahtarlarından birini kopyalayın ve **appsettings.json**'a ikinci girdi olarak yapıştırın. Anahtarlar, hazırlama sırasında hizmetiniz için oluşturulan alfasayısal dizelerdir ve hizmet işlemlerine yetkili erişim için gereklidir. 

  Her iki ayarı da ekledikten sonra, dosyanız şu örneğe benzer olmalıdır:

  ```json
  {
    "SearchServiceName": "azs-tutorial",
    "SearchServiceAdminApiKey": "A1B2C3D4E5F6G7H8I9J10K11L12M13N14",
    . . .
  }
  ```

## <a name="prepare-an-external-data-source"></a>Dış veri kaynağını hazırlama

Bu adımda, dizin oluşturucunun gezinebileceği bir dış veri kaynağı oluşturun. Bu öğreticide veri dosyası *hotels.sql*'dir ve \DotNetHowToIndexers çözüm klasöründe sağlanır. 

### <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL Veritabanı'nda veri kümesini oluşturmak için Azure Portal'ı ve örnekteki *hotels.sql* dosyasını kullanabilirsiniz. Azure Search, bir görünüm veya sorgudan oluşturulanlar gibi düzleştirilmiş satır kümelerini kullanır. Örnek çözümdeki SQL dosyası tek bir tablo oluşturur ve doldurur.

Aşağıdaki alıştırmada, mevcut sunucu veya veritabanı olmadığı varsayılır ve 2. adımda size her ikisini de oluşturma yönergeleri verilir. İsteğe bağlı olarak, kaynağınızın mevcut olması durumunda, oteller tablosunu buna ekleyebilir ve 4. adımdan başlayabilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın. 

2. Veritabanı, sunucu ve kaynak grubu oluşturmak için **Kaynak oluştur** > **SQL Veritabanı**'na tıklayın. Varsayılan değerleri ve en düşük düzey fiyatlandırma katmanını kullanabilirsiniz. Sunucu oluşturmanın bir avantajı, yönetici kullanıcı adı ve parolası belirtebilmenizdir; çünkü sonraki adımda tabloları oluşturmak ve yüklemek için bunlar gerekecektir.

   ![Yeni veritabanı sayfası](./media/search-indexer-tutorial/indexer-new-sqldb.png)

3. Yeni sunucuyu ve veritabanını dağıtmak için **Oluştur**'a tıklayın. Sunucu ve veritabanının dağıtılmasını bekleyin.

4. Yeni veritabanınızın SQL Veritabanı sayfasını (zaten açık değilse) açın. Kaynak adı *SQL Server* değil *SQL veritabanı* olmalıdır.

  ![SQL veritabanı sayfası](./media/search-indexer-tutorial/hotels-db.png)

4. Komut çubuğunda **Araçlar** > **Sorgu düzenleyicisi**'ne tıklayın.

5. **Oturum Aç**'a tıklayın ve sunucu yöneticisinin kullanıcı adıyla parolasını girin.

6. **Sorgu aç**'a tıklayın ve *hotels.sql* dosyasının konumuna gidin. 

7. Dosyayı seçin ve **Aç**'a tıklayın. Betik aşağıdaki ekran görüntüsüne benzer görünmelidir:

  ![SQL betiği](./media/search-indexer-tutorial/sql-script.png)

8. Sorguyu yürütmek için **Çalıştır**'a tıklayın. Sonuçlar bölmesinde, 3 satır için sorgu başarılı oldu iletisini görmeniz gerekir.

9. Bu tablodan bir satır kümesi döndürmek için, doğrulama adımı olarak aşağıdaki sorguyu yürütebilirsiniz:

   ```sql
   SELECT HotelId, HotelName, Tags FROM Hotels
   ```
   `SELECT * FROM Hotels` sorgu prototipi Sorgu Düzenleyicisi'nde çalışmaz. Örnek verilerde, Konum alanında coğrafi koordinatlar vardır ve bunlar şu anda düzenleyicide işlenmemektedir. Sorgulanacak diğer sütunların listesi için, şu deyimi yürütebilirsiniz: `SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.Hotels')`

10. Artık bir dış veri kümeniz olduğuna göre, veritabanının ADO.NET bağlantı dizesini kopyalayın. Veritabanınızın SQL Veritabanı sayfasında **Ayarlar** > **Bağlantı Dizeleri**'ne gidin ve ADO.NET bağlantı dizesini kopyalayın.
 
  ADO.NET bağlantı dizesi aşağıdaki örneğe benzer; geçerli bir veritabanı adı, kullanıcı adı ve parola kullanacak şekilde değiştirilmiştir.

  ```sql
  Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
  ```
11. Bağlantı dizesini, Visual Studio'da **appsettings.json** dosyasındaki üçüncü girdi olarak "AzureSqlConnectionString" içine yapıştırın.

    ```json
    {
      "SearchServiceName": "azs-tutorial",
      "SearchServiceAdminApiKey": "A1B2C3D4E5F6G7H8I9J10K11L12M13N14",
      "AzureSqlConnectionString": "Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security  Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

## <a name="understand-index-and-indexer-code"></a>Dizin ve dizin oluşturucu kodunu anlama

Kodunuz artık derlenmeye ve çalıştırılmaya hazırdır. Bunu yapmadan önce, birkaç dakikanızı ayırıp bu örnekteki dizin ve dizin oluşturucu tanımlarını öğrenin. İlgili kod iki dosyada yer alır:

  + **hotel.cs**, dizini tanımlayan şemayı içerir
  + **Program.cs**, hizmetinizdeki yapıları oluşturmaya ve yönetmeye yönelik işlevleri içerir

### <a name="in-hotelcs"></a>Hotel.cs dosyasında

Dizin şeması, aşağıdaki HotelName alan tanımında gösterildiği gibi alanın tam metin araması yapılabilir, filtrelenebilir veya sıralanabilir olup olmadığı gibi izin verilen işlemleri belirten öznitelikler de dahil olmak üzere alan koleksiyonunu tanımlar. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Şema başka öğeler, örneğin arama puanını artırmak için puanlama profilleri, özel çözümleyiciler ve başka yapılar içerebilir. Öte yandan, bizim amaçlarımıza uygun olarak şema yalnızca örnek veri kümelerinde bulunan alanlarla seyrek bir şekilde tanımlanmıştır.

Bu öğreticide, dizin oluşturucu verileri tek bir veri kaynağından çeker. Pratikte, aynı dizine birden çok dizin oIuşturucu ekleyebilir, birden çok veri kaynağı ve dizin oluşturucudan birleştirilmiş, arama yapılabilir bir dizin oluşturabilirsiniz. Nerede esnekliğe ihtiyacınız olduğuna bağlı olarak, yalnızca veri kaynaklarında değişiklik yapıp aynı dizin oluşturucu-dizin çiftini veya çeşitli dizin oluşturucu ve veri kaynağı bileşimleriyle tek bir dizini kullanabilirsiniz.

### <a name="in-programcs"></a>Program.cs dosyasında

Ana program üç temsili veri kaynağı için de işlevler içerir. Yalnızca Azure SQL Veritabanı'na odaklanıldığında, aşağıdaki nesneler dikkati çeker:

  ```csharp
  private const string IndexName = "hotels";
  private const string AzureSqlHighWaterMarkColumnName = "RowVersion";
  private const string AzureSqlDataSourceName = "azure-sql";
  private const string AzureSqlIndexerName = "azure-sql-indexer";
  ```

Azure Search'te, bağımsız olarak görüntüleyebildiğiniz, yapılandırabildiğiniz veya silebildiğiniz nesneler dizin, dizin oluşturucu ve veri kaynakları (sırasıyla *hotels*, *azure-sql-indexer*, *azure-sql*) içerir. 

*AzureSqlHighWaterMarkColumnName* sütununa özel olarak değinmek yararlı olur çünkü bu sütun değişiklik algılama bilgilerini sağlar. Bu bilgiler dizin oluşturucu tarafından bir satırın son dizin oluşturma iş yükünden bu yana değiştirilip değiştirilmediğini saptamak için kullanılır. [Değişiklik algılama ilkeleri](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) yalnızca dizin oluşturucularda desteklenir ve veri kaynağına göre değişir. Azure SQL Veritabanı için, veritabanının gereksinimlerine bağlı olarak iki ilkeden birini seçebilirsiniz.

Aşağıdaki kod, veri kaynağını ve dizin oluşturucuyu oluşturmak için kullanılan Program.cs dosyasındaki yöntemleri gösterir. Kod, bu programı birden çok kez çalıştırabileceğiniz varsayımıyla, aynı adı taşıyan kaynakları denetler ve siler.

  ```csharp
  private static string SetupAzureSqlIndexer(SearchServiceClient serviceClient, IConfigurationRoot configuration)
  {
    Console.WriteLine("Deleting Azure SQL data source if it exists...");
    DeleteDataSourceIfExists(serviceClient, AzureSqlDataSourceName);

    Console.WriteLine("Creating Azure SQL data source...");
    DataSource azureSqlDataSource = CreateAzureSqlDataSource(serviceClient, configuration);

    Console.WriteLine("Deleting Azure SQL indexer if it exists...");
    DeleteIndexerIfExists(serviceClient, AzureSqlIndexerName);

    Console.WriteLine("Creating Azure SQL indexer...");
    Indexer azureSqlIndexer = CreateIndexer(serviceClient, AzureSqlDataSourceName, AzureSqlIndexerName);

    return azureSqlIndexer.Name;
  }
  ```

Çağrılacak gezginin türünü belirten [DataSourceType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) dışında, dizin oluşturucu API çağrılarının platformdan bağımsız olduğuna dikkat edin.

## <a name="run-the-indexer"></a>Dizin oluşturucuyu çalıştırma

Bu adımda, programı derleyin ve çalıştırın. 

1. Çözüm Gezgini'nde **DotNetHowToIndexers** öğesine sağ tıklayın ve **Oluştur**'u seçin.
2. Bir kez daha **DotNetHowToIndexers** öğesine sağ tıklayın ve ardından **Hata Ayıkla** > **Yeni örnek başlat**'a tıklayın.

Program hata ayıklama modunda yürütülür. Konsol penceresinde her işlemin durumu bildirilir.

  ![SQL betiği](./media/search-indexer-tutorial/console-output.png)

Kodunuz, Azure'da arama hizmetinize bağlanarak Visual Studio'da yerel olarak çalıştırılır. Arama hizmeti de bağlantı dizesini kullanarak Azure SQL Veritabanı'na bağlanır ve veri kümesini alır. Bu kadar çok işlem olduğunda, bazı olası hata noktaları vardır ama hata alırsanız, önce aşağıdaki koşulları denetleyin:

+ Sağladığınız arama hizmeti bağlantı bilgileri, bu öğreticide hizmet adıyla sınırlıdır. Tam URL girdiyseniz, dizin oluşturma aşamasında bağlantı hatasıyla işlemler durdurulur.

+ **Appsettings.json** dosyasındaki veritabanı bağlantı bilgileri. Bu, portaldan alınmış ve veritabanınız için geçerli kullanıcı adı ve parolayı içerecek şekilde değiştirilmiş ADO.NET bağlantı dizesi olmalıdır. Kullanıcı hesabının verileri alma izni olmalıdır.

+ Kaynak sınırları. Paylaşılan (ücretsiz) hizmetin 3 dizin, dizin oluşturucu ve veri kaynağıyla sınırlı olduğunu hatırlayın. Sınırına dayanmış bir hizmet yeni nesneler oluşturamaz.

## <a name="search-the-index"></a>Dizinde arama yapma 

Azure Portal'da, arama hizmetinin Genel Bakış sayfasında üst kısımdaki **Arama gezgini**'ne tıklayarak yeni dizine birkaç sorgu gönderin.

1. Üst kısımdaki **Dizini değiştir**'e tıklayarak *hotels* dizinini seçin.

2. Boş bir arama göndermek için **Ara** düğmesine tıklayın. 

  Dizininizdeki üç girdi, JSON belgeleri olarak döndürülür. Yapının tamamını görebilmeniz için arama gezgini belgeleri JSON olarak döndürür.

3. Ardından, bir arama dizesi girin: `search=river&$count=true`. 

  Bu sorgu, `river` terimi için tam metin aramasını çağırır ve sonuç eşleşen belgelerin sayısını içerir. Binlerce veya milyonlarca belge içeren çok büyük bir dizininiz olduğunda, test senaryolarında eşleşen belge sayısının döndürülmesi yararlı olur. Bizim örneğimizde, sorguyla eşleşen tek bir belge vardır.

4. Son olarak, JSON çıkışını ilgilendiğiniz alanlarla sınırlandıran bir arama dizesi girin: `search=river&$count=true&$select=hotelId, baseRate, description`. 

  Sorgu yanıtı seçili alanlara daraltılır ve sonuçta daha kısa bir çıkış elde edilir.

## <a name="view-indexer-configuration"></a>Dizin oluşturucu yapılandırmasını görüntüleme

Az önce programlama yoluyla oluşturduğunuz dizin oluşturucu da dahil olmak üzere tüm dizin oluşturucular portalda listelenir. Dizin oluşturucu tanımını açabilir ve veri kaynağını görüntüleyebilir veya yeni ve değiştirilmiş satırları seçmek için bir yenileme zamanlaması yapılandırabilirsiniz.

1. Azure Search hizmetinizin hizmet Genel Bakış sayfasını açın.
2. Ekranı aşağı kaydırarak **Dizin Oluşturucular** ve **Veri Kaynakları** kutucuklarını bulun.
3. Kaynakların listesini açmak için kutucuğa tıklayın. Yapılandırma ayarlarını görüntülemek veya değiştirmek için, dizin oluşturucuları veya veri kaynaklarını tek tek seçebilirsiniz.

  ![Dizin oluşturucu ve veri kaynağı kutucukları](./media/search-indexer-tutorial/tiles-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hizmetleri kullanmaya devam etmeyecekseniz, şu adımları izleyerek Azure Portal'da bu öğretici tarafından oluşturulan tüm kaynakları silin. 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Kaynak grubunu sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek ve desteklenen diğer veri kaynaklarına özgü görevleri görmek için, aşağıdaki makalelere bakın:

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Search Blob dizin oluşturucu kullanarak CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
* [Azure Search Blob dizin oluşturucu ile JSON bloblarını dizine ekleme](search-howto-index-json-blobs.md)

