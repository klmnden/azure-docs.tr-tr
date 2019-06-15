---
title: Azure Search .NET SDK sürüm 9 - Azure Search yükseltme
description: Azure Search .NET SDK sürüm 9 eski sürümlerinden geçiş kodu. Nelerin yeni olduğunu öğrenin ve hangi kodda değişiklik yapmanız gerekmez.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: a59deed4ac0cec669ddc5e0335f7274586c702e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65541756"
---
# <a name="upgrade-to-the-azure-search-net-sdk-version-9"></a>Azure Search .NET SDK sürüm 9 yükseltme

Önizleme 7.0 ya da'nın eski bir sürümü kullanıyorsanız [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede, uygulamanızı sürüm 9 kullanacak şekilde yükseltin yardımcı olur.

> [!NOTE]
> Sürüm 8.0-preview henüz genel kullanıma açık olmayan özellikleri değerlendirmek için kullanmak istiyorsanız, ayrıca 8.0 Önizleme için önceki sürümlerinden yükseltmek için bu makaledeki yönergeleri izleyebilirsiniz.

Örnekler de dahil olmak üzere SDK'sının daha genel bir kılavuz için bkz. [bir .NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

Azure Search .NET SDK, sürüm 9, daha önceki sürümlerin birçok değişiklik içerir. Bunlardan bazıları değişiklikler ancak nispeten küçük değişiklikler, kodunuzun yalnızca istemeniz gerekir. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

> [!NOTE]
> Önizleme 4.0 veya daha eski bir sürümü kullanıyorsanız, 5 sürümüne yükseltmeniz ve ardından 9 sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 5 yükseltme](search-dotnet-sdk-migration-version-5.md) yönergeler için.
>
> Azure Search Hizmeti örneğinizi en son dahil olmak üzere çeşitli REST API sürümlerini destekler. Artık en son değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirme öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü ilgili REST API sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek üzere yükseltilir bile kod değişikliğine gerek kalmadan çalışmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-9"></a>Sürüm 9 yenilikler nelerdir?
Azure Search .NET SDK'sı 9 sürümünü hedefleyen en son Azure Search REST API'si, özellikle 2019-05-06 genel kullanıma sunulan sürümü. Bu aşağıdakiler de dahil, bir .NET uygulamasından Azure Search'ün yeni özelliklerini kullanmayı mümkün kılar:

* [Bilişsel arama](cognitive-search-concept-intro.md) görüntüleri, BLOB'ları ve diğer yapılandırılmamış veri kaynakları Azure Search dizini daha aranabilir yapmak için içerik zenginleştirilmesi - metin ayıklamak için kullanılan Azure Search, yapay ZEKA özelliğidir.
* Destek [karmaşık türler](search-howto-complex-data-types.md) Azure Search dizini neredeyse tüm iç içe geçmiş JSON yapısındaki modeli sağlar.
* [Otomatik Tamamlama](search-autocomplete-tutorial.md) bir alternatif sunar **Öner** arama---yazarken davranışı uygulamak için API. Otomatik Tamamlama "sözcük veya bir kullanıcı şu anda yazarak tümcecik biter".
* [Ayrıştırma modu JsonLines](search-howto-index-json-blobs.md), parçası Azure Blob dizin oluşturma, bir arama belge başına bir satır başı karakteri tarafından ayrılmış JSON varlığı oluşturur.

### <a name="new-preview-features-in-version-80-preview"></a>Yeni Önizleme özellikleri sürüm 8.0-Önizleme
Azure Search .NET SDK'sı sürüm 8.0-önizlemesini API Sürüm 2017-11-11-Preview hedefler. Bu sürüm, aynı sürüm 9, özelliklerini içerir ve:

* [Müşteri tarafından yönetilen bir şifreleme anahtarları](search-security-manage-encryption-keys.md) için hizmet tarafı şifreleme bekleyen yeni bir önizleme özelliğidir. Yerleşik şifreleme Microsoft tarafından yönetilen bekleyen ek olarak, şifreleme anahtarları tek sahibi olduğu bir ek katmanı uygulayabilirsiniz.

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, için NuGet başvuru güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya ile proje başvurularınızın sağ tıklayıp "Yönetme NuGet paketleri..." Visual Studio'da.

NuGet yeni paketler ve bağımlılıkları İndirildikten sonra projenizi yeniden derleyin. Kodunuzu nasıl yapılandırıldığını bağlı olarak başarıyla yeniden oluşturmak. Bu durumda, başlamaya hazırsınız!

