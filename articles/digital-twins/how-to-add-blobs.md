---
title: Azure dijital İkizlerini nesneleri blobları ekleme | Microsoft Docs
description: Azure dijital İkizlerini nesnelerine blobları eklemeyi öğrenin.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: c61544ce10c5a7d16b3ffc0009039e27f5feecb1
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670810"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Azure dijital İkizlerini nesnelerine BLOB Ekle

Resimler ve günlükleri gibi yaygın dosya türleri yapılandırılmamış temsillerini blobudur. BLOB'ları izlemek bir MIME türü kullanarak temsil ettikleri ne tür veriler (örneğin: "image/jpeg") ve meta verileri (adı, açıklamayı, türü ve benzeri).

Azure dijital İkizlerini cihazlara, boşluk ve kullanıcılara düğmelere bloblarını destekler. BLOB'ları, bir kullanıcı, cihaz fotoğraf, video, bir harita, üretici yazılımı zip, JSON verilerini, bir günlük, vb. bir profil resmi temsil edebilir.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-overview"></a>Karşıya BLOB'ları genel bakış

Çok bölümlü istekleri, özel uç noktaları ve onların ilgili işlevleri blobları karşıya yüklemek için kullanabilirsiniz.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>BLOB meta verileri

Ek olarak **Content-Type** ve **Content-Disposition**, dijital İkizlerini Azure blob çok parçalı istek, doğru JSON gövdesi belirtmeniz gerekir. Göndermek için hangi JSON gövdesi gerçekleştirilmekte olan HTTP isteği işlemi türüne bağlıdır.

Dört ana JSON şemalarının şunlardır:

