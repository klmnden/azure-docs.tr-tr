---
title: Bilgi Bankası araştırması hizmeti API'si şema biçiminde | Microsoft Docs
description: Şema biçimi içinde bilgi araştırması hizmet (KES) API Bilişsel Services hakkında bilgi edinin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 3009392a5acb12a8f4df3d30a2cbe5e74f2172fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351725"
---
# <a name="schema-format"></a>Şema biçimi
Şema nesneleri dizini oluşturmak için kullanılan veri dosyasında özniteliği yapısını açıklayan bir JSON dosyası belirtildi.  Her öznitelik için şema adı, veri türü, isteğe bağlı işlemleri ve isteğe bağlı eş anlamlı sözcükleri listesini belirtir.  Bir nesne, 0 veya daha fazla değer her özniteliğin olabilir.  Akademik yayın etki alanından basitleştirilmiş bir örnek aşağıdadır:

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

Öznitelik adları bir harfle başlamalı ve yalnızca harf (A-Z), sayılar (0-9) oluşur ve alt çizgi büyük küçük harfe duyarlı tanımlayıcıları olan (\_).  Ayrılmış "logprob" özniteliği nesneleri arasındaki göreli doğal günlük olasılıklar belirtmek için kullanılır.

## <a name="attribute-type"></a>Öznitelik Türü
Desteklenen öznitelik veri türlerinin listesi aşağıdadır:

| Tür | Açıklama | İşlemler | Örnek |
|------|-------------|------------|---------|
| Dize | Dize (1-1024 karakter) | eşittir, starts_with | "hello world" |
| Int32 | İşaretli 32 bit tam sayı | eşit, starts_with, is_between | 2016 |
| Int64 | İşaretli 64 bit tam sayı | eşit, starts_with, is_between | 9876543210 |
| çift | Çift duyarlıklı kayan noktalı değeri | eşit, starts_with, is_between | 1.602e-19 |
| Tarih | Tarih (1400-01-01-9999-12-31) | eşittir, is_between | ' 2016-03-14' |
| Guid | Genel benzersiz tanımlayıcı | şuna eşittir: | "602DD052-CC47-4B23-A16A-26B52D30C05B" |
| Blob | Dahili olarak sıkıştırılmış dizine olmayan verileri | *Yok* | "Her kişi ve her kuruluş gezegen hakkında daha fazla elde etmek için güç kazandırma" |
| Bileşik | Birden çok alt öznitelik bileşimi| *Yok* | {"Name": "harry shum", "Bağlantı": "microsoft"} |

Dize öznitelikler, kullanıcı sorgusu bir parçası olarak görünebilir dize değerlerini göstermek için kullanılır.  Tam eşleşme destekledikleri *eşittir* işlemi, yanı sıra *starts_with* "micros" "microsoft" ile eşleşen gibi sorgu tamamlama senaryolar için işlemi.  Büyük küçük harf duyarsız ve benzer yazım hataları işlemek için eşleşen bir sonraki sürümde desteklenmez.

Int64/Int32/çift öznitelikler sayısal değerleri temsil etmek için kullanılır.  *İs_between* işlemi, çalışma zamanında eşitsizlik desteği (lt, le, gt, ge) sağlar.  *Starts_with* işlemi destekler "20.", "2016" ile eşleşen gibi sorgu tamamlama senaryolar ya da "3." "3.14 ile".

Tarih öznitelikleri verimli bir şekilde tarih değerlerini kodlamak için kullanılır.  *İs_between* işlemi, çalışma zamanında eşitsizlik desteği (lt, le, gt, ge) sağlar.
  
GUID öznitelikleri verimli bir şekilde desteğiyle varsayılan GUID değerleri temsil etmek için kullanılan *eşittir* işlemi.

BLOB öznitelikler, potansiyel olarak büyük veri BLOB'ları karşılık gelen nesnesinden blob değerlerinin içeriğine göre herhangi bir dizin oluşturma işlemi desteği olmadan çalışma zamanı arama için verimli bir şekilde kodlamak için kullanılır.

### <a name="composite-attributes"></a>Bileşik öznitelikleri
Bileşik öznitelikler, öznitelik değerleri gruplandırması göstermek için kullanılır.  Her alt özniteliğinin adı başlatır ve ardından bileşik öznitelik adı ".".  Bileşik öznitelikleri için değerleri, iç içe geçmiş öznitelik değerlerini içeren bir JSON nesnesi olarak belirtilir.  Bileşik öznitelikleri birden çok nesne değerlerine sahip olabilir.  Ancak, bileşik öznitelikleri kendileri bileşik öznitelikleri alt öznitelikleri olmayabilir.

Akademik yayın yukarıdaki örnekte, bu kendisinin "microsoft" iken tarafından yazıları için "shum harry" sorgulamak hizmet sağlar.  Bileşik öznitelikleri hizmet yalnızca yazarlar birini olduğu yazıları "shum harry" ve "microsoft" yazarların, biri sorgulayabilirsiniz.  Daha fazla bilgi için bkz: [bileşik sorguları](SemanticInterpretation.md#composite-function).

## <a name="attribute-operations"></a>Öznitelik işlemleri
Varsayılan olarak, her özniteliği öznitelik veri türü için kullanılabilir tüm işlemleri desteklemek üzere dizine alınır.  Belirli bir işlem gerekli değilse, dizinli işlemler kümesini açıkça dizin boyutunu azaltmak için belirtilebilir.  Yukarıdaki örnek şemadan aşağıdaki parçacığında, yalnızca desteklemek için Author.Id özniteliği dizinlenir *eşittir* işlem ancak değil ek *starts_with* ve *is_between*  Int32 öznitelikler için işlemleri.
```json
{"name":"Author.Id", "type":"Int32", "operations":["equals"]}
```

Öznitelik bir dilbilgisi içinde başvurulduğunda *starts_with* işlemi hizmetinin tamamlamalar kısmi sorgularından oluşturmak sırayla şemadaki belirtilmesi gerekiyor.  

## <a name="attribute-synonyms"></a>Öznitelik anlamlıları
Genellikle, bir eş tarafından belirli dize özniteliği değeri başvurmak için tercih edilir.  Örneğin, kullanıcılar "MSFT" veya "MS" olarak "Microsoft" bakabilirsiniz.  Bu durumlarda, öznitelik tanımı şema dosyası ile aynı dizinde bulunan bir şema dosyası adını belirtebilirsiniz.  Eş anlamlı dosya her satırda bir eş anlamlı girişi aşağıdaki JSON biçiminde temsil eder: `["<canonical>", "<synonym>"]`.  Örnek şemada "AuthorName.syn" yazar.adi özniteliği için eş anlamlı değerleri içeren bir JSON dosyasıdır.

`{"name":"Author.Name", "type":"String", "synonyms":"AuthorName.syn"}`


Eş anlamlıları kurallı değerine sahip olabilir.  Birden çok kurallı değerlere ortak eşanlamlısı paylaşabilir.  Böyle durumlarda, hizmet üzerinden birden çok yorumlar karışıklığı çözmek.  Aşağıda bir örnek AuthorName.syn eş anlamlıları dosyası yukarıdaki şemayı karşılık gelen:
```json
["harry shum","heung-yeung shum"]
["harry shum","h shum"]
["henry shum","h shum"]
...
```
