---
title: Yapılandırılmış sorgu ifadelerinde bilgi araştırması hizmeti API'si | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'ndeki yapılandırılmış sorgu ifadeleri kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 070ee311a1153bc9fb59870dce68f385a43b15f1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351712"
---
# <a name="structured-query-expression"></a>Yapılandırılmış sorgu ifadesi
Yapılandırılmış sorgu ifadesi bir veri dizinini karşı değerlendirmek için işlemler kümesini belirtir.  Öznitelik sorgu ifadeleri ve üst düzey işlevleri oluşur.  Kullanım [ *değerlendirmek* ](evaluateMethod.md) ifade ile eşleşen nesneleri işlem yöntemi.  2013 yıl itibaren Jaime Teevan tarafından yazılan yayınlar döndürür akademik yayınlar etki alanından bir örnek verilmiştir.

`And(Composite(Author.Name=='jaime teevan'),Y>=2013)`

Yapılandırılmış sorgu ifadeleri öğesinden elde edilebilir [ *yorumlama* ](interpretMethod.md) istekleri, burada her yorumlama anlamsal çıktısı, eşleşen dizin nesneleri döndüren bir yapılandırılmış sorgu ifadesi Giriş doğal dil sorgusu.  Alternatif olarak, bunlar el ile bu bölümde açıklanan sözdizimi kullanılarak yazılmış olabilir.

## <a name="attribute-query-expression"></a>Öznitelik sorgu ifadesi
Bir öznitelik sorgu ifadesi karşı belirli bir özniteliği eşleşmesini temel alan bir nesne tanımlar.  Farklı eşleşen işlemlerini, öznitelik türüne bağlı olarak desteklenir ve dizinli işlemi belirtilen [şema](SchemaFormat.md):

| Tür | İşlem | Örnekler |
|------|-------------|------------|
| Dize | şuna eşittir: | Başlık 'görünmeyen semantik analizi' = (kurallı + eş anlamlıları) |
| Dize | şuna eşittir: | Author.Name=='susan t dumais (kurallı yalnızca)|
| Dize | starts_with | Başlık = 'görünmeyen s'... |
| Int64/Int32/çift | şuna eşittir: | Yıl 2000 = |
| Int64/Int32/çift | starts_with | Yıl '20' =... ("20" ile başlayan ondalık değer içermemeli) |
| Int64/Int32/çift | is_between | Yıl&lt;2000 <br/> Yıl&lt;2000 = <br/> Yıl&gt;2000 <br/> Yıl&gt;2000 = <br/> Year=[2010,2012) *(yalnızca sol sınır değeri içerir: 2010, 2011)* <br/> Yıl [2000,2012] = *(her iki sınır değerleri içerir: 2010, 2011, 2012)* |
| Tarih | şuna eşittir: | Doğum Tarihi ='1984-05-14' |
| Tarih | is_between | Doğum tarihi&lt;=' 03/2008/14' <br/> PublishDate = ['2000-01-01', ' 2009-12-31'] |
| Guid | şuna eşittir: | Kimliği '602DD052-CC47-4B23-A16A-26B52D30C05B' = |


Örneğin, ifade "Title 'görünmeyen s' =...", başlık "görünmeyen s" dizesi ile başlayan tüm akademik yayınları eşleşir.  Bu ifade değerlendirmek için başlık özniteliği şemanın dizin oluşturmak için kullanılan "starts_with" işlemi belirtmeniz gerekir.

İle ilişkili eş anlamlıları öznitelikleri için sorgu ifadesi "==" işleci veya nesneler kendi kurallı/eş anlamlı değerleri "=" işlecini kullanarak eşleştiği kullanarak belirli bir dize, kurallı değerinin eşleşen nesneleri belirtebilir.  Her ikisi de "eşittir" işleci öznitelik tanımı'nda belirtilmesini gerektirir.


## <a name="functions"></a>İşlevler
Yerleşik bir temel özniteliği sorgularından daha karmaşık sorgu ifadeleri yapımı sağlayan işlevler kümesi yok.

### <a name="and-function"></a>Ve işlevi
`And(expr1, expr2)`

İki giriş sorgu ifadeleri kesişimini döndürür.

Aşağıdaki örnek hakkında bilgi alma 2000 yılında yayınlanan akademik yayınlar döndürür:

`And(Year=2000, Keyword=='information retrieval')`

### <a name="or-function"></a>Ya da işlevi
`Or(expr1, expr2)`

İki giriş sorgu ifadeleri birleşimini döndürür.

Aşağıdaki örnek hakkında bilgi alma veya kullanıcı modelleme 2000 yılında yayınlanan akademik yayınlar döndürür:

`And(Year=2000, Or(Keyword='information retrieval', Keyword='user modeling'))`

### <a name="composite-function"></a>Bileşik işlevi
`Composite(expr)`

Ortak bir bileşik özniteliği alt özniteliklerini sorguları iç deyim yalıtan bir ifade oluşan döndürür.  Kapsülleme herhangi eşleşen veri nesnesinin tek tek iç ifade karşılayan en az bir değere sahip bileşik özniteliği gerektirir.  Bileşik özniteliğinin alt özniteliklerde bir sorgu ifadesi diğer sorgu ifadeleri ile birleştirilebilir önce Composite() işlevini kullanarak saklanmasını olduğuna dikkat edin.

Örneğin, kendisinin "microsoft" ederken aşağıdaki deyim tarafından akademik yayınlar "shum harry" döndürür:

```
Composite(And(Author.Name="harry shum", 
              Author.Affiliation="microsoft"))
```

Öte yandan, aşağıdaki ifade, yazarlar birini olduğu akademik yayınlar "shum harry" ve "microsoft" üyelikler, biri döndürür:

```
And(Composite(Author.Name="harry shum"), 
    Composite(Author.Affiliation="microsoft"))
```
