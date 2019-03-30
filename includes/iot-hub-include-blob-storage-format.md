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
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631045"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Verileri BLOB depolamaya her ikisinde yazılabilir [Apache Avro](https://avro.apache.org/) , varsayılan veya (Önizleme) JSON biçiminde. 
>    
> IOT hub'ı kullanılabilir olduğu, Doğu ABD, Batı ABD ve Batı Avrupa dışındaki tüm bölgelerde önizleme özelliği JSON biçiminde kodlamak için kullanılabilir. Kodlama biçimi, yalnızca blob depolama uç noktası yapılandırıldıktan zamanında ayarlanabilir. Biçim, önceden ayarlanmış bir uç nokta için değiştirilemez. JSON encoding kullanıldığında, JSON ve UTF-8 contentEncoding ileti sistemi özelliklerindeki contentType ayarlamanız gerekir. 
>
> Bir blob depolama uç noktası kullanma hakkında daha ayrıntılı bilgi için lütfen bkz [blob depolama için yönlendirme yönergeleri](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-blob-storage).
>