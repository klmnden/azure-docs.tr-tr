---
title: Dizin oluşturucular - Azure Search kullanarak dizin oluşturma için alan eşlemelerini otomatik
description: Alan adları ve veri gösterimleri farklılıkları dikkate almak için Azure Search dizin oluşturucu alan eşlemelerini yapılandırın.
ms.date: 05/02/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 3546e342b535a122ea4ed3f844cd5e28a76d551a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147803"
---
# <a name="field-mappings-and-transformations-using-azure-search-indexers"></a>Alan eşlemelerini ve Azure Search dizin oluşturucuyu kullanarak dönüştürmeler

Azure Search dizin oluşturucularında kullanılırken, giriş verilerinin şemasını hedef dizininizi oldukça eşleşmiyor bazen bulun. Bu gibi durumlarda, kullandığınız **alan eşlemeleri** dizin oluşturma işlemi sırasında verilerinizi şekillendirmek için.

Alan eşlemelerini yararlı olduğu bazı durumlar:

* Adlı bir alan veri kaynağınızda bulunan `_id`, ancak Azure Search, bir alt çizgiyle başlayan alan adları izin vermez. Alan eşlemeyi etkili bir şekilde alanı yeniden adlandırma olanak sağlar.
* Aynı veri kaynağına veri dizinden bazı alanları doldurmak istersiniz. Örneğin, bu alanlara farklı Çözümleyicileri uygulamak isteyebilirsiniz.
* Bir dizin alanı birden fazla veri kaynağından alınan verilerle doldurmak için kullanmak istediğiniz ve veri kaynakları her farklı alan adları kullanın.
* İhtiyaç Base64 kodlama veya kod çözme verilerinizi. Alan eşlemelerini desteği birkaç **işlevleri eşleme**, Base64 için kodlama ve kodunu çözme dahil olmak üzere işlevleri.

> [!NOTE]
> Azure Search dizin oluşturucularında alan eşleme özelliği, veri dönüştürme için birkaç seçenek ile dizin alanları veri alanları eşleştirmek için basit bir yol sağlar. Daha karmaşık veri dizini için kolay bir forma şekillendirmek için ön işleme gerektirebilir.
>
> Microsoft Azure Data Factory içeri aktarma ve veri dönüştürme için bir güçlü bulut tabanlı çözümüdür. Ayrıca, dizin oluşturma durdurulmadan önce kaynak verileri dönüştürmek için kod yazabilirsiniz. Kod örnekleri için bkz: [ilişkisel verileri modelleme](search-example-adventureworks-modeling.md) ve [çok düzeyli modeller Model](search-example-adventureworks-multilevel-faceting.md).
>

## <a name="set-up-field-mappings"></a>Alan eşlemelerini ayarlamak

Bir alan eşlemesi, üç bölümden oluşur:

