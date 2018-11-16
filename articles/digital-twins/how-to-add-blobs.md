---
title: Azure dijital İkizlerini nesneleri blobları ekleme | Microsoft Docs
description: Azure dijital İkizlerini nesneleri blobları ekleme anlama
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: 0929a4a63eee35d21723c980887d6b4217898682
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688187"
---
# <a name="how-to-add-blobs-to-objects-in-azure-digital-twins"></a>Azure dijital İkizlerini nesneleri BLOB'ları ekleme

Genel dosya türlerinden (örneğin, resimler ve günlükleri) yapılandırılmamış temsillerini blobudur. Ne tür verilerin temsil eden bir MIME türü kullanarak BLOB'ları izlemek (örneğin: "image/jpeg") ve meta verileri (adı, açıklamayı, türü, vb.).

Azure dijital İkizlerini, cihazlar, alanları ve kullanıcılar için BLOB'ları eklemeyi destekler. BLOB'ları, bir kullanıcı, cihaz fotoğraf, video, bir harita veya bir günlük için profil resmi temsil edebilir.

> [!NOTE]
> Bu makalede varsayılır:
> * Örneğiniz doğru yönetim API isteklerini almak için yapılandırılmış olduğunu.
> * Doğru tercih ettiğiniz bir REST istemcisi kullanarak kimlik doğrulaması yaptınız olduğunu.

## <a name="uploading-blobs-an-overview"></a>Blobları karşıya yükleme: bir genel bakış

Çok bölümlü istekleri, özel uç noktaları ve onların ilgili işlevleri Blobları karşıya yüklemek için kullanılır.

> [!IMPORTANT]
> Çok bölümlü istekleri temel üç parça bilgi gerektirir. Azure dijital çiftleri için:
> * A **Content-Type** üst bilgi:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
> * A **içerik düzeni**: `form-data; name="metadata"`.
> * Karşıya yüklenecek dosya içeriği.
>
> Tam **Content-Type** ve **Content-Disposition** kullanım senaryosuna bağlı olarak değişebilir.

Her çok parçalı istek, birkaç parçalara ayrılır. Azure dijital İkizlerini yönetim API'leri için çok bölümlü istekleri bozuk **iki** (**2**) gibi bölümler:

1. İlk bölümü gereklidir ve ilişkili bir MIME türü gibi Blob meta verilerini içeren **Content-Type** ve **Content-Disposition** yukarıda.

1. İkinci bölümü, gerçek Blob içeriklerini (dosya yapılandırılmamış içeriğini) içerir.  

İki bölümü hiçbiri için gerekli olan **düzeltme eki** istekleri. Her ikisi için gerekli olan **POST** veya oluşturma işlemleri.

### <a name="blob-metadata"></a>Blob meta verileri

Ek olarak **Content-Type** ve **Content-Disposition**, çok bölümlü isteklerini doğru JSON gövdesi de belirtmeniz gerekir. Göndermek için hangi JSON gövdesi gerçekleştirilmekte olan HTTP isteği işlemi türüne bağlıdır.

Kullanılan dört ana JSON şemalarının şunlardır:

