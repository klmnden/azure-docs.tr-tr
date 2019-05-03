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
ms.openlocfilehash: d3e0d876550b0c3baf89f3f13e0458fc97e11351
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025211"
---
# <a name="field-mappings-in-azure-search-indexers"></a>Azure Search dizin oluşturucularında alan eşlemeleri
Azure Search dizin oluşturucuyu kullanırken, kendiniz bazen burada giriş verileriniz, hedef dizin şemasını oldukça eşleşmiyor durumlarda bulabilirsiniz. Bu gibi durumlarda, kullandığınız **alan eşlemeleri** verilerinizi istediğiniz şekle dönüştürmek için.

Alan eşlemelerini yararlı olduğu bazı durumlar:

* Bir alanın veri kaynağınızda bulunan `_id`, ancak Azure Search alan adları alt çizgiyle başlayan izin vermez. Alan eşlemeyi "bir alanı yeniden adlandırmak" sağlar.
* Bu alanları için farklı Çözümleyicileri uygulamak istediğiniz çünkü dizin örneğin aynı veri kaynak verilere sahip çeşitli alanları doldurmak istersiniz. Alan eşlemelerini "veri kaynağı alanının çatal" olanak tanır.
* İhtiyaç Base64 kodlama veya kod çözme verilerinizi. Alan eşlemelerini desteği birkaç **işlevleri eşleme**, Base64 için kodlama ve kodunu çözme dahil olmak üzere işlevleri.   

