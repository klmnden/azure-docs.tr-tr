---
title: Azure Search .NET SDK sürüm 1.1 - Azure Search yükseltme
description: Azure Search .NET SDK sürüm 1.1 eski API sürümlerinden geçiş kodu. Nelerin yeni olduğunu öğrenin ve hangi kodda değişiklik yapmanız gerekmez.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 3f47656bb13d08ea56cf25a2a29897722abb1cdb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024161"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-11"></a>Azure Search .NET SDK sürüm 1.1 yükseltme

> [!Important]
> Bu içerik yine de tamamlanmamıştır. Azure Search .NET SDK, sürüm 9.0, NuGet üzerinde kullanılabilir. Bu makaledeki 9.0 için yükseltme açıklamak için çalışıyoruz. 
> 

Sürüm 1.0.2-preview kullanıyorsanız, eski veya [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede sürüm 1.1 kullanmak için uygulamanızı yükseltmenize yardımcı olur.

Örnekler de dahil olmak üzere SDK'sının daha genel bir kılavuz için bkz. [bir .NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

> [!NOTE]
> 1.1 sürümüne yükselttikten sonra veya bir sürüm 1.1 ve 2.0-preview kapsamlı arasında zaten kullanıyorsanız, 3. sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 3 yükseltme](search-dotnet-sdk-migration.md) yönergeler için.
>

İlk olarak, için NuGet başvuru güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya ile proje başvurularınızın sağ tıklayıp "Yönetme NuGet paketleri..." Visual Studio'da.

NuGet yeni paketler ve bağımlılıkları İndirildikten sonra projenizi yeniden derleyin.

Daha önce sürüm 1.0.0-preview, 1.0.1-preview veya 1.0.2-preview kullandıysanız, derlemenin başarılı olması gerekir ve başlamaya hazırsınız!

Sürüm 0.13.0-preview daha önce kullandıysanız veya daha eski görmelisiniz hataları aşağıdaki gibi oluşturun:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Sonraki adım, tek tek yapı hatalarını düzeltin sağlamaktır. SDK'yı yeniden adlandırıldığı bazı sınıf ve yöntem adlarını değiştirmek en gerektirir. [Sürüm 1.1 çarpıcı değişikliklerin dinamik listesi](#ListOfChangesV1) adı değişikliklerin bir listesini içerir.

