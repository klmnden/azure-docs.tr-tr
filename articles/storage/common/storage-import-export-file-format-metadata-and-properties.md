---
title: Azure içeri/dışarı aktarma meta verileri ve özellikleri dosya biçimi | Microsoft Docs
description: Meta veri ve alma işleminin parçası veya iş dışarı bir veya daha fazla BLOB'lar için özellikleri belirtin öğrenin.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: 840364c6-d9a8-4b43-a9f3-f7441c625069
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3f728ad94cdcbd32092b677f11a737ae91376720
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23873656"
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Azure içeri/dışarı aktarma hizmeti meta verileri ve özellikleri dosya biçimi
Meta veri ve bir veya daha fazla BLOB özelliklerini alma işi veya bir dışarı aktarma işinin parçası olarak belirtebilirsiniz. Meta veriler veya bir alma işinin bir parçası olarak oluşturulan BLOB'lar özelliklerini ayarlamak için bir meta veri ya da özellikler dosyası içeri aktarılacak veri içeren sabit sürücüde sağlar. Bir dışa aktarma işi için meta verileri ve özellikleri için döndürülen sabit sürücüde bulunan bir meta veri veya özellikler dosyasına yazılır.  
  
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
|`Metadata`|Kök öğesi|Meta veri dosyasının kök öğesinin.|  
|`metadata-name`|Dize|İsteğe bağlı. XML öğesi meta veriler blob'a belirtir ve meta veri ayarın değerini değerini belirtir.|  
  
## <a name="properties-file-format"></a>Özellikler dosya biçimi  
Özellikler dosyasının biçimi aşağıdaki gibidir:  
  
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
|`Properties`|Kök öğesi|Özellikler dosyasının kök öğesinin.|  
|`Last-Modified`|Dize|İsteğe bağlı. Son değişiklik zamanı blob'a. Dışarı aktarma işleri yalnızca.|  
|`Etag`|Dize|İsteğe bağlı. Blob'un ETag değeri. Dışarı aktarma işleri yalnızca.|  
|`Content-Length`|Dize|İsteğe bağlı. Blob bayt cinsinden boyutu. Dışarı aktarma işleri yalnızca.|  
|`Content-Type`|Dize|İsteğe bağlı. Blob içerik türü.|  
|`Content-MD5`|Dize|İsteğe bağlı. Blob'un MD5 karma değeri.|  
|`Content-Encoding`|Dize|İsteğe bağlı. Blob'un içerik kodlaması.|  
|`Content-Language`|Dize|İsteğe bağlı. Blob'un içerik dili.|  
|`Cache-Control`|Dize|İsteğe bağlı. Blob önbelleği denetim dizesi.|  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [blob özelliklerini ayarlama](/rest/api/storageservices/set-blob-properties), [Blob meta verileri ayarlama](/rest/api/storageservices/set-blob-metadata), ve [ayarı ve alınırken özelliklerini ve meta verileri için blob kaynakları](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) için blob meta verileri ve özellikleri ayarlama hakkında ayrıntılı kurallar.
