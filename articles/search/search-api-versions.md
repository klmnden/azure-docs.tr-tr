---
title: Azure Search'ün API sürümlerini | Microsoft Docs
description: Azure Search REST API'lerini ve istemci Kitaplığı'nda .NET SDK'sı için sürüm ilkesi.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: brjohnst
ms.openlocfilehash: 8d1e30b0bca3c63fe4528c06e5389d8cbe27a7e6
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113614"
---
# <a name="api-versions-in-azure-search"></a>Azure Search'te API sürümleri
Azure arama özellik güncelleştirmeleri düzenli olarak yapar. Bazen, ancak her zaman, bu güncelleştirmeleri API geriye dönük uyumluluğu korumak için yeni bir sürümünü gerektirir. Yeni bir sürüm yayımlama nasıl ve ne zaman kodunuzda arama hizmet güncelleştirmeleri tümleştirmek denetlemenize olanak verir.

Yeni bir API sürümü kullanmak için kodunuzu yükseltmek için bazı çaba içerebileceği bir kural olarak, yeni sürümler yalnızca gerekli olduğunda, Azure Search takım yayımlar. Yalnızca API bazı yönlerinin geriye dönük uyumluluk keser bir şekilde değişirse yeni bir sürümü gereklidir. Bu değişiklikleri nedeniyle var olan özellikleri düzeltmeleri veya varolan API yüzey alanını değiştirme yeni özellikleri nedeniyle gerçekleşebilir.

Aynı kural SDK güncelleştirmeler için geçerlidir. Azure Search SDK izleyen [anlamsal sürüm oluşturma](http://semver.org/) sürümü üç kısma sahip olduğu anlamına gelir kuralları: birincil, ikincil ve yapı numarası (örneğin, 1.1.0). SDK'sının yeni bir ana sürüm yalnızca geriye dönük uyumluluk bölün değişiklikler yayımlanır. Bölünemez özellik güncelleştirmeleri ikincil sürüm artırır ve hata düzeltmeleri yalnızca yapı sürümünü artırır.

> [!NOTE]
> Azure Search Hizmeti örneğinizi son de dahil olmak üzere birkaç REST API sürümlerini destekler. En son artık değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirmek öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü karşılık gelen REST API sürümü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek için yükseltilir olsa bile bu kodu ile herhangi bir değişiklik çalıştırmaya devam edebilirsiniz.

## <a name="snapshot-of-current-versions"></a>Geçerli sürümlerinin anlık görüntüsü
Aşağıda bir anlık görüntü sürümlerinin geçerli tüm Azure arama için programlama arabirimleri.

| Arabirimler | Son ana sürümle | Durum |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |5.0 |Genellikle Nisan 2018 yayımlanan kullanılabilir |
| [.NET SDK Önizleme](https://aka.ms/search-sdk-preview) |4.0.1-Preview |Mayıs 2017 yayımlanan Önizleme |
| [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/) |2017 11 11 |Genel olarak kullanılabilir |
| [Hizmet REST API 2017-11-11-Önizleme](search-api-2017-11-11-preview.md) |2017-11-11-Önizleme |Önizleme |
| [.NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk) |2.0 |Genel olarak kullanılabilir |
| [Yönetim REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Genel olarak kullanılabilir |

REST dahil olmak üzere API'ları için `api-version` her çağrıda gereklidir. Kullanarak `api-version` Önizleme API gibi belirli bir sürümü hedeflemesini daha kolay hale getirir. Aşağıdaki örnek gösterilmektedir nasıl `api-version` parametresi:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2017-11-11

> [!NOTE]
> Her istek olsa da bir `api-version`, tüm API istekler için aynı sürümünü kullanmanızı öneririz. Bu, özellikle yeni API sürümleri öznitelikler veya önceki sürümleri tarafından tanınmıyor işlemler aldığımızda geçerlidir. API sürümü karıştırma olabilir istenmeyen sonuçlara ve kaçınılmalıdır.
>
> Yönetim REST API ve hizmeti REST API'si diğer bağımsız olarak sürümlü. Sürüm numaraları herhangi benzerlik içerik olarak farklı olur.

Genel olarak kullanılabilir (veya GA) API'leri Azure hizmet düzeyi sözleşmelerine tabi olan ve üretimde kullanılabilir. Önizleme sürümleri, her zaman bir GA sürümü geçirilmez Deneysel özellikleri vardır. **Önizleme API'leri üretim uygulamalarında kullanmaktan kaçının önemle önerilir.**

## <a name="about-preview-and-generally-available-versions"></a>Önizleme ve genel olarak kullanılabilir sürümleri hakkında
Azure arama her zaman REST API'si aracılığıyla Deneysel özellikleri ilk olarak, ardından yayın öncesi sürümlerini .NET SDK'sı önceden serbest bırakır.

Önizleme özellikleri, test ve deneme, özellik tasarım ve uygulama geri bildirim toplama hedefi için kullanılabilir. Bu nedenle, Önizleme özellikleri zaman içerisinde, büyük olasılıkla geriye dönük uyumluluk bölün şekilde değiştirebilirsiniz. GA sürümündeki kararlı ve küçük geriye dönük olarak uyumlu düzeltmeler ve geliştirmeler hariç olmak üzere değiştirmek olasılığı olan özellikler aksine budur. Ayrıca, Önizleme özellikleri her zaman bu GA sürümü yapmayın.

Bu nedenlerle, bir bağımlılık Önizleme sürümlerinde alır üretim kod yazmaya karşı öneririz. Eski bir önizleme sürümünü kullanıyorsanız, genel olarak kullanılabilir (GA) sürümüne geçirme öneririz.

.NET SDK'sı: kod geçiş için yönergeler, adresinde bulunabilir [.NET SDK'sı yükseltme](search-dotnet-sdk-migration.md).

Genel kullanılabilirlik Azure Search hizmet düzeyi sözleşmesi (SLA) altında demektir. SLA bulunabilir [Azure Search hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
