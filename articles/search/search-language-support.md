---
title: "Azure arama çoklu dil | Microsoft Docs"
description: "Azure arama, dil Çözümleyicileri Lucene ve doğal dil işleme teknolojisi Microsoft'tan gelen yararlanarak 56 dilleri destekler."
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: dbbab31bac66ce73dbf9883992713a2c16581e19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Azure Search'te birden çok dilde belgeler için dizin oluşturma
> [!div class="op_single_selector"]
>
> * [Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

Dil Çözümleyicileri gücünü unleashing kadar dizin tanımı aranabilir alanında bulunan bir özellik ayarı olarak kolaydır. Şimdi portal Bu adımda yapabilirsiniz.

Aşağıda Azure arama için bir dizin şemasını tanımlamak kullanıcıların Azure Portal dikey penceresi ekran görüntüleri verilmiştir. Bu dikey pencereden kullanıcılar tüm alanlar oluşturabilir ve bunların her biri için Çözümleyicisi özelliğini ayarlayın.

> [!IMPORTANT]
> Yalnızca bir dil Çözümleyicisi alan tanımı sırasında olarak sıfırdan yeni bir dizin oluşturan oluştururken ya da mevcut bir dizine yeni bir alan ekleyerek ayarlayabilirsiniz. Tüm öznitelikler, alanın oluşturulurken Çözümleyicisi'ni de dahil olmak üzere tam olarak belirttiğinizden emin olun. Öznitelikleri düzenlemek veya değişiklikleri kaydettikten sonra Çözümleyicisi türünü değiştirmek mümkün olmayacaktır.
>
>

## <a name="define-a-new-field-definition"></a>Yeni bir alan tanımı tanımlayın
1. Oturum [Azure portal](https://portal.azure.com) ve search hizmetinizin hizmet dikey penceresini açın.
2. Tıklatın **Ekle dizin** yeni bir dizin başlatmak veya mevcut bir dizine ekleyeceğiniz yeni alanlar bir Çözümleyicisi ayarlamak için varolan bir dizini açmak için hizmet panosunu üst komut çubuğunda.
3. Alanları dikey penceresi görünür, dizin şemasını tanımlamak için seçenekler sunan Çözümleyicisi sekmesinde de dahil olmak üzere bir dil Çözümleyicisi seçmek için kullanılır.
4. Alanlarda alan tanımı sağlayan bir adı, veri türü seçme ve alan tam metin arama sonuçlarında, model Gezinti yapılarda kullanılabilir alınabilir, aranabilir sıralanabilir vb. işaretlemek için özniteliklerini ayarlama başlatın.
5. Sonraki alana taşımadan önce açmak **Çözümleyicisi** sekmesi.

![][1]
*Bir Çözümleyicisi seçmek için alanları dikey Çözümleyicisi sekmesinde Ek Yardım düğmesini tıklatın.*

## <a name="choose-an-analyzer"></a>Bir analyzer'ı seçin
1. Tanımladığınız alanı bulmak için kaydırın.
2. Alan aranabilir olarak işaretlenmiş yapmadıysanız, şimdi olarak işaretlemek için onay kutusuna tıklayın **aranabilir**.
3. Çözümleyici alanı kullanılabilir çözümleyiciler listesini görüntülemek için tıklatın.
4. Kullanmak istediğiniz Çözümleyicisi'ni seçin.

![][2]
*Her bir alan için desteklenen çözümleyiciler birini seçin*

Varsayılan olarak, tüm aranabilir alanları kullanın [standart Lucene Çözümleyicisi](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) dil belirsiz olduğu. Desteklenen çözümleyiciler tam listesini görüntülemek için bkz: [dil desteği Azure Search'te](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Bir alan için dil Çözümleyicisi'ni seçtikten sonra bu alan için dizin oluşturma ve arama her istek ile kullanılır. Bir sorgu farklı çözümleyicilerini kullanarak birden çok alan karşı verildiğinde sorgu bağımsız olarak her bir alan için doğru çözümleyiciler tarafından işlenir.

Birçok web ve mobil uygulamalar kullanıcılar farklı diller kullanan dünya hizmet. Desteklenen her dil için bir alan oluşturarak bu gibi bir senaryo için bir dizin tanımla mümkündür.

![][3]
*Desteklenen her dil için bir açıklama alanı ile dizin tanımı*

Bir sorgu verme aracı dilinin bilinen, arama isteği kullanarak bir özel alanın kapsamlı olabilir **searchFields** sorgu parametresi. Aşağıdaki sorgu, Lehçe açıklamaya karşı yalnızca verilir:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

Dizininizi portalı, sorgu kullanarak **arama Gezgini** yukarıda gösterilene benzer bir sorguda yapıştırın. Arama Gezgini hizmeti dikey komut çubuğunda kullanılabilir. Bkz: [portalda Azure Search dizininizi sorgulama](search-explorer.md) Ayrıntılar için.

Bazen bir sorgu verme aracı dili, bu durumda sorgu karşı tüm alanlar aynı anda verilebilir bilinmiyor. Gerekirse, belirli bir dilde sonuçları için tercih kullanılarak tanımlanabilir [profilleri Puanlama](https://msdn.microsoft.com/library/azure/dn798928.aspx). Aşağıdaki örnekte, İngilizce açıklamasında bulunan eşleşmeler yüksek eşleşmeleri Lehçe ve Fransızca göre belirtmek:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

.NET Geliştirici değilseniz, dil Çözümleyicileri kullanarak yapılandırabileceğinize dikkat edin [Azure Search .NET SDK'sı](http://www.nuget.org/packages/Microsoft.Azure.Search). En son sürüm Microsoft dil Çözümleyicileri de destekler.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
