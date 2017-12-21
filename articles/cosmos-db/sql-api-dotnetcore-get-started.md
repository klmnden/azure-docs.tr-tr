---
title: "Azure Cosmos DB: .NET Core eğitici Başlarken SQL API | Microsoft Docs"
description: "Çevrimiçi bir veritabanı ve Azure Cosmos DB SQL API .NET Core SDK'sını kullanarak C# konsol uygulaması oluşturan bir Öğreticisi."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.custom: devcenter
ms.openlocfilehash: 0b19071bf871029b488b26d3f125d08d7d2a2dd4
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="azure-cosmos-db-getting-started-with-the-sql-api-and-net-core"></a>Azure Cosmos DB: .NET Core ve SQL API'yi ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

SQL API'sine Azure Cosmos .NET Core eğitici Başlarken DB için Hoş Geldiniz! Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Bu öğreticide şunları içerir:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Visual Studio Çözümünüzü yapılandırma
* Çevrimiçi bir veritabanı oluşturma
* Koleksiyon oluşturma
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama
* Bir belgeyi değiştirme
* Bir belgeyi silme
* Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)'da bulabilirsiniz. Hızlı yönergeler için [Tam çözümü edinme bölümüne](#GetSolution) atlayın.

Xamarin iOS, Android veya formlar oluşturmak istediğiniz .NET Core SDK ve SQL API'yi kullanarak uygulama? Bkz: [yapı Xamarin ve Azure Cosmos DB mobil uygulamalar](mobile-apps-with-xamarin.md).

> [!NOTE]
> Azure Cosmos DB .NET Core Bu öğreticide kullanılan SDK henüz Evrensel Windows Platformu (UWP) uygulamaları ile uyumlu değil. .NET Core SDK'sının UWP uygulamalarını destekleyen bir önizleme sürümü için [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) adresine e-posta gönderin.

