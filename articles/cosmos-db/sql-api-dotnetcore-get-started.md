---
title: 'Azure Cosmos DB: .NET Core ile SQL API’si başlangıç öğreticisi | Microsoft Docs'
description: Azure Cosmos DB SQL API’si .NET Core SDK'sını kullanarak çevrimiçi bir veritabanı ve C# konsol uygulaması oluşturan öğretici.
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 03/12/2018
ms.author: sngun
ms.custom: devcenter
ms.openlocfilehash: feb44920c36f2d17dafb1d33ef1c1e680fd37db4
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163563"
---
# <a name="tutorial-build-a-net-core-app-to-manage-azure-cosmos-db-sql-api-data"></a>Öğretici: Azure Cosmos DB SQL API'si verilerini yönetmek için bir .Net Core uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Bu öğreticide Azure Cosmos DB SQL API'si verilerini oluşturmak ve sorgulamak için bir .Net Core uygulaması oluşturma adımları gösterilmektedir. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
> * Visual Studio Çözümünüzü yapılandırma
> * Çevrimiçi bir veritabanı oluşturma
> * Koleksiyon oluşturma
> * JSON belgeleri oluşturma
> * Öğelerde, kapsayıcıda ve veritabanında CRUD işlemleri gerçekleştirme

Uygulamayı oluşturmak için zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)'da bulabilirsiniz. Hızlı yönergeler için [Tam çözümü edinme bölümüne](#GetSolution) atlayın.

