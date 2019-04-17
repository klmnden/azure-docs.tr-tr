---
title: Azure Cosmos DB SQL API hesabı verileri yönetmek için bir .NET konsol uygulaması oluşturma
description: Çevrimiçi bir veritabanı oluşturan öğretici ve C# SQL API'sini kullanarak bir konsol uygulaması.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: sngun
ms.openlocfilehash: a8d144b2cb8ee18c69dc4c4768b09422d44bade2
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617330"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Azure Cosmos DB SQL API hesabı verileri yönetmek için bir .NET konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (Önizleme)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET core (Önizleme)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Azure Cosmos DB SQL API almak için Hoş Geldiniz Öğreticisi. Bu öğreticiyi tamamladıktan sonra oluşturan bir konsol uygulaması ve Azure Cosmos DB kaynaklarını sorguları da sahip olacaksınız.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
>
> - Azure Cosmos DB hesabı oluşturun ve ona bağlanma
> - Visual Studio çözümünüzü yapılandırma
> - Veritabanı oluşturma
> - Koleksiyon oluşturma
> - JSON belgeleri oluşturma
> - Sorgu koleksiyonu
> - Bir JSON belgesi güncelleştir
> - Bir belgeyi silme
> - Veritabanını silme

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2017 ile yüklü Azure geliştirme iş akışı:
- İndirip kullanabilirsiniz **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun. 

Bir Azure aboneliği veya ücretsiz Cosmos DB deneme hesabı:
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
  
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  
  
- Azure Cosmos DB öykünücüsü'nü kullanıyorsanız, bu adımları izleyin [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) öykünücünün kurulumunu için. Ardından başlangıç Öğreticisi, [Visual Studio çözümünü ayarlamak](#SetupVS).
  
## <a name="get-the-completed-solution"></a>Tamamlanan çözümü alın

