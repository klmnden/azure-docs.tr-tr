---
title: Azure Search .NET SDK sürüm 3 - Azure Search yükseltme
description: Azure Search .NET SDK sürüm 3'ü eski sürümlerinden geçiş kodu. Nelerin yeni olduğunu öğrenin ve hangi kodda değişiklik yapmanız gerekmez.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 4acf609ca1f81e69babfa1a319b43e20e84a8395
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61291544"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Azure Search .NET SDK sürüm 3 yükseltme
2.0 Önizlemesi veya, eski bir sürümü kullanıyorsanız [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede, uygulamanızı sürüm 3'ü kullanacak şekilde yükseltin yardımcı olur.

Örnekler de dahil olmak üzere SDK'sının daha genel bir kılavuz için bkz. [bir .NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

Azure Search .NET SDK'sı sürüm 3, bazı değişiklikler daha önceki sürümlerin içerir. Kodunuzu değiştirmek, yalnızca en az çaba istemeniz gerekir böylece bunlar çoğunlukla küçük. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

> [!NOTE]
> Sürüm 1.0.2-preview kullanıyorsanız veya daha eski sürüm 1.1, yükseltip daha sonra 3 sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 1.1 için yükseltmeden](search-dotnet-sdk-migration-version-1.md) yönergeler için.
>
> Azure Search Hizmeti örneğinizi en son dahil olmak üzere çeşitli REST API sürümlerini destekler. Artık en son değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirme öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü ilgili REST API sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek üzere yükseltilir bile kod değişikliğine gerek kalmadan çalışmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Sürüm 3 yenilikler nelerdir?
Azure Search .NET SDK'sı 3 sürümünü hedefleyen en son genel kullanıma sunulan Azure Search REST API'si, özellikle 2016-09-01 sürümü. Bu, aşağıdakiler dahil olmak üzere bir .NET uygulamasından Azure Search'ün birçok yeni özellikleri kullanmak mümkün kılar:

* [Özel çözümleyiciler](https://aka.ms/customanalyzers)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md) ve [Azure tablo depolama](search-howto-indexing-azure-tables.md) dizin oluşturucu desteği
* Dizin Oluşturucu özelleştirme aracılığıyla [alan eşlemeleri](search-indexer-field-mappings.md)
* Güvenli eşzamanlı dizin tanımları, dizin oluşturucular ve veri kaynaklarını güncelleştirme etkinleştirmek için Etag'ler desteği
* Model sınıfınızın dekorasyon ve kullanarak yeni bir dizin alan tanımları bildirimli olarak oluşturmak için destek `FieldBuilder` sınıfı.
* .NET Core ve .NET taşınabilir profil 111 desteği

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, için NuGet başvuru güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya ile proje başvurularınızın sağ tıklayıp "Yönetme NuGet paketleri..." Visual Studio'da.

NuGet yeni paketler ve bağımlılıkları İndirildikten sonra projenizi yeniden derleyin. Kodunuzu nasıl yapılandırıldığını bağlı olarak başarıyla yeniden oluşturmak. Bu durumda, başlamaya hazırsınız!

Yapı başarısız olursa, aşağıdaki gibi bir yapı hatası görmeniz gerekir:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Bu derleme hatayı düzeltmek için sonraki adımdır bakın. Bkz: [sürüm 3 için bozucu değişiklikler](#ListOfChanges) neyin hataya neden ve nasıl düzeltileceği hakkında ayrıntılı bilgi için.

Eski yöntemleri veya özellikleri ile ilgili ek derleme uyarıları görebilirsiniz. Uyarıların hangi kullanım dışı özelliği yerine kullanmak yönergeler içerir. Örneğin, uygulamanızın kullandığı `IndexingParameters.Base64EncodeKeys` özelliği bildiren bir uyarı almanız gerekir `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Derleme hataları düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sı yeni özellikleri ayrıntılı olarak [sürüm 3 yenilikler](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Sürüm 3 için bozucu değişiklikler
Uygulamanızı yeniden yanı sıra küçük bir kod gerektirebilecek sürüm 3 için bozucu değişiklikler sayısı var. değiştirir.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient dönüş türü
`Indexes.GetClient` Yöntemi yeni bir dönüş türüne sahip. Daha önce döndürülen `SearchIndexClient`, ancak bu olarak değiştirildi `ISearchIndexClient` 2.0 Önizleme sürümüne ve aynı değişikliği sürüm 3 taşır. Bu sahte isteyen müşteriler desteklemektir `GetClient` yöntemi için birim testleri, sahte bir uygulama döndürerek `ISearchIndexClient`.

#### <a name="example"></a>Örnek
Kodunuzu gibi görünüyorsa:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, veri türü ve diğerleri artık dizeleri için örtük olarak dönüştürülebilir
Öğesinden türetilen Azure Search .NET SDK'sı, birçok tür `ExtensibleEnum`. Bu tür daha önce tüm türüne örtük olarak dönüştürülebilir `string`. Ancak, bir hata bulunması `Object.Equals` bu sınıfları ve bu örtülü dönüştürme devre dışı bırakılması gereken hatayı düzeltmek için uygulama. Açık bir dönüştürme `string` yine de izin verilir.

#### <a name="example"></a>Örnek
Kodunuzu gibi görünüyorsa:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Bu derleme hatalarını düzeltmek için bunu değiştirebilirsiniz:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Eski üyeler kaldırıldı

Derleme hataları ile ilgili görebilirsiniz yöntemleri veya olarak eski sürüm 2.0 Önizleme ve sürüm 3 sonradan kaldırılan işaretlenen özellikleri. Bu tür hatalarla karşılaşırsanız, bunların nasıl çözüleceğine aşağıda verilmiştir:

- Bu oluşturucu kullandıysanız: `ScoringParameter(string name, string value)`, bunun yerine bunu kullanın: `ScoringParameter(string name, IEnumerable<string> values)`
- Kullandıysanız `ScoringParameter.Value` özelliği, kullanım `ScoringParameter.Values` özelliği veya `ToString` yöntemi yerine.
- Kullandıysanız `SearchRequestOptions.RequestId` özelliği, kullanım `ClientRequestId` özelliği bunun yerine.

### <a name="removed-preview-features"></a>Kaldırılan Önizleme özellikleri

Sürüm 3 sürüm 2.0 preview'dan yükseltme yapıyorsanız, JSON ve CSV ayrıştırma Blob dizin oluşturucuları için destek kaldırılmıştır, bu özellik hala Önizleme aşamasında olduğundan dikkat edin. Özellikle, aşağıdaki yöntemlerden birini `IndexingParametersExtensions` sınıfı kaldırıldı:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanızın bu özelliklere sabit bir bağımlılık varsa, Azure Search .NET SDK'sı 3 sürümüne yükseltmek mümkün olmayacaktır. Sürüm 2.0-preview'ı kullanmaya devam edebilirsiniz. Ancak, lütfen aklınızda **Önizleme SDK'ları üretim uygulamalarında kullanılması önerilmez**. Önizleme özellikleri, yalnızca değerlendirme amacıyla olan ve değişebilir.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK'sı kullanma hakkında daha fazla ayrıntı gerekiyorsa bkz [.NET ile ilgili nasıl yapılır](search-howto-dotnet-sdk.md).

Bildirimleriniz üzerindeki SDK bizim için değerli. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure Search MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Bir hata bulursanız, sorunu bildirin [Azure .NET SDK'sı GitHub deposu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", bir sorun başlığı önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
