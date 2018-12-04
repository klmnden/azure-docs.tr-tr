---
title: "Azure Cosmos DB: SQL API'si başlangıç öğreticisi | Microsoft Docs"
description: SQL API'sini kullanarak çevrimiçi bir veritabanı ve C# konsol uygulaması oluşturan öğretici.
keywords: nosql öğreticisi, çevrimiçi veritabanı, c# konsol uygulaması
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 08/16/2017
ms.author: sngun
ms.openlocfilehash: 2e7c36d095121dbdeb6f5f0d5c440df3038d2344
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834545"
---
# <a name="azure-cosmos-db-sql-api-getting-started-tutorial"></a>Azure Cosmos DB: SQL API’sini kullanmaya başlama öğreticisi

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (Önizleme)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET core (Önizleme)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Azure Cosmos DB SQL API'si başlangıç öğreticisine hoş geldiniz! Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Bu öğreticinin içindekiler:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Visual Studio Çözümünüzü yapılandırma
* Çevrimiçi bir veritabanı oluşturma
* Koleksiyon oluşturma
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama
* Bir belgeyi değiştirme
* Bir belgeyi silme
* Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started)'da bulabilirsiniz. Hızlı yönergeler için [NoSQL öğreticisi tam çözümünü edinme](#GetSolution) bölümüne atlayın.

