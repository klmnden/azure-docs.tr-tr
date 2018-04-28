---
title: Azure Search .NET SDK sürüm 3 yükseltme | Microsoft Docs
description: Azure Search .NET SDK sürüm 3 yükseltme
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: 161d22e0ff4ec4ab28107919a80ecc48cd027967
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Azure Search .NET SDK sürüm 3 yükseltme
2.0-Önizleme veya, daha eski bir sürümü kullanıyorsanız [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede, uygulamanızı sürüm 3'ü kullanacak şekilde yükseltmenize yardımcı olur.

Örnekler dahil olmak üzere SDK'ın daha genel bir kılavuz için bkz: [bir .NET uygulamasından Azure Search kullanmayı](search-howto-dotnet-sdk.md).

Azure Search .NET SDK'sı 3 sürümünü önceki sürümlerinden bazı değişiklikler içerir. Kodunuzu değiştirmeden yalnızca en az çaba gerektirmelidir şekilde çoğunlukla ikincil, bunlar. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

> [!NOTE]
> Sürüm 1.0.2-preview kullanıyorsanız veya daha eski, sürüm 1.1 ilk ve ardından yükseltme 3 sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 1.1 yükseltme](search-dotnet-sdk-migration-version-1.md) yönergeler için.
>
> Azure Search Hizmeti örneğinizi son de dahil olmak üzere birkaç REST API sürümlerini destekler. En son artık değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirmek öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü karşılık gelen REST API sürümü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek için yükseltilir olsa bile bu kodu ile herhangi bir değişiklik çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Sürüm 3 yenilikler nelerdir?
Azure Search .NET SDK'sı 3 sürümünü hedefleyen en son Azure Search REST API'sini özellikle 2016-09-01 genel olarak kullanılabilir sürümü. Aşağıdakiler de dahil olmak üzere, bir .NET uygulamasından Azure Search'ün birçok yeni özellik kullanmayı mümkün kılar:

* [Özel çözümleyiciler](https://aka.ms/customanalyzers)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) ve [Azure Table Storage](search-howto-indexing-azure-tables.md) dizin oluşturucu desteği
* Dizin Oluşturucu özelleştirme aracılığıyla [alan eşlemeleri](search-indexer-field-mappings.md)
* Güvenli eşzamanlı dizin tanımları, dizin oluşturucular ve veri kaynaklarını güncelleştirme etkinleştirmek için Etag'ler desteği
* Dizin alan tanımları bildirimli olarak model sınıfınız dekorasyon ve yeni kullanarak oluşturmak için destek `FieldBuilder` sınıfı.
* .NET Core ve .NET taşınabilir profil 111 desteği

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet başvuru için güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya göre proje başvuruları sağ tıklayıp Visual Studio'da "NuGet paketleri...'ı Yönet" seçerek.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Kodunuzu nasıl yapılandırıldığını bağlı olarak başarılı bir şekilde yeniden oluşturmak. Bu durumda, başlamaya hazırsınız!

Derleme başarısız olursa, bir derleme hatası aşağıdaki gibi görmeniz gerekir:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Bu yapı hatayı düzeltmek için sonraki adım olacaktır. Bkz: [önemli değişiklikler sürüm 3](#ListOfChanges) hatanın nedeni nedir ve nasıl düzeltileceği hakkında ayrıntılar için.

Artık kullanılmayan yöntemleri ya da özellikleri ilgili ek derleme uyarıları görebilirsiniz. Uyarıları ne yerine kullanım dışı özelliği kullanmak yönergeler içerir. Örneğin, uygulamanızın kullandığı `IndexingParameters.Base64EncodeKeys` özelliği bildiren bir uyarı almak `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Derleme hataları düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sındaki yeni özellikleri ayrıntılı olarak [sürüm 3 yenilikler](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Sürüm 3'de yeni değişiklikler
Var. Yeni kod gerektirebilecek sürümünde 3 değişiklikler, küçük bir sayı, uygulamanızın yeniden ek olarak değiştirir.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient dönüş türü
`Indexes.GetClient` Yöntemi yeni bir dönüş türüne sahip. Daha önce döndürülen `SearchIndexClient`, ancak bu şekilde değiştirilmiştir `ISearchIndexClient` sürüm 2.0-Önizleme ve bu değişiklik sürüm 3 taşır. Bu mock isteyen müşteriler desteklemektir `GetClient` yöntemi, sahte bir uygulama döndürerek birim testleri için `ISearchIndexClient`.

#### <a name="example"></a>Örnek
Kodunuzu şöyle ise:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, veri türü ve diğerleri artık dizelere örtük olarak dönüştürülebilir
Öğesinden türetilen Azure Search .NET SDK'sı birçok türlerinde vardır `ExtensibleEnum`. Bu tür daha önce tüm türüne örtük olarak dönüştürülebilir `string`. Ancak, bir hata bulunması `Object.Equals` bu sınıfların ve bu örtük dönüştürme devre dışı bırakılması gereken hatayı düzeltmek için uygulama. Açık dönüşüm `string` hala izin verilir.

#### <a name="example"></a>Örnek
Kodunuzu şöyle ise:

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

Bu yapı hataları düzeltmek için bunu değiştirebilirsiniz:

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

Derleme hataları ile ilgili görebilirsiniz yöntemleri ya da geçersiz sürüm olarak 2.0 Önizleme ve daha sonra kaldırılan sürüm 3 işaretlenen özellikleri. Bu tür hatalarla karşılaşırsanız, bunların nasıl çözüleceği şöyledir:

- Bu oluşturucu kullanıyorsanız: `ScoringParameter(string name, string value)`, bunun yerine bunu kullanın: `ScoringParameter(string name, IEnumerable<string> values)`
- Kullanmakta olduğunuz varsa `ScoringParameter.Value` özelliği, kullanım `ScoringParameter.Values` özelliği veya `ToString` yöntemi yerine.
- Kullanmakta olduğunuz varsa `SearchRequestOptions.RequestId` özelliği, kullanım `ClientRequestId` özelliği yerine.

### <a name="removed-preview-features"></a>Kaldırılan Önizleme özellikleri

Sürüm 3 sürüm 2.0-Önizlemesi'nden yükseltiyorsanız, JSON ve Blob dizin oluşturucular için destek ayrıştırma CSV kaldırıldı, bu özellikler hala önizlemede olduğundan unutmayın. Özellikle, aşağıdaki yöntemlerden birini `IndexingParametersExtensions` sınıfı kaldırıldı:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanız bu özelliklere sıkı bir bağımlılık varsa, Azure Search .NET SDK'sı 3 sürümüne yükseltmeniz mümkün olmaz. Sürüm 2.0-Önizleme kullanmaya devam edebilirsiniz. Bununla birlikte, lütfen aklınızda **üretim uygulamaları SDK'ları önizlemede kullanmanızı önermiyoruz**. Önizleme özellikleri yalnızca değerlendirme ve değişiklik.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK'sını kullanma hakkında daha fazla ayrıntı gerekirse bkz [.NET yapılır](search-howto-dotnet-sdk.md).

SDK'bildiriminiz bizim. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure arama MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Bir hata bulursanız, bir sorunun dosya [Azure .NET SDK GitHub deposunu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", sorunu başlık önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