1. A `sourceFieldName`, veri kaynağınızdaki bir alanı temsil eder. Bu özellik gereklidir.
2. İsteğe bağlı `targetFieldName`, arama dizininizdeki bir alanı temsil eder. Atlanırsa, veri kaynağına olduğu gibi aynı adı kullanılır.
3. İsteğe bağlı `mappingFunction`, önceden tanımlanmış İşlevler, birkaç birini kullanarak verilerinizi dönüştürebilirsiniz. İşlevlerin tam listesi [aşağıda](#mappingFunctions).

Alan eşlemelerini eklenir `fieldMappings` dizin oluşturucu tanımı dizisi.

## <a name="map-fields-using-the-rest-api"></a>REST API kullanarak alanlarını eşleme

Alan eşlemelerini kullanarak yeni bir dizin oluşturucu oluştururken ekleyebilirsiniz [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-Indexer) API isteği. Alan eşlemelerini kullanarak varolan bir dizin oluşturucu, yönettiğiniz [güncelleştirme dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/update-indexer) API isteği.

Örneğin, bir hedef alana farklı bir ada sahip bir kaynak alan eşleme şöyledir:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

Kaynak alan üzerinde birden çok alan eşlemeleri başvurulabilir. Aşağıdaki örnek, "aynı kaynak alanı iki farklı dizin alanlarına kopyalanıyor alana dizisinde çatallaştırmak" gösterilmektedir:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }
]
```

> [!NOTE]
> Azure arama, alan eşlemelerini alan ve işlev adları çözümlemek için büyük küçük harf duyarsız bir karşılaştırma kullanır. Bu (tüm büyük küçük harfleri doğru hale getirmek gerekmez) uygundur, ancak veri kaynağı ya da dizin küçük harfe göre farklılık alanları olamayacağı anlamına gelir.  
>
>

## <a name="map-fields-using-the-net-sdk"></a>.NET SDK kullanarak alanlarını eşleme

.NET SDK kullanarak alan eşlemelerini tanımlamak [FieldMapping](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.fieldmapping) özellikleri olan sınıf `SourceFieldName` ve `TargetFieldName`ve isteğe bağlı `MappingFunction` başvuru.

Alan eşlemelerini dizin oluşturucu oluştururken veya daha sonra doğrudan ayarlayarak belirtebilirsiniz `Indexer.FieldMappings` özelliği.

Aşağıdaki C# örnek, bir dizin oluşturucu oluştururken alan eşlemelerini ayarlar.

```csharp
  List<FieldMapping> map = new List<FieldMapping> {
    // removes a leading underscore from a field name
    new FieldMapping("_custId", "custId"),
    // URL-encodes a field for use as the index key
    new FieldMapping("docPath", "docId", FieldMappingFunction.Base64Encode() )
  };

  Indexer sqlIndexer = new Indexer(
    name: "azure-sql-indexer",
    dataSourceName: sqlDataSource.Name,
    targetIndexName: index.Name,
    fieldMappings: map,
    schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

  await searchService.Indexers.CreateOrUpdateAsync(indexer);
```

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Alan eşlemesi işlevleri

Dizin içinde depolanmadan önce bir alanda eşleştirme işlevi, bir alanın içeriğini dönüştürür. Aşağıdaki eşleme işlevleri şu anda desteklenmektedir:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode-function"></a>base64Encode işlevi

Gerçekleştirir *URL güvenli* giriş dizesi Base64 kodlaması. UTF-8 olarak kodlanmış giriş olduğunu varsayar.

#### <a name="example---document-key-lookup"></a>Örnek - Belge anahtar arama

URL güvenli yalnızca karakter, bir Azure Search belge anahtarında görünebilir (Müşteriler kullanarak belgenin yönelik verebilmesi gerektiğinden [arama API'si](https://docs.microsoft.com/rest/api/searchservice/lookup-document) ). Kaynak alanı anahtarınız için URL güvenli olmayan karakterleri içeriyorsa, kullanabileceğiniz `base64Encode` zaman dizin oluşturma sırasında dönüştürmek için işlevi.

Arama zaman şifrelenmiş anahtar aldığınızda, ardından kullanabilirsiniz `base64Decode` özgün anahtar değerini almak için işlev ve, kaynak belge almak için kullanın.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : {
      "name" : "base64Encode",
      "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false }
    }
  }]
 ```

Eşleme işleviniz için bir parametre özelliği dahil etmezseniz, değeri varsayılan olarak `{"useHttpServerUtilityUrlTokenEncode" : true}`.

Azure arama, iki farklı Base64 kodlamaları destekler. Kodlama ve kod çözme aynı alanı aynı parametreleri kullanmanız gerekir. Daha fazla bilgi için [base64 kodlama seçenekleri](#base64details) hangi parametrelerin kullanılacağını karar vermek için.

<a name="base64DecodeFunction"></a>

### <a name="base64decode-function"></a>base64Decode işlevi

Base64 giriş dizesi çözer. Girdi olarak kabul edilir bir *URL güvenli* Base64 ile kodlanmış dize.

#### <a name="example---decode-blob-metadata-or-urls"></a>Örnek - blob meta verilerini veya URL'leri kodunu çözme

Veri kaynağınızı blob meta verileri dizeler veya düz metin olarak aranabilir yapmak istediğiniz web URL'leri gibi Base64 ile kodlanmış dizeleri içerebilir. Kullanabileceğiniz `base64Decode` kodlanmış verileri geri arama dizininizi doldurulurken normal dizelere etkinleştirmek için işlevi.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { 
      "name" : "base64Decode", 
      "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false }
    }
  }]
