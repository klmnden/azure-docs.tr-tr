---
title: Azure Search .NET SDK sürüm 1.1 yükseltme | Microsoft Docs
description: Azure Search .NET SDK sürüm 1.1 yükseltme
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: ccefd21e2aa89a2b46129956b3c4417d548cbf32
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31796753"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-11"></a>Azure Search .NET SDK sürüm 1.1 yükseltme

Sürüm 1.0.2-preview kullanıyorsanız, eski veya [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede, sürüm 1.1 kullanmak için uygulamanızı yükseltmenize yardımcı olur.

Örnekler dahil olmak üzere SDK'ın daha genel bir kılavuz için bkz: [bir .NET uygulamasından Azure Search kullanmayı](search-howto-dotnet-sdk.md).

> [!NOTE]
> Sürüm 1.1 yükseltme yaptıktan sonra veya bir sürüm 1.1 ve 2.0-Önizleme (bunlar dahil) arasında zaten kullanıyorsanız, 3 sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 3 yükseltme](search-dotnet-sdk-migration.md) yönergeler için.
>

İlk olarak, NuGet başvuru için güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya göre proje başvuruları sağ tıklayıp Visual Studio'da "NuGet paketleri...'ı Yönet" seçerek.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden derleyin.

Daha önce sürüm 1.0.0-preview, 1.0.1-preview veya 1.0.2-preview kullanıyorsanız, derleme başarısız olması ve başlamaya hazırsınız!

Daha önce sürüm 0.13.0-preview kullanıyorsanız veya daha eski, görmeniz gerekir derleme hataları aşağıdaki gibi:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Tek tek derleme hataları düzeltmek için sonraki adım olacaktır. Çoğu SDK'ın yeniden adlandırıldığı bazı sınıfı ve yöntemi adlarını değiştirme gerektirir. [Yeni sürüm 1.1 değişiklikler listesi](#ListOfChangesV1) adı değişikliklerin listesini içerir.

