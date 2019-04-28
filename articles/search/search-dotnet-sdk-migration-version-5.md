---
title: Azure Search .NET SDK sürüm 5 - Azure Search yükseltme
description: Azure Search .NET SDK sürüm 5 eski sürümlerinden geçiş kodu. Nelerin yeni olduğunu öğrenin ve hangi kodda değişiklik yapmanız gerekmez.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d7684aa79ac9f58c2a047b01a6d9f5263795221d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61291716"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-5"></a>Azure Search .NET SDK sürüm 5 yükseltme
4.0 Önizleme veya, eski bir sürümü kullanıyorsanız [Azure Search .NET SDK'sı](https://aka.ms/search-sdk), bu makalede, uygulamanızı 5 sürümünü kullanacak şekilde yükseltmek yardımcı olur.

Örnekler de dahil olmak üzere SDK'sının daha genel bir kılavuz için bkz. [bir .NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

Azure Search .NET SDK'sı sürüm 5, bazı değişiklikler daha önceki sürümlerin içerir. Kodunuzu değiştirmek, yalnızca en az çaba istemeniz gerekir böylece bunlar çoğunlukla küçük. Bkz: [yükseltme adımları](#UpgradeSteps) yeni SDK sürümü kullanmak kodunuzu değiştirmek konusunda yönergeler için.

> [!NOTE]
> 2.0 Önizlemesi veya eski bir sürümü kullanıyorsanız, 3. sürümüne yükseltmeniz ve ardından 5 sürümüne yükseltmeniz gerekir. Bkz: [Azure Search .NET SDK sürüm 3 yükseltme](search-dotnet-sdk-migration.md) yönergeler için.
>
> Azure Search Hizmeti örneğinizi en son dahil olmak üzere çeşitli REST API sürümlerini destekler. Artık en son değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirme öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü ilgili REST API sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek üzere yükseltilir bile kod değişikliğine gerek kalmadan çalışmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Sürüm 5 yenilikleri
Azure Search .NET SDK'sı 5 sürümünü hedefleyen en son Azure Search REST API'si, özellikle 2017-11-11'ın genel kullanıma sunulan sürümü. Bu aşağıdakiler de dahil, bir .NET uygulamasından Azure Search'ün yeni özelliklerini kullanmayı mümkün kılar:

* [Eş Anlamlılar](search-synonyms.md).
* Artık uyarılar dizin oluşturucusu yürütme geçmişi ile program aracılığıyla erişebileceğiniz (bkz `Warning` özelliği `IndexerExecutionResult` içinde [.NET başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) daha fazla ayrıntı için).
* .NET Core 2 desteği.
* Yeni paket yapısı, yalnızca gereksinim duyduğunuz SDK'sı parçalarını kullanımını destekler (bkz [sürüm 5 bozucu değişiklikler](#ListOfChanges) Ayrıntılar için).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, için NuGet başvuru güncelleştirme `Microsoft.Azure.Search` NuGet Paket Yöneticisi konsolu kullanılarak veya ile proje başvurularınızın sağ tıklayıp "Yönetme NuGet paketleri..." Visual Studio'da.

NuGet yeni paketler ve bağımlılıkları İndirildikten sonra projenizi yeniden derleyin. Kodunuzu nasıl yapılandırıldığını bağlı olarak başarıyla yeniden oluşturmak. Bu durumda, başlamaya hazırsınız!

Yapı başarısız olursa, aşağıdaki gibi bir yapı hatası görmeniz gerekir:

    The name 'SuggesterSearchMode' does not exist in the current context

Bu derleme hatayı düzeltmek için sonraki adımdır bakın. Bkz: [sürüm 5 bozucu değişiklikler](#ListOfChanges) neyin hataya neden ve nasıl düzeltileceği hakkında ayrıntılı bilgi için.

Azure Search .NET SDK'sı paketinin değişikliklerden dolayı Lütfen dikkat edin, sürüm 5 kullanmak için uygulamanızı yeniden oluşturmanız gerekir. Bu değişiklikler ayrıntıları [sürüm 5 bozucu değişiklikler](#ListOfChanges).

Eski yöntemleri veya özellikleri ile ilgili ek derleme uyarıları görebilirsiniz. Uyarıların hangi kullanım dışı özelliği yerine kullanmak yönergeler içerir. Örneğin, uygulamanızın kullandığı `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` yöntemi, "Bu davranış artık varsayılan olarak etkindir, bu yöntemin çağrılması artık gerekli olmayacak biçimde." ifadesini içeren bir uyarı almanız gerekir

Herhangi bir derleme hataları veya uyarıları düzelttik sonra isterseniz yeni işlevsellikten yararlanmak için uygulamanızı değişiklik yapabilirsiniz. SDK'sı yeni özellikleri ayrıntılı olarak [sürüm 5 yenilikleri](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Sürüm 5 bozucu değişiklikler

### <a name="new-package-structure"></a>Yeni paket yapısı

5 sürümündeki en önemli ölçüde değişiklik olan `Microsoft.Azure.Search` derleme ve içeriği bölündüğü artık dört ayrı NuGet paketleri olarak dağıtılan dört ayrı derlemeler:

 - `Microsoft.Azure.Search`: Diğer tüm Azure Search paketleri bağımlılık olarak içeren bir meta-package budur. SDK'ın önceki bir sürümden yükseltiyorsanız, yalnızca bu paket yükseltildikten ve yeniden oluşturma, yeni sürümü kullanmaya başlamak için yeterli olmalıdır.
 - `Microsoft.Azure.Search.Data`: Azure Search kullanarak bir .NET uygulaması geliştirmeye devam ediyoruz ve sorgu veya dizinleri belgeleri güncelleştirmek yalnızca ihtiyacınız varsa bu paketi kullanın. Ayrıca oluşturmak veya dizinleri güncelleştirme gerekirse eş anlamlı sözcük eşlemelerini veya diğer hizmet düzeyi kaynakları kullanmak `Microsoft.Azure.Search` bunun yerine paket.
 - `Microsoft.Azure.Search.Service`: Azure Search dizinlerini, eş anlamlı sözcük eşlemelerini, dizin oluşturucular, veri kaynakları veya diğer hizmet düzeyi kaynakları yönetmek için. NET'te Otomasyon geliştiriyorsanız, bu paketi kullanın. Dizinlerinizi içinde sorgu veya güncelleştirme belgelere yalnızca ihtiyacınız varsa `Microsoft.Azure.Search.Data` bunun yerine paket. Azure Search'ün tüm işlevlerine ihtiyacınız varsa, `Microsoft.Azure.Search` bunun yerine paket.
 - `Microsoft.Azure.Search.Common`: Azure Search .NET kitaplıkları tarafından gereken genel türler. Bu paket, uygulamanızda doğrudan kullanmak gerekmez; Yalnızca, bir bağımlılık olarak kullanılmak üzere tasarlanmıştır.
 
Derlemeler arasında çok sayıda türü taşınan olduğundan bu değişikliği teknik olarak kesiliyor. Uygulamanızı yeniden SDK'sının 5 sürümüne yükseltmek için gerekli olmasının nedenidir.

Uygulamanızı yeniden yanı sıra var. kod gerektiren diğer kesme değişiklikleri 5 sürümündeki az sayıda değiştirir.

### <a name="change-to-suggesters"></a>İçin öneri araçlarını değiştirme 

`Suggester` Oluşturucusu artık sahip bir `enum` parametresi için `SuggesterSearchMode`. Bu sabit listesi yalnızca bir değer vardı ve bu nedenle yedekli. Görürseniz, bunun sonucunda ortaya çıkan hataları yapı, başvuruları kaldırmanız `SuggesterSearchMode` parametresi.

### <a name="removed-obsolete-members"></a>Eski üyeler kaldırıldı

Görebileceğiniz yöntemler veya önceki sürümlerde eski olarak işaretlenen ve bundan sonra 5 sürümünde kaldırılan özellikler için ilgili hatalar oluşturun. Bu tür hatalarla karşılaşırsanız, bunların nasıl çözüleceğine aşağıda verilmiştir:

- Kullandıysanız `IndexingParametersExtensions.IndexStorageMetadataOnly` yöntemi, kullanım `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` yerine.
- Kullandıysanız `IndexingParametersExtensions.SkipContent` yöntemi, kullanım `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` yerine.

### <a name="removed-preview-features"></a>Kaldırılan Önizleme özellikleri

Sürüm 5 sürüm 4.0 preview'dan yükseltme yapıyorsanız, JSON dizisi ve CSV ayrıştırma Blob dizin oluşturucuları için destek kaldırılmıştır, bu özellik hala Önizleme aşamasında olduğundan dikkat edin. Özellikle, aşağıdaki yöntemlerden birini `IndexingParametersExtensions` sınıfı kaldırıldı:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanızın bu özelliklere sabit bir bağımlılık varsa, Azure Search .NET SDK'sı 5 sürümüne yükseltmek mümkün olmayacaktır. Sürüm 4.0-preview'ı kullanmaya devam edebilirsiniz. Ancak, lütfen aklınızda **Önizleme SDK'ları üretim uygulamalarında kullanılması önerilmez**. Önizleme özellikleri, yalnızca değerlendirme amacıyla olan ve değişebilir.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK'sı kullanma hakkında daha fazla ayrıntı gerekiyorsa bkz [.NET ile ilgili nasıl yapılır](search-howto-dotnet-sdk.md).

Bildirimleriniz üzerindeki SDK bizim için değerli. Sorunlarla karşılaşırsanız, bize yardım isteyin çekinmeyin [Azure Search MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Bir hata bulursanız, sorunu bildirin [Azure .NET SDK'sı GitHub deposu](https://github.com/Azure/azure-sdk-for-net/issues). "[Azure Search]", bir sorun başlığı önek emin olun.

Azure Search kullandığınız için teşekkür ederiz!