SQL API'sini ve .NET Core SDK’yı kullanarak bir Xamarin iOS, Android, veya Forms uygulaması mı derlemek istiyorsunuz? Bkz. [Xamarin ve Azure Cosmos DB ile mobil uygulamalar derleme](mobile-apps-with-xamarin.md).

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Henüz Visual Studio 2017’yi yüklemediyseniz, ücretsiz [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Evrensel Windows Platformu (UWP) uygulaması geliştiriyorsanız, **Visual Studio 2017 sürüm 15.4** veya üstünü kullanmalısınız. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

    * MacOS veya Linux’ta çalışıyorsanız tercih ettiğiniz platforma yönelik [.NET Core SDK](https://www.microsoft.com/net/core#macos)’yı yükleyerek komut satırından .NET Core uygulamaları geliştirebilirsiniz. 

    * Windows’da çalışıyorsanız [.NET Core SDK](https://www.microsoft.com/net/core#windows)’yı yükleyerek komut satırından .NET Core uygulamaları geliştirebilirsiniz. 

    * Kendi düzenleyicinizi kullanabilir ya da ücretsiz olan ve Windows, Linux ve MacOS’de çalışan [Visual Studio Code](https://code.visualstudio.com/)’u indirebilirsiniz. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Visual Studio Çözümünüzü Kurma](#SetupVS)'ya atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Visual Studio Çözümünüzü Ayarlama](#SetupVS) adımına atlayın.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>2. Adım: Visual Studio çözümünüzü kurma

1. Bilgisayarınızda **Visual Studio 2017**'yi açın.

2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.

3. **Yeni Proje** iletişim kutusunda **Şablonlar** / **Visual C#** / **.NET Core**/**Konsol Uygulaması (.NET Core)** seçeneğini belirleyin, projenizi **DocumentDBGettingStarted** olarak adlandırın ve **Tamam**’ı seçin.

   ![Yeni Proje penceresinin ekran görüntüsü](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)

4. **Çözüm Gezgini** içinde, **DocumentDBGettingStarted**’a sağ tıklayın.

5. Aynı menüde **NuGet Paketlerini Yönet...** öğesini seçin.

   ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)

6. **NuGet** sekmesinde, pencerenin üst kısmındaki **Gözat**'ı seçin ve arama kutusuna **azure documentdb** yazın.

7. Sonuçlardan **Microsoft.Azure.DocumentDB.Core** seçeneğini bulup **Yükle**’yi seçin.

   Kitaplığın paket kimliği: [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Bu .NET Core NuGet paketi tarafından desteklenmeyen bir .NET Framework sürümünü (net461 gibi) hedefliyorsanız, .NET Framework 4.5’ten itibaren tüm .NET Framework sürümlerini destekleyen [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)’yi kullanın.

8. İstendiğinde NuGet paket yüklemelerini ve lisans sözleşmesini kabul edin.

Harika! Artık kurulum tamamlandığına göre, biraz kod yazmaya başlayabiliriz. Bu öğreticinin tamamlanmış kod projesini [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)'da bulabilirsiniz.

## <a id="Connect"></a>3. Adım: Azure Cosmos DB hesabına bağlanma

Aşağıdaki kodu C# uygulamanızın Program.cs dosyasının başına ekleyerek gerekli bağımlılıkları içeri aktarın:

```csharp
using System;

// ADD THIS PART TO YOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

Şimdi bu iki sabiti ve *client* değişkeninizi *Program* ortak sınıfının altına ekleyin.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Ardından, URI ve birincil anahtarınızı almak için [Azure portala](https://portal.azure.com) gidin. Azure Cosmos DB URI'si ve birincil anahtarı, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir.

Azure portalda Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’ı seçin.

Portaldaki URI’yi kopyalayın ve program.cs dosyasındaki `<your endpoint URI>` içine yapıştırın. Ardından portaldan BİRİNCİL ANAHTARI kopyalayın ve `<your key>` içine yapıştırın. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız uç nokta olarak `https://localhost:8081` değerini ve [Azure Cosmos DB Öykünücüsü’nü kullanarak geliştirme](local-emulator.md) bölümünden elde edilen iyi tanımlanmış yetkilendirme anahtarını kullanın. < ve > işaretini kaldırdığınızdan, ancak uç noktası ve anahtarın başı ile sonundaki çift tırnağı bıraktığınızdan emin olun.

![Azure portaldan anahtarları alma][keys]

**DocumentClient**'ın yeni bir örneğini oluşturarak başlarken uygulamasına başlayacağız.

**Main** yönteminin altına yeni **DocumentClient** öğenizin örneğini oluşturacak **GetStartedDemo** adlı bu zaman uyumsuz yeni görevi ekleyin.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART TO YOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Zaman uyumsuz görevinizi **Main** yönteminizden çalıştırmak için aşağıdaki kodu ekleyin. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART TO YOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key to exit.");
                Console.ReadKey();
        }
```

Uygulamayı derlemek ve çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız, şimdi Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.  

## <a name="step-4-create-a-database"></a>4. Adım: Veritabanı oluşturma

Bir veritabanı oluşturmak için kodu eklemeden önce, konsola yazma için bir yardımcı yöntemi ekleyin.

**WriteToConsoleAndPromptToContinue** yöntemini kopyalayın ve **GetStartedDemo** yönteminin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Azure Cosmos DB [veritabanınız](databases-containers-items.md#azure-cosmos-databases), **DocumentClient** sınıfının [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) yöntemi kullanılarak oluşturulabilir. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

Aşağıdaki kodu kopyalayın ve istemci oluşturmanın altında **GetStartedDemo** yönteminize yapıştırın. Bu, *FamilyDB* adlı bir veritabanı oluşturur.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.  

## <a id="CreateColl"></a>5. Adım: Koleksiyon oluşturma

> [!WARNING]
> **CreateDocumentCollectionAsync**, ayrılmış işleme ile yeni bir koleksiyon oluşturur, bu da ücret ödenmesini gerektirebilir. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.

Bir koleksiyon kullanarak oluşturulabilir [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) yöntemi **DocumentClient** sınıfı. Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

Aşağıdaki kodu kopyalayın ve veritabanı oluşturmanın altında **GetStartedDemo** yönteminize yapıştırın. Bu kod, *FamilyCollection_oa* adlı bir belge koleksiyonu oluşturur.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExistsAsync("FamilyDB_oa");

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belge koleksiyonu oluşturdunuz.  

## <a id="CreateDoc"></a>6. Adım: JSON belgeleri oluşturma

Bir belge kullanarak oluşturulabilir [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) yöntemi **DocumentClient** sınıfı. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Şimdi bir veya daha fazla belge ekleyebiliriz. Veritabanınızda depolamak istediğiniz veriler zaten varsa Azure Cosmos DB'nin [Veri Geçiş Aracı](import-data.md)'nı kullanabilirsiniz.

İlk olarak, Azure Cosmos DB'de depolanan nesneleri temsil eden bir **Family** sınıfı oluşturmanız gerekir. Ayrıca **Family**'nin içinde kullanılan **Parent**, **Child**, **Pet**, **Address** alt sınıflarını da oluşturacaksınız. Belgelerin, JSON'da **id** olarak seri hale getirilmiş bir **Id** özelliğine sahip olması gerekir. Bu sınıfları oluşturmak için **GetStartedDemo** yönteminden sonra aşağıdaki iç alt sınıfları ekleyin.

**Family**, **Parent**, **Child**, **Pet** ve **Address** sınıflarını kopyalayın ve **WriteToConsoleAndPromptToContinue** yönteminin altına yapıştırın.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key to continue ...");
    Console.ReadKey();
}

// ADD THIS PART TO YOUR CODE
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

**CreateFamilyDocumentIfNotExists** yöntemini kopyalayın ve **CreateDocumentCollectionIfNotExists** yönteminizin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

Andersen Ailesi ve Wakefield Ailesi için birer tane olmak üzere iki belge yerleştirin.

`// ADD THIS PART TO YOUR CODE` öğesinden sonraki kodu kopyalayın ve belge koleksiyonu oluşturmanın altında **GetStartedDemo** yönteminize yapıştırın.

```csharp
await this.CreateDatabaseIfNotExistsAsync("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
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

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
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

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Başarılı bir şekilde iki Azure Cosmos DB belgesi oluşturdunuz.  

![Hesap, çevrimiçi veritabanı ve koleksiyon arasındaki hiyerarşik ilişki](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](how-to-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, önceki adımda yerleştirdiğimiz belgelerde hem Azure Cosmos DB SQL söz dizimi hem de LINQ kullanarak çalıştırabileceğimiz çeşitli sorguları gösterir.

**ExecuteSimpleQuery** yöntemini kopyalayın ve **CreateFamilyDocumentIfNotExists** yönteminizin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find the Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute the same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Aşağıdaki kodu kopyalayın ve ikinci belge oluşturmanın altında **GetStartedDemo** yönteminize yapıştırın.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART TO YOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Bir Azure Cosmos DB koleksiyonunu başarıyla sorguladınız.

Aşağıdaki diyagram oluşturduğunuz koleksiyonda Azure Cosmos DB SQL sorgusu söz diziminin nasıl çağrıldığını gösterir, aynı mantık LINQ sorgusu için de geçerlidir.

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan sorgunun kapsamını ve anlamını gösteren diyagram](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

Azure Cosmos DB sorguları zaten tek bir koleksiyon kapsamında olduğundan, sorgudaki [FROM](how-to-sql-query.md#FromClause) anahtar sözcüğü isteğe bağlıdır. Bu nedenle, "FROM Families f", "FROM root r" veya seçtiğiniz herhangi bir başka değişken adıyla değiştirilebilir. Azure Cosmos DB; Families, root veya seçtiğiniz değişken adının varsayılan olarak geçerli koleksiyona başvurduğu sonucuna varır.

## <a id="ReplaceDocument"></a>8. Adım: JSON belgesini değiştirme

Azure Cosmos DB, JSON belgelerini değiştirmeyi destekler.  

**ReplaceFamilyDocument** yöntemini kopyalayın ve **ExecuteSimpleQuery** yönteminizin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
    this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
}
```

Aşağıdaki kodu kopyalayın ve sorgu yürütmenin altında **GetStartedDemo** yönteminize yapıştırın. Belgeyi değiştirdikten sonra, aynı sorgu tekrar çalıştırılarak değiştirilen belge görüntülenir.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
// Update the Grade of the Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini değiştirdiniz.

## <a id="DeleteDocument"></a>9. Adım: JSON belgesini silme

Azure Cosmos DB, JSON belgelerini silmeyi destekler.  

**DeleteFamilyDocument** yöntemini kopyalayın ve **ReplaceFamilyDocument** yönteminizin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
    Console.WriteLine("Deleted Family {0}", documentName);
}
```

Aşağıdaki kodu kopyalayın ve ikinci sorguyu yürütmenin altında **GetStartedDemo** yönteminize yapıştırın.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini sildiniz.

## <a id="DeleteDatabase"></a>10. Adım: Veritabanını silme

Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (koleksiyonlar, belgeler vb.) kaldırılır.

Tüm veritabanını ve tüm alt kaynaklarını silmek için aşağıdaki kodu kopyalayın ve belge silmenin altında **GetStartedDemo** yönteminize yapıştırın.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesini seçin.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanını sildiniz.

## <a id="Run"></a>11. Adım: C# konsol uygulamanızı çalıştırın

Uygulamayı hata ayıklama modunda derlemek için, Visual Studio’da **DocumentDBGettingStarted** düğmesini seçin.

Konsol penceresinde başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir ve aşağıdaki örnek metinle eşleşmelidir.

```
Created FamilyDB_oa
Press any key to continue ...
Created FamilyCollection_oa
Press any key to continue ...
Created Family Andersen.1
Press any key to continue ...
Created Family Wakefield.7
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key to exit.
```

Tebrikler! Bu öğreticiyi tamamladınız ve çalışan bir C# konsol uygulamasına sahipsiniz!

## <a id="GetSolution"></a> Tam öğretici çözümünü edinin

Bu makaledeki tüm örnekleri içeren GetStarted çözümünü derlemek için aşağıdakilere ihtiyacınız vardır:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* Bir [Azure Cosmos DB hesabı][create-sql-api-dotnet.md#create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) çözümü.

Başvuruları Visual Studio'daki Azure Cosmos DB için SQL API'si .NET Core SDK'ya geri yüklemek için, Çözüm Gezgini'nde **GetStarted** çözümüne sağ tıklayın ve ardından **NuGet Paketi Geri Yüklemeyi Etkinleştir**'i seçin. Ardından, Program.cs dosyasında EndpointUrl ve AuthorizationKey değerlerini [Azure Cosmos DB hesabına bağlanma](#Connect) bölümünde açıklandığı gibi güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Cosmos DB SQL API verilerini yönetmek için bir .Net Core uygulaması oluşturmayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Azure Cosmos DB SQL API'si hesabıyla Java konsol uygulaması oluşturma](sql-api-java-get-started.md)

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
