---
title: Şema biçimi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Şema biçimi, bilgi keşfetme hizmeti (KES) API hakkında daha fazla bilgi edinin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 51a812762659bcc67762b82e9c120772065aab53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814404"
---
# <a name="schema-format"></a>Şema biçimi

Şema nesneleri dizini oluşturmak için kullanılan veri dosyasındaki özniteliği yapısını açıklayan bir JSON dosyasında belirtilir.  Her öznitelik için şema adı, veri türü, isteğe bağlı işlem ve isteğe bağlı bir eş anlamlı sözcükler listesi belirtir.  0 veya daha fazla değer her özniteliğin bir nesne olabilir.  Akademik yayın etki alanındaki basitleştirilmiş bir örnek aşağıda verilmiştir:

``` json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Öznitelik adları olduğundan, bir harf ile başlamalı ve yalnızca harf (A-Z), sayılar (0-9) oluşur ve alt çizgi büyük küçük harf duyarlı tanımlayıcıları (\_).  Ayrılmış "logprob" öznitelik nesneleri arasındaki göreli doğal logaritmayı olasılıklar belirtmek için kullanılır.

## <a name="attribute-type"></a>Öznitelik Türü

Desteklenen öznitelik veri türlerinin bir listesi aşağıdadır:

| Tür | Açıklama | İşlemler | Örnek |
|------|-------------|------------|---------|
| `String` | Dize (1-1024 karakter) | Equals, starts_with | "hello world" |
| `Int32` | İşaretli 32 bit tam sayı | is_between starts_with, eşittir | 2016 |
| `Int64` | İşaretli 64 bit tam sayı | is_between starts_with, eşittir | 9876543210 |
| `Double` | Çift duyarlıklı kayan nokta değeri | is_between starts_with, eşittir | 1.602e-19 |
| `Date` | Tarih (1400-01-01-9999-12-31) | Equals, is_between | '2016-03-14' |
| `Guid` | Genel benzersiz tanıtıcısı | şuna eşittir: | "602DD052-CC47-4B23-A16A-26B52D30C05B" |
| `Blob` | Dahili olarak sıkıştırılmış veri dizini oluşturulmamış | *Yok.* | "Her kişi ve her kuruluşun gezegendeki daha fazlasını başarmak için güçlendirin" |
| `Composite` | Birden çok alt öznitelikler oluşturma| *Yok* | {"Name": "harry shum", "Bağlantı": "microsoft"} |

Dize öznitelikler kullanıcı sorgunun bir parçası görünebilir dize değerleri temsil etmek için kullanılır.  Tam eşleşme destekledikleri *eşittir* işlemi, hem de *starts_with* işlemi "micros", "microsoft" ile eşleşen gibi sorgu tamamlama senaryolar için.  Yazım hatalarını işlemek için büyük/küçük harfe ve benzer eşleştirme, gelecekteki bir sürümde desteklenecek.

Int64/Int32/çift öznitelikler, sayısal değerleri temsil etmek için kullanılır.  *İs_between* işlemi, çalışma zamanında eşitsizlik desteği (lt, le, gt, ge) sağlar.  *Starts_with* işlemi destekler "20", "2016" ile eşleşen gibi sorgu tamamlama senaryoları veya "3." "3.14 ile".

Tarih öznitelikleri tarih değerlerinin verimli bir şekilde kodlama için kullanılır.  *İs_between* işlemi, çalışma zamanında eşitsizlik desteği (lt, le, gt, ge) sağlar.
  
GUID öznitelikleri verimli bir şekilde varsayılan desteğiyle GUID değerleri temsil etmek için kullanılan *eşittir* işlemi.

BLOB öznitelikler, potansiyel olarak büyük veri BLOB'ları için blob değerleri içeriğine göre herhangi bir dizin oluşturma işlemi için desteği olmadan karşılık gelen nesne çalışma zamanı aramasından verimli bir şekilde kodlamak için kullanılır.

### <a name="composite-attributes"></a>Bileşik öznitelikleri

Bileşik öznitelikler, bir gruplandırma öznitelik değerleri temsil etmek için kullanılır.  Her alt özniteliğin adı arkasından bir bileşik öznitelik adı ile başlayan ".".  Bileşik özniteliklerinin değerleri, iç içe geçmiş öznitelik değerlerini içeren bir JSON nesnesi olarak belirtilir.  Bileşik öznitelikleri, birden çok nesne değerlerine sahip olabilir.  Ancak, bileşik öznitelikler kendilerini bileşik öznitelikleri alt öznitelikleri olmayabilir.

Akademik yayın yukarıdaki örnekte, bu he "microsoft" iken tarafından incelemeler "harry shum" sorgulamak bir hizmet sağlar.  Bileşik öznitelikleri hizmeti yalnızca raporlar burada yazarlar, "harry shum" ve "microsoft" yazarlar, biridir sorgulayabilirsiniz.  Daha fazla bilgi için [bileşik sorguları](SemanticInterpretation.md#composite-function).

## <a name="attribute-operations"></a>Öznitelik işlemleri

Varsayılan olarak, her özniteliği öznitelik veri türü için kullanılabilir tüm işlemleri desteklemek için dizine alınır.  Belirli bir işlem gerekli değilse, dizi dizini oluşturulmuş işlemlerini açıkça dizinin boyutunu azaltmak için belirtilebilir.  Aşağıdaki kod parçacığında Yukarıdaki örnek şemadan Author.Id özniteliği yalnızca desteklemek üzere dizine alınır *eşittir* işlemi ancak değil ek *starts_with* ve *is_between*  işlemleri için Int32 öznitelikleri.
```json
{"name":"Author.Id", "type":"Int32", "operations":["equals"]}
```

Öznitelik bir dilbilgisi içinde başvurulduğunda *starts_with* işlem hizmetinin tamamlamaları kısmi sorguları oluşturmak sırayla şemada belirtilmesi gerekiyor.  

## <a name="attribute-synonyms"></a>Öznitelik eş anlamlı sözcükler

Genellikle belirli dize özniteliği değeri bir anlamlının başvuruda bulunmak için tercih edilir.  Örneğin, kullanıcılar, "MSFT" veya "MS" olarak "Microsoft" başvurabilir.  Bu durumlarda, öznitelik tanımı şema dosyası ile aynı dizinde bulunan bir şema dosyasının adını belirtebilirsiniz.  Eş anlamlı dosya her satırda bir eş anlamlı girişi aşağıdaki JSON biçiminde temsil eder: `["<canonical>", "<synonym>"]`.  Örnek Şeması'nda, "AuthorName.syn" yazar.adi özniteliği için eş değerleri içeren bir JSON dosyasıdır.

`{"name":"Author.Name", "type":"String", "synonyms":"AuthorName.syn"}`


Birden çok eş anlamlılar kurallı bir değer olabilir.  Birden çok kurallı değerlere, ortak bir eş anlamlı paylaşabilir.  Bu gibi durumlarda, hizmet üzerinden birden çok yorumlaması belirsizlik çözülecektir.  Aşağıdaki örnek bir AuthorName.syn eş anlamlılar dosyası yukarıdaki şemaya karşılık gelip gelmediği:
```json
["harry shum","heung-yeung shum"]
["harry shum","h shum"]
["henry shum","h shum"]
...
```