Bu öğreticiyi tamamlamak veya yalnızca kod örneklerini istediğiniz zaman yoksa, tam çözüm indirebilirsiniz [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

İndirilen çözümü çalıştırmak için: 

1. Olduğundan emin olun [önkoşulları](#prerequisites) yüklü. 
1. İndirilen açın *GetStarted.sln* Visual Studio'daki çözüm dosyası.
1. İçinde **Çözüm Gezgini**, sağ **GetStarted** proje ve seçin **NuGet paketlerini Yönet**.
1. Üzerinde **NuGet** sekmesinde **geri** Azure Cosmos DB .NET SDK başvuruları geri yüklemek için.
1. İçinde *App.config* dosyası, güncelleştirme `EndpointUrl` ve `PrimaryKey` değerleri açıklandığı [Azure Cosmos DB hesabına bağlanma](#Connect) bölümü.
1. Seçin **hata ayıklama** > **hata ayıklama olmadan Başlat** veya basın **Ctrl**+**F5** oluşturun ve uygulamayı çalıştırın.

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Azure portalında bir Azure Cosmos DB hesabı oluşturmak için bu yönergeleri izleyin. Kullanılacak bir Azure Cosmos DB hesabı zaten varsa atlayın [Visual Studio çözümünü ayarlamak](#SetupVS). 

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Visual Studio çözümünü ayarlamak

1. Visual Studio 2017'de seçin **dosya** > **yeni** > **proje**.
   
1. İçinde **yeni proje** iletişim kutusunda **Visual C#**   >  **konsol uygulaması (.NET Framework)**, projenizi adlandırın *AzureCosmosDBApp* ve ardından **Tamam**.
   
   ![Yeni Proje penceresinin ekran görüntüsü](./media/sql-api-get-started/nosql-tutorial-new-project-2.png)
   
1. İçinde **Çözüm Gezgini**, sağ **AzureCosmosDBApp** seçin ve proje **NuGet paketlerini Yönet**.
   
   ![Proje bağlam menüsü](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges.png)
   
1. Üzerinde **NuGet** sekmesinde **Gözat**girin *azure documentdb* arama kutusuna.
   
1. Bulmak ve seçmek **Microsoft.Azure.DocumentDB**seçip **yükleme** zaten yüklü değilse.
   
   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)’dır.
   
   ![Azure Cosmos DB istemci SDK'sını bulmak için NuGet menüsünün ekran görüntüsü](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)
   
   Çözümdeki değişiklikleri önizleme hakkında bir ileti alırsanız seçin **Tamam**. Lisans kabulü hakkında bir ileti alırsanız seçin **kabul ediyorum**.

## <a id="Connect"></a>Azure Cosmos DB hesabına bağlanma

Şimdi, biraz kod yazmaya başlayın. Tam *Project.cs* de Bu öğretici için dosya [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

1. İçinde **Çözüm Gezgini**seçin *Program.cs*ve Kod Düzenleyicisi'nde dosyasının başına aşağıdaki başvuruları ekleyin:
   
   ```csharp
   using System.Net;
   using Microsoft.Azure.Documents;
   using Microsoft.Azure.Documents.Client;
   using Newtonsoft.Json;
   ```
   
1. Ardından, aşağıdaki iki sabitleri ekleyin ve `client` değişkenini `public class Program`.
   
   ```csharp
   
   public class Program
   {
      private const string EndpointUrl = "<your endpoint URL>";
      private const string PrimaryKey = "<your primary key>";
      private DocumentClient client;
   ```
   
1. Uç nokta URL'nizi ve birincil anahtar, Azure Cosmos DB hesabınız ve Azure Cosmos DB hesap bağlantısına güvenmesi için bağlanmak için uygulamanızı sağlar. Anahtarlarını kopyalama [Azure portalında](https://portal.azure.com)ve kodunuzun yapıştırın. 

   
   1. Azure Cosmos DB hesabı sol gezinti bölmesinde **anahtarları**.
      
      ![Azure portalında erişim anahtarlarını görüntüleme ve kopyalama](./media/sql-api-get-started/nosql-tutorial-keys.png)
      
   1. Altında **okuma-yazma anahtarları**, kopyalama **URI** sağ taraftaki Kopyala düğmesini kullanarak değer ve yapıştırın `<your endpoint URL>` içinde *Program.cs*. Örneğin: 
      
      `private const string EndpointUrl = "https://mysqlapicosmosdb.documents.azure.com:443/";`
      
   1. Kopyalama **birincil anahtar** yapıştırın ve değer `<your primary key>` içinde *Program.cs*. Örneğin: 
      
      `private const string PrimaryKey = "19ZDNJAiYL26tmnRvoez6hmtIfBGwjun50PWRjNYMC2ig8Ob9hYk7Fq1RYSv8FcIYnh1TdBISvCh7s6yyb0000==";`
   
1. Sonra `Main` yöntemi olarak adlandırılan yeni bir zaman uyumsuz görev eklemek `GetStartedDemo`, hangi örnekleyen bir yeni `DocumentClient` adlı `client`.
   
   ```csharp
      private async Task GetStartedDemo()
      {
        client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
      }
   ```
   
1. Aşağıdaki kodu ekleyin `Main` çalıştırılacak yöntemi `GetStartedDemo` görev. `Main` Yöntemi özel durumlarını yakalayan ve bunları konsola yazar.
   
   ```csharp
      static void Main(string[] args)
      {
        try
        {
           Program p = new Program();
           p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
           Exception baseException = de.GetBaseException();
           Console.WriteLine($"{de.StatusCode} error occurred: {de.Message}, Message: {baseException.Message}");
        }
        catch (Exception e)
        {
           Exception baseException = e.GetBaseException();
           Console.WriteLine($"Error: {e.Message}, Message: {baseException.Message}");
        }
        finally
        {
           Console.WriteLine("End of demo, press any key to exit.");
           Console.ReadKey();
        }
      }
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için. 
   
1. İleti gördüğünüzde **son Tanıtımı, çıkmak için herhangi bir tuşa basın** konsol penceresinde, bu sunucuya bağlantı başarılı oldu anlamına gelir. Konsol penceresini kapatmak için herhangi bir tuşa basın. 

Azure Cosmos DB hesabınıza başarıyla bağlandınız. Şimdi, bazı Azure Cosmos DB kaynaklarıyla çalışır.  

## <a name="create-a-database"></a>Veritabanı oluşturma

Bir Azure Cosmos DB [veritabanı](databases-containers-items.md#azure-cosmos-databases) koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal kapsayıcısıdır. Kullanarak bir veritabanı oluşturma [Createdatabaseasync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync) yöntemi `DocumentClient` sınıfı. 

1. Bir veritabanı oluşturmak için kodu eklemeden önce, konsola yazma için bir yardımcı yöntemi ekleyin. Kopyala ve Yapıştır `WriteToConsoleAndPromptToContinue` sonrasına `GetStartedDemo` kodunuzda yöntem.
   
   ```csharp
   private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
   {
      Console.WriteLine(format, args);
      Console.WriteLine("Press any key to continue...");
      Console.ReadKey();
   }
   ```
   
1. Aşağıdaki satırı kopyalayıp, `GetStartedDemo` yöntemi, sonra `client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);` satır. Bu kod adlı bir veritabanı oluşturur ve `FamilyDB`.
   
   ```csharp
      await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

Azure Cosmos DB veritabanı başarıyla oluşturdunuz. Veritabanında gördüğünüz [Azure portalında](https://portal.azure.com) seçerek **Veri Gezgini** Azure Cosmos DB hesabı sol gezinti. 

## <a id="CreateColl"></a>Koleksiyon oluşturma

Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. Kullanarak bir koleksiyon oluşturduğunuzda [Createdocumentcollectionıfnotexistsasync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync#overloads) yöntemi `DocumentClient` sınıfı. 

> [!IMPORTANT]
> **Createdocumentcollectionıfnotexistsasync** ödenmesini ayrılmış işleme ile yeni bir koleksiyon oluşturur. Daha fazla bilgi için ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 

1. Aşağıdaki kodu kopyalayıp, `GetStartedDemo` sonrasına `await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });` satır. Bu kod adlı bir belge koleksiyonu oluşturur `FamilyCollection`.
   
   ```csharp
      await client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

Bir Azure Cosmos DB belge koleksiyonunu başarıyla oluşturdunuz. Koleksiyon altında görebilirsiniz, **FamilyDB** veritabanını **Veri Gezgini** Azure portalında.  

## <a id="CreateDoc"></a>JSON belgeleri oluşturma

İsteğe bağlı, kullanıcı tanımlı JSON içeriği belgelerdir. Belgeler olarak seri hale getirilmiş bir ID özelliği olmalıdır `id` JSON. Kullanarak belge oluşturma [CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync#overloads) yöntemi `DocumentClient` sınıfı. 

> [!TIP]
> Veritabanınızda depolamak istediğiniz veriler zaten varsa, Azure Cosmos DB kullanabileceğiniz [veri geçiş aracı](import-data.md) içe aktarmak için.
>

Aşağıdaki kod oluşturur ve iki belge veritabanı koleksiyonunuza ekler. İlk olarak, oluşturduğunuz bir `Family` sınıfı ve `Parent`, `Child`, `Pet`, ve `Address` içinde kullanmak için alt sınıfların `Family`. Ardından, oluşturduğunuz bir `CreateFamilyDocumentIfNotExists` yöntemi, oluşturma ve iki belge yerleştirin. 

1. Kopyala ve Yapıştır `Family`, `Parent`, `Child`, `Pet`, ve `Address` sonra sınıfları `WriteToConsoleAndPromptToContinue` kodunuzda yöntem.
   
   ```csharp
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }
   ```
   
1. Kopyala ve Yapıştır `CreateFamilyDocumentIfNotExists` sonrasına `Address` eklediğiniz sınıfı.
   
   ```csharp
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            WriteToConsoleAndPromptToContinue($"Found {family.Id}");
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                WriteToConsoleAndPromptToContinue($"Created Family {family.Id}");
            }
            else
            {
                throw;
            }
        }
    }
   ```
   
1. Sonuna aşağıdaki kodu yapıştırın, `GetStartedDemo` yöntemi, sonra `await client.CreateDocumentCollectionIfNotExistsAsync` satır. Bu kod oluşturur ve iki belge Andersen ve Wakefield ailesi için birer ekler.
   
   ```csharp
    Family andersenFamily = new Family
    {
        Id = "AndersenFamily",
        LastName = "Andersen",
        Parents = new Parent[]
        {
            new Parent { FirstName = "Thomas" },
            new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
            new Child
            {
                FirstName = "Henriette Thaulow",
                Gender = "female",
                Grade = 5,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Fluffy" }
                }
            }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
    };

    await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
        Id = "WakefieldFamily",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
            new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
            new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
            new Child
            {
                FamilyName = "Merriam",
                FirstName = "Jesse",
                Gender = "female",
                Grade = 8,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Goofy" },
                    new Pet { GivenName = "Shadow" }
                }
            },
            new Child
            {
                FamilyName = "Miller",
                FirstName = "Lisa",
                Gender = "female",
                Grade = 1
            }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
    };

    await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

