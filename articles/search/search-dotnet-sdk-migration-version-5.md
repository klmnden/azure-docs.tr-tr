---
title: Azure Search .NET SDK sürüm 5 yükseltme | Microsoft Docs
description: Azure Search .NET SDK sürüm 5 yükseltme
author: brjohnstmsft
manager: jlembicz
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 018388cd2bd85eb86ad7b62ee247bccd6329e9ac
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-5"></a>Azure Search .NET SDK sürüm 5 yükseltme
4.0 Önizleme veya, daha eski bir sürümü kullanıyorsanız [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede, uygulamanızı sürüm 5 kullanacak şekilde yükseltmenize yardımcı olur.

Örnekler dahil olmak üzere SDK'ın daha genel bir kılavuz için bkz: [bir .NET uygulamasından Azure Search kullanmayı](search-howto-dotnet-sdk.md).

Azure Search .NET SDK'sı sürüm 5 önceki sürümlerinden bazı değişiklikler içerir. Kodunuzu değiştirmeden yalnızca en az çaba gerektirmelidir şekilde çoğunlukla ikincil, bunlar. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

> [!NOTE]
> 2.0-Önizleme veya eski bir sürümü kullanıyorsanız, 3 sürümüne yükseltmeniz ve ardından 5 sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 3 yükseltme](search-dotnet-sdk-migration.md) yönergeler için.
>
> Azure Search Hizmeti örneğinizi son de dahil olmak üzere birkaç REST API sürümlerini destekler. En son artık değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirmek öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü karşılık gelen REST API sürümü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek için yükseltilir olsa bile bu kodu ile herhangi bir değişiklik çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Sürüm 5 yenilikler nelerdir?
Azure Search .NET SDK'sı sürüm 5 hedefler en son Azure Search REST API'si, özellikle 2017-11-11 genel olarak kullanılabilir sürümü. Bu, yeni Azure arama özellikleri aşağıdakiler de dahil, bir .NET uygulamasından kullanmak mümkün kılar:

* [Eş anlamlıları](search-synonyms.md).
* Artık dizin oluşturucusu yürütme geçmişini uyarılarla program aracılığıyla erişebilirsiniz (bkz `Warning` özelliği `IndexerExecutionResult` içinde [.NET başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) daha fazla ayrıntı için).
* .NET Core 2 desteği.
* Yeni paket yapısı destekleyen yalnızca gereksinim duyduğunuz SDK'sı bölümlerini kullanarak (bkz [sürüm 5 önemli değişiklikler](#ListOfChanges) Ayrıntılar için).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet başvuru için güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya göre proje başvuruları sağ tıklayıp Visual Studio'da "NuGet paketleri...'ı Yönet" seçerek.

