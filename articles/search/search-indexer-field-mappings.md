---
title: Azure Search'te dizin oluşturucular üzerinde alan eşlemeleri
description: Alan adları ve veri Beyanları farklılıklar hesaba Azure Search dizin oluşturucu alan eşlemelerini yapılandırın
author: chaosrealm
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 08/30/2017
ms.author: eugenesh
ms.openlocfilehash: 041866cd1c290bc576577771abcae31db747095e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="field-mappings-in-azure-search-indexers"></a>Azure Search'te dizin oluşturucular üzerinde alan eşlemeleri
Azure Search'te dizin oluşturucular kullanırken, kendiniz bazen burada giriş verilerinizi, hedef dizin şeması oldukça eşleşmediği durumlarda bulabilirsiniz. Bu durumda, kullandığınız **alan eşlemelerini** istenen şekle verilerinizi dönüştürecek.

Alan eşlemelerini yararlı olduğu bazı durumlar:

* Bir alan veri kaynağınız sahip `_id`, ancak Azure Search, alt çizgi ile başlayan alan adları izin vermez. Alan eşlemeyi "alanı yeniden adlandırma" sağlar.
* Bu alanlar farklı çözümleyiciler uygulamak istediğiniz olduğundan örneğin aynı veri kaynağı verilerinin dizinde birkaç alanları doldurmak istiyor. Alan eşlemelerini "veri kaynağı alanı çatallaştırma" olanak tanır.
* Gerek Base64 için kodlamak veya verilerinizi kodunu çözer. Alan eşlemelerini birkaç Destek **işlevleri eşleme**, kodlama ve kod çözme dahil olmak üzere işlevleri Base64 için.   