Belgelerinizi modellemek için özel sınıflar kullanıyorsanız ve bu sınıfların null ilkel türler özelliklerini sahip (örneğin, `int` veya `bool` C#), hangisinin olmanız gerekir kullanan SDK 1.1 sürümünde hata düzeltmesi yoktur. Bkz: [hata düzeltmeleri sürüm 1.1](#BugFixesV1) daha fazla ayrıntı için.

Derleme hataları düzelttik sonra son olarak, isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Yeni sürüm 1.1 değişiklikler listesi
Aşağıdaki listede değişiklik uygulama kodunuz etkileyecek olasılığını göre sıralanır.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch ve IndexAction değişiklikleri
`IndexBatch.Create` adlandırıldı `IndexBatch.New` ve artık sahip bir `params` bağımsız değişkeni. Kullanabileceğiniz `IndexBatch.New` Eylemler (birleştirme, silme, vb.) farklı türlerde karışık toplu işlemler için. Ayrıca, toplu işleri oluşturmak için yeni statik yöntemler vardır tüm eylemleri nerede aynı: `Delete`, `Merge`, `MergeOrUpload`, ve `Upload`.

`IndexAction` Genel oluşturucular artık sahiptir ve özelliklerini şimdi değişmez. Farklı amaçlar için Eylemler oluşturmak için yeni statik yöntemler kullanmalısınız: `Delete`, `Merge`, `MergeOrUpload`, ve `Upload`. `IndexAction.Create` kaldırılmıştır. Yalnızca bir belge alan aşırı kullandıysanız, kullandığınızdan emin olun `Upload` yerine.

##### <a name="example"></a>Örnek
Kodunuzu şöyle ise:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

İsterseniz, daha fazla için basitleştirin:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException değişiklikleri
`IndexBatchException.IndexResponse` Özelliği adlandırılmıştır `IndexingResults`, ve türünü şimdi `IList<IndexingResult>`.

##### <a name="example"></a>Örnek
Kodunuzu şöyle ise:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>Operasyon yöntemi değişiklikleri
Azure Search .NET SDK'sı her işlem için zaman uyumlu ve zaman uyumsuz arayanlar yöntemi aşırı bir dizi açıktır. İmzalar ve bu yöntem aşırı Finansman sürüm 1.1 değişti.

Örneğin, "Dizin istatistikleri Al" işlemi SDK'ın eski sürümlerinde bu imzaları kullanıma sunulan:

İçinde `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

İçinde `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Aynı işlem içinde sürüm 1.1 yöntemi imzaları şöyle görünür:

İçinde `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

İçinde `IndexesOperationsExtensions`:

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

Sürüm 1.1 ile başlayarak, Azure Search .NET SDK'sı farklı işlemi yöntemleri düzenler:

* Parametreleri bunun yerine varsayılan olarak, isteğe bağlı parametreler Modellenen artık ek yöntemi aşırı daha. Bu yöntem aşırı yüklemeleri, bazen önemli ölçüde azaltır.
* Genişletme yöntemleri şimdi ayrıntılarını yabancı HTTP çağrıyı yapandan çok gizleyin. Örneğin, SDK'ın eski sürümleri işlemi yöntemleri throw çünkü denetlemek için genellikle ihtiyacım kalmadı bir HTTP durum kodu ile bir yanıt nesnesi döndürülen `CloudException` bir hata gösterir herhangi bir durum kodu için. Yeni Uzantı yöntemleri, yalnızca model nesneleri, kodunuzda açılacak sahip olmanın zahmetinden kurtarır döndürür.
* Buna karşılık, çekirdek artık ihtiyacınız olursa, HTTP düzeyinde daha fazla denetime sunmaya yöntemleri arabirimleri. Şimdi istekleri ve yeni dahil edilecek özel HTTP üst bilgilerinde geçirebilirsiniz `AzureOperationResponse<T>` dönüş türü, doğrudan erişim verir `HttpRequestMessage` ve `HttpResponseMessage` işlemi için. `AzureOperationResponse` tanımlanan `Microsoft.Rest.Azure` ad alanı ve değiştirir `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters değişiklikleri
Adlı yeni bir sınıf `ScoringParameter` bir arama sorgusu profilleri Puanlama için parametreleri sağlayın kolaylaştırmak için en son SDK eklendi. Daha önce `ScoringProfiles` özelliği `SearchParameters` sınıfı olarak yazıldığından `IList<string>`; Olarak yazılan artık `IList<ScoringParameter>`.

##### <a name="example"></a>Örnek
Kodunuzu şöyle ise:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Model sınıfı değişiklikleri
Açıklanan imza değişiklikler nedeniyle [işlemi yöntemi değişiklikleri](#OperationMethodChanges), birçok sınıflarda `Microsoft.Azure.Search.Models` ad alanı yeniden adlandırılmış veya kaldırılmış. Örneğin:

* `IndexDefinitionResponse` ile değiştirilmiştir `AzureOperationResponse<Index>`
* `DocumentSearchResponse`, `DocumentSearchResult` olarak yeniden adlandırıldı
* `IndexResult`, `IndexingResult` olarak yeniden adlandırıldı
* `Documents.Count()` Şimdi döndüren bir `long` ile belge sayısı yerine bir `DocumentCountResponse`
* `IndexGetStatisticsResponse`, `IndexGetStatisticsResult` olarak yeniden adlandırıldı
* `IndexListResponse`, `IndexListResult` olarak yeniden adlandırıldı

Özetlemek için `OperationResponse`-türetilmiş sarmalamak için bir model nesnesi kaldırılmış yalnızca var olan sınıfları. Kalan sınıfları değiştirildi kendi soneki beklendiğinden `Response` için `Result`.

##### <a name="example"></a>Örnek
Kodunuzu şöyle ise:

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

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

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

##### <a name="response-classes-and-ienumerable"></a>Yanıt sınıfları ve IEnumerable
Koleksiyonları tutun yanıt sınıfları artık uygulama kodunuz etkileyebilecek ek bir değişiklik olduğunu `IEnumerable<T>`. Bunun yerine, koleksiyon özelliği doğrudan erişebilirsiniz. Örneğin, kod aşağıdaki gibidir:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Web uygulamaları için özel durum
Serileştiren bir web uygulaması varsa `DocumentSearchResponse` doğrudan arama sonuçları tarayıcıya göndermek için kodunu değiştirmeniz gerekir veya sonuçları doğru seri değildir. Örneğin, kod aşağıdaki gibidir:

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

Alarak değiştirebilirsiniz `.Results` arama sonucu işleme düzeltmek için arama yanıtının özelliği:

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

Böyle durumlarda, kodunuzda kendiniz Ara gerekir; **Derleyici sizi uyarır değil** çünkü `JsonResult.Data` türü `object`.

#### <a name="cloudexception-changes"></a>CloudException değişiklikleri
`CloudException` Sınıfı taşınmış `Hyak.Common` ad alanına `Microsoft.Rest.Azure` ad alanı. Ayrıca, kendi `Error` özelliği adlandırılmıştır `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient ve Searchındexclient değişiklikleri
Türü `Credentials` özelliği değiştiğini `SearchCredentials` için temel sınıfı olan `ServiceClientCredentials`. Erişmeniz gerekiyorsa `SearchCredentials` , bir `SearchIndexClient` veya `SearchServiceClient`, Lütfen yeni kullanın `SearchCredentials` özelliği.

SDK ' nın eski sürümlerinde `SearchServiceClient` ve `SearchIndexClient` sürdü oluşturucular sahip bir `HttpClient` parametresi. Bu alan Oluşturucuları ile değiştirilmiştir bir `HttpClientHandler` ve bir dizi `DelegatingHandler` nesneleri. Bu, HTTP isteklerini gerekirse önceden işlemek için özel işleyicileri yüklemek kolaylaştırır.

Son olarak, sürdü oluşturucular bir `Uri` ve `SearchCredentials` değiştirilmiştir. Örneğin, şöyle bir kodunuz varsa:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ayrıca kimlik bilgileri parametresinin türü için değiştirildiğine dikkat edin `ServiceClientCredentials`. Bu yana kodunuzu etkilemek düşüktür `SearchCredentials` türetildiği `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>İstek Kimliği geçirme
SDK eski sürümleri üzerinde bir istek kimliği ayarlayabilirsiniz `SearchServiceClient` veya `SearchIndexClient` ve her istek için REST API dahil. Bu, destek ile iletişime geçmeniz gerekiyorsa arama hizmetinizi sorunlarını gidermek için kullanışlıdır. Bununla birlikte, her işlem için bir benzersiz istek kimliği ayarlamak için yerine tüm işlemler için aynı kimlik kullanmak için daha yararlı olur. Bu nedenle, `SetClientRequestId` yöntemlerinin `SearchServiceClient` ve `SearchIndexClient` kaldırıldı. Bunun yerine, bir istek kimliği her işlemi yöntemi isteğe bağlı geçirebilirsiniz `SearchRequestOptions` parametresi.

> [!NOTE]
> Gelecekteki bir SDK sürümünde, diğer Azure SDK tarafından kullanılan bir yaklaşım ile tutarlı olan bir istek kimliği nesneler genel istemcide ayarlamak için yeni bir yöntem ekleyeceğiz.
> 
> 

#### <a name="example"></a>Örnek
Şuna benzer bir kod varsa:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Arabirim adı değişikliği
İşlem grubu arabirimi adları tüm karşılık gelen özellik adları ile tutarlı olacak şekilde değiştirilmiştir:

* Türü `ISearchServiceClient.Indexes` adlandırılmıştır `IIndexOperations` için `IIndexesOperations`.
* Türü `ISearchServiceClient.Indexers` adlandırılmıştır `IIndexerOperations` için `IIndexersOperations`.
* Türü `ISearchServiceClient.DataSources` adlandırılmıştır `IDataSourceOperations` için `IDataSourcesOperations`.
* Türü `ISearchIndexClient.Documents` adlandırılmıştır `IDocumentOperations` için `IDocumentsOperations`.

Bu değişiklik, test amacıyla bu arabirimleri mocks oluşturduğunuz sürece kodunuzu etkilemek olası değil.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Hata düzeltmeleri sürüm 1.1
Eski sürümleri, Azure Search .NET SDK özel model sınıfları serileştirmek için ilgili bir hata vardı. Bir null değer türünde bir özellik özel model sınıfı oluşturduysanız, bu hata oluşabilir.

#### <a name="steps-to-reproduce"></a>Yeniden oluşturma adımları
Bir özel model sınıfı null değer türünde bir özellik oluşturun. Örneğin, bir ortak ekleyin `UnitCount` türündeki özelliği `int` yerine `int?`.

Varsayılan değer bu tür bir belgeyle sıralıyorsanız (örneğin, 0 `int`), alanın Azure Search'te null olur. Daha sonra bu belge için arama yaparsanız `Search` çağrısı throw `JsonSerializationException` , onu dönüştürülemiyor şikayetçi `null` için `int`.

Ayrıca, filtreleri null amaçlanan değer yerine dizine yazıldı beri beklendiği gibi çalışmayabilir.

#### <a name="fix-details"></a>Ayrıntılar Düzelt
Sürüm SDK 1.1 Biz bu sorunu düzelttik. Şimdi, böyle bir model sınıfı varsa:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

ve ayarladığınız `IntValue` 0 olarak bu değer şimdi doğru hattaki 0 olarak serileştirilmiş ve dizindeki 0 olarak depolanır. Yuvarlak tripping de beklendiği gibi çalışır.

Bu yaklaşımda farkında olması için bir olası sorun: bir model türü ile atanamayan bir özellik kullanırsanız, için sahip **garanti** hiçbir belgeleri dizininize karşılık gelen alan için bir null değer içeriyor. Bunu zorlamanıza ne SDK ne de Azure Search REST API'sini yardımcı olur.

Bu yalnızca kuramsal bir sorun değildir: Var olan `Edm.Int32` türünde bir dizine yeni bir alan eklediğiniz bir senaryoyu düşünün. Dizin tanımını güncelleştirdikten sonra, tüm belgelerin bu yeni alan için boş bir değeri olur (bunun nedeni, Azure Search'te tüm türlerin boş değer atanabilir olmasıdır). Ardından bu alan için boş değer atanamayan bir `int` özelliğiyle bir model sınıfı kullanırsanız belgeleri almaya çalışırken bunun gibi bir `JsonSerializationException` alırsınız:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Bu nedenle, hala bir en iyi uygulama olarak model sınıflarınızda boş değer atanabilir türler kullanmanızı öneririz.

Bu hatayı ve düzeltme hakkında daha fazla ayrıntı için lütfen bkz. [github'daki bu sorunu](https://github.com/Azure/azure-sdk-for-net/issues/1063).

