---
title: Kaynaklar - Azure Search için eş zamanlı yazma yönetme
description: Güncelleştirme ve silme için Azure Search dizinlerini, dizin oluşturucular veri kaynakları üzerinde Orta hava çarpışmalardan kaçınmak için iyimser eşzamanlılığı kullanın.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/21/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4599498918b7a01a1207f20135c26924c6758eb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871374"
---
# <a name="how-to-manage-concurrency-in-azure-search"></a>Azure Search'te eşzamanlılığı yönetme

Özellikle kaynakları, uygulamanızın farklı bileşenler tarafından eşzamanlı olarak erişilen, dizinleri ve veri kaynakları gibi Azure Search kaynaklarını yönetirken, kaynaklara güvenli bir şekilde, güncelleştirmek önemlidir. İki istemcisi eşzamanlı olarak koordinasyon olmadan bir kaynak güncelleştirdiğinizde, yarış durumlarını mümkündür. Bunu önlemek için Azure Search sunan bir *iyimser eşzamanlılık model*. Bir kaynakta kilit yok. Bunun yerine, kaynağın sürümünü tanımlayan ve böylece yanlışlıkla önlemek istekler oluşturabileceği her kaynak üzerine yazar için ETag yoktur.

> [!Tip]
> Kavramsal kodda bir [örnek C# çözüm](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) eşzamanlılık denetimi Azure arama'yı nasıl çalıştığı açıklanmaktadır. Eşzamanlılık denetimi çağırma koşullar kod oluşturur. Okuma [aşağıdaki kod parçası](#samplecode) çalıştırmak istediğiniz ancak çoğu geliştirici, hizmet adını ve yönetici api anahtarı eklemek için appsettings.json düzenlemek için büyük olasılıkla yeterlidir. Bir hizmet URL'sini verilen `http://myservice.search.windows.net`, hizmet adı `myservice`.

## <a name="how-it-works"></a>Nasıl çalışır?

İyimser eşzamanlılık uygulanan Erişim koşul dizinler, dizin oluşturucular, veri kaynakları ve synonymMap kaynaklara yazma API çağrıları olarak denetler.

Tüm kaynaklara sahip bir [ *varlık etiketi (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) nesne sürüm bilgilerini sağlar. ETag ilk denetleyerek, tipik bir iş akışında eş zamanlı güncelleştirmelerin kaçınabilirsiniz (Al, yerel olarak değiştirmek, güncelleştirme) kaynağın ETag eşleşen yerel kopyanızı sağlayarak.

+ REST API kullanan bir [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) isteği başlığı.
+ .NET SDK'sını ayarlama accessCondition nesneyi aracılığıyla ETag ayarlar [IF-Match | IF-Match-hiçbiri üstbilgi](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) kaynağı. Öğesinden devralan herhangi bir nesne [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) accessCondition nesnesi vardır.

Dosyanın etag değeri, bir kaynak her güncelleştirdiğinizde otomatik olarak değiştirir. Eşzamanlılık yönetimini uyguladığınızda, tüm yaptığınız sokarak önkoşulu güncelleştirme isteğinde uzak kaynak istemcide değiştirilen kaynak kopyası olarak aynı ETag olmasını gerektirir. Eşzamanlı bir işlem zaten uzak kaynak değiştirilmesi durumunda ETag önkoşulu eşleşmez ve İstek HTTP 412 ile başarısız olur. .NET SDK'sı kullanıyorsanız olarak bu bildirimleri bir `CloudException` burada `IsAccessConditionFailed()` genişletme yöntemi true döndürür.

> [!Note]
> Tek bir eşzamanlılık mekanizması yoktur. Kaynak güncelleştirmeleri için kullanılan API bağımsız olarak her zaman kullanılır.

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Kullanım örnekleri ve örnek kod

Aşağıdaki kod, anahtar güncelleştirme işlemlerinde accessCondition denetler gösterir:

+ Kaynak artık mevcut değilse, bir güncelleştirme başarısız
+ Kaynak sürümü değişirse bir güncelleştirme başarısız

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a>Örnek koddan [DotNetETagsExplainer programı](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)

```
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Search has an associated ETag that keeps track of which version
            // of the resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once the resource exists in Azure Search, its ETag will be populated. Make sure to use the object
            // returned by the SearchServiceClient! Otherwise, you will still have the old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that the update will only
            // succeed if the index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates to the same resource. If another
            // client tries to update the resource, it will fail as long as all clients are using the right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. To start, both clients see the same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates the index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries to update the index, but fails, thanks to the ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed to update the index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of the DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using the Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // the resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a>Tasarım deseni

Uygulamak için bir tasarım deseni iyimser eşzamanlılık erişim koşulu yeniden deneme bir döngü içermelidir, test erişim koşulu için kontrol edin ve isteğe bağlı olarak, değişiklikleri yeniden uygulayın çalışmadan önce güncelleştirilmiş bir kaynak alır.

Bu kod parçacığı bir synonymMap zaten bir dizine eklenmesi gösterilmektedir. Bu kod dandır [eş anlamlı (Önizleme) C# Azure Search örneğin](search-synonyms-tutorial-sdk.md).

Kod parçacığı, "hotels" dizinini alır, bir güncelleştirme işlemi nesne sürümünde denetler, koşul başarısız olursa, bir özel durum oluşturur ve en son sürümünü almak için sunucu dizini alma başlayarak bu işlemi (en fazla üç kez), deneme.

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // The IfNotChanged condition ensures that the index is updated only if the ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated the index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }


## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [eş anlamlılar C# örnek](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) varolan bir dizini güvenli bir şekilde güncelleştirmek hakkında daha fazla bağlam için.

Etag'ler veya AccessCondition nesneleri eklemek için aşağıdaki örnekleri birini değiştirmeyi deneyin.

+ [Github'daki örnek REST API](https://github.com/Azure-Samples/search-rest-api-getting-started)
+ [.NET SDK'sı örneği github'daki](https://github.com/Azure-Samples/search-dotnet-getting-started). Bu çözüm, bu makalede gösterilen kodu içeren "DotNetEtagsExplainer" proje içerir.

## <a name="see-also"></a>Ayrıca bkz.

[Ortak HTTP istek ve yanıt üst bilgilerini](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)
[HTTP durum kodları](https://docs.microsoft.com/rest/api/searchservice/http-status-codes)
[dizin işlemler (REST API'si)](https://docs.microsoft.com/rest/api/searchservice/index-operations)
