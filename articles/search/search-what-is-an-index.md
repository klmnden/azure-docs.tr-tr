---
title: Azure Search'te dizinler | Microsoft Docs
description: "Azure Search'de dizin kavramlarını ve dizinlerin nasıl kullanıldığını öğrenin."
services: search
documentationcenter: 
author: ashmaka
ms.assetid: a395e166-bf2e-4fca-8bfc-116a46c5f7b1
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 11/08/2017
ms.author: ashmaka
ms.openlocfilehash: 87f1121594d8577b5dacac4026aa7d86b2921d10
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="indexes-in-azure-search"></a>Azure Search'te dizinler
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Azure Search'te *dizin*, *belgeler* ve Azure Search hizmeti tarafından kullanılan diğer yapılar için kalıcı bir depodur. Bir belge, dizininizdeki aranabilir verilerin tek bir birimidir. Örneğin, bir e-ticaret satıcısında sattığı her bir öğe için bir belge, bir haber kuruluşunda her bir makale için bir belge, vb. olabilir. Bu kavramları daha çok bilinen veritabanı eşdeğerlerine eşleyen bir *dizin*, kavramsal olarak bir *tabloya* benzer ve *belgeler* de bir tablodaki *satırlarla* kabaca eşdeğerdir.

Azure Search'te belge eklediğinizde/yüklediğinizde ve arama sorguları gönderdiğinizde, isteklerinizi arama hizmetinizdeki belirli bir dizine gönderirsiniz.

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

