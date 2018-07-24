---
title: Azure Search’te C# eş anlamlıları öğreticisi | Microsoft Docs
description: Bu öğreticide, eş anlamlılar özelliğini Azure Search'teki bir dizine eklersiniz.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 8340c4dc2a855911073905a3aea93e19fc7b520d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990570"
---
# <a name="tutorial-add-synonyms-for-azure-search-in-c"></a>Öğretici: Azure Search’de C# kodunda eş anlamlıları ekleme

Eş anlamlılar, giriş terimine anlam bakımından eşdeğer olan terimlerle eşleşerek bir sorguyu genişletir. Örneğin, "araba" aramasının "otomobil" veya "araç" terimlerini içeren belgelerle eşleşmesini isteyebilirsiniz. 

Azure Search’te, eş anlamlılar eşdeğer terimleri ilişkilendiren *eşleme kuralları* aracılığıyla bir *eş anlamlı eşleminde* tanımlanır. Bu öğretici varolan bir dizine eş anlamlılar ekleme ve kullanma işleminin temel adımlarını kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Eşleme kuralları oluşturup göndererek eş anlamlıları etkinleştirme 
> * Sorgu dizesinde bir eş anlamlı eşlemine başvurma

Birden çok eş anlamlı eşlemi oluşturabilir, bunları bir dizin için kullanılabilen hizmet genelinde kaynak olarak gönderebilir ve alan düzeyinde hangisinin kullanılacağını belirtebilirsiniz. Sorgu zamanında Azure Search, sorguda kullanılan alanlarda belirtilmişse dizinde aramaya ek olarak bir eş anlamlı eşleminde arama yapar.

> [!NOTE]
> Eş anlamlılar en son API ve SDK sürümlerinde (api-version=2017-11-11, SDK sürümü 5.0.0) desteklenir. Şu anda Azure portalı desteği yoktur. Eş anlamlılar için Azure portalı desteği sizin için kullanışlı olacaksa, lütfen [UserVoice](https://feedback.azure.com/forums/263029-azure-search)’te geri bildiriminizi sağlayın

## <a name="prerequisites"></a>Ön koşullar

Öğretici gereksinimleri şunları içerir:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Search hizmeti](search-create-service-portal.md)
* [Microsoft.Azure.Search .NET kitaplığı](https://aka.ms/search-sdk)
* [Bir .NET Uygulamasından Azure Search kullanma](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Genel Bakış

Öncesi ve sonrası sorguları, eş anlamlıların değerini gösterir. Bu öğreticide, sorguları yürüten ve sonuçları bir örnek dizinde döndüren örnek bir uygulama kullanın. Örnek uygulama, iki belgeyle doldurulmuş "oteller" adlı küçük bir dizin oluşturur. Uygulama, dizinde görünmeyen terim ve ifadeleri kullanarak arama sorguları yürütür, eş anlamlılar özelliğini etkinleştirir, ardından aynı aramaları tekrar gerçekleştirir. Aşağıdaki kod genel akışı gösterir.

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

`RunQueriesWithNonExistentTermsInIndex` içinde "beş yıldızlı", "internet" ve "ekonomik VE otel" ile arama sorguları gönderin.
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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini içeren kaynak grubunu silmektir. Kaynak grubunu silerek içindeki her şeyi kalıcı olarak silebilirsiniz. Portalda kaynak grubu adı, Azure Search hizmetinin Genel Bakış sayfasında bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, eşleme kuralları oluşturmak ve göndermek için C# kodunda [Eş anlamlılar REST API'si](https://aka.ms/rgm6rq) ve ardından eş anlamlılar eşlemini dizine çağırma işlemi gösterildi. [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) başvuru belgelerinde daha fazla bilgi bulabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Search’te eş anlamlıları kullanma](search-synonyms.md)