## <a name="setting-up-field-mappings"></a>Alan eşlemelerini ayarlama
Alan eşlemelerini kullanarak yeni bir dizin oluşturucu oluştururken ekleyebilirsiniz [oluşturma dizin oluşturucu](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. Alan eşlemelerini dizin dizin oluşturucu kullanarak yönetebileceğiniz [güncelleştirme dizin oluşturucu](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

Alan eşlemeyi üç bölümden oluşur:

1. A `sourceFieldName`, veri kaynağında bir alan temsil eder. Bu özellik gereklidir.
2. İsteğe bağlı bir `targetFieldName`, search dizininizi bir alanı temsil eder. Atlanırsa, veri kaynağı olduğu gibi aynı adı kullanılır.
3. İsteğe bağlı bir `mappingFunction`, önceden tanımlanmış işlevleri, çeşitli birini kullanarak verilerinizi dönüştürebilirsiniz. İşlevlerin tam listesi [aşağıda](#mappingFunctions).

Alan eşlemeleri eklenir `fieldMappings` dizin oluşturucu tanımı dizi.

Örneğin, işte alan adları farklılıkları nasıl uyum sağlayabilir:

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

Bir dizin oluşturucu, birden çok alan eşlemelerini olabilir. Nasıl, "bir alan çatallaştırma" Örneğin, burada'nın:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }
]
```

> [!NOTE]
> Azure arama büyük küçük harf duyarsız karşılaştırma alan eşlemelerini alan ve işlev adları çözümlemek için kullanır. Bu (tüm büyük küçük harf sağ almak gerekmez) kullanışlıdır ancak bu veri kaynağı veya dizin yalnızca örneğe göre farklı alanlara sahip olamaz anlamına gelir.  
>
>

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Alan eşleme işlevleri
Bu işlevler şu anda desteklenir:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

## <a name="base64encode"></a>base64Encode
Gerçekleştirir *URL için güvenli* giriş dizesi Base64 kodlaması. Giriş UTF-8 ile kodlanmış olduğunu varsayar.

### <a name="sample-use-case---document-key-lookup"></a>Örnek Kullanım örneği - belge anahtar arama
URL uyumlu yalnızca karakter, bir Azure Search belge anahtarında görüntülenebilir (çünkü müşterilerin kullanarak belgenin adres gerekir [arama API](https://docs.microsoft.com/rest/api/searchservice/lookup-document), örneğin). Verilerinizi URL güvenli olmayan karakterler içeriyor ve bir anahtar alanı search dizininizi doldurmak için kullanmak istiyorsanız, bu işlevi kullanın. Anahtar kodlanmış sonra kullanabileceğiniz base64 kod çözme özgün değeri alınamadı. Ayrıntılar için bkz [base64 kodlama ve kod çözme](#base64details) bölümü.

#### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

### <a name="sample-use-case---retrieve-original-key"></a>Kullanım örneği örnek - özgün anahtar alma
Dizinleri BLOB belge anahtarı olarak blob yolu meta verilerle bir blob dizin oluşturucu sahip. Kodlanmış belge anahtarı aldıktan sonra yolun kod çözme ve blob indirmek istediğiniz.

#### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode", "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false } }
  }]
```

Anahtarlar tarafından belgeleri aramak gerek yoktur ve ayrıca kodlanmış kodunu çözmek için içeriği, yalnızca bırakabilirsiniz yok `parameters` eşleme işlevi için varsayılan olarak `useHttpServerUtilityUrlTokenEncode` için `true`. Aksi takdirde bkz [base64 ayrıntıları](#base64details) kullanmak için hangi ayarları karar vermek için bölüm.

<a name="base64DecodeFunction"></a>

## <a name="base64decode"></a>base64Decode
Base64 giriş dizesi çözer. Giriş varsayılır bir *URL için güvenli* Base64 ile kodlanmış dize.

### <a name="sample-use-case"></a>Örnek Kullanım örneği
BLOB özel meta verileri değerleri ASCII kodlanmış olmalıdır. Base64 kodlaması blob özel meta verileri rasgele UTF-8 dizelerde temsil etmek için kullanabilirsiniz. Ancak, arama anlamlı için kodlanmış verileri geri arama dizininize doldurulurken "Normal" dizeleri etkinleştirmek için bu işlevi kullanabilirsiniz.

#### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode", "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false } }
  }]
```

Herhangi bir belirtmezseniz `parameters`, ardından varsayılan değerini `useHttpServerUtilityUrlTokenDecode` olan `true`. Bkz: [base64 ayrıntıları](#base64details) kullanmak için hangi ayarları karar vermek için bölüm.

<a name="base64details"></a>

### <a name="details-of-base64-encoding-and-decoding"></a>Base64 kodlama ve kod çözme ayrıntıları
Azure arama iki base64 kodlaması destekler: HttpServerUtility URL doldurma olmadan belirteci ve URL için güvenli base64 kodlaması. Görünüm için bir belge anahtar yukarı kodlamak, Oluşturucu tarafından çözülecek bir değer kodlamak veya Oluşturucu tarafından kodlanmış bir alan kodunu çözmek istiyorsanız aynı kodlama eşleme işlevleri kullanmanız gerekir.

.NET Framework kullanırsanız, ayarlayabileceğiniz `useHttpServerUtilityUrlTokenEncode` ve `useHttpServerUtilityUrlTokenDecode` için `true`, kodlama ve sırasıyla kod çözme için. Ardından `base64Encode` gibi davranır [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) ve `base64Decode` gibi davranır [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

.NET Framework kullanmıyorsanız sonra ayarlamanız gerekir `useHttpServerUtilityUrlTokenEncode` ve `useHttpServerUtilityUrlTokenDecode` için `false`. Kullandığınız kitaplığı bağlı olarak, base64 kodlamak ve yardımcı programı kod çözme işlevleri Azure aramadan farklı olabilir.

Aşağıdaki tabloda farklı base64 kodlaması dizesinin karşılaştırır `00>00?00`. Base64 işlevleri için gerekli ek işleme (varsa) belirlemek için kitaplığınızın uygulamak işlevi dizesini kodlayın `00>00?00` ve beklenen çıktı çıkışıyla karşılaştırın `MDA-MDA_MDA`.

| Encoding | Base64 kodlama çıktı | Ek kitaplığı kodlama sonra işleme | Ek kitaplığı kod çözme önce işleme |
| --- | --- | --- | --- |
| Base64 ile doldurma | `MDA+MDA/MDA=` | URL için güvenli karakterler kullanmak ve doldurma kaldırın | Standart base64 karakterler ve doldurma ekleyin |
| Base64 doldurma olmadan | `MDA+MDA/MDA` | URL için güvenli karakterler kullanın | Standart base64 karakterler kullanın |
| URL için güvenli base64 ile doldurma | `MDA-MDA_MDA=` | Doldurma Kaldır | Doldurma ekleme |
| URL için güvenli base64 doldurma olmadan | `MDA-MDA_MDA` | None | None |

<a name="extractTokenAtPositionFunction"></a>

## <a name="extracttokenatposition"></a>extractTokenAtPosition
Belirtilen sınırlayıcıyı kullanarak bir dize alanı ayırır ve sonuçta elde edilen bölünmüş belirtilen konumda belirteci alır.

Örneğin, giriş ise `Jane Doe`, `delimiter` olan `" "`(boşluk) ve `position` 0'dır, sonuç `Jane`if `position` 1 ' dir sonuç `Doe`. Konumu yok bir belirteç başvuruyorsa, hata döndürülür.

### <a name="sample-use-case"></a>Örnek Kullanım örneği
Veri kaynağınızı içeren bir `PersonName` alan ve iki ayrı dizin istediğinizde `FirstName` ve `LastName` alanları. Boşluk karakteri sınırlayıcıyı kullanarak giriş bölmek için bu işlevi kullanabilirsiniz.

### <a name="parameters"></a>Parametreler
* `delimiter`: ayırıcı olarak giriş dizesi ayırma sırasında kullanılacak bir dize.
* `position`: belirtecin giriş dizesi bölündükten sonra seçmek için bir tamsayı sıfır tabanlı konumu.    

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
Doldurmak için kullanılan bir dize dizisi dizeleri bir JSON dizisi olarak biçimlendirilmiş bir dize dönüştüren bir `Collection(Edm.String)` dizinindeki alan.

Örneğin, giriş dizesi ise `["red", "white", "blue"]`, ardından hedef alan türü `Collection(Edm.String)` üç değerlerle doldurulur `red`, `white`, ve `blue`. JSON dizesi dizileri olarak ayrıştırılamıyor giriş değerleri için bir hata döndürülür.

### <a name="sample-use-case"></a>Örnek Kullanım örneği
Azure SQL veritabanı doğal eşleyen bir yerleşik veri türüne sahip değil `Collection(Edm.String)` Azure Search'te alanları. Dize koleksiyonu alanları doldurmak için kaynak verilerinizi JSON dize dizisi olarak biçimlendirmek ve bu işlevi kullanın.

### <a name="example"></a>Örnek
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```


## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya fikir geliştirmeleri için varsa, lütfen bize üzerinde ulaşmak bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