## <a name="setting-up-field-mappings"></a>Alan eşlemelerini ayarlama
Alan eşlemelerini kullanarak yeni bir dizin oluşturucu oluştururken ekleyebilirsiniz [dizin oluşturucu oluşturma](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. Bir dizin oluşturma kullanarak dizin oluşturucu üzerinde alan eşlemeleri yönetebileceğiniz [güncelleştirme dizin oluşturucu](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

Bir alan eşlemesi, üç bölümden oluşur:

1. A `sourceFieldName`, veri kaynağınızdaki bir alanı temsil eder. Bu özellik gereklidir.
2. İsteğe bağlı `targetFieldName`, arama dizininizdeki bir alanı temsil eder. Atlanırsa, veri kaynağına olduğu gibi aynı adı kullanılır.
3. İsteğe bağlı `mappingFunction`, önceden tanımlanmış İşlevler, birkaç birini kullanarak verilerinizi dönüştürebilirsiniz. İşlevlerin tam listesi [aşağıda](#mappingFunctions).

Alan eşlemelerini eklenir `fieldMappings` dizin oluşturucu tanımı üzerindeki dizi.

Örneğin, işte alan adları farklılıkları nasıl uyum:

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

Bir dizin oluşturucu, birden çok alan eşlemelerini olabilir. Nasıl, "bir alan çatal" Örneğin, aşağıda verilmiştir:

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

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Alan eşlemesi işlevleri
Bu işlevler şu anda desteklenmektedir:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

## <a name="base64encode"></a>base64Encode
Gerçekleştirir *URL güvenli* giriş dizesi Base64 kodlaması. UTF-8 olarak kodlanmış giriş olduğunu varsayar.

### <a name="sample-use-case---document-key-lookup"></a>Örnek Kullanım örneği - belge anahtar arama
URL güvenli yalnızca karakter, bir Azure Search belge anahtarında görünebilir (Müşteriler kullanarak belgenin yönelik verebilmesi gerektiğinden [arama API'si](https://docs.microsoft.com/rest/api/searchservice/lookup-document), örneğin). Verilerinizi URL güvenli olmayan karakterler içeriyor ve bir anahtar alanı arama dizininizi doldurmak için kullanmak istediğiniz, bu işlevi kullanın. Anahtar kodlanmış sonra kullanabileceğiniz base64 kod çözme orijinal değerini almak için. Ayrıntılar için bkz [base64 kodlama ve kodunu çözme](#base64details) bölümü.

#### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

### <a name="sample-use-case---retrieve-original-key"></a>Kullanım örneği örneği - orijinal anahtarını alma
Dizinleri blobları ile belge anahtarı olarak blob yolu meta veriler bir blob dizin oluşturucu var. Kodlanmış belge anahtarını aldıktan sonra yolun kod çözme ve blob indirmek istersiniz.

#### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode", "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false } }
  }]
 ```

Anahtarlar tarafından belgeleri aramak gerekmez ve de kodlanmış kodunu çözmek için içeriği, yalnızca bırakabilirsiniz yoksa `parameters` eşleme işlev için bunun varsayılan `useHttpServerUtilityUrlTokenEncode` için `true`. Aksi takdirde bkz [base64 ayrıntıları](#base64details) hangi ayarların kullanılacağı karar vermek için bölüm.

<a name="base64DecodeFunction"></a>

## <a name="base64decode"></a>base64Decode
Base64 giriş dizesi çözer. Giriş varsayılır bir *URL güvenli* Base64 ile kodlanmış dize.

### <a name="sample-use-case"></a>Örnek Kullanım örneği
BLOB özel meta veri değerleri, ASCII kodlamalı olmalıdır. Base64 kodlaması özel meta verileri blob içinde rastgele UTF-8 dizeleri temsil etmek için kullanabilirsiniz. Ancak, arama anlamlı olacak şekilde, kodlanmış verileri geri arama dizininizi doldurulurken "Normal" dizelere etkinleştirmek için bu işlevi kullanabilirsiniz.

#### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode", "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false } }
  }]
```

Tüm belirtmezseniz `parameters`, ardından varsayılan değerini `useHttpServerUtilityUrlTokenDecode` olduğu `true`. Bkz: [base64 ayrıntıları](#base64details) hangi ayarların kullanılacağı karar vermek için bölüm.

<a name="base64details"></a>

### <a name="details-of-base64-encoding-and-decoding"></a>Base64 kodlama ve kodunu çözme ayrıntıları
Azure arama, iki base64 kodlamaları destekler: HttpServerUtility URL belirtecini ve URL güvenli olmayan doldurma base64 kodlaması. Görünüm için bir belge anahtarını ayarlama kodlayın, dizin oluşturucu tarafından çözülecek bir değer kodlayın veya dizin oluşturucu tarafından kodlanmış bir alan kodunu çözme istiyorsanız aynı kodlama eşleme işlevlerine kullanmanız gerekir.

Varsa `useHttpServerUtilityUrlTokenEncode` veya `useHttpServerUtilityUrlTokenDecode` kodlama ve sırasıyla kod çözme için parametre ayarlanmış `true`, ardından `base64Encode` gibi davranan [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) ve `base64Decode` gibi davranır [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

Tam .NET Framework kullanmıyorsanız (örneğin, .NET Core veya diğer programlama ortamı kullanıyorsanız) Azure arama davranışını benzetmek için anahtar değerler üretmek için sonra ayarlamalısınız `useHttpServerUtilityUrlTokenEncode` ve `useHttpServerUtilityUrlTokenDecode` için `false`. Kullandığınız kitaplığı bağlı olarak base64 kodlama ve kod çözme yardımcı programı işlevleri Azure arama farklı olabilir.

Aşağıdaki tabloda farklı base64 Kodlamalar dizenin karşılaştırır `00>00?00`. Base64 işlevleriniz için gereken ek işlem (varsa) belirlemek için kitaplığınızın uygulamak işlevi dizesini kodlayın `00>00?00` ve beklenen çıktıyı çıkışıyla karşılaştırın `MDA-MDA_MDA`.

| Encoding | Base64 kodlama çıkış | Ek Kitaplık kodladıktan sonra işleme | Ek Kitaplık kodunu çözme önce işleme |
| --- | --- | --- | --- |
| Base64 ile doldurma | `MDA+MDA/MDA=` | Güvenli URL karakterleri kullanın ve doldurma Kaldır | Standart base64 karakter kullanın ve doldurma ekleyin |
| Base64 doldurma olmadan | `MDA+MDA/MDA` | URL güvenli karakter kullanın | Standart base64 karakter kullanın |
| URL güvenli base64 ile doldurma | `MDA-MDA_MDA=` | Doldurma Kaldır | Doldurma Ekle |
| URL güvenli base64 doldurma olmadan | `MDA-MDA_MDA` | None | None |

<a name="extractTokenAtPositionFunction"></a>

## <a name="extracttokenatposition"></a>extractTokenAtPosition
Belirtilen sınırlayıcıyı kullanarak bir dize alanı ayırır ve belirteci, sonuç bölme belirtilen konumda seçer.

Örneğin, giriş ise `Jane Doe`, `delimiter` olduğu `" "`(boşluk) ve `position` 0 ise, sonuç `Jane`if `position` 1 ' dir sonuç `Doe`. Mevcut olmayan bir belirteç konumuna başvuruyorsa, hata döndürülür.

### <a name="sample-use-case"></a>Örnek Kullanım örneği
Veri kaynağı bir `PersonName` alan ve iki ayrı dizin istediğiniz `FirstName` ve `LastName` alanları. Boşluk karakteri sınırlayıcıyı kullanarak giriş bölmek için bu işlevi kullanabilirsiniz.

### <a name="parameters"></a>Parametreler
* `delimiter`: giriş dizesi bölünürken ayırıcı olarak kullanılacak bir dize.
* `position`: giriş dizesi bölündükten sonra seçmek için belirteci bir tamsayı sıfır tabanlı konumu.    

### <a name="example"></a>Örnek
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

## <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection
Dizeleri bir JSON dizisi olarak doldurmak için kullanılan bir dize dizisi olarak biçimlendirilen dizeyi dönüştüren bir `Collection(Edm.String)` dizinindeki alan.

Örneğin, giriş dizesi ise `["red", "white", "blue"]`, ardından hedef alan türü `Collection(Edm.String)` üç değerlerle doldurulur `red`, `white`, ve `blue`. JSON dizesi dizileri olarak ayrıştırılamıyor. giriş değerleri için hata döndürülür.

### <a name="sample-use-case"></a>Örnek Kullanım örneği
Azure SQL veritabanı, doğal olarak eşleşen yerleşik veri türü yok `Collection(Edm.String)` Azure Search'te alanları. Dize koleksiyonu alanları doldurmak için kaynak verileri JSON dize dizisi olarak biçimlendirin ve bu işlevi kullanın.

### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya geliştirmeleri için fikriniz varsa, lütfen bize ulaşın bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