NuGet yeni paketleri ve bunların bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Yeni GA SDK'ın değil bir önizleme özelliği kullanmadığınız sürece, başarılı bir şekilde yeniden oluşturmalısınız (üzerinde değilse,'ın bize bildirin [GitHub](https://github.com/azure/azure-sdk-for-net/issues)). Bu durumda, başlamaya hazırsınız!

Lütfen Azure Search .NET SDK paketinde değişiklikler nedeniyle unutmayın, sürüm 5 kullanmak için uygulamanızı yeniden oluşturmanız gerekir. Bu değişiklikler ayrıntıları [sürüm 5 önemli değişiklikler](#ListOfChanges).

Artık kullanılmayan yöntemleri ya da özellikleri ilgili ek derleme uyarıları görebilirsiniz. Uyarıları ne yerine kullanım dışı özelliği kullanmak yönergeler içerir. Örneğin, uygulamanızın kullandığı `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` yöntemi, "Bu davranış şimdi etkin olduğundan varsayılan olarak, bu yöntemi çağırmadan artık gerekli değildir." bildiren bir uyarı almak

Yapı uyarılar düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sındaki yeni özellikleri ayrıntılı olarak [sürüm 5 yenilikler](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Sürüm 5'de yeni değişiklikler
Sürüm 5 en önemli sonu değişikliği olan `Microsoft.Azure.Search` derleme ve içeriği bölündüğü şimdi dört ayrı NuGet paketleri olarak dağıtılan dört ayrı derlemeler:

 - `Microsoft.Azure.Search`: Tüm diğer Azure Search paketler bağımlılık içeren bir meta-paket budur. SDK'ın önceki bir sürümden yükseltirken, yalnızca bu paketi yükseltme ve yeniden oluşturmak, yeni sürüm kullanmaya başlamak için yeterli olmalıdır.
 - `Microsoft.Azure.Search.Data`: Bu paket Azure arama'yı kullanarak bir .NET Uygulama geliştirirken ve sorgu veya dizinleri belgelerde güncelleştirmek yalnızca ihtiyacınız varsa kullanın. Ayrıca oluşturmak veya dizinleri güncelleştirmek gerekirse, eş anlamlı eşlemeleri ya da hizmet düzeyi kaynaklar kullanın `Microsoft.Azure.Search` bunun yerine paket.
 - `Microsoft.Azure.Search.Service`: Bu paket Azure Search dizinlerini, eş anlamlı eşlemeleri, dizin oluşturucular, veri kaynakları veya diğer hizmet düzeyi kaynakları yönetmek için .NET otomasyon geliştiriyorsanız kullanın. Dizinlerinizi içinde yalnızca sorgu veya güncelleştirme belgelere gerekiyorsa kullanın `Microsoft.Azure.Search.Data` bunun yerine paket. Azure Search tüm işlevselliğini gerekiyorsa kullanın `Microsoft.Azure.Search` bunun yerine paket.
- `Microsoft.Azure.Search.Common`: Azure Search .NET kitaplıkları tarafından gerekli genel türleri. Bu paket, uygulamanızda doğrudan kullanmak gerekmez; Yalnızca bir bağımlılık olarak kullanılmak üzere tasarlanmıştır.
 
Birçok türleri derlemeler arasında taşındı beri bu değişikliği teknik kesiliyor. Uygulamanızı yeniden SDK 5 sürümüne yükseltmek için gerekli olan nedeni budur.

Uygulamanızı yeniden yanı sıra var. kod gerektirebilecek diğer önemli değişiklikler sürüm 5 olarak az sayıda değiştirir.

### <a name="removed-obsolete-members"></a>Eski üyeler kaldırıldı

Görebileceğiniz derleme yöntemler veya önceki sürümlerde kullanımdan kaldırılmış olarak işaretlenmiş ve daha sonra sürüm 5 kaldırılan özellikler ile ilgili hataları. Bu tür hatalarla karşılaşırsanız, bunların nasıl çözüleceği şöyledir:

- Kullanmakta olduğunuz varsa `IndexingParametersExtensions.IndexStorageMetadataOnly` yöntemi, kullanım `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` yerine.
- Kullanmakta olduğunuz varsa `IndexingParametersExtensions.SkipContent` yöntemi, kullanım `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` yerine.

### <a name="removed-preview-features"></a>Kaldırılan Önizleme özellikleri

Sürüm 4.0-Önizlemesi'nden 5 sürümüne yükseltme yapıyorsanız, JSON dizisi ve Blob dizin oluşturucular için destek ayrıştırma CSV kaldırıldı, bu özellikler hala önizlemede olduğundan unutmayın. Özellikle, aşağıdaki yöntemlerden birini `IndexingParametersExtensions` sınıfı kaldırıldı:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanız bu özelliklere sıkı bir bağımlılık varsa, Azure Search .NET SDK'sı 5 sürümüne yükseltmeniz mümkün olmaz. Sürüm 4.0-Önizleme kullanmaya devam edebilirsiniz. Bununla birlikte, lütfen aklınızda **üretim uygulamaları SDK'ları önizlemede kullanmanızı önermiyoruz**. Önizleme özellikleri yalnızca değerlendirme ve değişiklik.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK'sını kullanma hakkında daha fazla ayrıntı gerekirse bkz [.NET yapılır](search-howto-dotnet-sdk.md).

SDK'bildiriminiz bizim. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure arama MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Bir hata bulursanız, bir sorunun dosya [Azure .NET SDK GitHub deposunu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", sorunu başlık önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
