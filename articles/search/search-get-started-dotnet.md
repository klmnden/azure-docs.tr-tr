---
title: "C#Hızlı Başlangıç: -Azure Search .NET SDK'sını kullanarak dizin sorgulama oluşturma ve yükleme"
description: Dizin oluşturma, veri yüklemek ve sorguları kullanarak çalıştırma açıklanmaktadır C# ve Azure Search .NET SDK'sı.
author: heidisteen
manager: cgronlun
ms.author: heidist
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/20/2019
ms.openlocfilehash: a5cbd2036f92c27709d92d0cf415cc9837645fb8
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485611"
---
# <a name="quickstart-create-an-azure-search-index-in-c-using-the-net-sdk"></a>Hızlı Başlangıç: Azure Search dizini oluşturma C# .NET SDK kullanarak
> [!div class="op_single_selector"]
> * [C#](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)
>*

.NET Core oluşturma C# konsol uygulaması oluşturur, yükler ve Visual Studio kullanarak Azure Search dizini sorgular ve [Azure Search .NET SDK'sı](https://aka.ms/search-sdk). Bu makalede, uygulamayı oluşturma adım adım açıklanmaktadır. Alternatif olarak, [indirme ve çalıştırma uygulamanın](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/Quickstart).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Bu makalede tanıtım kod, kolaylık sağlamak için Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Ancak, üretim senaryoları için zaman uyumsuz yöntemler, kendi uygulamalarınızda bunları ölçeklenebilir ve esnek tutmanızı kullanmanızı öneririz. Örneğin, kullanabileceğinizi `CreateAsync` ve `DeleteAsync` yerine `Create` ve `Delete`.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki hizmetleri, araçları ve verileri kullanılır. 

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/), herhangi bir sürümü. Örnek kodu ve yönergeleri ücretsiz Community edition üzerinde test edilmiştir.

