---
title: Azure Cosmos DB SQL API hesabı (SDK sürüm 3 Önizleme) verileri yönetmek için bir .NET Core konsol uygulaması oluşturma
description: Azure Cosmos DB SQL API’si .NET Core SDK'sını kullanarak çevrimiçi bir veritabanı ve C# konsol uygulaması oluşturan öğretici.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: dech
ms.openlocfilehash: 828d0ced775d46b0e966969acb3220d2a678c57b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60686068"
---
# <a name="build-a-net-core-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account-sdk-version-3-preview"></a>Azure Cosmos DB SQL API hesabı (SDK sürüm 3 Önizleme) verileri yönetmek için bir .NET Core konsol uygulaması oluşturma

> [!div class="op_single_selector"]
>
> - [.NET core (Önizleme)](sql-api-dotnet-core-get-started-preview.md)
> - [.NET Core](sql-api-dotnetcore-get-started.md)
> - [.NET (Önizleme)](sql-api-dotnet-get-started-preview.md)
> - [.NET](sql-api-get-started.md)
> - [Java](sql-api-java-get-started.md)
> - [Async Java](sql-api-async-java-get-started.md)
> - [Node.js](sql-api-nodejs-get-started.md)

Azure Cosmos DB SQL API .NET Core öğreticisini kullanmaya başlamak için Hoş Geldiniz! Bu öğreticiden yararlandıktan sonra bir .NET Core konsol uygulaması oluşturan ve sorgulayan Azure Cosmos DB kaynaklarını sahip olacaksınız. Bu öğreticide [sürüm 3.0 +](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) , Azure Cosmos DB .NET SDK, hangi hedeflerin [.NET Standard 2.0.](https://docs.microsoft.com/dotnet/standard/net-standard)

Bu öğreticinin içindekiler:

> [!div class="checklist"]
>
> - Oluşturma ve bir Azure Cosmos hesabına bağlanma
> - Projenizi Visual Studio'da yapılandırma
> - Bir veritabanı ve kapsayıcı oluşturma
> - Kapsayıcıya öğeleri ekleme
> - Kapsayıcıyı sorgulama
> - Öğesi CRUD işlemleri
> - Veritabanını silme

Uygulamayı oluşturmak için zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started)'da bulabilirsiniz. Hızlı yönergeler için [Tam çözümü edinme bölümüne](#GetSolution) atlayın.

SQL API'sini ve .NET Core SDK’yı kullanarak bir Xamarin iOS, Android, veya Forms uygulaması mı derlemek istiyorsunuz? Bkz. [Xamarin ve Azure Cosmos DB ile mobil uygulamalar derleme](mobile-apps-with-xamarin.md).

## <a name="prerequisites"></a>Önkoşullar

- Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

