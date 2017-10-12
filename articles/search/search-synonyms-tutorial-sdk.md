---
title: "Azure Search’te eş anlamlıları önizleme öğreticisi | Microsoft Docs"
description: "Eş anlamlıları önizleme özelliğini Azure Search'teki bir dizine ekleyin."
services: search
manager: jhubbard
documentationcenter: 
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 03/31/2017
ms.author: heidist
ms.openlocfilehash: 014959ed471f796d2184f0f8ff10d15cdc8a2ec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="synonym-preview-c-tutorial-for-azure-search"></a>Azure Search için eş anlamlı (önizleme) C# öğreticisi

Eş anlamlılar, giriş terimine anlam bakımından eşdeğer olan terimlerle eşleşerek bir sorguyu genişletir. Örneğin, "araba" aramasının "otomobil" veya "araç" terimlerini içeren belgelerle eşleşmesini isteyebilirsiniz.

Azure Search’te, eş anlamlılar eşdeğer terimleri ilişkilendiren *eşleme kuralları* aracılığıyla bir *eş anlamlı eşleminde* tanımlanır. Birden çok eş anlamlı eşlemi oluşturabilir, bunları bir dizin için kullanılabilen hizmet genelinde kaynak olarak gönderebilir ve alan düzeyinde hangisinin kullanılacağını belirtebilirsiniz. Sorgu zamanında Azure Search, sorguda kullanılan alanlarda belirtilmişse dizinde aramaya ek olarak bir eş anlamlı eşleminde arama yapar.

> [!NOTE]
> Eş anlamlılar özelliği şu anda önizleme aşamasındadır ve yalnızca en son önizleme API ve SDK sürümlerinde desteklenir (api-version=2016-09-01-Preview, SDK sürüm 4.x-preview). Şu anda Azure portalı desteği yoktur. Önizleme API’leri SLA kapsamında değildir ve önizleme özellikleri değişebilir; bu nedenle, üretim uygulamalarında kullanılması önerilmez.

## <a name="prerequisites"></a>Ön koşullar