İki Azure Cosmos DB Belge başarıyla oluşturdunuz. Belgeleri görebilirsiniz, **FamilyDB** veritabanı ve **FamilyCollection** koleksiyonda **Veri Gezgini** Azure portalında.   

![Hesap, çevrimiçi veritabanı, koleksiyon ve belgeler arasındaki hiyerarşik ilişkiyi gösteren diyagram](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB destekleyen zengin [sorguları](how-to-sql-query.md) koleksiyonlarında depolanan JSON belgelerde. Aşağıdaki örnek kod, örnek belgelerde bir sorgu çalıştırmak için LINQ ve Azure Cosmos DB SQL söz dizimini kullanır.

1. Kopyala ve Yapıştır `ExecuteSimpleQuery` sonrasına `CreateFamilyDocumentIfNotExists` kodunuzda yöntem.
   
   ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options.
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Find the Andersen family by its LastName.
        IQueryable<Family> familyQuery = client.CreateDocumentQuery<Family>(
            UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
            .Where(f => f.LastName == "Andersen");

        // Execute the query synchronously. 
        // You could also execute it asynchronously using the IDocumentQuery<T> interface.
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine($"\tRead {family}");
        }

        // Now execute the same query using direct SQL.
        IQueryable<Family> familyQueryInSql = client.CreateDocumentQuery<Family>(
            UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
            "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
            queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
            Console.WriteLine($"\tRead {family}");
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
   ```
   
1. Sonuna aşağıdaki kodu yapıştırın, `GetStartedDemo` yöntemi, sonra `await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);` satır.
   
   ```csharp
      ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

Önceki sorgunun Andersen ailesi için tüm öğeyi döndürür. Ayrıca, bir Azure Cosmos DB koleksiyonunu başarıyla sorguladı.

Aşağıdaki diyagram, Azure Cosmos DB SQL sorgusu söz dizimi koleksiyonunda çağırması gösterir. Aynı mantık LINQ sorgusu için geçerlidir.

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan sorgunun kapsamını ve anlamını gösteren diyagram](./media/sql-api-get-started/nosql-tutorial-collection-documents.png)