- Henüz Visual Studio 2017’yi yüklemediyseniz, ücretsiz [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Evrensel Windows Platformu (UWP) uygulaması geliştiriyorsanız, **Visual Studio 2017 sürüm 15.4** veya üstünü kullanmalısınız. Etkinleştirdiğinizden emin olun **Azure geliştirme** Visual Studio Kurulumu sırasında iş yükü.

  - MacOS veya Linux’ta çalışıyorsanız tercih ettiğiniz platforma yönelik [.NET Core SDK](https://www.microsoft.com/net/core#macos)’yı yükleyerek komut satırından .NET Core uygulamaları geliştirebilirsiniz.

  - Windows’da çalışıyorsanız [.NET Core SDK](https://www.microsoft.com/net/core#windows)’yı yükleyerek komut satırından .NET Core uygulamaları geliştirebilirsiniz.

  - Kendi düzenleyicinizi kullanabilir ya da ücretsiz olan ve Windows, Linux ve MacOS’de çalışan [Visual Studio Code](https://code.visualstudio.com/)’u indirebilirsiniz.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Visual Studio Çözümünüzü Kurma](#SetupVS)'ya atlayabilirsiniz. Azure Cosmos DB öykünücüsü'nü kullanıyorsanız, bu adımları izleyin [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) öykünücünün kurulumunu ve atlayın [Kurulum, Visual Studio projesi](#SetupVS).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>2. adım: Kurulum, Visual Studio projesi

1. Bilgisayarınızda **Visual Studio 2017**'yi açın.
1. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
1. İçinde **yeni proje** iletişim kutusunda **Visual C#**   /  **konsol uygulaması (.NET Core)**, projenizi adlandırın ve ardından **Tamam**.
   ![Yeni Proje penceresinin ekran görüntüsü](./media/sql-api-dotnetcore-get-started/dotnetcore-tutorial-visual-studio-new-project.png)
1. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.

   ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/sql-api-dotnetcore-get-started/dotnetcore-tutorial-visual-studio-manage-nuget.png)

1. İçinde **NuGet** sekmesinde **Gözat**ve türü **Microsoft.Azure.Cosmos** arama kutusuna.
1. Bul sonuçları içinde **Microsoft.Azure.Cosmos** tıklatıp **yükleme**.
   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)’dır.
   ![Azure Cosmos DB istemci SDK'sını bulmak için NuGet menüsünün ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

   Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

Harika! Kurulumu tamamladığımıza göre, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanmış kod projesini [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started/)'da bulabilirsiniz.

## <a id="Connect"></a>3. adım: Bir Azure Cosmos DB hesabına bağlanma

1. İlk olarak, başında başvuruları değiştirin, C# içinde uygulama **Program.cs** bu başvuruları dosyasıyla:

   ```csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.Azure.Cosmos;
   using System.Collections.Generic;
   using System.Net;
   ```

1. Şimdi bu sabitleri ve değişkenleri ortak sınıfının ekleyin `Program`.

   ```csharp
   public class Program
   {
       // ADD THIS PART TO YOUR CODE

       // The Azure Cosmos DB endpoint for running this sample.
       private static readonly string EndpointUri = "<your endpoint here>";
       // The primary key for the Azure Cosmos account.
       private static readonly string PrimaryKey = "<your primary key>";

       // The Cosmos client instance
       private CosmosClient cosmosClient;

       // The database we will create
       private CosmosDatabase database;

       // The container we will create.
       private CosmosContainer container;

       // The name of the database and container we will create
       private string databaseId = "FamilyDatabase";
       private string containerId = "FamilyContainer";
   }
   ```

   Unutmayın, .NET SDK'sının önceki sürümüyle biliyorsanız, koşulları 'collection' ve 'document' görmeye kullanılabilir Azure Cosmos DB, birden çok API modelini desteklediğinden, sürüm 3.0 + .NET SDK'sının genel Koşulları 'kapsayıcısı' ve 'öğesi' kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. [Daha fazla ilgili veritabanları, kapsayıcıları ve öğeleri öğrenin.](databases-containers-items.md)

1. Uç nokta URL'nizi ve birincil anahtarı almak [Azure portalında](https://portal.azure.com).

   Azure portalında Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’a tıklayın.

   Portaldaki URI'yi kopyalayın ve yapıştırın `<your endpoint URL>` içinde `Program.cs` dosya. Portaldan birincil anahtarı kopyalayın ve yapıştırın `<your primary key>`.

   ![Azure portalında Azure Cosmos DB anahtarlarını almak için ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. Ardından, yeni bir örneğini oluşturacağız `CosmosClient` ve programımız için bazı iskele kurun.

   Aşağıda **ana** yöntemi olarak adlandırılan yeni bir zaman uyumsuz görev eklemek **GetStartedDemoAsync**, örneğini oluşturacak yeni `CosmosClient`. Kullanacağız **GetStartedDemoAsync** yöntemler, çağıran giriş noktası olarak Azure Cosmos DB kaynakları üzerinde çalışır.

   ```csharp
   public static async Task Main(string[] args)
   {
   }

   // ADD THIS PART TO YOUR CODE
   /*
       Entry point to call methods that operate on Azure Cosmos DB resources in this sample
   */
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
   }
   ```

1. Çalıştırmak için aşağıdaki kodu ekleyin **GetStartedDemoAsync** zaman uyumsuz görevi, **ana** yöntemi. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

   ```csharp
   public static async Task Main(string[] args)
   {
       // ADD THIS PART TO YOUR CODE
       try
       {
           Console.WriteLine("Beginning operations...\n");
           Program p = new Program();
           await p.GetStartedDemoAsync();
       }
       catch (CosmosException de)
       {
           Exception baseException = de.GetBaseException();
           Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
       }
       catch (Exception e)
       {
           Console.WriteLine("Error: {0}\n", e);
       }
       finally
       {
           Console.WriteLine("End of demo, press any key to exit.");
           Console.ReadKey();
       }
   }
   ```

1. Seçin **F5** uygulamanızı çalıştırmak için. Konsol penceresi çıktısı görüntülenir `End of demo, press any key to exit.` Azure Cosmos DB bağlantının kurulduğunu onaylayan. Ardından konsol penceresini kapatabilirsiniz.

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız.

## <a name="step-4-create-a-database"></a>4. Adım: Veritabanı oluşturma

Bir veritabanını kullanarak oluşturulabilir [ **Createdatabaseasync** ](https://aka.ms/CosmosDotnetAPIDocs) veya [ **Documentclient** ](https://aka.ms/CosmosDotnetAPIDocs) işlevi **CosmosDatabases** sınıfı. Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır.

1. Kopyalama ve yapıştırma **CreateDatabase** yöntemi aşağıdaki, **GetStartedDemoAsync** yöntemi. **CreateDatabase** kimliğine sahip yeni bir veritabanı oluşturur `FamilyDatabase` , zaten, öğesinden belirtilen kimliğe sahip yoksa `databaseId` alan.

   ```csharp
   /*
       Create the database if it does not exist
   */
   private async Task CreateDatabase()
   {
       // Create a new database
       this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
       Console.WriteLine("Created Database: {0}\n", this.database.Id);
   }
   ```

1. Çağrılacak CosmosClient örneği burada aşağıdaki kodu yapıştırın **CreateDatabase** eklediğiniz yöntemi.

   ```csharp
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

       //ADD THIS PART TO YOUR CODE
       await this.CreateDatabase();
   }
   ```

   Bu noktada, kodunuzu gibi uç noktanızı ve birincil anahtar doldurulmuş ile görünmesi gerekir. Not ad farklılık gösterir, projenizin adına göre.

   ```csharp
   using System;
   using System.Threading.Tasks;
   using Microsoft.Azure.Cosmos;
   using System.Collections.Generic;
   using System.Net;

   namespace CosmosGettingStartedDotnetCoreTutorial
   {
       class Program
       {
           // The Azure Cosmos DB endpoint for running this sample.
           private static readonly string EndpointUri = "<your endpoint here>";
           // The primary key for the Azure Cosmos account.
           private static readonly string PrimaryKey = "<your primary key>";

           // The Cosmos client instance
           private CosmosClient cosmosClient;

           // The database we will create
           private CosmosDatabase database;

           // The container we will create.
           private CosmosContainer container;

           // The name of the database and container we will create
           private string databaseId = "FamilyDatabase";
           private string containerId = "FamilyContainer";

           public static async Task Main(string[] args)
           {
               try
               {
                   Console.WriteLine("Beginning operations...");
                   Program p = new Program();
                   await p.GetStartedDemoAsync();
               }
               catch (CosmosException de)
               {
                   Exception baseException = de.GetBaseException();
                   Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
               }
               catch (Exception e)
               {
                   Console.WriteLine("Error: {0}\n", e);
               }
               finally
               {
                   Console.WriteLine("End of demo, press any key to exit.");
                   Console.ReadKey();
               }
           }

           /*
               Entry point to call methods that operate on Azure Cosmos DB resources in this sample
           */
           public async Task GetStartedDemoAsync()
           {
               // Create a new instance of the Cosmos Client
               this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
               await this.CreateDatabase();
           }

           /*
               Create the database if it does not exist
           */
           private async Task CreateDatabase()
           {
               // Create a new database
               this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
               Console.WriteLine("Created Database: {0}\n", this.database.Id);
           }
       }
   }
   ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.

## <a id="CreateColl"></a>5. adım: Bir kapsayıcı oluşturma

> [!WARNING]
> Yöntemini çağırarak **CreateContainerIfNotExistsAsync** ödenmesini yeni bir kapsayıcı oluşturur. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.

Bir kapsayıcı kullanarak oluşturulabilir [ **CreateContainerIfNotExistsAsync** ](https://aka.ms/CosmosDotnetAPIDocs) veya [ **CreateContainerAsync** ](https://aka.ms/CosmosDotnetAPIDocs) işlevi **CosmosContainers** sınıfı. Bir kapsayıcı (Bu SQL API'si söz konusu olduğunda JSON belgelerini) öğelerden oluşur ve ilişkili JavaScript sunucu tarafı uygulama mantığı, örneğin saklı yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler.

1. Kopyalama ve yapıştırma **CreateContainer** yöntemi aşağıdaki, **CreateDatabase** yöntemi. **CreateContainer** kimliğine sahip yeni bir kapsayıcı oluşturacak `FamilyContainer` , zaten, öğesinden belirtilen kimliğe sahip yoksa `containerId` alan.

   ```csharp
   /*
       Create the container if it does not exist.
       Specify "/LastName" as the partition key since we're storing family information, to ensure good distribution of requests and storage.
   */
   private async Task CreateContainer()
   {
       // Create a new container
       this.container = await this.database.Containers.CreateContainerIfNotExistsAsync(containerId, "/LastName");
       Console.WriteLine("Created Container: {0}\n", this.container.Id);
   }
   ```

1. Çağrılacak CosmosClient örneği burada aşağıdaki kodu yapıştırın **CreateContainer** eklediğiniz yöntemi.

   ```csharp
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
       await this.CreateDatabase();

       //ADD THIS PART TO YOUR CODE
       await this.CreateContainer();
   }
   ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB kapsayıcısı oluşturdunuz.

## <a id="CreateDoc"></a>6. adım: Öğeleri kapsayıcıya Ekle

Bir öğeyi kullanarak oluşturulabilir [ **Createıtemasync** ](https://aka.ms/CosmosDotnetAPIDocs) işlevi **CosmosItems** sınıfı. SQL API'sini kullanarak öğeler, kullanıcı tanımlı (rastgele) JSON içeriği olan belgeler olarak görüntülenir. Bu gibi durumlarda, bir öğe artık Azure Cosmos DB kapsayıcınız ekleyebilirsiniz.

İlk olarak, bu örnekte Azure Cosmos DB içinde depolanan nesneleri temsil edecek bir **Family** sınıfı oluşturmamız gerekir. **Family**'nin içinde kullanılan **Parent**, **Child**, **Pet**, **Address** alt sınıflarını da oluşturacağız. Belgelerin, JSON'da **id** olarak seri hale getirilmiş bir **Id** özelliğine sahip olmaları gerektiğini unutmayın.

1.  Seçin **Ctrl + Shift + A** açmak için **Yeni Öğe Ekle** iletişim. Yeni bir sınıf ekleyin **Family.cs** projenize.

    ![Projeye yeni Family.cs sınıf ekleme işleminin ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1.  Kopyalama ve yapıştırma **ailesi**, **üst**, **alt**, **evcil hayvan**, ve **adresi** sınıfına**Family.cs**. Not ad farklılık gösterir, projenizin adına göre.

    ```csharp
    using Newtonsoft.Json;

    namespace CosmosGettingStartedDotnetCoreTutorial
    {
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
    }
    ```

1.  Geri gidin **Program.cs** ve ekleme **AddItemsToContainer** altındaki yöntemin, **CreateContainer** yöntemi.
    Kod oluşturmadan önce aynı kimliğe sahip bir öğe zaten yok emin olmak için kontrol eder. Biz, her biri Andersen ailesi ve Wakefield ailesi için iki öğeyi ekler.

    ```csharp
     /*
         Add Family items to the container
     */
     private async Task AddItemsToContainer()
     {
         // Create a family object for the Andersen family
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

         // Read the item to see if it exists. Note ReadItemAsync will not throw an exception if an item does not exist. Instead, we check the StatusCode property off the response object.
         CosmosItemResponse<Family> andersenFamilyResponse = await this.container.Items.ReadItemAsync<Family>(andersenFamily.LastName, andersenFamily.Id);

         if (andersenFamilyResponse.StatusCode == HttpStatusCode.NotFound)
         {
             // Create an item in the container representing the Andersen family. Note we provide the value of the partition key for this item, which is "Andersen"
             andersenFamilyResponse = await this.container.Items.CreateItemAsync<Family>(andersenFamily.LastName, andersenFamily);

             // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse.
             //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
             Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", andersenFamilyResponse.Resource.Id, andersenFamilyResponse.RequestCharge);
         }
         else
         {
             Console.WriteLine("Item in database with id: {0} already exists\n", andersenFamilyResponse.Resource.Id);
         }

         // Create a family object for the Wakefield family
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

         // Read the item to see if it exists
         CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily.Id);

         if (wakefieldFamilyResponse.StatusCode == HttpStatusCode.NotFound)
         {
             // Create an item in the container representing the Wakefield family. Note we provide the value of the partition key for this item, which is "Wakefield"
             wakefieldFamilyResponse = await this.container.Items.CreateItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily);

             // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse.
             //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
             Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", wakefieldFamilyResponse.Resource.Id, wakefieldFamilyResponse.RequestCharge);
         }
         else
         {
             Console.WriteLine("Item in database with id: {0} already exists\n", wakefieldFamilyResponse.Resource.Id);
         }
     }
    
    ```

1.  Bir çağrı ekleyin `AddItemsToContainer` içinde `GetStartedDemoAsync` yöntemi.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase();
        await this.CreateContainer();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainer();
    }
    ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! İki Azure Cosmos DB öğeleri başarıyla oluşturdunuz.

## <a id="Query"></a>7. adım: Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, önceki adımda eklememizden öğeleri karşı sorgu çalıştırmak gösterilmektedir.

1. Kopyalama ve yapıştırma **RunQuery** yöntemi aşağıdaki, **AddItemsToContainer** yöntemi.

   ```csharp
   /*
       Run a query (using Azure Cosmos DB SQL syntax) against the container
   */
   private async Task RunQuery()
   {
       var sqlQueryText = "SELECT * FROM c WHERE c.LastName = 'Andersen'";
       var partitionKeyValue = "Andersen";

       Console.WriteLine("Running query: {0}\n", sqlQueryText);

       CosmosSqlQueryDefinition queryDefinition = new CosmosSqlQueryDefinition(sqlQueryText);
       CosmosResultSetIterator<Family> queryResultSetIterator = this.container.Items.CreateItemQuery<Family>(queryDefinition, partitionKeyValue);

       List<Family> families = new List<Family>();

       while (queryResultSetIterator.HasMoreResults)
       {
           CosmosQueryResponse<Family> currentResultSet = await queryResultSetIterator.FetchNextSetAsync();
           foreach (Family family in currentResultSet)
           {
               families.Add(family);
               Console.WriteLine("\tRead {0}\n", family);
           }
       }
   }
   ```

1. Bir çağrı ekleyin `RunQuery` içinde `GetStartedDemoAsync` yöntemi.

   ```csharp
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
       await this.CreateDatabase();
       await this.CreateContainer();
       await this.AddItemsToContainer();

       //ADD THIS PART TO YOUR CODE
       await this.RunQuery();
   }
   ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Bir Azure Cosmos DB kapsayıcısı karşı başarıyla sorguladınız.

## <a id="ReplaceItem"></a>8. adım: Bir JSON öğesini değiştirin

Şimdi, Azure Cosmos DB içindeki bir öğeyi güncelleştireceğiz.

1. Kopyalama ve yapıştırma **ReplaceFamilyItem** yöntemi aşağıdaki, **RunQuery** yöntemi. Değiştirmeyi unutmayın `IsRegistered` özellik ailesinin ve `Grade` alt birinin.

   ```csharp
   /*
   Update an item in the container
   */
   private async Task ReplaceFamilyItem()
   {
       CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>("Wakefield", "Wakefield.7");
       var itemBody = wakefieldFamilyResponse.Resource;

       // update registration status from false to true
       itemBody.IsRegistered = true;
       // update grade of child
       itemBody.Children[0].Grade = 6;

       // replace the item with the updated content
       wakefieldFamilyResponse = await this.container.Items.ReplaceItemAsync<Family>(itemBody.LastName, itemBody.Id, itemBody);
       Console.WriteLine("Updated Family [{0},{1}]\n. Body is now: {2}\n", itemBody.LastName, itemBody.Id, wakefieldFamilyResponse.Resource);
   }
   ```

1. Bir çağrı ekleyin `ReplaceFamilyItem` içinde `GetStartedDemo` yöntemi.

   ```csharp
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
       await this.CreateDatabase();
       await this.CreateContainer();
       await this.AddItemsToContainer();
       await this.RunQuery();

       //ADD THIS PART TO YOUR CODE
       await this.ReplaceFamilyItem();
   }
   ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesini değiştirdiniz.

## <a id="DeleteDocument"></a>9. adım: Öğeyi sil

Şimdi, size Azure Cosmos DB içindeki bir öğeyi siler.

1. Kopyalama ve yapıştırma **DeleteFamilyItem** yöntemi aşağıdaki, **ReplaceFamilyItem** yöntemi.

   ```csharp
   /*
   Delete an item in the container
   */
   private async Task DeleteFamilyItem()
   {
       var partitionKeyValue = "Wakefield";
       var familyId = "Wakefield.7";

       // Delete an item. Note we must provide the partition key value and id of the item to delete
       CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.DeleteItemAsync<Family>(partitionKeyValue, familyId);
       Console.WriteLine("Deleted Family [{0},{1}]\n", partitionKeyValue, familyId);
   }
   ```

1. Bir çağrı ekleyin `DeleteFamilyItem` içinde `GetStartedDemo` yöntemi.

   ```csharp
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
       await this.CreateDatabase();
       await this.CreateContainer();
       await this.AddItemsToContainer();
       await this.RunQuery();
       await this.ReplaceFamilyItem();

       //ADD THIS PART TO YOUR CODE
       await this.DeleteFamilyItem();
   }
   ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesini sildiniz.

## <a id="DeleteDatabase"></a>10. adım: Veritabanını silme

Şimdi biz veritabanımızdaki silecek. Oluşturulan veritabanı silindiğinde kaldırılır veritabanını ve tüm alt kaynaklarını (kapsayıcılar, öğeleri ve depolanan yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler). Biz de elden **CosmosClient** örneği.

1. Kopyalama ve yapıştırma **DeleteDatabaseAndCleanup** yöntemi aşağıdaki, **DeleteFamilyItem** yöntemi.

   ```csharp
   /*
   Delete the database and dispose of the Cosmos Client instance
   */
   private async Task DeleteDatabaseAndCleanup()
   {
       CosmosDatabaseResponse databaseResourceResponse = await this.database.DeleteAsync();
       // Also valid: await this.cosmosClient.Databases["FamilyDatabase"].DeleteAsync();

       Console.WriteLine("Deleted Database: {0}\n", this.databaseId);

       //Dispose of CosmosClient
       this.cosmosClient.Dispose();
   }
   ```

1. Bir çağrı ekleyin `DeleteDatabaseAndCleanup` içinde `GetStartedDemo` yöntemi.

   ```csharp
   public async Task GetStartedDemoAsync()
   {
       // Create a new instance of the Cosmos Client
       this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
       await this.CreateDatabase();
       await this.CreateContainer();
       await this.AddItemsToContainer();
       await this.RunQuery();
       await this.ReplaceFamilyItem();
       await this.DeleteFamilyItem();

       //ADD THIS PART TO YOUR CODE
       await this.DeleteDatabaseAndCleanup();
   }
   ```

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanını sildiniz.

## <a id="Run"></a>11. adım: Çalıştırın, C# konsol uygulaması tümünü bir araya!

Seçin **F5** uygulamayı hata ayıklama modunda oluşturmak için Visual Studio'da.

Tüm konsol penceresinde başlatılan uygulama alma çıktısını görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir ve aşağıdaki örnek metinle eşleşmelidir.

```
Beginning operations...

Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.43 RUs.

Created item in database with id: Wakefield.7 Operation consumed 14.29 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

Updated Family [Wakefield,Wakefield.7].
        Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"},{"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6,"Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1,"Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}

Deleted Family [Wakefield,Wakefield.7]

Deleted Database: FamilyDatabase

End of demo, press any key to exit.
```

Tebrikler! Bu öğreticiyi tamamladınız ve çalışan bir C# konsol uygulamasına sahipsiniz!

## <a id="GetSolution"></a> Tam öğretici çözümünü edinin

Bu makaledeki tüm örnekleri içeren GetStarted çözümünü derlemek için aşağıdakilere ihtiyacınız vardır:

- Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Bir [Azure Cosmos DB hesabı][cosmos-db-create-account].
- GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started) çözümü.

Visual Studio'da Azure Cosmos DB .NET Core SDK için başvuruları SQL API'sine geri yüklemek için sağ **GetStarted** çözümde, Çözüm Gezgini ve ardından **NuGet paketlerini geri yükle**. Ardından **Program.cs** dosyası, güncelleştirme `EndpointUri` ve `PrimaryKey` değerleri açıklandığı [bir Azure Cosmos DB hesabına bağlanma](#Connect).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB SQL API verileri yönetmek için bir .NET Core uygulaması derleme öğrendiniz. Şimdi Cosmos DB hesabınıza ek veri aktarabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)

[cosmos-db-create-account]: create-sql-api-dotnet-preview.md#create-account