Özel sınıflar belgelerinizi model oluşturmak için kullandığınız ve bu sınıflar atanamayan temel türlerin özelliklerine sahip (örneğin, `int` veya `bool` içinde C#), hangi olmanız gerekir kullanan SDK 1.1 sürümünde hata düzeltmesi yoktur. Bkz: [hata düzeltmeleri sürüm 1.1](#BugFixesV1) daha fazla ayrıntı için.

Derleme hataları düzelttik sonra son olarak, uygulamanızı isterseniz yeni işlevsellikten yararlanmak için değişiklik yapabilirsiniz.

<a name="ListOfChangesV1"></a>

## <a name="list-of-breaking-changes-in-version-11"></a>Sürüm 1.1 çarpıcı değişikliklerin dinamik listesi
Aşağıdaki listede, değişiklik uygulama kodunuzun etkileyecektir olasılığına göre sıralanır.

### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch ve IndexAction değişiklikleri
`IndexBatch.Create` yeniden adlandırıldı `IndexBatch.New` ve artık bir `params` bağımsız değişken. Kullanabileceğiniz `IndexBatch.New` eylemleri (birleştirme, silme, vb.) farklı türlerini karıştırmak toplu işlemler için. Ayrıca, toplu işleri oluşturmak için yeni statik yöntem vardır tüm eylemleri olduğu aynı: `Delete`, `Merge`, `MergeOrUpload`, ve `Upload`.

`IndexAction` artık genel oluşturucular sahiptir ve özelliklerini artık sabittir. Farklı amaçlara yönelik eylem oluşturmak için yeni statik yöntemler kullanmanız gerekir: `Delete`, `Merge`, `MergeOrUpload`, ve `Upload`. `IndexAction.Create` kaldırıldı. Yalnızca bir belge alan aşırı yüklemesini kullandıysanız, kullandığınızdan emin olun `Upload` yerine.

#### <a name="example"></a>Örnek
Kodunuzu gibi görünüyorsa:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

İsterseniz, daha fazla, bu basitleştirebilirsiniz:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

### <a name="indexbatchexception-changes"></a>IndexBatchException değişiklikleri
`IndexBatchException.IndexResponse` Özelliği adlandırıldı `IndexingResults`, ve onun türü artık `IList<IndexingResult>`.

#### <a name="example"></a>Örnek
Kodunuzu gibi görünüyorsa:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

### <a name="operation-method-changes"></a>İşlem yöntemi değişiklikleri
Azure Search .NET SDK'sı her işlem için zaman uyumlu ve zaman uyumsuz çağıranlar yöntemi aşırı bir dizi kullanıma sunulur. Sürüm 1.1 imzalar ve bu yöntem aşırı yüklemeleri hesaba katacak şekilde değişti.

Örneğin, "Dizin istatistiklerini alma" işlemi SDK'ın eski sürümlerinde bu imzalar exposed:

`IIndexOperations` içinde:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

`IndexOperationsExtensions` içinde:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Yöntem imzaları için aynı işlem içinde sürüm 1.1 şöyle görünür:

`IIndexesOperations` içinde:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

`IndexesOperationsExtensions` içinde:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

1.1 sürümünden itibaren Azure Search .NET SDK'sı farklı işlem yöntemlerine düzenler:

* Parametreler yerine varsayılan olarak, isteğe bağlı parametreler modellenmiş artık ek bir yöntem aşırı yüklemeleri daha. Bu yöntem aşırı sayıda bazen önemli ölçüde azaltır.
* Genişletme yöntemleri, şimdi çok miktarda yabancı HTTP ayrıntılarını arayandan gizle. Örneğin, SDK'sının daha eski sürümleri işlem yöntemlerine çünkü denetlemek için genellikle ihtiyacım kalmadı bir HTTP durum kodu ile bir yanıt nesnesi döndürülen `CloudException` için hata veren herhangi bir durum kodu. Yeni Uzantı yöntemleri yalnızca bunları kodunuzda sarmalamadan çıkarma gerek kalmadan zahmetinden model nesneleri döndürür.
* Buna karşılık, çekirdek artık ihtiyacınız olursa, HTTP düzeyinde daha fazla denetim verir sunmaya yöntemleri arabirimleri. İstekleri ve yeni dahil edilecek özel HTTP üstbilgileri artık geçirebilirsiniz `AzureOperationResponse<T>` dönüş türü, doğrudan erişimi verir `HttpRequestMessage` ve `HttpResponseMessage` işlemi için. `AzureOperationResponse` tanımlanan `Microsoft.Rest.Azure` ad alanı ve değiştirir `Hyak.Common.OperationResponse`.

### <a name="scoringparameters-changes"></a>ScoringParameters değişiklikleri
Adlı yeni bir sınıf `ScoringParameter` Puanlama profilleri arama sorgusu için parametreleri sağlamak daha kolay hale getirmek için en son SDK eklendi. Daha önce `ScoringProfiles` özelliği `SearchParameters` sınıfı olarak yazıldığından `IList<string>`; Artık olarak yazılan `IList<ScoringParameter>`.

#### <a name="example"></a>Örnek
Kodunuzu gibi görünüyorsa:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

### <a name="model-class-changes"></a>Model sınıfı değişiklikleri
Açıklanan imza değişiklikleri nedeniyle [işlemi yöntemi değişiklikleri](#OperationMethodChanges), birçok sınıflarda `Microsoft.Azure.Search.Models` ad alanı olarak yeniden adlandırıldı veya kaldırıldı. Örneğin:

* `IndexDefinitionResponse` ile değiştirilmiştir `AzureOperationResponse<Index>`
* `DocumentSearchResponse`, `DocumentSearchResult` olarak yeniden adlandırıldı
* `IndexResult`, `IndexingResult` olarak yeniden adlandırıldı
* `Documents.Count()` artık döndürür bir `long` yerine belge sayısı ile bir `DocumentCountResponse`
* `IndexGetStatisticsResponse`, `IndexGetStatisticsResult` olarak yeniden adlandırıldı
* `IndexListResponse`, `IndexListResult` olarak yeniden adlandırıldı

Özetlemek gerekirse, `OperationResponse`-türetilmiş sınıfları sarmalamak için bir model nesnesi kaldırıldı yalnızca var olan. Kalan sınıfları değiştirildi soneklerine çalıştırılmış `Response` için `Result`.

#### <a name="example"></a>Örnek
Kodunuzu gibi görünüyorsa:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

#### <a name="response-classes-and-ienumerable"></a>Yanıt sınıfları ve IEnumerable
Kodunuzu etkileyebilecek bir ek koleksiyonlar tutar yanıt sınıfları artık uygulama değişikliktir `IEnumerable<T>`. Bunun yerine, koleksiyon özelliği doğrudan erişebilirsiniz. Örneğin, kod şöyle görünür:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

#### <a name="special-case-for-web-applications"></a>Web uygulamaları için özel durum
Serileştiren bir web uygulamanız varsa `DocumentSearchResponse` doğrudan arama sonuçları tarayıcıya göndermek için kodunuzu değiştirmeniz gerekir veya sonuçları doğru serileştirilecek değil. Örneğin, kod şöyle görünür:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Alarak değiştirebilirsiniz `.Results` arama sonucu işleme düzeltmek için arama yanıtın özelliği:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Böyle durumlarda, kodunuzda kendiniz bakmanız gerekir; **Derleyici sizi uyarmaz** çünkü `JsonResult.Data` türünde `object`.

### <a name="cloudexception-changes"></a>CloudException değişiklikleri
`CloudException` Sınıfı öğesinden taşındı `Hyak.Common` ad alanına `Microsoft.Rest.Azure` ad alanı. Ayrıca, kendi `Error` özelliği adlandırıldı `Body`.

### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient ve Searchındexclient değişiklikleri
Türünü `Credentials` özelliği değiştiğinde `SearchCredentials` için temel sınıfı olan `ServiceClientCredentials`. Erişmeniz gerekiyorsa `SearchCredentials` , bir `SearchIndexClient` veya `SearchServiceClient`, Lütfen yeni kullanın `SearchCredentials` özelliği.

SDK ' nın eski sürümlerinde `SearchServiceClient` ve `SearchIndexClient` sürdü oluşturuculara sahip bir `HttpClient` parametresi. Bu alan oluşturucular ile değiştirilmiş bir `HttpClientHandler` ve bir dizi `DelegatingHandler` nesneleri. Bu, önceden gerekiyorsa, HTTP isteklerini işlemek için özel işleyicileri yüklemek kolaylaştırır.

Son olarak, geçen oluşturucular bir `Uri` ve `SearchCredentials` değiştirilmiştir. Örneğin, şuna benzeyen bir kodunuz varsa:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ayrıca kimlik bilgileri parametresinin türü için değiştirildiğine dikkat edin `ServiceClientCredentials`. Bu, kodunuzun bu yana etkilemek düşüktür `SearchCredentials` türetilir `ServiceClientCredentials`.

### <a name="passing-a-request-id"></a>İstek Kimliği geçirme
SDK'ın eski sürümlerinde, bir istek kimliği üzerinde ayarlayabilirsiniz `SearchServiceClient` veya `SearchIndexClient` ve REST API'sine yapılan her isteği dahil. Bu desteğe ihtiyacınız varsa, arama hizmetinizin sorunlarını gidermek için kullanışlıdır. Ancak, her işlem için bir benzersiz istek Kimliğini ayarlamak için yerine tüm işlemler için aynı kimliği kullanmaya daha kullanışlı olur. Bu nedenle, `SetClientRequestId` yöntemlerinin `SearchServiceClient` ve `SearchIndexClient` kaldırıldı. Bunun yerine, bir istek kimliği her işlem yönteme isteğe bağlı geçirebilirsiniz `SearchRequestOptions` parametresi.

> [!NOTE]
> Gelecekteki bir sürümde SDK'ın, diğer Azure SDK'ları tarafından kullanılan yaklaşım ile tutarlı bir istek kimliği istemcide nesneleri genel olarak ayarlamak için bir yeni mekanizması ekleyeceğiz.
> 
> 

### <a name="example"></a>Örnek
Şuna benzer bir kodunuz varsa:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

### <a name="interface-name-changes"></a>Ad değişiklikleri arabirimi
İşlem grubu arabirimi adları tüm karşılık gelen özellik adlarını ile tutarlı olacak şekilde değiştirilmiştir:

* Türünü `ISearchServiceClient.Indexes` adlandırılmıştır `IIndexOperations` için `IIndexesOperations`.
* Türünü `ISearchServiceClient.Indexers` adlandırılmıştır `IIndexerOperations` için `IIndexersOperations`.
* Türünü `ISearchServiceClient.DataSources` adlandırılmıştır `IDataSourceOperations` için `IDataSourcesOperations`.
* Türünü `ISearchIndexClient.Documents` adlandırılmıştır `IDocumentOperations` için `IDocumentsOperations`.

Bu değişiklik, test amacıyla bu arabirimlerin mocks oluşturduğunuz sürece kodunuzun etkileyen beklenmez.

<a name="BugFixesV1"></a>

## <a name="bug-fixes-in-version-11"></a>1.1 sürüm hata düzeltmeleri
Özel model sınıfların serileştirmek için ilgili Azure Search .NET SDK'ın eski sürümlerinde bir hata oluştu. Bir NULL olmayan bir değer türü özelliği ile özel model sınıfı oluşturduysanız, bu hata oluşabilir.

### <a name="steps-to-reproduce"></a>Yeniden oluşturma adımları
Özel model sınıfı, NULL olmayan değer türünde bir özellik oluşturun. Örneğin, bir genel ekleme `UnitCount` türünün özelliği `int` yerine `int?`.

Varsayılan değer türü sahip bir belge dizini varsa (örneğin, 0 `int`), alan Azure Search'te null olur. Bu belgede, ardından olarak aratırsanız `Search` çağrı oluşturur `JsonSerializationException` , dönüştüremezsiniz şikayetçi `null` için `int`.

Ayrıca, filtreler, dizin amaçlanan değer yerine null yazılmıştır beri beklendiği gibi çalışmayabilir.

### <a name="fix-details"></a>Düzeltme ayrıntıları
SDK 1.1 sürümünde bu sorunu düzelttik. Şimdi, böyle bir model sınıfı varsa:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

ve `IntValue` 0'a değeri artık doğru kablo 0 olarak serileştirilmiş ve dizin 0 olarak depolanır. Yuvarlak tripping ayrıca beklendiği gibi çalışır.

Bu yaklaşımı izleme konusunda dikkat edilmesi gereken olası bir sorunu vardır: Bir model türü NULL olmayan bir özellik kullanırsanız, için sahip **garanti** hiçbir belgeleri dizininize karşılık gelen alan için bir null değer içeriyor. Bunu ne SDK ne de Azure Search REST API'sine yardımcı olur.

Bu yalnızca kuramsal bir sorun değildir: Eklediğiniz yeni alan türü var olan bir dizin için bir senaryoyu düşünün `Edm.Int32`. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, yine de en iyi uygulama model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

Bu hata ve düzeltme hakkında daha fazla ayrıntı için lütfen bkz [github'da Bu sorun](https://github.com/Azure/azure-sdk-for-net/issues/1063).

