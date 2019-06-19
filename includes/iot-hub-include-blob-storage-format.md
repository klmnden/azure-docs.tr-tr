---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: c779147e464a592d45da8a9a2d8e812320dc23e8
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188994"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Verileri BLOB depolamaya her ikisinde yazılabilir [Apache Avro](https://avro.apache.org/) , varsayılan veya (Önizleme) JSON biçiminde. 
>    
> IOT hub'ı kullanılabilir olduğu, Doğu ABD, Batı ABD ve Batı Avrupa dışındaki tüm bölgelerde önizleme özelliği JSON biçiminde kodlamak için kullanılabilir. Kodlama biçimi, yalnızca blob depolama uç noktası yapılandırıldıktan zamanında ayarlanabilir. Biçim, önceden ayarlanmış bir uç nokta için değiştirilemez. JSON encoding kullanıldığında, JSON ve UTF-8 contentEncoding ileti sistemi özelliklerindeki contentType ayarlamanız gerekir. 
>
> Bir blob depolama uç noktası kullanma hakkında daha ayrıntılı bilgi için lütfen bkz [blob depolama için yönlendirme yönergeleri](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-blob-storage).
>