Öğretici gereksinimleri şunları içerir:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Search hizmeti](search-create-service-portal.md)
* [Microsoft.Azure.Search .NET kitaplığının önizleme sürümü](https://aka.ms/search-sdk-preview)
* [Bir .NET Uygulamasından Azure Search kullanma](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Genel Bakış

Öncesi ve sonrası sorguları, eş anlamlıların değerini gösterir. Bu öğreticide, sorguları yürüten ve sonuçları bir örnek dizinde döndüren örnek bir uygulama kullanılır. Örnek uygulama, iki belgeyle doldurulmuş "oteller" adlı küçük bir dizin oluşturur. Uygulama, dizinde görünmeyen terim ve ifadeleri kullanarak arama sorguları yürütür, eş anlamlılar özelliğini etkinleştirir, ardından aynı aramaları tekrar gerçekleştirir. Aşağıdaki kod genel akışı gösterir.

```csharp
  static void Main(string[] args)
  {
      SearchServiceClient serviceClient = CreateSearchServiceClient();

      Console.WriteLine("{0}", "Cleaning up resources...\n");
      CleanupResources(serviceClient);

      Console.WriteLine("{0}", "Creating index...\n");
      CreateHotelsIndex(serviceClient);

      ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

      Console.WriteLine("{0}", "Uploading documents...\n");
      UploadDocuments(indexClient);

      ISearchIndexClient indexClientForQueries = CreateSearchIndexClient();

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Adding synonyms...\n");
      UploadSynonyms(serviceClient);
      EnableSynonymsInHotelsIndex(serviceClient);
      Thread.Sleep(10000); // Wait for the changes to propagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");

      Console.ReadKey();
  }
```
Örnek dizini oluşturma ve doldurma adımları [Bir .NET Uygulamasından Azure Search kullanma](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk) bölümünde açıklanmıştır.

## <a name="before-queries"></a>"Öncesi" sorguları

`RunQueriesWithNonExistentTermsInIndex` içinde "beş yıldızlı", "internet" ve "ekonomik VE otel" ile arama sorguları gönderdik.
```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
Dizini oluşturulan iki belgenin hiçbiri terimleri içermediği için, ilk `RunQueriesWithNonExistentTermsInIndex` için aşağıdaki çıkış alınır.
~~~
Search the entire index for the phrase "five star":

no document matched

Search the entire index for the term 'internet':

no document matched

Search the entire index for the terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Eş anlamlıları etkinleştirme

Eş anlamlıların etkinleştirilmesi iki adımlı bir işlemdir. İlk olarak eş anlamlı kuralları tanımlanıp karşıya yüklenir, ardından bunları kullanacak alanlar yapılandırılır. İşlem, `UploadSynonyms` ve `EnableSynonymsInHotelsIndex` içinde ana hatlarıyla açıklanmıştır.

1. Arama hizmetinize bir eş anlamlı eşlemi ekleyin. `UploadSynonyms` içinde, 'desc-synonymmap' adlı eş anlamlı eşleminde dört kural tanımlanır ve hizmete yüklenir.
```csharp
    var synonymMap = new SynonymMap()
    {
        Name = "desc-synonymmap",
        Format = "solr",
        Synonyms = "hotel, motel\n
                    internet,wifi\n
                    five star=>luxury\n
                    economy,inexpensive=>budget"
    };

    serviceClient.SynonymMaps.CreateOrUpdate(synonymMap);
```
Bir eş anlamlı eşlemi, açık kaynak standart `solr` biçimine uygun olmalıdır. Biçim, `Apache Solr synonym format` bölümündeki [Azure Search’te Eş Anlamlılar](search-synonyms.md) içinde açıklanmıştır.

2. Dizin tanımında eş anlamlı eşlemini kullanacak aranabilir alanları yapılandırın. `EnableSynonymsInHotelsIndex` içinde, `synonymMaps` özelliği yeni yüklenen eş anlamlı eşleminin adına ayarlanarak `category` ve `tags` alanlarında eş anlamlılar etkinleştirilir.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
Bir eş anlamlı eşlemi eklediğinizde, dizinin yeniden oluşturulması gerekmez. Hizmetinize bir eş anlamlı eşlemi ekleyebilir ve sonra herhangi bir dizindeki mevcut alan tanımlarını yeni eş anlamlı eşlemini kullanacak şekilde değiştirebilirsiniz. Yeni özniteliklerin eklenmesi, dizin kullanılabilirliğini etkilemez. Aynı durum, bir alan için eş anlamlıları devre dışı bırakırken de geçerlidir. `synonymMaps` özelliğini boş bir listeye ayarlayabilirsiniz.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>"Sonrası" sorguları

Eş anlamlı eşlemi karşıya yüklendikten ve dizin, eş anlamlı eşlemini kullanacak şekilde güncelleştirildikten sonra, ikinci `RunQueriesWithNonExistentTermsInIndex` çağrısı aşağıdaki çıkışı verir:

~~~
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
İlk sorgu, `five star=>luxury` kuralından belgeyi bulur. İkinci sorgu, `internet,wifi` kullanarak aramayı genişletir, üçüncü sorgu ise eşleştikleri belgeleri bulmak için `hotel, motel` ve `economy,inexpensive=>budget` kullanır.

Eş anlamlıların eklenmesi, arama deneyimini tamamen değiştirir. Bu öğreticide, dizinimizdeki belgelerin alakalı olmasına rağmen ilk sorgular anlamlı sonuçlar döndürememiştir. Eş anlamlıları etkinleştirerek, dizinde temel alınan verileri değiştirmeden dizini yaygın olarak kullanılan terimleri içerecek şekilde genişletebiliriz.

## <a name="sample-application-source-code"></a>Örnek uygulama kaynak kodu
Bu kılavuzda kullanılan örnek uygulamanın tam kaynak kodunu [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) üzerinde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Search’te eş anlamlıları kullanma](search-synonyms.md) bölümünü gözden geçirin
* [Eş anlamlılar REST API belgelerini](https://aka.ms/rgm6rq) gözden geçirin
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvurularına göz atın.