Yapı başarısız olursa, her yapı hatayı düzeltmek gerekir. Bkz: [sürüm 9 bozucu değişiklikler](#ListOfChanges) birleştiremiyorsa her olası çözmek derleme hatası için.

Eski yöntemleri veya özellikleri ile ilgili ek derleme uyarıları görebilirsiniz. Uyarıların hangi kullanım dışı özelliği yerine kullanmak yönergeler içerir. Örneğin, uygulamanızın kullandığı `DataSourceType.DocumentDb` özelliği, "Bu üye kullanım dışı. bildiren bir uyarı almanız gerekir CosmosDb kullanın".

Herhangi bir derleme hataları veya uyarıları düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sı yeni özellikleri ayrıntılı olarak [sürüm 9 yenilikler](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-9"></a>Sürüm 9 bozucu değişiklikler

Sürüm 9, uygulamanızı yeniden ek olarak, kod değişiklikleri yapılmasını gerektiren bazı önemli değişiklikler vardır.

> [!NOTE]
> Aşağıdaki değişiklikleri listesi kapsamlı değildir. Bazı değişiklikler büyük olasılıkla derleme hataları neden olmaz, ancak ikili uyumluluğu ile Azure Search .NET SDK bütünleştirilmiş kodları önceki sürümlerinde bağımlı derlemeleri kesintiye beri teknik ayırırsınız. Bu tür değişiklikler aşağıda listelenen değil. Lütfen ikili uyumluluğu sorunlardan kaçınmak için 9 sürümüne yükseltirken uygulamanızı yeniden oluşturun.

### <a name="immutable-properties"></a>Sabit özellikler

Birden fazla model sınıfları genel özelliklerini artık sabittir. Özel test etmek için bu sınıfların örnekleri oluşturmak için ihtiyacınız varsa yeni parametreli yapıcıları kullanabilirsiniz:

  - `AutocompleteItem`
  - `DocumentSearchResult`
  - `DocumentSuggestResult`
  - `FacetResult`
  - `SearchResult`
  - `SuggestResult`

### <a name="changes-to-field"></a>Alan değişiklikleri

`Field` Sınıfı karmaşık alanları gösterebilir göre değişti.

Aşağıdaki `bool` özellikleri boş değer atanabilir şimdi:

  - `IsFilterable`
  - `IsFacetable`
  - `IsSearchable`
  - `IsSortable`
  - `IsRetrievable`
  - `IsKey`

Bu özellikler artık olması gerektiğinden budur `null` karmaşık alanları söz konusu olduğunda. Bu özellikler okuyan kod varsa, işlemeye hazırlıklı olmak zorundadır `null`. Unutmayın, diğer tüm özellikler `Field` her zaman kaldırıldı ve null yapılabilir, olmaya devam ve bazıları da olacaktır `null` karmaşık alanları--özellikle de söz konusu olduğunda:

  - `Analyzer`
  - `SearchAnalyzer`
  - `IndexAnalyzer`
  - `SynonymMaps`

Parametresiz oluşturucusu `Field` yapılmadığını `internal`. Şu andan itibaren her `Field` oluşturma zamanında açık bir ad ve veri türü gerektirir.

### <a name="simplified-batch-and-results-types"></a>Basitleştirilmiş toplu ve sonuç türleri

Sürüm 7.0 Önizleme ve önceki, belgelerin gruplarını kapsayan çeşitli sınıfları paralel sınıf Hiyerarşiler yapılandırılmış:

  -  `DocumentSearchResult` ve `DocumentSearchResult<T>` devralındı `DocumentSearchResultBase`
  -  `DocumentSuggestResult` ve `DocumentSuggestResult<T>` devralındı `DocumentSuggestResultBase`
  -  `IndexAction` ve `IndexAction<T>` devralındı `IndexActionBase`
  -  `IndexBatch` ve `IndexBatch<T>` devralındı `IndexBatchBase`
  -  `SearchResult` ve `SearchResult<T>` devralındı `SearchResultBase`
  -  `SuggestResult` ve `SuggestResult<T>` devralındı `SuggestResultBase`

Genel tür parametresi olmadan türetilen türler için "dinamik olarak yazılmış" senaryolarda kullanılması amaçlanmıştır ve kullanımını kabul `Document` türü.

Sürüm 8.0-preview ile başlayarak, temel sınıflar ve genel olmayan türetilmiş sınıfları tüm kaldırıldı. Dinamik tür belirtilmiş senaryolar için kullanabileceğiniz `IndexBatch<Document>`, `DocumentSearchResult<Document>`ve benzeri.
 
### <a name="removed-extensibleenum"></a>Kaldırılan ExtensibleEnum

`ExtensibleEnum` Temel sınıf kaldırıldı. Ondan türetilen tüm sınıflar, yapılar gibi sunulmuştur `AnalyzerName`, `DataType`, ve `DataSourceType` örneğin. Kendi `Create` yöntemleri kaldırılmıştır. Çağrı yalnızca kaldırabilirsiniz `Create` olduğundan bu tür dizelerden örtük olarak dönüştürülebilir. Bu derleyici hata ile sonuçlanırsa, açıkça türleri belirsizliğini ortadan kaldırmak için dönüştürme işleci atama aracılığıyla çağırabilirsiniz. Örneğin, bu kodu değiştirebilirsiniz:

```csharp
var index = new Index()
{
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("message", AnalyzerName.Create("my_email_analyzer")) { IsSearchable = true }
    },
    ...
}
```

Şu şekilde:

```csharp
var index = new Index()
{
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("message", (AnalyzerName)"my_email_analyzer") { IsSearchable = true }
    },
    ...
}
```

İsteğe bağlı olarak devam etmek için bu tür isteğe bağlı değerler tutulan özellikleri artık açıkça null yazılmalıdır.

### <a name="removed-facetresults-and-hithighlights"></a>Kaldırılan FacetResults ve HitHighlights

`FacetResults` Ve `HitHighlights` sınıfları kaldırıldı. Modeli sonuçları artık olarak yazılan `IDictionary<string, IList<FacetResult>>` ve isabet vurguları olarak `IDictionary<string, IList<string>>`. Bu değişiklikten tanıtılan derleme hataları gidermek için hızlı bir şekilde eklemektir `using` kaldırılan türlerini kullanan her dosya üst kısmındaki diğer adları. Örneğin:

```csharp
using FacetResults = System.Collections.Generic.IDictionary<string, System.Collections.Generic.IList<Models.FacetResult>>;
using HitHighlights = System.Collections.Generic.IDictionary<string, System.Collections.Generic.IList<string>>;
```

### <a name="change-to-synonymmap"></a>SynonymMap için değiştirin 

`SynonymMap` Oluşturucusu artık sahip bir `enum` parametresi için `SynonymMapFormat`. Bu sabit listesi yalnızca bir değer vardı ve bu nedenle yedekli. Görürseniz, bunun sonucunda ortaya çıkan hataları yapı, başvuruları kaldırmanız `SynonymMapFormat` parametresi.

### <a name="miscellaneous-model-class-changes"></a>Çeşitli model sınıfı değişiklikleri

`AutocompleteMode` Özelliği `AutocompleteParameters` artık boş değer atanabilir değil. Bu özelliğe atayan bir kodunuz varsa `null`özelliği için varsayılan değer otomatik olarak başlatılır ve yalnızca kaldırabilirsiniz.

Parametreler sırası `IndexAction` Oluşturucusu Bu oluşturucu otomatik olarak oluşturulan olduğuna göre değişti. Oluşturucu kullanmak yerine, Fabrika yöntemleri kullanmanızı öneririz `IndexAction.Upload`, `IndexAction.Merge`ve benzeri.

### <a name="removed-preview-features"></a>Kaldırılan Önizleme özellikleri

Sürüm 9 sürüm 8.0 preview'dan yükseltme yapıyorsanız, dikkat bu özellik hala Önizleme aşamasında olduğundan, müşteri tarafından yönetilen anahtarlarla şifrelemenin kaldırıldı. Özellikle, `EncryptionKey` özelliklerini `Index` ve `SynonymMap` kaldırıldı.

Uygulamanızın bu özelliği bir sabit bağımlılık varsa, Azure Search .NET SDK'sı 9 sürümüne yükseltmek mümkün olmayacaktır. Sürüm 8.0-preview'ı kullanmaya devam edebilirsiniz. Ancak, lütfen aklınızda **Önizleme SDK'ları üretim uygulamalarında kullanılması önerilmez**. Önizleme özellikleri, yalnızca değerlendirme amacıyla olan ve değişebilir.

> [!NOTE]
> Şifrelenmiş oluşturduysanız dizinlere veya sürüm 8.0-preview SDK kullanarak eşlemeleri, hala mümkün olmayacak eş anlamlı bunları kullanabilir ve SDK sürüm 9, şifreleme durumu olumsuz etkilemeden kullanarak tanımlarını değiştirmek. SDK sürüm 9 değil gönderecek `encryptionKey` özelliği için REST API ve REST API sonuç olarak, kaynak şifreleme durumunu değiştirmez. 

### <a name="behavioral-change-in-data-retrieval"></a>Davranış değişikliği veri alma

"Dinamik olarak yazılan" kullanıyorsanız, `Search`, `Suggest`, veya `Get` türün örneklerini döndüren API'leri `Document`, dikkat edin, artık boş JSON dizileri için serisini `object[]` yerine `string[]`.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK'sı kullanma hakkında daha fazla ayrıntı gerekiyorsa bkz [.NET ile ilgili nasıl yapılır](search-howto-dotnet-sdk.md).

Bildirimleriniz üzerindeki SDK bizim için değerli. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search). Bir hata bulursanız, sorunu bildirin [Azure .NET SDK'sı GitHub deposu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", bir sorun başlığı önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