Şimdi başlayalım!

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)] 
    * MacOS ya da Linux üzerinde çalışıyorsanız, yükleyerek komut satırından .NET Core uygulamaları geliştirebilirsiniz [.NET Core SDK](https://www.microsoft.com/net/core#macos) tercih ettiğiniz bir platform için. 
    * Windows üzerinde çalışıyorsanız, yükleyerek komut satırından .NET Core uygulamaları geliştirebilirsiniz [.NET Core SDK](https://www.microsoft.com/net/core#windows). 
    * Kendi düzenleyicisini kullanın veya indirme [Visual Studio Code](https://code.visualstudio.com/), ücretsiz ve Windows, Linux ve MacOS üzerinde çalışır. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Visual Studio Çözümünüzü Kurma](#SetupVS)'ya atlayabilirsiniz. Azure Cosmos DB öykünücüsü kullanıyorsanız, verilen adımları izleyin [Azure Cosmos DB öykünücüsü](local-emulator.md) öykünücü kurulması ve için İleri atlayabilirsiniz [Visual Studio çözümünüzü kurma](#SetupVS).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>2. Adım: Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio 2017**'yi açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. **Yeni Proje** iletişim kutusunda **Şablonlar** / **Visual C#** / **.NET Core**/**Konsol Uygulaması (.NET Core)** seçeneğini belirleyin, projenizi **DocumentDBGettingStarted** olarak adlandırın ve **Tamam**’a tıklayın.

   ![Yeni Proje penceresinin ekran görüntüsü](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. **Çözüm Gezgini** içinde, **DocumentDBGettingStarted**’a sağ tıklayın.
5. Ardından, menüden çıkmadan **NuGet Paketlerini Yönet...** öğesine tıklayın.

   ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. **NuGet** sekmesinde, pencerenin üst kısmındaki **Gözat**'a tıklayın ve arama kutusuna **azure documentdb** yazın.
7. Sonuçlardan **Microsoft.Azure.DocumentDB.Core** seçeneğini bulup **Yükle**’ye tıklayın.
   Kitaplık için paket kimliği [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Bu .NET Core NuGet paketi tarafından desteklenmeyen bir .NET Framework sürümünü (net461 gibi) hedefliyorsanız, .NET Framework 4.5’ten itibaren tüm .NET Framework sürümlerini destekleyen [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)’yi kullanın.
8. Komut istemlerinde, NuGet paket yüklemelerini ve lisans sözleşmesini kabul edin.

Harika! Kurulum tamamlandığında, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanmış kod projesini [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)'da bulabilirsiniz.

## <a id="Connect"></a>3. Adım: Azure Cosmos DB hesabına bağlanma
İlk olarak, Program.cs dosyasında C# uygulamanızın başlangıcına bu başvuruları ekleyin:

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

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için, yukarıdaki bağımlılıkları eklediğinizden emin olun.

Şimdi bu iki sabiti ve *client* değişkeninizi *Program* ortak sınıfının altına ekleyin.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Ardından, head için [Azure portal](https://portal.azure.com) URI ve birincil anahtarınızı almak için. Azure Cosmos DB URI ve birincil anahtar, uygulamanızın nereye bağlanacağını anlamak ve Azure Cosmos DB uygulamanızın bağlantısına güvenmesi gereklidir.

Azure portalında Azure Cosmos DB hesabınıza gidin ve ardından **anahtarları**.

Portaldaki URI’yi kopyalayın ve program.cs dosyasındaki `<your endpoint URI>` içine yapıştırın. Ardından portaldan BİRİNCİL ANAHTARI kopyalayın ve `<your key>` içine yapıştırın. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız uç nokta olarak `https://localhost:8081` değerini ve [Azure Cosmos DB Öykünücüsü’nü kullanarak geliştirme](local-emulator.md) bölümünden elde edilen iyi tanımlanmış yetkilendirme anahtarını kullanın. < ve > işaretini kaldırdığınızdan, ancak uç noktası ve anahtarın başı ile sonundaki çift tırnağı bıraktığınızdan emin olun.

![C# konsol uygulaması oluşturmak için NoSQL Öğreticisi tarafından kullanılan Azure portal ekran görüntüsü. Vurgulanan etkin hub'ı, Azure Cosmos DB hesabını sayfasında ANAHTARLAR düğmesi ve anahtarları sayfasında vurgulanmış URI, birincil anahtar ve ikincil anahtar değerleri ile bir Azure Cosmos DB hesabını gösterir][keys]

**DocumentClient**'ın yeni bir örneğini oluşturarak başlarken uygulamasına başlayacağız.

**Main** yönteminin altına yeni **DocumentClient**'ımızın örneğini oluşturacak **GetStartedDemo** adlı bu zaman uyumsuz yeni görevi ekleyin.

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

Zaman uyumsuz görevinizi **Main** yönteminizden çalıştırmak için aşağıdaki kodu ekleyin. **Ana** yöntemi özel durumları yakalar ve bunları konsola yazar.

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

Uygulamayı derlemek ve çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

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

Azure Cosmos DB [veritabanı](sql-api-resources.md#databases) kullanılarak oluşturulan [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) yöntemi **DocumentClient** sınıfı. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

Aşağıdaki kodu kopyalayın ve istemci oluşturmanın altında **GetStartedDemo** yönteminize yapıştırın. Bu adlı bir veritabanı oluşturur *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.  

## <a id="CreateColl"></a>5. Adım: Koleksiyon oluşturma
> [!WARNING]
> **CreateDocumentCollectionAsync** fiyatlandırmaya olan ayrılmış işleme ile yeni bir koleksiyon oluşturur. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.

Bir [koleksiyon](sql-api-resources.md#collections), **DocumentClient** sınıfının [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) yöntemi kullanılarak oluşturulabilir. Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

Aşağıdaki kodu kopyalayın ve veritabanı oluşturmanın altında **GetStartedDemo** yönteminize yapıştırın. Bu adlı bir belge koleksiyonu oluşturur *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belge koleksiyonu oluşturdunuz.  

## <a id="CreateDoc"></a>6. Adım: JSON belgeleri oluşturma
Bir [belge](sql-api-resources.md#documents), **DocumentClient** sınıfının [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) yöntemi kullanılarak oluşturulabilir. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Şimdi bir veya daha fazla belge ekleyebiliriz. Veritabanınızda depolamak istediğiniz veriler zaten varsa, Azure Cosmos veritabanı kullanabilirsiniz [veri geçiş aracı](import-data.md).

İlk olarak, bu örnekte Azure Cosmos DB içinde depolanan nesneleri temsil edecek bir **Family** sınıfı oluşturmamız gerekir. **Family**'nin içinde kullanılan **Parent**, **Child**, **Pet**, **Address** alt sınıflarını da oluşturacağız. Belgelerin, JSON'da **id** olarak seri hale getirilmiş bir **Id** özelliğine sahip olmaları gerektiğini unutmayın. Bu sınıfları oluşturmak için **GetStartedDemo** yönteminden sonra aşağıdaki iç alt sınıfları ekleyin.

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
await this.CreateDatabaseIfNotExists("FamilyDB_oa");

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

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

Tebrikler! Başarılı bir şekilde iki Azure Cosmos DB belgesi oluşturdunuz.  

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan belgeler, hesap, çevrimiçi veritabanı ve koleksiyon arasındaki hiyerarşik ilişkiyi gösteren diyagram](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler.  Aşağıdaki örnek kod, önceki adımda yerleştirdiğimiz belgelerde hem Azure Cosmos DB SQL söz dizimi hem de LINQ kullanarak çalıştırabileceğimiz çeşitli sorguları gösterir.

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

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

Tebrikler! Bir Azure Cosmos DB koleksiyonunu başarıyla sorguladınız.

Aşağıdaki diyagram oluşturduğunuz koleksiyonda Azure Cosmos DB SQL sorgusu söz diziminin nasıl çağrıldığını gösterir, aynı mantık LINQ sorgusu için de geçerlidir.

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan sorgunun kapsamını ve anlamını gösteren diyagram](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

[FROM](sql-api-sql-query.md#FromClause) Azure Cosmos DB sorguları zaten tek bir koleksiyon kapsamında olduğundan anahtar sözcüğü sorguda isteğe bağlıdır. Bu nedenle, "FROM Families f", "FROM root r" veya seçtiğiniz herhangi bir başka değişken adıyla değiştirilebilir. Azure Cosmos DB aileleri, root veya seçtiğiniz, değişken adı başvurusu geçerli koleksiyonu varsayılan olarak oluşturur.

## <a id="ReplaceDocument"></a>8. Adım: JSON belgesini değiştirme
Azure Cosmos DB, JSON belgelerini değiştirmeyi destekler.  

**ReplaceFamilyDocument** yöntemini kopyalayın ve **ExecuteSimpleQuery** yönteminizin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    try
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
        this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
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

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini değiştirdiniz.

## <a id="DeleteDocument"></a>9. Adım: JSON belgesini silme
Azure Cosmos DB, JSON belgelerini silmeyi destekler.  

**DeleteFamilyDocument** yöntemini kopyalayın ve **ReplaceFamilyDocument** yönteminizin altına yapıştırın.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    try
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine("Deleted Family {0}", documentName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

Aşağıdaki kodu kopyalayın ve ikinci sorguyu yürütmenin altında **GetStartedDemo** yönteminize yapıştırın.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

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

Uygulamanızı çalıştırmak için **DocumentDBGettingStarted** düğmesine basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanını sildiniz.

## <a id="Run"></a>11. Adım: C# konsol uygulamanızı hep birlikte çalıştırın!
Uygulamayı hata ayıklama modunda derlemek için, Visual Studio’da **DocumentDBGettingStarted** düğmesine basın.

Konsol penceresinde get Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir ve aşağıdaki örnek metinle eşleşmelidir.

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

SQL API başvuruları Visual Studio'daki Azure Cosmos DB .NET Core SDK geri yüklemek için sağ **GetStarted** Çözüm Gezgini ve ardından çözüm **NuGet paketi geri yüklemeyi etkinleştir**. Bölümünde açıklandığı gibi Program.cs dosyasında EndpointUrl ve AuthorizationKey değerlerini sonraki, güncelleştirme [Azure Cosmos DB hesabına Bağlan](#Connect).

## <a name="next-steps"></a>Sonraki adımlar
* Daha karmaşık bir ASP.NET MVC öğreticisi mi istiyorsunuz? Bkz: [ASP.NET MVC Öğreticisi: Web uygulaması geliştirme Azure Cosmos DB ile](sql-api-dotnet-application.md).
* Xamarin iOS, Android veya formlar geliştirmek istediğiniz Azure Cosmos DB .NET Core SDK için SQL API'yi kullanarak uygulama? Bkz: [yapı Xamarin ve Azure Cosmos DB mobil uygulamalar](mobile-apps-with-xamarin.md).
* Azure Cosmos DB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz: [performansı ve ölçeği Azure Cosmos DB ile test etme](performance-testing.md)
* Bilgi edinmek için nasıl [İzleyici Azure Cosmos DB istekleri, kullanım ve depolama](monitor-accounts.md).
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* Programlama modeli hakkında daha fazla bilgi için bkz: [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
