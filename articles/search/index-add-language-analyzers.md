---
title: Dil Çözümleyicileri - Azure Search Ekle
description: Çok dilli sözcük metin analizi İngilizce olmayan sorguları ve Azure Search'te dizinler.
ms.date: 02/14/2019
services: search
ms.service: search
ms.topic: conceptual
author: Yahnoosh
ms.author: jlembicz
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: deea16b8670623acd2ae92ba62f579f5474d12ec
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790896"
---
# <a name="add-language-analyzers-to-an-azure-search-index"></a>Dil Çözümleyicileri için bir Azure Search dizini Ekle

A *dil Çözümleyicisi* belirli bir tür [metin Çözümleyicisi](search-analyzers.md) dilsel kurallar hedef dili kullanarak sözcük temelli analiz gerçekleştirir. Aranabilir alan her bir **Çözümleyicisi** özelliği. Dizininizi İngilizce ve Çince metin için ayrı alanlara gibi çevrilmiş dizeleri içeriyorsa, zengin dil özelliklerini bu Çözümleyicileri erişmek için her bir alanına dil Çözümleyicileri belirtebilirsiniz.  

Azure Search, Lucene tarafından desteklenen 35 Çözümleyicileri ve özel Microsoft doğal dil işleme Office ve Bing kullanılan teknoloji tarafından desteklenen 50 Çözümleyicileri destekler.

## <a name="comparing-analyzers"></a>Çözümleyiciler karşılaştırma

