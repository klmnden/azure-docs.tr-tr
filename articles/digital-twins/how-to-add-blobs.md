---
title: Azure dijital İkizlerini nesneleri blobları ekleme | Microsoft Docs
description: Azure dijital İkizlerini nesnelerine blobları eklemeyi öğrenin.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/28/2018
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 604093dcec048b0991bbc9beac3ef998cc47e351
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53974531"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Azure dijital İkizlerini nesnelerine BLOB Ekle

Resimler ve günlükleri gibi yaygın dosya türleri yapılandırılmamış temsillerini blobudur. Bir MIME türü kullanarak temsil ettikleri ne tür veriler BLOB'ları izlemek (örneğin: "image/jpeg") ve meta verileri (adı, açıklamayı, türü ve benzeri).

Azure dijital İkizlerini cihazlara, boşluk ve kullanıcılara düğmelere bloblarını destekler. BLOB'ları, bir kullanıcı, cihaz fotoğraf, video, bir harita veya bir günlük için bir profil resmi temsil edebilir.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-an-overview"></a>Blobları karşıya yükleme: bir genel bakış

Çok bölümlü istekleri, özel uç noktaları ve onların ilgili işlevleri blobları karşıya yüklemek için kullanabilirsiniz.

> [!IMPORTANT]
> Çok bölümlü istekleri üç parça bilgi gerekir:
> * A **Content-Type** üst bilgi:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * A **içerik düzeni**: `form-data; name="metadata"`
> * Karşıya yüklenecek dosya içeriği
>
> **Content-Type** ve **Content-Disposition** bilgi kullanım senaryosuna bağlı olarak farklılık gösterir.

Azure dijital İkizlerini yönetim API'leri için çok bölümlü isteklerini iki bölümden oluşur:

* Gösterildiği şekilde ilişkili olan bir MIME türü gibi meta veriler BLOB **Content-Type** ve **Content-Disposition** bilgileri

* BLOB içeriklerini (dosya yapılandırılmamış içeriğini)  

İki bölümü hiçbiri için gerekli olan **düzeltme eki** istekleri. Her ikisi için gerekli olan **POST** veya oluşturma işlemleri.

### <a name="blob-metadata"></a>Blob meta verileri

Ek olarak **Content-Type** ve **Content-Disposition**, çok bölümlü isteklerini doğru JSON gövdesi belirtmeniz gerekir. Göndermek için hangi JSON gövdesi gerçekleştirilmekte olan HTTP isteği işlemi türüne bağlıdır.

Dört ana JSON şemalarının şunlardır:

![JSON şemaları][1]

Swagger belgeler, bu modeli şemaları tam ayrıntılı açıklar.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Başvuru belgeleri okuyarak kullanma hakkında bilgi edinin [Swagger'ı kullanmayı](./how-to-use-swagger.md).

### <a name="examples"></a>Örnekler

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Yapmak için bir **POST** bir metin dosyası olarak bir blob yükleyen ve boşluk ile ilişkilendirir isteği:

```plaintext
POST YOUR_MANAGEMENT_API_URL/spaces/blobs HTTP/1.1
Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"

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

| Değer | Şununla değiştir |
| --- | --- |
| USER_DEFINED_BOUNDARY | Çok bölümlü içerik sınır adı |

Aşağıdaki kod bir sınıf kullanarak aynı blob karşıya yükleme, .NET uygulamasıdır [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

## <a name="api-endpoints"></a>API uç noktaları

Aşağıdaki bölümlerde, çekirdek blob ile ilgili API uç noktaları ve kendi işlevler açıklanmaktadır.

### <a name="devices"></a>Cihazlar

Bloblar için cihazlar ekleyebilirsiniz. Aşağıdaki görüntüde Yönetimi API'leri için Swagger başvuru belgeleri gösterilmektedir. Bu, cihazla ilgili API uç noktaları için blob tüketimi ve uygulamasına geçirmek için gerekli yol parametreleri belirtir.

![Cihaz BLOB'ları][2]

Örneğin, güncelleştirmek veya blob oluşturma ve blob için bir cihaz eklemek için olun bir **düzeltme eki** isteği:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen blob kimliği |

Başarılı istekler döndürür bir **DeviceBlob** yanıt JSON nesnesi. **DeviceBlob** nesneleri uymak için aşağıdaki JSON şeması:

| Öznitelik | Tür | Açıklama | Örnekler |
| --- | --- | --- | --- |
| **DeviceBlobType** | Dize | Bir cihaza bağlı bir blob kategorisi | `Model` ve `Specification` |
| **DeviceBlobSubtype** | Dize | Daha fazla belirli bir blob subcategory **DeviceBlobType** | `PhysicalModel`, `LogicalModel`, `KitSpecification`, ve `FunctionalSpecification` |

> [!TIP]
> Yukarıdaki tabloda başarıyla döndürülen istek verileri işlemek için kullanın.

### <a name="spaces"></a>Boşluklar

Ayrıca, blobları için alanları ekleyebilirsiniz. Aşağıdaki görüntü, alanı API uç noktalarını blobları işlenmesinden sorumludur listeler. Ayrıca, bu uç noktalarına geçmek için herhangi bir yol parametreleri listeler.

![Alanı BLOB'ları][3]

Örneğin, bir blob için bir alanı eklenmiş döndürülecek olun bir **alma** isteği:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen blob kimliği |

Yapmadan bir **düzeltme eki** isteği aynı uç noktasına bir meta veri açıklamasını güncelleştirin ve blob yeni bir sürümünü oluşturmak sağlar. HTTP isteği aracılığıyla yapılan **düzeltme eki** yöntemi, tüm gerekli meta ve çok bölümlü form verilerinin yanı sıra.

Başarılı işlemler dönüş bir **SpaceBlob** aşağıdaki şemaya uygun nesne. Döndürülen veri tüketmek için kullanabilirsiniz.

| Öznitelik | Tür | Açıklama | Örnekler |
| --- | --- | --- | --- |
| **SpaceBlobType** | Dize | Bir alana bağlı bir blob kategorisi | `Map` ve `Image` |
| **SpaceBlobSubtype** | Dize | Daha fazla belirli bir blob subcategory **SpaceBlobType** | `GenericMap`, `ElectricalMap`, `SatelliteMap`, ve `WayfindingMap` |

### <a name="users"></a>Kullanıcılar

Blobları kullanıcı modelleri (örneğin, bir profil resmi ilişkilendirmek için) ekleyebilirsiniz. Aşağıdaki resim gibi ilgili kullanıcı API uç noktaları ve tüm gerekli yol parametreleri gösterir `id`:

![Kullanıcı BLOB'ları][4]

Örneğin, bir kullanıcıya bağlı olarak bir blob getirilecek olun bir **alma** gerekli form veri isteği:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen blob kimliği |

Döndürülen JSON (**UserBlob** nesneleri) için aşağıdaki JSON modelleri uyar:

| Öznitelik | Tür | Açıklama | Örnekler |
| --- | --- | --- | --- |
| **UserBlobType** | Dize | Bir kullanıcıya bağlı bir blob kategorisi | `Image` ve `Video` |
| **UserBlobSubtype** |  Dize | Daha fazla belirli bir blob subcategory **UserBlobType** | `ProfessionalImage`, `VacationImage`, ve `CommercialVideo` |

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Doğru üst bilgi bilgileri dahil yaygın bir hatadır:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital çiftleri için Swagger başvuru belgeleri hakkında daha fazla bilgi edinmek için [kullanımı Azure dijital İkizlerini Swagger](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
