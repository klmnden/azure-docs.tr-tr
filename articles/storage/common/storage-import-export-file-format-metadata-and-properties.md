---
title: Azure içeri/dışarı aktarma meta veriler ve özellikler dosyası biçimi | Microsoft Docs
description: Meta veriler ve Özellikler bölümü içeri aktarma veya dışarı aktarma işi bir veya daha fazla bloblar için belirleme konusunda bilgi edinin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 5a886244b43ad006a95e9be0350d9c69fd987ad9
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526241"
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Azure içeri/dışarı aktarma hizmeti meta veriler ve özellikler dosyası biçimi
Meta veriler ve özellikler için bir veya daha fazla BLOB'ları içeri aktarma işi veya dışarı aktarma işi bir parçası olarak belirtebilirsiniz. Meta veri veya içeri aktarma işi bir parçası olarak oluşturulan BLOB'ları için özellikleri ayarlamak için sabit sürücüde içeri aktarılacak veri içeren bir meta veri veya özellikleri dosyası sağlayın. Bir dışarı aktarma işi için meta veriler ve özellikler için döndürülen sabit sürücüde bulunan bir meta veri veya özellikler dosyasına yazılır.  
  
## <a name="metadata-file-format"></a>Meta veri dosyası biçimi  
Meta veri dosyasının biçimi aşağıdaki gibidir:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|XML öğesi|Tür|Açıklama|  
|-----------------|----------|-----------------|  
|`Metadata`|Kök öğe|Meta veri dosyası kök öğesi.|  
|`metadata-name`|Dize|İsteğe bağlı. XML öğesi için blob meta verileri adını belirtir ve meta verilerini ayarın değerini değerini belirtir.|  
  
## <a name="properties-file-format"></a>Özellikler dosyası biçimi  
Özellikler dosyası biçimi aşağıdaki gibidir:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|XML öğesi|Tür|Açıklama|  
|-----------------|----------|-----------------|  
|`Properties`|Kök öğe|Özellikler dosyası kök öğesi.|  
|`Last-Modified`|Dize|İsteğe bağlı. Blob için son değiştirilme zamanı. Yalnızca dışarı aktarma işleri için.|  
|`Etag`|Dize|İsteğe bağlı. Blobun ETag değeri. Yalnızca dışarı aktarma işleri için.|  
|`Content-Length`|Dize|İsteğe bağlı. Blob bayt cinsinden boyutu. Yalnızca dışarı aktarma işleri için.|  
|`Content-Type`|Dize|İsteğe bağlı. Blob içerik türü.|  
|`Content-MD5`|Dize|İsteğe bağlı. Blobun MD5 karma değeri.|  
|`Content-Encoding`|Dize|İsteğe bağlı. Blobun içeriği kodlama.|  
|`Content-Language`|Dize|İsteğe bağlı. Blobun içerik dili.|  
|`Cache-Control`|Dize|İsteğe bağlı. Blob için önbellek denetimi dizesi.|  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [blob özelliklerini ayarlama](/rest/api/storageservices/set-blob-properties), [Blob meta verileri ayarlama](/rest/api/storageservices/set-blob-metadata), ve [ayarlama ve alma özellikleri ve meta verileri için blob kaynakları](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) ayarı blob meta verileri hakkında ayrıntılı kuralları için ve özellikleri.
