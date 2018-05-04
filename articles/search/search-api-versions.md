---
title: Azure Search'ün API sürümlerini | Microsoft Docs
description: Azure Search REST API'lerini ve istemci Kitaplığı'nda .NET SDK'sı için sürüm ilkesi.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 3235813708b3c097e7f66610796f75eafd06aa05
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="api-versions-in-azure-search"></a>Azure Search'te API sürümleri
Azure arama özellik güncelleştirmeleri düzenli olarak yapar. Bazen, ancak her zaman, bu güncelleştirmeleri bize API'mize geriye dönük uyumluluğu korumak için yeni bir sürümünü yayımlamak gerektirir. Yeni bir sürüm yayımlama nasıl ve ne zaman kodunuzda arama hizmet güncelleştirmeleri tümleştirmek denetlemenize olanak verir.

Bir kural olarak, biz yeni bir API sürümü kullanmak için kodunuzu yükseltmek için bazı çaba gerektirebilir bu yana yeni sürümler yalnızca gerekli olduğunda yayımlamayı deneyin. Biz, geriye dönük uyumluluk keser şekilde API bazı yönlerinin değiştirmeniz gerekirse, biz yalnızca yeni bir sürümünü yayımlar. Bu düzeltmeler var olan özellikleri nedeniyle veya varolan API yüzey alanını değiştirme yeni özellikleri nedeniyle ortaya çıkabilir.

Biz SDK güncelleştirmeleri için aynı kural izleyin. Azure Search SDK izleyen [anlamsal sürüm oluşturma](http://semver.org/) sürümü üç kısma sahip olduğu anlamına gelir kuralları: birincil, ikincil ve yapı numarası (örneğin, 1.1.0). Biz yalnızca geriye dönük uyumluluk bölün değişiklikler durumunda SDK'ın yeni bir ana sürüm serbest bırakır. Bölünemez özelliği güncelleştirmeleri biz alt sürüm artırır ve hata düzeltmeleri için biz yalnızca yapı sürümü artmasına neden olur.

> [!NOTE]
> Azure Search Hizmeti örneğinizi son de dahil olmak üzere birkaç REST API sürümlerini destekler. En son artık değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirmek öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü karşılık gelen REST API sürümü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek için yükseltilir olsa bile bu kodu ile herhangi bir değişiklik çalıştırmaya devam edebilirsiniz.

## <a name="snapshot-of-current-versions"></a>Geçerli sürümlerinin anlık görüntüsü
Aşağıda bir anlık görüntü sürümlerinin geçerli tüm Azure arama için programlama arabirimleri.

| Arabirimleri | Son ana sürümle | Durum |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |5.0 |Genellikle Nisan 2018 yayımlanan kullanılabilir |
| [.NET SDK Önizleme](https://aka.ms/search-sdk-preview) |4.0.1-Preview |Mayıs 2017 yayımlanan Önizleme |
| [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/) |2017 11 11 |Genel olarak kullanılabilir |
| [Hizmet REST API Önizleme](search-api-2016-09-01-preview.md) |2016-09-01-Önizleme |Önizleme |
| [.NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk) |2.0 |Genel olarak kullanılabilir |
| [Yönetim REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Genel olarak kullanılabilir |

REST dahil olmak üzere API'ları için `api-version` her çağrıda gereklidir. Bu bir önizleme API gibi belirli bir sürümü hedeflemesini kolaylaştırır. Aşağıdaki örnek gösterilmektedir nasıl `api-version` parametresi:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2017-11-11

> [!NOTE]
> Her istek olsa da bir `api-version`, tüm API istekler için aynı sürümünü kullanmanızı öneririz. Bu, özellikle yeni API sürümleri öznitelikler veya önceki sürümleri tarafından tanınmıyor işlemler aldığımızda geçerlidir. API sürümü karıştırma olabilir istenmeyen sonuçlara ve kaçınılmalıdır.
>
> Yönetim REST API ve hizmeti REST API'si diğer bağımsız olarak sürümlü. Sürüm numaraları herhangi benzerlik içerik olarak farklı olur.

Genel olarak kullanılabilir (veya GA) API'leri Azure hizmet düzeyi sözleşmelerine tabi olan ve üretimde kullanılabilir. Önizleme sürümleri, her zaman bir GA sürümü geçirilmez Deneysel özellikleri vardır. **Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.**

## <a name="about-preview-and-generally-available-versions"></a>Önizleme ve genel olarak kullanılabilir sürümleri hakkında
Azure arama her zaman REST API'si aracılığıyla Deneysel özellikleri ilk olarak, ardından yayın öncesi sürümlerini .NET SDK'sı önceden serbest bırakır.

Önizleme özellikleri geçirilecek garanti edilmez GA yayın. GA sürümündeki özellikler kararlı ve tahmin edilemez küçük geriye dönük olarak uyumlu düzeltmeler ve geliştirmeler hariç olmak üzere değiştirmek kabul edilen gelirken, Önizleme özellikleri için test ve deneme, geri bildirim özelliğini toplama amacı ile kullanılabilir Tasarım ve uygulama.

Ancak, Önizleme özellikleri değiştirilebilir olduğundan, bir bağımlılık Önizleme sürümlerinde alır üretim kod yazmaya karşı öneririz. Eski bir önizleme sürümünü kullanıyorsanız, genel olarak kullanılabilir (GA) sürümüne geçirme öneririz.

.NET SDK'sı: kod geçiş için yönergeler, adresinde bulunabilir [.NET SDK'sı yükseltme](search-dotnet-sdk-migration.md).

Genel kullanılabilirlik Azure Search hizmet düzeyi sözleşmesi (SLA) altında demektir. SLA bulunabilir [Azure Search hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