[FROM](how-to-sql-query.md#FromClause) Azure Cosmos DB sorguları zaten tek bir koleksiyon kapsamında olduğundan SQL sorgusunda anahtar sözcüğü isteğe bağlıdır. Takas edebilirsiniz `FROM Families f` ile `FROM root r`, veya seçtiğiniz diğer herhangi bir değişken adı. Azure Cosmos DB, Infer `Families`, `root`, veya seçtiğiniz değişken adı geçerli koleksiyonla ifade eder.

## <a id="ReplaceDocument"></a>Bir JSON belgesi güncelleştir

Azure Cosmos DB SQL API güncelleştiriliyor ve JSON belgelerini değiştirmeyi destekler.  

1. Kopyala ve Yapıştır `ReplaceFamilyDocument` sonrasına `ExecuteSimpleQuery` kodunuzda yöntem.
   
   ```csharp
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
       await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
       WriteToConsoleAndPromptToContinue($"Replaced Family {familyName}");
    }
   ```
   
1. Sonuna aşağıdaki kodu yapıştırın, `GetStartedDemo` yöntemi, sonra `ExecuteSimpleQuery("FamilyDB", "FamilyCollection");` satır. Kod belgeleri birindeki verileri güncelleştirir ve sonra yeniden değiştirilen belge göstermek için bir sorgu çalıştırır.
   
   ```csharp
   // Update the Grade of the Andersen Family child
   andersenFamily.Children[0].Grade = 6;
   await ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily", andersenFamily);
   ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

Sorgu çıkışının gösteren `Grade` güncelleştirildi Andersen ailesi çocuk `5` için `6`. Başarıyla güncelleştirildi ve bir Azure Cosmos DB belgesini yerine. 

## <a id="DeleteDocument"></a>JSON belgesini silme

Azure Cosmos DB SQL API'si, JSON belgelerini silmeyi destekler.  

1. Kopyala ve Yapıştır `DeleteFamilyDocument` sonrasına `ReplaceFamilyDocument` yöntemi.
   
   ```csharp
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
        await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine($"Deleted Family {documentName}");
    }
   ```
   
1. Sonuna aşağıdaki kodu yapıştırın, `GetStartedDemo` yöntemi, ikinci sonra `ExecuteSimpleQuery("FamilyDB", "FamilyCollection");` satır.
   
   ```csharp
   await DeleteFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily");
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

