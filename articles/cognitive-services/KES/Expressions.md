---
title: Yapılandırılmış sorgu ifadeleri - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Bilgi keşfetme hizmeti (KES içinde) API yapılandırılmış sorgu ifadeleri kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: a544cdca1ef4be56fcf368a39040f4ee85076a9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60815107"
---
# <a name="structured-query-expression"></a>Yapılandırılmış sorgu ifadesi

Bir yapılandırılmış sorgu ifadesi bir veri dizinine göre değerlendirilecek işlemleri belirtir.  Bu öznitelik sorgu ifadeleri ve daha üst düzey işlevler oluşur.  Kullanım [ *değerlendirmek* ](evaluateMethod.md) ifade ile eşleşen nesneleri hesaplamak için yöntemi.  2013 seneden beri Jaime Teevan tarafından yazılan yayınlar döndüren akademik yayınlar etki alanından bir örnek verilmiştir.

`And(Composite(Author.Name=='jaime teevan'),Y>=2013)`

Yapılandırılmış sorgu ifadeleri öğesinden alınan [ *yorumlama* ](interpretMethod.md) istekleri, burada her yorumu anlam çıktısı, eşleşen dizin nesneleri döndüren bir yapılandırılmış sorgu ifadesi Giriş doğal dil sorgusu.  Alternatif olarak, bunlar el ile bu bölümde açıklanan söz dizimi kullanılarak yazılmış olabilir.

## <a name="attribute-query-expression"></a>Öznitelik sorgu ifadesi

Bir öznitelik sorgu ifadesi karşı belirli bir öznitelik eşleşmesi temeline göre bir nesne tanımlar.  Farklı eşleşen işlemlerini, öznitelik türüne bağlı olarak desteklenir ve dizinli işlemi belirtilen [şema](SchemaFormat.md):

| Tür | İşlem | Örnekler |
|------|-------------|------------|
| String | eşittir | Başlık 'görünmeyen anlam çözümleme' = (kurallı + eş anlamlılar) |
| String | eşittir | Author.Name=='susan t dumais (kurallı yalnızca)|
| String | starts_with | Başlık 'görünmeyen s' =... |
| Int64/Int32/çift | eşittir | Yıl 2000 = |
| Int64/Int32/çift | starts_with | Yıl = '20'... ("20" ile başlayan ondalık değer içermemeli) |
| Int64/Int32/çift | is_between | Yıl&lt;2000 <br/> Yıl&lt;2000 = <br/> Yıl&gt;2000 <br/> Yıl&gt;2000 = <br/> Year=[2010,2012) *(yalnızca sol sınır değeri içerir: 2010, 2011)* <br/> Yıl [2000,2012] = *(her iki sınır değerleri içerir: 2010, 2011, 2012)* |
| Tarih | eşittir | Doğum tarihini ='1984-05-14' |
| Tarih | is_between | Doğum tarihi&lt;=' 2008/03/14' <br/> PublishDate=['2000-01-01','2009-12-31'] |
| Guid | eşittir | Id='602DD052-CC47-4B23-A16A-26B52D30C05B' |


Örneğin, ifade "Title 'görünmeyen s' =..." ayarlanmış başlık "görünmeyen s" dizesi ile başlayan tüm akademik yayınları eşleşir.  Bu ifadeyi değerlendirmek için ' % s'özniteliği başlık şemanın dizin oluşturmak için kullanılan "starts_with" işlemi belirtmeniz gerekir.

İlişkili eş anlamlı sözcüklerle öznitelikler için kendi kurallı/eş anlamlı değerlerden herhangi birini "=" işlecini kullanarak eşleştiği "==" işleci veya nesneleri kullanarak belirli bir dize kurallı değeri ile eşleşen nesnelerini bir sorgu ifadesi belirtebilirsiniz.  Her ikisi de "eşittir" işleci öznitelik tanımını belirtilmesini gerektirir.


## <a name="functions"></a>İşlevler

Yerleşik işlevleri temel öznitelik sorgulardan daha karmaşık sorgu ifadeleri oluşumu sağlayan birtakım yoktur.

### <a name="and-function"></a>Ve işlevi

`And(expr1, expr2)`

İki giriş sorgu ifadeleri kesişimini döndürür.

Aşağıdaki örnek, akademik yayınlar hakkında bilgi alma 2000 yılında yayınlanan döndürür:

`And(Year=2000, Keyword=='information retrieval')`

### <a name="or-function"></a>Veya işlevi

`Or(expr1, expr2)`

İki giriş sorgu ifadeleri birleşimini döndürür.

Aşağıdaki örnek, akademik yayınlar hakkında bilgi alma veya kullanıcı modelleme 2000 yılında yayınlanan döndürür:

`And(Year=2000, Or(Keyword='information retrieval', Keyword='user modeling'))`

### <a name="composite-function"></a>Bileşik işlevi

`Composite(expr)`

Ortak bir bileşik özniteliği alt özniteliklerini sorguları iç deyim kapsülleyen bir ifade oluşan döndürür.  Kapsülleme iç ifade tek tek karşılayan en az bir değere sahip tüm eşleşen veri nesne bileşik özniteliğini gerektirir.  Bileşik özniteliği alt özniteliklerde bir sorgu ifadesinden önce diğer sorgu ifadeleri ile birleştirilebilir Composite() işlevini kullanarak kapsüllenmiş olduğuna dikkat edin.

He "microsoft" durumundayken göre akademik yayınlar "harry shum" Örneğin, aşağıdaki ifade döndürür:

```
Composite(And(Author.Name="harry shum", 
              Author.Affiliation="microsoft"))
```

Öte yandan, aşağıdaki ifade, akademik yayınlar burada yazarlar, "harry shum" ve "microsoft" ise ilişkileri birini döndürür:

```
And(Composite(Author.Name="harry shum"), 
    Composite(Author.Affiliation="microsoft"))
```