Şimdi başlayalım!

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Visual Studio Çözümünüzü Kurma](#SetupVS)'ya atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Visual Studio Çözümünüzü Ayarlama](#SetupVS) adımına atlayın.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>2. Adım: Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio 2017**'yi açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. **Yeni Proje** iletişim kutusunda, **Şablonlar** / **Visual C#** / **Konsol Uygulaması**'nı seçin, projenizi adlandırın ve ardından **Tamam**'a tıklayın.
   ![Yeni Proje penceresinin ekran görüntüsü](./media/sql-api-get-started/nosql-tutorial-new-project-2.png)
4. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.
    
    ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. **NuGet** sekmesinde **Gözat**'a tıklayın ve arama kutusuna **azure documentdb** yazın.
6. Sonuçlarda **Microsoft.Azure.DocumentDB**'yi bulun ve **Yükle**'ye tıklayın.
   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)’dır.
   ![Azure Cosmos DB İstemci SDK'sını bulmak için NuGet Menüsünün ekran görüntüsü](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

Harika! Kurulumu tamamladığımıza göre, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanmış kod projesini [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs)'da bulabilirsiniz.

## <a id="Connect"></a>3. Adım: Azure Cosmos DB hesabına bağlanma
İlk olarak, Program.cs dosyasında C# uygulamanızın başlangıcına bu başvuruları ekleyin:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için, yukarıdaki bağımlılıkları eklediğinizden emin olun.
> 
> 

Şimdi bu iki sabiti ve *client* değişkeninizi *Program* ortak sınıfının altına ekleyin.

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

Ardından, uç nokta URL’nizi ve birincil anahtarınızı almak için tekrar [Azure portalına](https://portal.azure.com) gidin. Uç nokta URL’si ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir.

Azure portalında Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’a tıklayın.

Portaldaki URI’yi kopyalayın ve program.cs dosyasındaki `<your endpoint URL>` içine yapıştırın. Ardından portaldan BİRİNCİL ANAHTARI kopyalayın ve `<your primary key>` içine yapıştırın.

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan Azure portalının ekran görüntüsü. Azure Cosmos DB hesabı sayfasında ANAHTARLAR düğmesi vurgulanmış, ETKİN hub'ı vurgulanmış ve Anahtarlar sayfasında URI, BİRİNCİL ANAHTAR ve İKİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösterir][keys]

Ardından **DocumentClient**'ın yeni bir örneğini oluşturarak uygulamayı başlatacağız.

**Main** yönteminin altına yeni **DocumentClient**'ımızın örneğini oluşturacak **GetStartedDemo** adlı bu zaman uyumsuz yeni görevi ekleyin.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

Zaman uyumsuz görevinizi **Main** yönteminizden çalıştırmak için aşağıdaki kodu ekleyin. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

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

Uygulamanızı çalıştırmak için **F5**'e basın. Konsol penceresi çıktısı, bağlantının kurulduğunu onaylayan `End of demo, press any key to exit.` iletisini görüntüler.  Ardından konsol penceresini kapatabilirsiniz. 

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız, şimdi Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.  

## <a name="step-4-create-a-database"></a>4. Adım: Veritabanı oluşturma
Bir veritabanı oluşturmak için kodu eklemeden önce, konsola yazma için bir yardımcı yöntemi ekleyin.

**WriteToConsoleAndPromptToContinue** yöntemini kopyalayın ve **GetStartedDemo** yönteminin sonrasında yapıştırın.

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

Azure Cosmos DB [veritabanınız](databases-containers-items.md#azure-cosmos-databases), **DocumentClient** sınıfının [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) yöntemi kullanılarak oluşturulabilir. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

Aşağıdaki kodu kopyalayın ve istemci oluşturmanın sonrasında **GetStartedDemo** yönteminize yapıştırın. Bu, *FamilyDB* adlı bir veritabanı oluşturur.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.  

## <a id="CreateColl"></a>5. Adım: Koleksiyon oluşturma
> [!WARNING]
> **CreateDocumentCollectionIfNotExistsAsync**, ayrılmış işleme ile yeni bir koleksiyon oluşturur, bu da ücret ödenmesini gerektirebilir. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
> 
> 

Bir koleksiyon kullanarak oluşturulabilir [Createdocumentcollectionıfnotexistsasync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) yöntemi **DocumentClient** sınıfı. Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

Aşağıdaki kodu kopyalayın ve veritabanı oluşturmanın sonrasında **GetStartedDemo** yönteminize yapıştırın. Bu, *FamilyCollection* adlı bir belge koleksiyonu oluşturur.

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART TO YOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belge koleksiyonu oluşturdunuz.  

## <a id="CreateDoc"></a>6. Adım: JSON belgeleri oluşturma
Bir belge kullanarak oluşturulabilir [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) yöntemi **DocumentClient** sınıfı. Belgeler, kullanıcı tanımlı (rastgele) JSON içeriğidir. Şimdi bir veya daha fazla belge ekleyebiliriz. Veritabanınızda depolamak istediğiniz veriler zaten varsa, verileri veritabanına aktarmak için Azure Cosmos DB [Veri Geçiş Aracı](import-data.md)'nı kullanabilirsiniz.

İlk olarak, bu örnekte Azure Cosmos DB içinde depolanan nesneleri temsil edecek bir **Family** sınıfı oluşturmamız gerekir. **Family**'nin içinde kullanılan **Parent**, **Child**, **Pet**, **Address** alt sınıflarını da oluşturacağız. Belgelerin, JSON'da **id** olarak seri hale getirilmiş bir **Id** özelliğine sahip olmaları gerektiğini unutmayın. Bu sınıfları oluşturmak için **GetStartedDemo** yönteminden sonra aşağıdaki iç alt sınıfları ekleyin.

**Family**, **Parent**, **Child**, **Pet** ve **Address** sınıflarını kopyalayın ve **WriteToConsoleAndPromptToContinue** yönteminin sonrasına yapıştırın.

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

**CreateFamilyDocumentIfNotExists** yöntemini kopyalayın ve **Address** sınıfınızın altına yapıştırın.

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

Andersen Ailesi ve Wakefield Ailesi için birer tane olmak üzere iki belge yerleştirin.

Aşağıdaki kodu kopyalayın ve belge koleksiyonu oluşturmanın sonrasında **GetStartedDemo** yönteminize yapıştırın.

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Başarılı bir şekilde iki Azure Cosmos DB belgesi oluşturdunuz.  

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan belgeler, hesap, çevrimiçi veritabanı ve koleksiyon arasındaki hiyerarşik ilişkiyi gösteren diyagram](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](how-to-sql-query.md) gerçekleştirilmesini destekler.  Aşağıdaki örnek kod, önceki adımda yerleştirdiğimiz belgelerde hem Azure Cosmos DB SQL söz dizimi hem de LINQ kullanarak çalıştırabileceğimiz çeşitli sorguları gösterir.

**ExecuteSimpleQuery** yöntemini kopyalayın ve **CreateFamilyDocumentIfNotExists** yönteminizin sonrasına yapıştırın.

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

Aşağıdaki kodu kopyalayın ve ikinci belge oluşturmanın sonrasında **GetStartedDemo** yönteminize yapıştırın.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Bir Azure Cosmos DB koleksiyonunu başarıyla sorguladınız.

Aşağıdaki diyagram oluşturduğunuz koleksiyonda Azure Cosmos DB SQL sorgusu söz diziminin nasıl çağrıldığını gösterir, aynı mantık LINQ sorgusu için de geçerlidir.

![Bir C# konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan sorgunun kapsamını ve anlamını gösteren diyagram](./media/sql-api-get-started/nosql-tutorial-collection-documents.png)

Azure Cosmos DB sorguları zaten tek bir koleksiyon kapsamında olduğundan, sorgudaki [FROM](how-to-sql-query.md#FromClause) anahtar sözcüğü isteğe bağlıdır. Bu nedenle, "FROM Families f", "FROM root r" veya seçtiğiniz herhangi bir başka değişken adıyla değiştirilebilir. Azure Cosmos DB; Families, root veya seçtiğiniz değişken adının varsayılan olarak geçerli koleksiyona başvurduğu sonucuna varır.

## <a id="ReplaceDocument"></a>8. Adım: JSON belgesini değiştirme
Azure Cosmos DB, JSON belgelerini değiştirmeyi destekler.  

**ReplaceFamilyDocument** yöntemini kopyalayın ve **ExecuteSimpleQuery** yönteminizin sonrasına yapıştırın.

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

Aşağıdaki kodu kopyalayın ve sorgu yürütmenin sonrasına, **GetStartedDemo** yönteminizin sonuna yapıştırın. Belgeyi değiştirdikten sonra, aynı sorgu tekrar çalıştırılarak değiştirilen belge görüntülenir.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini değiştirdiniz.

## <a id="DeleteDocument"></a>9. Adım: JSON belgesini silme
Azure Cosmos DB, JSON belgelerini silmeyi destekler.  

**DeleteFamilyDocument** yöntemini kopyalayın ve **ReplaceFamilyDocument** yönteminizin sonrasına yapıştırın.

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

Aşağıdaki kodu kopyalayın ve ikinci sorgu yürütmenin sonrasına, **GetStartedDemo** yönteminizin sonuna yapıştırın.

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini sildiniz.

## <a id="DeleteDatabase"></a>10. Adım: Veritabanını silme
Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (koleksiyonlar, belgeler vb.) kaldırılır.

Tüm veritabanını ve tüm alt kaynaklarını silmek için aşağıdaki kodu kopyalayın ve belge silmenin sonrasında **GetStartedDemo** yönteminize yapıştırın.

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

Uygulamanızı çalıştırmak için **F5**'e basın.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanını sildiniz.

## <a id="Run"></a>11. Adım: C# konsol uygulamanızı hep birlikte çalıştırın!
Uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da F5'e basın.

Konsol penceresinde başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir ve aşağıdaki örnek metinle eşleşmelidir.

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
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

Tebrikler! Bu öğreticiyi tamamladınız ve çalışan bir C# konsol uygulamasına sahipsiniz!

## <a id="GetSolution"></a> Tam öğretici çözümünü edinin
Bu öğreticideki adımları tamamlama fırsatınız olmadıysa veya yalnızca kod örneklerini indirmek isterseniz [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started)'dan ulaşabilirsiniz. 

GetStarted çözümünü oluşturmak için aşağıdakilere ihtiyacınız olacak:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* Bir [Azure Cosmos DB hesabı][cosmos-db-create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) çözümü.

Başvuruları Visual Studio'daki Azure Cosmos DB .NET SDK'sına geri yüklemek için, Çözüm Gezgini'nde **GetStarted** çözümüne sağ tıklayın ve ardından **NuGet Paketi Geri Yüklemeyi Etkinleştir**'e tıklayın. Ardından, App.config dosyasında EndpointUrl ve AuthorizationKey değerlerini [Azure Cosmos DB hesabına bağlanma](#Connect) bölümünde açıklandığı gibi güncelleştirin.

Hepsi bu kadar, derleyin ve devam edin!


## <a name="next-steps"></a>Sonraki adımlar
* Daha karmaşık bir ASP.NET MVC öğreticisi mi istiyorsunuz? Bkz. [ASP.NET MVC Öğreticisi: Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md).
* Azure Cosmos DB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz. [Azure Cosmos DB ile performans ve ölçek testi](performance-testing.md)
* [Azure Cosmos DB isteklerini, kullanımını ve depolamasını izlemeyi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](https://docs.microsoft.com/azure/cosmos-db/introduction).

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-sql-api-dotnet.md#create-account