Bazı geliştiriciler Lucene daha tanıdık, basit, açık kaynaklı çözüm tercih edebilirsiniz. Lucene dil çözümleyicilerini daha hızlıdır, ancak Microsoft Çözümleyicileri başsözcüğe, sözcük (gibi dillerde Almanca, Danca, Felemenkçe, İsveççe, Norveççe, Estonca, bitiş, Macarca, Slovakya) decompounding ve varlık gibi özellikler Gelişmiş tanıma (URL'ler, e-postaları, tarih, sayı). Mümkünse, hangisinin daha uygun olduğuna karar vermek üzere hem Microsoft hem de Lucene çözümleyici karşılaştırmalarını çalıştırmanız gerekir. 

Microsoft çözümleyicileriyle dizin ortalama iki ila üç kat daha yavaş dile bağlı olarak Lucene eşdeğerlerine daha açıktır. Arama performansını önemli ölçüde ortalama boyutu sorgularında etkilenmemesi gerekir. 

### <a name="english-analyzers"></a>İngilizce çözümleyiciler

Varsayılan çözümleyici için İngilizce, ancak belki de değil yanı sıra Lucene'nın İngilizce analyzer'ı veya Microsoft'un İngilizce Çözümleyicisi düşünülerek standart Lucene kullanılır. 
 
+ Lucene'nın İngilizce Çözümleyicisi standart Çözümleyicisi genişletir. Sözcük (ın sondaki) iyelik kaldırır, bağlantısı dallanma algoritması göre dallanma uygular ve İngilizce durdurma sözcükleri kaldırır.  

+ Microsoft'un İngilizce Çözümleyicisi dallanma yerine başsözcüğe gerçekleştirir. Bu çok daha iyi ne daha ilgili arama sonuçlarında sonuçları bükümlü ve düzensiz sözcük biçimlerini işleyebileceği anlamına gelir. 

## <a name="configuring-analyzers"></a>Çözümleyicilerini yapılandırma

Dil Çözümleyicileri olarak kullanılan-olduğu. Dizin tanımındaki her alan için ayarladığınız **Çözümleyicisi** özelliğini dil ve linguistik yığını (Microsoft veya Lucene) belirten bir çözümleyici ad. Aynı analyzer, dizin oluşturma ve bu alan için arama yaparken uygulanır. Örneğin, yan yana aynı dizinde mevcut İngilizce, Fransızca ve İspanyolca otel açıklamaları için ayrı alanlara sahip olabilir. Alternatif olarak, yerine, **Çözümleyicisi**, kullanabileceğiniz **indexAnalyzer** ve **searchAnalyzer** saati ve sorgu saati dizin oluşturma sırasında farklı analiz kuralları için. 

Kullanım **searchFields** sorgu parametresi sorgularınızdaki arama için dile özgü alanı belirtmek için. Çözümleyici özelliğini içeren sorgu örnekleri inceleyebilirsiniz [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents). 

Dizin özellikleri hakkında daha fazla bilgi için bkz: [Create Index &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index). Azure Search'te çözümleme hakkında daha fazla bilgi için bkz. [Azure Search'te çözümleyiciler](https://docs.microsoft.com/azure/search/search-analyzers).

<a name="language-analyzer-list"></a>

## <a name="language-analyzer-list"></a>Dil Çözümleyicisi listesi 
 Lucene ve çözümleyici adları ile birlikte desteklenen dillerin listesi aşağıda verilmiştir.  

|Dil|Microsoft Çözümleyicisi adı|Lucene çözümleyici adı|  
|--------------|-----------------------------|--------------------------|  
|Arapça|ar.Microsoft|ar.lucene|  
|Ermenice||hy.lucene|  
|Bangla|bn.microsoft||  
|Bask dili||eu.lucene|  
|Bulgarca|bg.microsoft|BG.lucene|  
|Katalanca|ca.microsoft|CA.lucene|  
|Çince Basitleştirilmiş|zh-Hans.microsoft|zh-Hans.lucene|  
|Geleneksel Çince|zh-Hant.microsoft|zh-Hant.lucene|  
|Hırvatça|hr.microsoft||  
|Çekçe|cs.microsoft|cs.lucene|  
|Danca|da.microsoft|da.lucene|  
|Felemenkçe|nl.microsoft|NL.lucene|  
|İngilizce|en.microsoft|en.lucene|  
|Estonca|et.microsoft||  
|Fince|fi.microsoft|Fi.lucene|  
|Fransızca |fr.microsoft|fr.lucene|  
|Galiçya dili||GL.lucene|  
|Almanca |de.microsoft|de.lucene|  
|Yunanca|el.microsoft|el.lucene|  
|Gucerat dili|gu.microsoft||  
|İbranice|he.microsoft||  
|Hintçe|hi.microsoft|Hi.lucene|  
|Macarca|hu.microsoft|hu.lucene|  
|İzlanda dili|is.microsoft||  
|Endonezya dili (Bahasa)|id.microsoft|id.lucene|  
|İrlanda dili||GA.lucene|  
|İtalyanca |it.microsoft|it.lucene|  
|Japonca|ja.microsoft|ja.lucene|  
|Kannada dili|kn.microsoft||  
|Korece|ko.microsoft|Ko.lucene|  
|Letonca|lv.microsoft|LV.lucene|  
|Litvanca|lt.microsoft||  
|Malayalam dili|ml.microsoft||  
|Malay Dili (Latin)|ms.microsoft||  
|Marathi dili|mr.microsoft||  
|Norveççe|nb.microsoft|No.lucene|  
|Farsça||FA.lucene|  
|Lehçe|pl.microsoft|PL.lucene|  
|Portekizce (Brezilya)|pt-Br.microsoft|PT Br.lucene|  
|Portekizce (Portekiz)|pt-Pt.microsoft|PT Pt.lucene|  
|Pencap dili|pa.microsoft||  
|Rumence|ro.microsoft|Ro.lucene|  
|Rusça|ru.microsoft|RU.lucene|  
|Sırpça (Kiril)|SR-cyrillic.microsoft||  
|Sırpça (Latin)|sr-latin.microsoft||  
|Slovakça|sk.microsoft||  
|Slovence|sl.microsoft||  
|İspanyolca |es.microsoft|Es.lucene|  
|İsveççe|sv.microsoft|sv.lucene|  
|Tamil dili|ta.microsoft||  
|Telugu dili|te.microsoft||  
|Tay Dili|th.microsoft|TH.lucene|  
|Türkçe|tr.microsoft|tr.lucene|  
|Ukrayna dili|uk.microsoft||  
|Urduca|ur.microsoft||  
|Vietnam dili|vi.microsoft||  

 Tüm Çözümleyicileri ile açıklanan adlarla **Lucene** tarafından desteklenen [Apache Lucene'nın dil Çözümleyicileri](https://lucene.apache.org/core/4_9_0/core/overview-summary.html ).

## <a name="see-also"></a>Ayrıca bkz.  
 [Dizin oluşturma &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
 [AnalyzerName sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  
 [Video: Azure arama MVA sunu modül 7](https://channel9.msdn.com/Series/Adding-Microsoft-Azure-Search-to-Your-Websites-and-Apps/07).  