+ Bu makalede de örnek dizin ve belgeler olarak sağlanan [Visual Studio çözümü](https://github.com/Azure-Samples/azure-search-dotnet-samples/quickstart) Bu Hızlı Başlangıç için.

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu Hızlı Başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

<a name="get-service-info"></a>

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

Hizmete çağrı, bir URL uç noktası ve her istekte bir erişim anahtarı gerektirir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

   Sorgu anahtarı da alın. Sorgu istekleri ile salt okunur erişim vermek için en iyi bir uygulamadır.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-get-started-postman/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Visual Studio açılırken ve .NET Core üzerinde çalışabilen yeni bir konsol uygulaması projesi oluşturarak başlayın.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

[Azure Search .NET SDK'sı](https://aka.ms/search-sdk) NuGet paketleri olarak dağıtılan bazı istemci kitaplıkları içerir.

Bu proje için 9 sürümünü kullanan `Microsoft.Azure.Search` NuGet paketi ve en son `Microsoft.Extensions.Configuration.Json` NuGet paketi.

1. İçinde **Araçları** > **NuGet Paket Yöneticisi**seçin **çözüm için NuGet paketlerini Yönet...** . 

1. **Gözat**’a tıklayın.

1. Arama `Microsoft.Azure.Search` ve sürüm 9.0.1 seçin veya üzeri.

1. Tıklayın **yükleme** çözüm ve proje derleme eklemek için sağ taraftaki.

1. Yineleme için `Microsoft.Extensions.Configuration.Json`, sürüm 2.2.0 seçerek veya üzeri.


### <a name="add-azure-search-service-information"></a>Azure arama hizmeti bilgilerini ekleme

1. Çözüm Gezgini'nde sağ tıklatın ve proje tıklayın **Ekle** > **yeni öğe...**  . 

1. Yeni Öğe Ekle "JSON ile ilgili bir öğe türlerinin listesi döndürülecek JSON" için arama yapın.

1. Seçin **JSON dosyası**"appsettings.json" dosya adı ve tıklayın **Ekle**. 

1. Dosyayı, çıktı dizinine ekleyin. AppSettings.JSON sağ tıklayıp **özellikleri**. İçinde **çıkış dizinine Kopyala**seçin **yeniyse Kopyala**.

1. Aşağıdaki JSON yeni JSON dosyanıza kopyalayın. Arama hizmeti adı (YOUR-SEARCH-hizmet-adı) ve yönetici API anahtarını (YOUR-ADMIN-API-KEY) geçerli değerlerle değiştirin. Hizmet uç noktanıza ise `https://mydemo.search.windows.net`, hizmet adı "mydemo" olacaktır.

```json
{
  "SearchServiceName": "<YOUR-SEARCH-SERVICE-NAME>",
  "SearchServiceAdminApiKey": "<YOUR-ADMIN-API-KEY>",
  "SearchIndexName": "hotels-quickstart"
}
```

### <a name="add-class-method-files-to-your-project"></a>Sınıf ekleme ". Projenize yöntemi"dosyaları

Sonuçları konsol penceresinde yazdırırken, otel nesnesinden tek tek alanları dize olarak döndürülmelidir. Uygulayabileceğiniz [ToString()](https://docs.microsoft.com/dotnet/api/system.object.tostring?view=netframework-4.8) gerekli kodu için iki yeni dosya kopyalama, bu görevi gerçekleştirmek için.

1. İki boş sınıf tanımları, projenize ekleyin: Address.Methods.cs, Hotel.Methods.cs

1. Address.Methods.cs içinde varsayılan içeriklerini aşağıdaki kodla üzerine [1-32 satırları](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/Quickstart/AzureSearchQuickstart/Address.Methods.cs/#L1-L32).

1. Hotel.Methods.cs içinde kopyalama [1 66 satırları](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/Quickstart/AzureSearchQuickstart/Hotel.Methods.cs/#L1-L66).


## <a name="1---create-index"></a>1 - dizin oluşturma

Burada basit bir alan "HotelName" veya "Description" ve karmaşık alanları odaları koleksiyonunu veya alt sahip bir adresi olan basit ve karmaşık alanları, Oteller dizinini oluşur. Dizin karmaşık türler içerdiğinde, ayrı sınıflardaki karmaşık alan tanımları yalıtın.

1. İki boş sınıf tanımları, projenize ekleyin: Address.cs, Hotel.cs

1. Address.cs içinde varsayılan içeriklerini aşağıdaki kodla üzerine yaz:

    ```csharp
    using System;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Newtonsoft.Json;

    namespace AzureSearchQuickstart
    {
        public partial class Address
        {
            [IsSearchable]
            public string StreetAddress { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string City { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string StateProvince { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string PostalCode { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string Country { get; set; }
        }
    }
    ```

1. Hotel.cs dosyasında sınıfın genel adres sınıfı başvuruları dahil olmak üzere, dizin yapısını tanımlar.

    ```csharp
    namespace AzureSearchQuickstart
    {
        using System;
        using Microsoft.Azure.Search;
        using Microsoft.Azure.Search.Models;
        using Newtonsoft.Json;

        public partial class Hotel
        {
            [System.ComponentModel.DataAnnotations.Key]
            [IsFilterable]
            public string HotelId { get; set; }

            [IsSearchable, IsSortable]
            public string HotelName { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
            public string Description { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.FrLucene)]
            [JsonProperty("Description_fr")]
            public string DescriptionFr { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string Category { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string[] Tags { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public bool? ParkingIncluded { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public DateTimeOffset? LastRenovationDate { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public double? Rating { get; set; }

            public Address Address { get; set; }
        }
    }
    ```

    Alandaki öznitelikler, bir uygulamada nasıl kullanılacağını belirler. Örneğin, `IsSearchable` özniteliği bir tam metin aramasına dahil edilmesi gereken her alanı atanır. .NET SDK'da açıkça etkin olmayan alan davranışları devre dışı bırakmak için varsayılandır.

    Türündeki dizininizde yalnızca bir alanın `string` olmalıdır *anahtar* her belgenin benzersiz olarak tanımlayan alan. Bu şemada anahtardır `HotelId`.

    Bu dizinde varsayılan standart olarak Lucene çözümleyici geçersiz kılmak istediğiniz zaman belirtilen isteğe bağlı Çözümleyicisi özellik, açıklama alanları kullanın. `description_fr` Alanını Fransızca Lucene çözümleyici kullanarak ([FrLucene](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername.frlucene?view=azure-dotnet)) Fransızca metin depoladığından. `description` İsteğe bağlı Microsoft dil çözümleyicisini kullanarak ([EnMicrosoft](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername.enmicrosoft?view=azure-dotnet)).

1. Program.cs içinde bir örneğini oluşturmak [ `SearchServiceClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet) (appsettings.json) uygulamanın yapılandırma dosyasında depolanan değerleri kullanarak hizmetine bağlanmak için sınıf. 

   `SearchServiceClient` sahip bir [ `Indexes` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexes?view=azure-dotnet) listesinde, güncelleştirme veya Azure Search dizinlerini silme özelliği oluşturmak için gereken tüm yöntemleri sağlar. 

    ```csharp
    using System;
    using System.Linq;
    using System.Threading;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Microsoft.Extensions.Configuration;

    namespace AzureSearchQuickstart
    {
        class Program
            // Demonstrates index delete, create, load, and query
            // Commented-out code is uncommented in later steps
            static void Main(string[] args)
            {
                IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
                IConfigurationRoot configuration = builder.Build();

                SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

                string indexName = configuration["SearchIndexName"];

                Console.WriteLine("{0}", "Deleting index...\n");
                DeleteIndexIfExists(indexName, serviceClient);

                Console.WriteLine("{0}", "Creating index...\n");
                CreateIndex(indexName, serviceClient);

                // Uncomment next 3 lines in "2 - Load documents"
                // ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
                // Console.WriteLine("{0}", "Uploading documents...\n");
                // UploadDocuments(indexClient);

                // Uncomment next 2 lines in "3 - Search an index"
                // Console.WriteLine("{0}", "Searching index...\n");
                // RunQueries(indexClient);

                Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
                Console.ReadKey();
            }

            // Create the search service client
            private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
            {
                string searchServiceName = configuration["SearchServiceName"];
                string adminApiKey = configuration["SearchServiceAdminApiKey"];

                SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
                return serviceClient;
            }

            // Delete an existing index to reuse its name
            private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
            {
                if (serviceClient.Indexes.Exists(indexName))
                {
                    serviceClient.Indexes.Delete(indexName);
                }
            }

            // Create an index whose fields correspond to the properties of the Hotel class.
            // The Address property of Hotel will be modeled as a complex field.
            // The properties of the Address class in turn correspond to sub-fields of the Address complex field.
            // The fields of the index are defined by calling the FieldBuilder.BuildForType() method.
            private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
            {
                var definition = new Index()
                {
                    Name = indexName,
                    Fields = FieldBuilder.BuildForType<Hotel>()
                };

                serviceClient.Indexes.Create(definition);
            }
        }
    }    
    ```

    Mümkünse, tek bir örneğini paylaşmak `SearchServiceClient` uygulamanızda çok fazla bağlantı açmayı önlemek için. Tür paylaşımları etkinleştirmek için iş parçacığı açısından güvenli sınıf yöntemleridir.

   Sınıfın birkaç Oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

    Oluşturmanın en kolay yolu dizin tanımında `Field` nesneleri, çağırarak `FieldBuilder.BuildForType` yöntemi, tür parametresi için bir model sınıfı geçirme. Bir model sınıfında dizininizin alanlarıyla eşlenen özellikler mevcuttur. Bu eşleme arama dizininizdeki belgeleri model sınıfınızın örneklerine bağlanacak sağlar.

    > [!NOTE]
    > Bir model sınıfı kullanmayı planlamıyorsanız, `Field` nesnelerini doğrudan oluşturarak dizininizi tanımlayabilirsiniz. Oluşturucuya alan adını veri türü (veya dize alanları çözümleyicisi) ile birlikte sağlayabilirsiniz. Gibi diğer özellikleri de ayarlayabilirsiniz `IsSearchable`, `IsFilterable`, birkaçıdır.
    >

1. Uygulama oluşturmak ve dizin oluşturma için F5 tuşuna basın. 

    Proje başarıyla oluşturulursa, dizin oluşturma ve silme için ekrandaki durum iletileri yazma bir konsol penceresi açılır. 

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - belge yükleme

Azure Search'te, dizin oluşturma için her iki girişleri ve çıkışları sorgularından veri yapıları belgelerdir. Bir dış veri kaynağından elde edilmiş olarak bir veritabanında satır BLOB'lar, Blob Depolama veya diskte JSON belgelerini belge girişleri olabilir. Bu örnekte, biz bir kısayol almak ve JSON belgeleri için dört hotels kodda ekleme. 

Belgeler karşıya yüklenirken kullanmalısınız bir [ `IndexBatch` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) nesne. Bir `IndexBatch` koleksiyonunu içeren [ `IndexAction` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) nesneleri, her biri içeren belge ve Azure Search gerçekleştirilecek eylem belirten bir özellik ([karşıya yükleme, birleştirme, silme ve mergeOrUpload](search-what-is-data-import.md#indexing-actions)).

1. Program.cs, belgeler ve dizin eylemleri bir dizi oluşturun ve ardından diziye geçirin `IndexBatch`. Aşağıdaki belgeler, otel ve adresi sınıfları tarafından tanımlandığı şekilde otel-quickstart dizine uygun.

    ```csharp
    // Upload documents as a batch
    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var actions = new IndexAction<Hotel>[]
        {
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "1",
                    HotelName = "Secret Point Motel",
                    Description = "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
                    DescriptionFr = "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "air conditioning", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(1970, 1, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.6,
                    Address = new Address()
                    {
                        StreetAddress = "677 5th Ave",
                        City = "New York",
                        StateProvince = "NY",
                        PostalCode = "10022",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "2",
                    HotelName = "Twin Dome Motel",
                    Description = "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "free wifi", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate =  new DateTimeOffset(1979, 2, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.60,
                    Address = new Address()
                    {
                        StreetAddress = "140 University Town Center Dr",
                        City = "Sarasota",
                        StateProvince = "FL",
                        PostalCode = "34243",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "3",
                    HotelName = "Triple Landscape Hotel",
                    Description = "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Resort and Spa",
                    Tags = new[] { "air conditioning", "bar", "continental breakfast" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(2015, 9, 20, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4.80,
                    Address = new Address()
                    {
                        StreetAddress = "3393 Peachtree Rd",
                        City = "Atlanta",
                        StateProvince = "GA",
                        PostalCode = "30326",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "4",
                    HotelName = "Sublime Cliff Hotel",
                    Description = "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
                    DescriptionFr = "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
                    Category = "Boutique",
                    Tags = new[] { "concierge", "view", "24-hour front desk service" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1960, 2, 06, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4.6,
                    Address = new Address()
                    {
                        StreetAddress = "7400 San Pedro Ave",
                        City = "San Antonio",
                        StateProvince = "TX",
                        PostalCode = "78216",
                        Country = "USA"
                    }
                }
            ),
        };

        var batch = IndexBatch.New(actions);

        try
        {
            indexClient.Documents.Index(batch);
        }
        catch (IndexBatchException e)
        {
            // When a service is under load, indexing might fail for some documents in the batch. 
            // Depending on your application, you can compensate by delaying and retrying. 
            // For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
        }

        // Wait 2 seconds before starting queries 
        Console.WriteLine("Waiting for indexing...\n");
        Thread.Sleep(2000);
    }
    ```

    Sonra Başlat`IndexBatch` nesne gönderebilirsiniz, dizine çağırarak [ `Documents.Index` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) üzerinde [ `SearchIndexClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) nesne. `Documents` bir özelliği `SearchIndexClient` ekleme, değiştirme, silme veya belgeleri dizininize sorgulama için yöntemler sağlar.

    `try` / `catch` Çağrıyı çevrelediğine `Index` yöntemi önbellek hizmetiniz ağır yük altında olursa gerçekleşebilir hataları, dizin oluşturma. Üretim kodunda, gecikme ve sonra başarısız, veya oturum açın ve örnek gibi devam veya uygulamanızın veri tutarlılığı gereksinimlerini karşılayan başka bir şekilde işlemek olan belgelere dizin oluşturmayı yeniden deneyin.

    2 saniyelik gecikme dizin oluşturma işlemi için sorgu yürütülmeden önce tüm belgeler dizine böylece zaman uyumsuz olduğu dengeler. Gecikmeden kodlama genellikle yalnızca tanıtımları, testleri ve örnek uygulamalarda gereklidir.

1. Program.CS'de ana, içinde "2 - yük belgeler" satırları işletilir satırlara çevir. 

    ```csharp
    // Uncomment next 3 lines in "2 - Load documents"
    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);
    ```
1. Uygulamayı yeniden derlemek için F5 tuşuna basın. 

    Proje başarıyla oluşturulursa, durum iletileri, karşıya yükleme belgeler hakkında bir ileti bu sefer yazma bir konsol penceresi açılır. Azure portalında arama hizmeti **genel bakış** sayfasında, Oteller-quickstart dizin şu anda 4 belge olmalıdır.

Belge işleme hakkında daha fazla bilgi için bkz. [".NET SDK belgeleri nasıl işler"](search-howto-dotnet-sdk.md#how-dotnet-handles-documents).

## <a name="3---search-an-index"></a>3 - Dizin arama

İlk belgenin dizine alınır, ancak tüm belgeler dizine kadar dizininizi gerçek test beklemesi gereken sürede sorgu sonuçları elde edebilirsiniz. 

Bu bölüm iki işlev parçalarını ekler: Sorgu mantığının ve sonuçları. Sorgular için [ `Search` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.search?view=azure-dotnet
) yöntemi. Bu yöntem, arama metni hem de diğer alır [parametreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet). 

[ `DocumentsSearchResult` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1?view=azure-dotnet) Sınıf sonuçları temsil eder.


1. Program.cs içinde arama sonuçlarını konsola yazdırır WriteDocuments yöntemi oluşturun.

    ```csharp
    private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
    {
        foreach (SearchResult<Hotel> result in searchResults.Results)
        {
            Console.WriteLine(result.Document);
        }

        Console.WriteLine();
    }
    ```

1. Sorguları yürütmek ve sonuçları döndürmek için bir RunQueries yöntemi oluşturun. Sonuçlar otel nesneler olanlardır. Yüzey tek tek alanları için select parametresini kullanabilirsiniz. Bir alan seçin parametreyi dahil edilmemişse, karşılık gelen otel özelliği null olur.

    ```csharp
    private static void RunQueries(ISearchIndexClient indexClient)
    {
        SearchParameters parameters;
        DocumentSearchResult<Hotel> results;

        // Query 1 
        Console.WriteLine("Query 1: Search for term 'Atlanta' with no result trimming");
        parameters = new SearchParameters();
        results = indexClient.Documents.Search<Hotel>("Atlanta", parameters);
        WriteDocuments(results);

        // Query 2
        Console.WriteLine("Query 2: Search on the term 'Atlanta', with trimming");
        Console.WriteLine("Returning only these fields: HotelName, Tags, Address:\n");
        parameters =
            new SearchParameters()
            {
                Select = new[] { "HotelName", "Tags", "Address" },
            };
        results = indexClient.Documents.Search<Hotel>("Atlanta", parameters);
        WriteDocuments(results);

        // Query 3
        Console.WriteLine("Query 3: Search for the terms 'restaurant' and 'wifi'");
        Console.WriteLine("Return only these fields: HotelName, Description, and Tags:\n");
        parameters =
            new SearchParameters()
            {
                Select = new[] { "HotelName", "Description", "Tags" }
            };
        results = indexClient.Documents.Search<Hotel>("restaurant, wifi", parameters);
        WriteDocuments(results);

        // Query 4 -filtered query
        Console.WriteLine("Query 4: Filter on ratings greater than 4");
        Console.WriteLine("Returning only these fields: HotelName, Rating:\n");
        parameters =
            new SearchParameters()
            {
                Filter = "Rating gt 4",
                Select = new[] { "HotelName", "Rating" }
            };
        results = indexClient.Documents.Search<Hotel>("*", parameters);
        WriteDocuments(results);

        // Query 5 - top 2 results
        Console.WriteLine("Query 5: Search on term 'boutique'");
        Console.WriteLine("Sort by rating in descending order, taking the top two results");
        Console.WriteLine("Returning only these fields: HotelId, HotelName, Category, Rating:\n");
        parameters =
            new SearchParameters()
            {
                OrderBy = new[] { "Rating desc" },
                Select = new[] { "HotelId", "HotelName", "Category", "Rating" },
                Top = 2
            };
        results = indexClient.Documents.Search<Hotel>("boutique", parameters);
        WriteDocuments(results);
    }
    ```

    İki [sorgu eşleşen terimleriyle yolları](search-query-overview.md#types-of-queries): tam metin arama ve filtre. Tam metin arama sorgusu, bir veya daha çok terimi arar `IsSearchable` dizininizdeki alanları. Değerlendirilen bir Boole ifadesi bir filtredir `IsFilterable` dizin alanları. Tam metin arama ve filtreleri birlikte veya ayrı olarak kullanabilirsiniz.

    Arama ve filtrelerin her ikisi de `Documents.Search` yöntemi kullanılarak gerçekleştirilir. Bir arama sorgusu `searchText` parametresinde geçirilebilirken bir filtre ifadesi `SearchParameters` sınıfının `Filter` özelliğinden geçirilebilir. Arama yapmadan filtrelemek üzere `searchText` parametresi için `"*"` geçirmeniz yeterlidir. Filtrelemeden arama yapmak için `Filter` özelliğini ayarlamadan bırakmanız veya bir `SearchParameters` örneği geçirmemeniz yeterlidir.

1. Program.CS'de, ana, "3 - arama" satırları işletilir satırlara çevir. 

    ```csharp
    // Uncomment next 2 lines in "3 - Search an index"
    Console.WriteLine("{0}", "Searching documents...\n");
    RunQueries(indexClient);
    ```
1. Çözüm artık tamamlandı. Uygulamayı yeniden oluşturmanız ve tamamen programı çalıştırmak için F5 tuşuna basın. 

    Çıktı aynı ileti olarak daha önce sorgu bilgileri ve sonuçları ile içerir.

## <a name="clean-up"></a>Temizleme

Bir dizin ile işiniz bittiğinde ve bunu silmek istediğinizde, çağrı `Indexes.Delete` yöntemi, `SearchServiceClient`.

```csharp
serviceClient.Indexes.Delete("hotels");
```

Ayrıca arama hizmeti ile işiniz bittiğinde, Azure portalından kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu C# hızlı, dizin oluşturma, belgelerle yüklemek ve sorguları çalıştırmak için görevleri bir dizi aracılığıyla çalıştı. Farklı aşamalarında okunabilirlik ve içeriği kavrama için kodu basitleştirmek için kısayollar attık. Temel kavramları kullanabiliyorsanız, inceleme için sonraki makaleye alternatif yaklaşımlar ve bilginiz güçlendirmenizi kavramları öneririz. 

Örnek kod ve dizin bu biri genişletilmiş sürümleridir. Sonraki örnek odaları koleksiyona ekler, farklı sınıflar ve Eylemler kullanır ve işleme nasıl çalıştığını daha yakından göz alır.

> [!div class="nextstepaction"]
> [. NET'te geliştirme](search-howto-dotnet-sdk.md)