![Alanı BLOB'ları][1]

Bu model şemaları, tam ayrıntılı sağlanan Swagger belgelerinde açıklanmıştır.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Sağlanan başvuru belgeleri okuyarak kullanma hakkında bilgi edinin [Swagger'ı kullanmayı](./how-to-use-swagger.md).

### <a name="examples"></a>Örnekler

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Bir metin dosyası olarak bir Blob yükleyen ve boşluk ile ilişkilendiren bir POST isteğinde bulunmak için:

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

| Parametre değeri | Şununla değiştir |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Çok bölümlü içerik sınır adı |

Bir .NET uygulaması aynı Blob karşıya yükleme sınıfını kullanarak aşağıda sağlanan [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metaData in a suitable format
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

Aşağıda, ana uç noktalarının ve kendi özel işlevler bir kılavuz sağlanır.

### <a name="devices"></a>Cihazlar

Blobları cihazlara eklenebilir. (Yönetim API'leri için Swagger başvuru belgeleri gösteren) aşağıdaki görüntü, Blob tüketimi için cihaz ile ilgili API uç noktaları ve bunları geçirmek için gerekli yol parametreleri belirtir:

![Cihaz BLOB'ları][2]

Örneğin, güncelleştirmek veya Blob oluşturma ve Blob için bir cihaz eklemek için bir PATCH isteği için yapılır:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen Blob kimliği |

Başarılı istekler yanıtta DeviceBlob JSON nesnesi döndürür. DeviceBlobs aşağıdaki JSON şemaya uygun:

| Öznitelik | Tür | Açıklama | Örnekler |
| --- | --- | --- | --- |
| **DeviceBlobType** | Dize | Bir cihaza bağlı bir Blob kategorisi | `Model` ve `Specification` |
| **DeviceBlobSubtype*** | Dize | DeviceBlobType daha ayrıntılı bir Blob alt kategorisi | `PhysicalModel`, `LogicalModel`, `KitSpecification`, ve `FunctionalSpecification` |

> [!TIP]
> Yukarıdaki tabloda başarıyla döndürülen istek verileri işlemek için kullanın.

### <a name="spaces"></a>Alanları

Bloblar için alanları bağlı olmalıdır. Aşağıdaki görüntüde uygulamasına geçirmek için bir yol parametreleri birlikte BLOB'ları işlemek için sorumlu tüm boşluk API uç noktaları listeler:

![Alanı BLOB'ları][3]

Örneğin, bir Blob için bir alanı eklenmiş döndürmek için bir GET isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen Blob kimliği |

PATCH isteğinde aynı uç noktaya bir meta veri açıklamasını güncelleştirin ve Blob yeni bir sürümünü oluşturmak izin verir. HTTP isteği, tüm gerekli meta ve çok bölümlü form verilerinin yanı sıra düzeltme eki yöntemi kullanılarak yapılır.

Başarılı işlemler aşağıdaki şemaya uyan ve döndürülen veri tüketmek için kullanılan bir SpaceBlob döndürür:

| Öznitelik | Tür | Açıklama | Örnekler |
| --- | --- | --- | --- |
| **SpaceBlobType** | Dize | Bir alana bağlı bir Blob kategorisi | `Map` ve `Image` |
| **SpaceBlobSubtype** | Dize | SpaceBlobType daha ayrıntılı bir Blob alt kategorisi | `GenericMap`, `ElectricalMap`, `SatelliteMap`, ve `WayfindingMap` |

### <a name="users"></a>Kullanıcılar

Blobları kullanıcı modellerine bağlı (söylemek profil resmi ilişkilendirmek). Aşağıdaki görüntüde ilgili kullanıcıları API uç noktalarını gösterir ve tüm gerekli yol parametreleri gibi bir `id`:

![Kullanıcı BLOB'ları][4]

Örneğin, bir kullanıcıya bağlı olarak bir Blob getirmek için gerekli tüm form verilerini içeren bir GET isteği olun:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_BLOB_ID* | İstenen Blob kimliği |

Döndürülen JSON (UserBlobs), aşağıdaki JSON modelleri için de uyumlu olacaktır:

| Öznitelik | Tür | Açıklama | Örnekler |
| --- | --- | --- | --- |
| **UserBlobType** | Dize | Bir kullanıcıya bağlı bir Blob kategorisi | `Image` ve `Video` |
| **UserBlobSubtype** |  Dize | UserBlobType daha ayrıntılı bir Blob alt kategorisi | `ProfessionalImage`, `VacationImage`, ve `CommercialVideo` |

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Doğru üst bilgi bilgileri dahil değil:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Sağlanan Azure İkizlerini dijital Swagger başvuru belgeleri hakkında daha fazla bilgi edinmek için [dijital Swagger İkizlerini kullanma](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