Bir Azure Cosmos DB belgesini başarıyla sildiğiniz. 

## <a id="DeleteDatabase"></a>Veritabanını silme

Veritabanını ve koleksiyonu ve belgeleri de dahil olmak üzere tüm alt kaynaklarını, kaldırmak için oluşturduğunuz silin. 

1. Sonuna aşağıdaki kodu yapıştırın, `GetStartedDemo` yöntemi, sonra `await DeleteFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily");` satır. 
   
   ```csharp
   // Clean up - delete the database
   await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));
   ```
   
1. Tuşuna **F5** uygulamanızı çalıştırmak için.

Azure Cosmos DB veritabanına başarıyla sildiğiniz. Görebilirsiniz **Veri Gezgini** Azure Cosmos DB hesabınızın FamilyDB veritabanı silinir. 

## <a id="Run"></a>Tüm çalıştırma C# konsol uygulaması

Tuşuna **F5** oluşturmak ve tam olarak çalıştırmak için Visual Studio'da C# hata ayıklama modunda konsol uygulaması. Konsol penceresinde aşağıdaki çıktıyı görmeniz gerekir:

```bash
Created Family AndersenFamily
Press any key to continue ...
 Created Family WakefieldFamily
Press any key to continue ...
 Running LINQ query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Press any key to continue ...
 Replaced Family AndersenFamily
Press any key to continue ...
 Running LINQ query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Press any key to continue ...
 Deleted Family AndersenFamily
End of demo, press any key to exit.
```

Tebrikler! Öğreticisini tamamladınız ve çalışan bir sahip C# oluşturan, sorgular, konsol uygulaması güncelleştirir ve Azure Cosmos DB kaynaklarını siler.  

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](introduction.md).
* Daha karmaşık bir ASP.NET MVC öğreticisi için bkz. [ASP.NET MVC Öğreticisi: Azure Cosmos DB ile uygulama geliştirme Web](sql-api-dotnet-application.md).
* Ölçek ve performans ile Azure Cosmos DB testi gerçekleştirmek için bkz: [performans ve ölçek testi Azure Cosmos DB ile](performance-testing.md).
* Azure Cosmos DB istekleri, kullanım ve depolama izleme öğrenmek için bkz. [izleme hesapları](monitor-accounts.md).
* [Query Playground](https://www.documentdb.com/sql/demo)’daki bir örnek veri kümesinde sorgular çalıştırın.