```

Parametre özelliği dahil etmezseniz, değeri varsayılan olarak `{"useHttpServerUtilityUrlTokenEncode" : true}`.

Azure arama, iki farklı Base64 kodlamaları destekler. Kodlama ve kod çözme aynı alanı aynı parametreleri kullanmanız gerekir. Daha fazla ayrıntı için [base64 kodlama seçenekleri](#base64details) hangi parametrelerin kullanılacağını karar vermek için.

<a name="base64details"></a>

#### <a name="base64-encoding-options"></a>Base64 kodlama seçenekleri

Azure arama, iki farklı Base64 kodlamaları destekler: **HttpServerUtility URL belirteci**, ve **doldurma olmadan URL güvenli Base64 kodlaması**. Dizin oluşturma sırasında base64 ile kodlanmış bir dize daha sonra aynı kodlama seçeneklerle çözülmüş, aksi takdirde sonuç, özgün eşleşmeyecektir.

Varsa `useHttpServerUtilityUrlTokenEncode` veya `useHttpServerUtilityUrlTokenDecode` kodlama ve sırasıyla kod çözme için parametre ayarlanmış `true`, ardından `base64Encode` gibi davranan [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) ve `base64Decode` gibi davranır [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

Tam .NET Framework kullanmıyorsanız (diğer bir deyişle, .NET Core veya başka bir framework kullanıyorsanız) Azure arama davranışını benzetmek için anahtar değerler üretmek için sonra ayarlamalısınız `useHttpServerUtilityUrlTokenEncode` ve `useHttpServerUtilityUrlTokenDecode` için `false`. Kullandığınız kitaplığı bağlı olarak, Azure Search tarafından kullanılan gördüğünüzden base64 kodlama ve kodunu çözme işlevler farklı olabilir.

Aşağıdaki tabloda farklı base64 Kodlamalar dizenin karşılaştırır `00>00?00`. Base64 işlevleriniz için gereken ek işlem (varsa) belirlemek için kitaplığınızın uygulamak işlevi dizesini kodlayın `00>00?00` ve beklenen çıktıyı çıkışıyla karşılaştırın `MDA-MDA_MDA`.

| Encoding | Base64 kodlama çıkış | Ek Kitaplık kodladıktan sonra işleme | Ek Kitaplık kodunu çözme önce işleme |
| --- | --- | --- | --- |
| Base64 ile doldurma | `MDA+MDA/MDA=` | Güvenli URL karakterleri kullanın ve doldurma Kaldır | Standart base64 karakter kullanın ve doldurma ekleyin |
| Base64 doldurma olmadan | `MDA+MDA/MDA` | URL güvenli karakter kullanın | Standart base64 karakter kullanın |
| URL güvenli base64 ile doldurma | `MDA-MDA_MDA=` | Doldurma Kaldır | Doldurma Ekle |
| URL güvenli base64 doldurma olmadan | `MDA-MDA_MDA` | None | None |

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition-function"></a>extractTokenAtPosition işlevi

Belirtilen sınırlayıcıyı kullanarak bir dize alanı ayırır ve belirteci, sonuç bölme belirtilen konumda seçer.

Bu işlev, şu parametreleri kullanır:

* `delimiter`: giriş dizesi bölünürken ayırıcı olarak kullanılacak bir dize.
* `position`: giriş dizesi bölündükten sonra seçmek için belirteci bir tamsayı sıfır tabanlı konumu.

Örneğin, giriş ise `Jane Doe`, `delimiter` olduğu `" "`(boşluk) ve `position` 0 ise, sonuç `Jane`if `position` 1 ' dir sonuç `Doe`. Mevcut olmayan bir belirteç konumuna başvuruyorsa, hata döndürülür.

#### <a name="example---extract-a-name"></a>Örnek - adını Ayıkla

Veri kaynağı bir `PersonName` alan ve iki ayrı dizin istediğiniz `FirstName` ve `LastName` alanları. Boşluk karakteri sınırlayıcıyı kullanarak giriş bölmek için bu işlevi kullanabilirsiniz.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection-function"></a>jsonArrayToStringCollection işlevi

Dizeleri bir JSON dizisi olarak doldurmak için kullanılan bir dize dizisi olarak biçimlendirilen dizeyi dönüştüren bir `Collection(Edm.String)` dizinindeki alan.

Örneğin, giriş dizesi ise `["red", "white", "blue"]`, ardından hedef alan türü `Collection(Edm.String)` üç değerlerle doldurulur `red`, `white`, ve `blue`. JSON dizesi dizileri olarak ayrıştırılamıyor. giriş değerleri için hata döndürülür.

#### <a name="example---populate-collection-from-relational-data"></a>Örnek - ilişkisel veri koleksiyonundan Doldur

Azure SQL veritabanı, doğal olarak eşleşen yerleşik veri türü yok `Collection(Edm.String)` Azure Search'te alanları. Dize koleksiyonu alanları doldurmak için kaynak verileri JSON dize dizisi olarak önceden işleyebilir ve ardından `jsonArrayToStringCollection` işlev eşlemesi.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "tags", 
    "mappingFunction" : { "name" : "jsonArrayToStringCollection" }
  }]
```

Dizin koleksiyon alanlarına ilişkisel verileri dönüştüren ayrıntılı bir örnek için bkz. [ilişkisel verileri modelleme](search-example-adventureworks-modeling.md).
