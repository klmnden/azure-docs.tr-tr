---
title: Sorguları - Azure Search çok dili İngilizce olmayan için dizin arama
description: Azure Search, Lucene ve doğal dil işleme teknolojisinden Microsoft gelen dil Çözümleyicileri yararlanarak 56 dilleri destekler.
author: yahnoosh
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: jlembicz
ms.custom: seodec2018
ms.openlocfilehash: cc9f271c1c79f34ba62fa22d6ce4fd6bf16738f1
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025262"
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Azure Search'te birden çok dilde belgeler için dizin oluşturma
> [!div class="op_single_selector"]
>
> * [Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

Dil Çözümleyicileri gücünü unleashing aranabilir bir alanı dizin tanımında bir özellik ayarı olarak kadar kolay hale gelir. Bu adımda portalı artık bunu yapabilirsiniz.

Aşağıda Azure arama için bir dizin şemasını tanımlamak kullanıcıların Azure portalı dikey pencereleri ekran görüntüleri verilmiştir. Bu dikey pencereden, kullanıcılar tüm alanları oluşturabilir ve bunların her biri için çözümleyici özelliğini ayarlayın.

> [!IMPORTANT]
> Yalnızca bir dil Çözümleyicisi alan tanımı sırasında olarak sıfırdan yeni bir dizin oluşturan oluştururken veya mevcut bir dizine yeni alan ekleme ayarlayabilirsiniz. Tüm öznitelikler, alanın oluşturulurken Çözümleyicisi dahil olmak üzere tam olarak belirttiğinizden emin olun. Öznitelikleri düzenlemek veya değişiklikleri kaydettikten sonra çözümleyici türünü değiştirmek mümkün olmayacaktır.
>
>

## <a name="define-a-new-field-definition"></a>Yeni bir alan tanımı tanımlayın
1. Oturum [Azure portalında](https://portal.azure.com) ve search hizmetinizin hizmet dikey penceresini açın.
2. Tıklayın **dizin** yeni bir dizin başlatmak veya mevcut bir dizine eklediğiniz yeni alanları bir çözümleyici ayarlamak için varolan bir dizini açmak için hizmet panosuna üst kısmındaki komut çubuğunda.
3. Alanları dikey penceresi görünür ve dizin şemasını tanımlamak için seçenekler sunan Çözümleyicisi sekmesinde de dahil olmak üzere bir dil Çözümleyicisi seçmek için kullanılır.
4. Alanları, bir ad sağlamayı, veri türünü seçme ve öznitelikleri ayarlarıyla alan tam metin aranabilir, arama sonuçlarında, model Gezinti yapılarda kullanılabilir alınabilir sıralanabilir ve benzeri işaretlemek için bir alan tanımı başlatın.
5. Sonraki alana geçmeden önce açık **Çözümleyicisi** sekmesi.

![][1]
*Bir çözümleyici seçilecek alanlar dikey Çözümleyicisi sekmesini tıklatın.*

## <a name="choose-an-analyzer"></a>Bir çözümleyici seçin
1. Tanımladığınız alanı bulmak için kaydırın.
2. Alan aranabilir olarak işaretliyse yüklemediyseniz şimdi olarak işaretlemek için onay kutusuna tıklayın **aranabilir**.
3. Çözümleyici alanın kullanılabilir Çözümleyicileri listesini görüntülemek için tıklayın.
4. Kullanmak istediğiniz çözümleyiciyi seçin.

![][2]
*Her bir alan için desteklenen Çözümleyicileri birini seçin*

Varsayılan olarak, tüm aranabilir alanları kullanın [standart olarak Lucene çözümleyici](https://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) dil belirsiz olduğu. Desteklenen Çözümleyicileri tam listesini görüntülemek için bkz: [Azure Arama'da dil desteği](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Bir alan için bir dil Çözümleyicisi seçtikten sonra bu alan için dizin oluşturma ve arama her istekle kullanılır. Bir sorgu farklı çözümleyicilerini kullanarak birden çok alan karşı verildiğinde sorgu bağımsız olarak her alan için doğru çözümleyiciler tarafından işlenir.

Kullanıcılar farklı dilleri kullanarak dünya çapındaki çok sayıda web ve mobil uygulamaları hizmet. Desteklenen her dil için bir alan oluşturarak bu gibi bir senaryo için dizin tanımlamak mümkündür.

![][3]
*Desteklenen her dil için bir açıklama alanı ile dizin tanımı*

Bir sorgu verme aracı dili biliniyorsa, arama isteği kullanarak bir özel alan kapsamlandırılabilir **searchFields** sorgu parametresi. Aşağıdaki sorguda yalnızca Lehçe açıklamaya karşı verilir:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2019-05-06`

Dizininizi portalından sorgulayabilirsiniz kullanarak **arama Gezgini** yukarıda gösterilene benzer bir sorgu içinde yapıştırın. Arama Gezgini hizmet dikey penceresinde komut çubuğundan kullanılabilir. Bkz: [portalda Azure Search dizininizi sorgulama](search-explorer.md) Ayrıntılar için.

Bazen bir sorgu verme aracı dili, bu durumda sorgu karşı tüm alanlar aynı anda verilebilmesi için bilinmiyor. Gerekirse, belirli bir dilde sonuçları tercih kullanılarak tanımlanabilir [Puanlama profilleri](https://msdn.microsoft.com/library/azure/dn798928.aspx). Aşağıdaki örnekte, İngilizce açıklamada bulunan eşleşen Lehçe ve Fransızca eşleşme göre daha yüksek puanlanması:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2019-05-06`

.NET geliştiricisiyseniz, dil Çözümleyicileri kullanarak yapılandırabileceğinize dikkat edin [Azure Search .NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Search). En son sürümü Microsoft dil Çözümleyicileri de destekler.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