[![JSON şemaları](media/how-to-add-blobs/blob-models-img.png)](media/how-to-add-blobs/blob-models-img.png#lightbox)

JSON blob meta verilerini aşağıdaki modele uyar:

```JSON
{
    "parentId": "00000000-0000-0000-0000-000000000000",
    "name": "My First Blob",
    "type": "Map",
    "subtype": "GenericMap",
    "description": "A well chosen description",
    "sharing": "None"
  }
```

| Öznitelik | Type | Açıklama |
| --- | --- | --- |
| **parentId** | Dize | Blob (boşluk, cihazları veya kullanıcıları ile) ilişkilendirilecek üst varlık |
| **name** |Dize | Blob için bir insan kolay ad |
| **type** | Dize | Blob - türünü kullanamaz *türü* ve *typeId*  |
| **typeId** | Tamsayı | Blob türü kimliği - kullanamaz *türü* ve *typeId* |
| **subtype** | Dize | Blob alt - kullanamaz *alt* ve *subtypeId* |
| **subtypeId** | Tamsayı | Alt tür kimliği - blob için kullanamaz *alt* ve *subtypeId* |
| **description** | Dize | Blob özelleştirilmiş açıklaması |
| **sharing** | Dize | Blob olup paylaşılabilir - sabit listesi [`None`, `Tree`, `Global`] |

BLOB meta verileri ile ilk öbek olarak sağlanan her zaman **Content-Type** `application/json` veya farklı bir `.json` dosya. Dosya verileri ikinci öbekte sağlanan ve desteklenen bir MIME türü olabilir.

Swagger belgeler, bu modeli şemaları tam ayrıntılı açıklar.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Başvuru belgeleri okuyarak kullanma hakkında bilgi edinin [Swagger'ı kullanmayı](./how-to-use-swagger.md).

<div id="blobModel"></div>

### <a name="blobs-response-data"></a>BLOB'ları yanıt verileri

Döndürülen tek tek bloblar için aşağıdaki JSON şeması uyar:

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "parentId": "00000000-0000-0000-0000-000000000000",
  "type": "string",
  "subtype": "string",
  "typeId": 0,
  "subtypeId": 0,
  "sharing": "None",
  "description": "string",
  "contentInfos": [
    {
      "type": "string",
      "sizeBytes": 0,
      "mD5": "string",
      "version": "string",
      "lastModifiedUtc": "2019-01-12T00:58:08.689Z",
      "metadata": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "fullName": "string",
  "spacePaths": [
    "string"
  ]
}
```

| Öznitelik | Type | Açıklama |
| --- | --- | --- |
| **id** | Dize | Blob için benzersiz tanımlayıcı |
| **name** |Dize | Blob için bir insan kolay ad |
| **parentId** | Dize | Blob (boşluk, cihazları veya kullanıcıları ile) ilişkilendirilecek üst varlık |
| **type** | Dize | Blob - türünü kullanamaz *türü* ve *typeId*  |
| **typeId** | Tamsayı | Blob türü kimliği - kullanamaz *türü* ve *typeId* |
| **subtype** | Dize | Blob alt - kullanamaz *alt* ve *subtypeId* |
| **subtypeId** | Tamsayı | Alt tür kimliği - blob için kullanamaz *alt* ve *subtypeId* |
| **sharing** | Dize | Blob olup paylaşılabilir - sabit listesi [`None`, `Tree`, `Global`] |
| **description** | Dize | Blob özelleştirilmiş açıklaması |
| **contentInfos** | Array | Sürüm dahil olmak üzere yapılandırılmamış meta veri bilgilerini belirtir. |
| **fullName** | Dize | Blob tam adı |
| **spacePaths** | Dize | Alan yolu |

BLOB meta verileri ile ilk öbek olarak sağlanan her zaman **Content-Type** `application/json` veya farklı bir `.json` dosya. Dosya verileri ikinci öbekte sağlanan ve desteklenen bir MIME türü olabilir.

### <a name="blob-multipart-request-examples"></a>BLOB çok parçalı istek örnekleri

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Bir metin dosyası bir blob olarak karşıya yükleyin ve boşluk ile ilişkilendirmek için kimliği doğrulanmış bir HTTP POST isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Aşağıdaki gövdesi:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Value | Şununla değiştir |
| --- | --- |
| USER_DEFINED_BOUNDARY | Çok bölümlü içerik sınır adı |

Aşağıdaki kod bir sınıf kullanarak aynı blob karşıya yükleme, .NET uygulamasıdır [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

//MY_BLOB.txt is the String representation of your text file
var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

Son olarak, [cURL](https://curl.haxx.se/) kullanıcılar, çok bölümlü form isteklerini aynı şekilde yapabilirsiniz:

[![Cihaz BLOB'ları](media/how-to-add-blobs/curl-img.png)](media/how-to-add-blobs/curl-img.png#lightbox)

```bash
curl
 -X POST "YOUR_MANAGEMENT_API_URL/spaces/blobs"
 -H "Authorization: Bearer YOUR_TOKEN"
 -H "Accept: application/json"
 -H "Content-Type: multipart/form-data"
 -F "meta={\"ParentId\": \"YOUR_SPACE_ID\",\"Name\":\"My CURL Blob",\"Type\":\"Map\",\"SubType\":\"GenericMap\",\"Description\": \"A well chosen description\", \"Sharing\": \"None\"};type=application/json"
 -F "text=PATH_TO_FILE;type=text/plain"
```

| Value | Şununla değiştir |
| --- | --- |
| YOUR_TOKEN | Geçerli, OAuth 2.0 belirteç |
| YOUR_SPACE_ID | Blob ile ilişkilendirmek için alan kimliği |
| PATH_TO_FILE | Metin dosyasının yolu |

Başarılı bir GÖNDERİ (daha önce vurgulanan kırmızı renkte) blob yeni Kimliğini döndürür.

## <a name="api-endpoints"></a>API uç noktaları

Aşağıdaki bölümlerde, çekirdek blob ile ilgili API uç noktaları ve kendi işlevler açıklanmaktadır.

### <a name="devices"></a>Cihazlar

Bloblar için cihazlar ekleyebilirsiniz. Aşağıdaki görüntüde Yönetimi API'leri için Swagger başvuru belgeleri gösterilmektedir. Bu, cihazla ilgili API uç noktaları için blob tüketimi ve uygulamasına geçirmek için gerekli yol parametreleri belirtir.

[![Cihaz BLOB'ları](media/how-to-add-blobs/blobs-device-api-img.png)](media/how-to-add-blobs/blobs-device-api-img.png#lightbox)

Örneğin, güncelleştirmek veya blob oluşturma ve blob için bir cihaz eklemek için kimliği doğrulanmış bir HTTP PATCH isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen blob kimliği |

Başarılı istekler döndürür bir JSON nesnesi olarak [daha önce açıklanan](#blobModel).

### <a name="spaces"></a>Alanları

Ayrıca, blobları için alanları ekleyebilirsiniz. Aşağıdaki görüntü, alanı API uç noktalarını blobları işlenmesinden sorumludur listeler. Ayrıca, bu uç noktalarına geçmek için herhangi bir yol parametreleri listeler.

[![Alanı BLOB'ları](media/how-to-add-blobs/blobs-space-api-img.png)](media/how-to-add-blobs/blobs-space-api-img.png#lightbox)

Örneğin, bir blob için bir alanı eklenmiş döndürmek için kimliği doğrulanmış bir HTTP GET isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen blob kimliği |

Başarılı istekler döndürür bir JSON nesnesi olarak [daha önce açıklanan](#blobModel).

Bir PATCH isteği aynı uç noktasına, meta veri açıklamalarını güncelleştirir ve blob sürümleri oluşturur. HTTP isteği, tüm gerekli meta ve çok bölümlü form verilerinin yanı sıra düzeltme eki yöntemi aracılığıyla yapılır.

### <a name="users"></a>Kullanıcılar

Blobları kullanıcı modelleri (örneğin, bir profil resmi ilişkilendirmek için) ekleyebilirsiniz. Aşağıdaki resim gibi ilgili kullanıcı API uç noktaları ve tüm gerekli yol parametreleri gösterir `id`:

[![Kullanıcı BLOB'ları](media/how-to-add-blobs/blobs-users-api-img.png)](media/how-to-add-blobs/blobs-users-api-img.png#lightbox)

Örneğin, bir kullanıcıya bağlı olarak bir blob getirmek için kimliği doğrulanmış bir HTTP GET isteği için gerekli form verilerle olun:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen blob kimliği |

Başarılı istekler döndürür bir JSON nesnesi olarak [daha önce açıklanan](#blobModel).

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Sık karşılaşılan doğru üst bilgi bilgileri sağlayarak değil içerir:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

Bu hatayı gidermek için genel istek uygun olduğunu doğrulayın **Content-Type** üst bilgi:

* `multipart/mixed`
* `multipart/form-data`

Ayrıca, çok bölümlü her öbek karşılık gelen olduğunu doğrulayın. **Content-Type** gerektiğinde.

## <a name="next-steps"></a>Sonraki adımlar

- Azure dijital çiftleri için Swagger başvuru belgeleri hakkında daha fazla bilgi edinmek için [kullanımı Azure dijital İkizlerini Swagger](how-to-use-swagger.md).

- Postman ile blobları karşıya yüklemek için okuma [Postman yapılandırma](./how-to-configure-postman.md).