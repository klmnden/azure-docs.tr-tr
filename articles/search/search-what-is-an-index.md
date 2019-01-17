---
title: Dizin tanımını ve kavramlar - Azure Search
description: Dizin terimleri ve kavramları ters bir dizinin fiziksel structre dahil olmak üzere Azure Search giriş.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/08/2017
ms.custom: seodec2018
ms.openlocfilehash: 5a39021367c2f51125876081e9174eb372d7b9c9
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353167"
---
# <a name="indexes-and-indexing-overview-in-azure-search"></a>Dizinler ve Azure Search'te dizin oluşturma genel bakış

Azure Search'te bir *dizin* kalıcı bir deposudur *belgeleri* ve Azure Search Hizmeti filtrelenmiş ve tam metin araması için kullanılan diğer yapılar. Bir belge, dizininizdeki aranabilir verilerin tek bir birimidir. Örneğin, bir e-ticaret satıcısında sattığı her bir öğe için bir belge, bir haber kuruluşunda her bir makale için bir belge, vb. olabilir. Bu kavramları daha çok bilinen veritabanı eşdeğerlerine eşleyen bir *dizin*, kavramsal olarak bir *tabloya* benzer ve *belgeler* de bir tablodaki *satırlarla* kabaca eşdeğerdir.

Ekleyin veya belgeleri karşıya yüklemesine veya Azure Search'e arama sorguları gönderme, arama hizmetinizdeki belirli bir dizine istekleri gönderiyorsunuz. Belgeler için bir dizin ekleme işlemini adlı *dizin*.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Bir Azure Search dizinindeki alan türleri ve öznitelikleri
Şemanızı tanımlarken, dizininizdeki her bir alan için ad, tür ve öznitelikler belirtmeniz gerekir. Alan türü, bu alanda depolanan verileri sınıflandırır. Öznitelikler, alanın nasıl kullanıldığını belirtmek için tek tek alanlarda ayarlanır. Aşağıdaki tablolar belirtebileceğiniz türleri ve öznitelikleri numaralandırır.

### <a name="field-types"></a>Alan türleri
| Tür | Açıklama |
| --- | --- |
| *Edm.String* |Tam metin arama için isteğe bağlı olarak belirteç haline getirilebilen metin (sözcük bölünmesi, kök ayırma, vb.). |
| *Collection(Edm.String)* |Tam metin araması için isteğe bağlı olarak belirteç haline getirilebilen dize listesi. Bir koleksiyondaki öğelerin sayısında teorik bir üst sınır yoktur ancak yük boyutundaki 16 MB'lık üst sınır, koleksiyonlar için geçerlidir. |
| *Edm.Boolean* |True/false değerlerini içerir. |
| *Edm.Int32* |32 bit tamsayı değerleri. |
| *Edm.Int64* |64 bit tamsayı değerleri. |
| *Edm.Double* |Çift duyarlıklı sayısal veriler. |
| *Edm.DateTimeOffset* |OData V4 biçiminde (ör. `yyyy-MM-ddTHH:mm:ss.fffZ` ve `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`) temsil edilen tarih ve saat değerleri. |
| *Edm.GeographyPoint* |Dünya üzerindeki bir coğrafi konumu temsil eden bir nokta. |

Azure Search'ün [desteklediği veri türleri hakkında burada](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) daha ayrıntılı bilgiler edinebilirsiniz.

### <a name="field-attributes"></a>Alan öznitelikleri
| Öznitelik | Açıklama |
| --- | --- |
| *Anahtar* |Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| *Alınabilir* |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| *Filtrelenebilir* |Alanın filtre sorgularında kullanılmasını sağlar. |
| *Sıralanabilir* |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| *Modellenebilir* |Kullanıcının bağımsız filtrelemesi için [modellenmiş bir gezinmede](search-faceted-navigation.md) bir alanın kullanılmasını sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler. |
| *Aranabilir* |Alanı tam metin aranabilir şeklinde işaretler. |

Azure Search'ün [dizin öznitelikleri hakkında burada](https://docs.microsoft.com/rest/api/searchservice/Create-Index) daha ayrıntılı bilgiler edinebilirsiniz.

## <a name="guidance-for-defining-an-index-schema"></a>Bir dizin şemasını tanımlama kılavuzu
Dizininizi tasarlarken, her bir kararı düşünmek için planlama aşamasında zaman ayırın. Her bir alan için [uygun öznitelikler](https://docs.microsoft.com/rest/api/searchservice/Create-Index) atanması gerektiğinden, dizininizi tasarlarken arama kullanıcı deneyimini ve iş gereksinimlerinizi göz önünde bulundurmanız önemlidir. Bir dizinin dağıtıldıktan sonra değiştirilmesi, verilerin yeniden oluşturulmasını ve yüklenmesini içerir.

Veri depolama gereksinimleri zamanla değişiyorsa bölüm ekleyerek veya kaldırarak kapasiteyi artırabilir ya da azaltabilirsiniz. Ayrıntılı bilgi için bkz. [Azure'da Search hizmetinizi yönetme](search-manage.md) veya [Hizmet Sınırları](search-limits-quotas-capacity.md).

