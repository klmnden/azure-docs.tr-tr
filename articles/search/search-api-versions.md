---
title: .NET SDK'sını ve REST API'ler - Azure Search API Sürüm Yönetimi
description: Azure Search REST API'lerini ve istemci Kitaplığı'ndaki .NET SDK'sı sürüm ilkesi.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: d72901653e995e811a1d3e89cef8a5f77a9ea8bd
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523801"
---
# <a name="api-versions-in-azure-search"></a>Azure Search API sürümleri
Azure arama özelliği güncelleştirmeleri düzenli olarak yapar. Bazen, ancak her zaman, bu güncelleştirmeleri API geriye dönük uyumluluğu korumak için yeni bir sürümü gerektirir. Yeni bir sürüm yayımlama, ne zaman ve nasıl kodunuzda arama hizmet güncelleştirmelerini tümleştirme denetlemenizi sağlar.

Kodunuzu yeni bir API sürümünü kullanacak şekilde yükseltmek için bazı çaba içerebileceği bir kural olarak, yalnızca gerekli olduğunda, yeni sürümleri Azure Search ekibine yayımlar. Yalnızca bazı yönlerini API, geriye dönük uyumluluk keser şekilde değiştiyse yeni bir sürüm gereklidir. Bu tür değişiklikleri nedeniyle var olan özellikleri düzeltmeleri veya var olan API yüzey alanı değiştirmek yeni özellikler nedeniyle oluşabilir.

Aynı kural SDK güncelleştirmeleri için geçerlidir. Azure Search SDK'sı aşağıdaki [semantic versioning](https://semver.org/) sürümünün üç bölümden oluşur, yani kuralları: büyük, küçük ve derleme numarası (örneğin, 1.1.0). SDK'sının yeni bir ana sürüm geriye dönük uyumluluk kesme değişiklikleri için yayımlanır. Hataya neden olmayan özellik güncelleştirmeleri, alt sürüm artırır ve hata düzeltmeleri yalnızca derleme sürümü artacaktır.

> [!NOTE]
> Azure Search Hizmeti örneğinizi en son dahil olmak üzere çeşitli REST API sürümlerini destekler. Artık en son değildir, ancak en yeni sürümü kullanmak için kodunuzu geçirme öneririz bir sürümünü kullanmaya devam edebilirsiniz. REST API kullanırken, api-version parametresi aracılığıyla her istekte API sürümü belirtmeniz gerekir. .NET SDK kullanarak, kullanmakta olduğunuz SDK sürümü ilgili REST API sürümünü belirler. Eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümü desteklemek üzere yükseltilir bile kod değişikliğine gerek kalmadan çalışmaya devam edebilirsiniz.

## <a name="snapshot-of-current-versions"></a>Anlık görüntü geçerli sürümleri
Aşağıda tüm geçerli sürümlerinde anlık görüntüsünü Azure Search için programlama arabirimi.


| Arabirimler | En son ana sürüm | Durum |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |9.0 |Genel olarak kullanılabilir, yayımlanan Mayıs 2019 |
| [.NET SDK'sı Önizleme](https://aka.ms/search-sdk-preview) |8.0-Önizleme |Nisan 2019 yayımlanan Önizleme |
| [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/) |2019-05-06 |Genel Kullanıma Sunuldu |
| [Hizmet REST API 2019-05-06-Önizleme](search-api-preview.md) |2019-05-06-Önizleme |Preview |
| [.NET Yönetim SDK'sı](https://aka.ms/search-mgmt-sdk) |3.0 |Genel Kullanıma Sunuldu |
| [Yönetim REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Genel Kullanıma Sunuldu |

Gibi REST API'leri için `api-version` her çağrıda gereklidir. Kullanarak `api-version` API önizlemesi gibi belirli bir sürümü hedeflemesini daha kolay hale getirir. Aşağıdaki örnekte nasıl `api-version` parametresi belirtildi:

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> Her isteğin olsa da bir `api-version`, tüm API istekleri için aynı sürümü kullanmanızı öneririz. Yeni API sürümlerinde öznitelikler veya önceki sürümleri tarafından tanınmayan işlemler aldığımızda bu özellikle doğrudur. API sürümleri karıştırılması olabilir istenmeyen sonuçları ve kaçınılmalıdır.
>
> Hizmet REST API ve yönetim REST API'si birbirinden tutulur. Herhangi bir benzerlik sürüm numaraları içerik olarak farklı olur.

Genel kullanıma açık (veya GA) API'ler üretimde kullanılabilir ve Azure hizmet düzeyi sözleşmelerine tabidir. Önizleme sürümü için bir GA sürümü her zaman geçirilmez Deneysel özellikler bulunuyor. **Önizleme API'leri üretim uygulamalarında kullanmaktan kaçınmak için kesinlikle önerilir.**

## <a name="about-preview-and-generally-available-versions"></a>Önizleme ve genel kullanıma sunulan sürümleri hakkında
Azure arama her zaman REST API aracılığıyla Deneysel özellikler ilk olarak, ardından .NET SDK'ın yayın öncesi sürümler ile önceden serbest bırakır.

Önizleme özellikleri, test ve deneme, özellik tasarımı ve uygulaması hakkında geri bildirim toplama amacıyla kullanılabilir. Bu nedenle, büyük olasılıkla geriye dönük uyumluluk sonu şekilde zaman içindeki Önizleme özellikleri değiştirebilirsiniz. Bir GA sürümündeki kararlı ve tahmin edilemez küçük geriye dönük olarak uyumlu düzeltmeler ve geliştirmeler hariç olmak üzere değiştirmek özellikler aksine budur. Ayrıca, Önizleme özellikleri her zaman bunu bir GA sürümündeki yapmayın.

Bu nedenle, bir bağımlılık Önizleme sürümlerinde alan üretim kod yazmaya karşı öneririz. Eski bir önizleme sürümünü kullanıyorsanız, genel kullanıma (GA) sürümüne geçiş öneririz.

.NET SDK'sı için: Kod geçiş için yönergeler bulunabilir [.NET SDK'yı yükseltme](search-dotnet-sdk-migration-version-9.md).

Genel kullanılabilirlik, Azure Search artık hizmet düzeyi sözleşmesi (SLA) altında olduğunu gösterir. SLA'sı şu yolda bulunabilir: [Azure Search hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
