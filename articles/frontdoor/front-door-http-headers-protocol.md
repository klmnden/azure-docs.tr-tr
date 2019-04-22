---
title: Protokol HTTP üst bilgilerini Azure ön kapısı hizmetinde desteği | Microsoft Docs
description: Bu makale ön kapısı hizmetinin desteklediği HTTP üst bilgisi protokolleri açıklar.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 92e8435e4336c68982e4becc2a95f99b2c776c0e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58861851"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door-service"></a>Azure ön kapısı hizmetinde HTTP üstbilgileri için protokol desteği
Bu makalede çağrı yolun bir kısmı ile ön kapısı hizmetinin desteklediği protokolü açıklar (resme bakın). Aşağıdaki bölümler, ön kapısı hizmeti tarafından desteklenen HTTP üstbilgileri hakkında daha fazla bilgi sağlar.

![Azure ön kapısı hizmet HTTP üstbilgileri Protokolü][1]

>[!IMPORTANT]
>Ön kapısı hizmet burada belgelenmemiş HTTP üst bilgilerini sertifika değil.

## <a name="client-to-front-door-service"></a>Ön kapısı hizmeti istemcisi
Ön kapısı hizmet gelen istek çoğu üst bilgiye, değişiklik yapmadan kabul eder. Bazı ayrılmış üstbilgiler X içeren üst bilgiler dahil gönderirse, gelen istek kaldırılır - FD-* öneki.

## <a name="front-door-service-to-backend"></a>Arka uç hizmetine ön kapısı

Ön kapısı hizmet kısıtlamaları nedeniyle kaldırıldı sürece gelen bir istek üst bilgileri içerir. Ön kapısı, aşağıdaki üst bilgileri de ekler:

| Üst bilgi  | Örnek ve açıklaması |
| ------------- | ------------- |
| Şunun aracılığıyla: |  Aracılığıyla: 1.1 azure </br> Ardından istemcinin HTTP sürümü ön kapısı ekler *Azure* Via üstbilgisinin değeri. Bu istemcinin HTTP sürümü gösterir ve bu ön kapısı istemci ve arka uç arasındaki istek için bir ara alıcı idi.  |
| X-Azure-ClientIP | X-Azure-ClientIP: 127.0.0.1 </br> İşlenmekte olan istek ile ilişkili istemci IP adresini temsil eder. Örneğin, bir proxy sunucudan gelen bir istek özgün arayan IP adresini belirtmek için X-iletilen-için üstbilgi ekleyebilirsiniz. |
| X-Azure-SocketIP |  X-Azure-SocketIP: 127.0.0.1 </br> Geçerli İsteğin kaynaklandığı TCP bağlantısıyla ilişkili yuva IP adresini temsil eder. Bir isteğin istemci IP adresi, rasgele bir kullanıcı tarafından kılınabilir çünkü yuva IP adresine eşit olmayabilir.|
| X-Azure-Ref |  X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz </br> Ön kapısı tarafından sunulan bir isteği tanımlayan bir başvuru benzersiz dize. Erişim günlükleri aramak için kullanılır ve sorun giderme için kritik.|
| X Azure RequestChain |  X-Azure-RequestChain: atlama sayısı = 1 </br> Üstbilgi isteği döngü algılamak için ön kapı kullanır ve kullanıcılar üzerinde bir bağımlılık almamalıdır. |
| X-iletilen-için | X-iletilen-için: 127.0.0.1 </br> X-Forwarded-For (XFF) HTTP üstbilgi alanı genellikle bir HTTP Ara sunucu veya yük dengeleyici aracılığıyla bir web sunucusuna bağlanan bir istemci isteğin kaynak IP adresini tanımlar. Varolan bir XFF üstbilgi varsa, ön kapı için istemci yuvası IP ekler veya istemci yuvası IP XFF üstbilgisi ekler. |
| X iletilen konak | X iletilen konak: contoso.azurefd.net </br> X iletilen konak HTTP üstbilgisi alanını konak HTTP istek üst bilgisinde istemci tarafından istenen özgün ana bilgisayar tanımlamak için kullanılan yaygın bir yöntemdir. İsteği işleyen arka uç sunucusu için ön kapı ana bilgisayar adından farklı olabilir çünkü. |
| X iletilen Proto | X iletilen Proto: http </br> X iletilen Proto HTTP üstbilgisi alanını, genellikle bir HTTP isteğinin kaynak Protokolü ön kapısı, yapılandırmasına bağlı olarak, arka uç ile HTTPS kullanarak iletişim kurabileceği çünkü tanımlamak için kullanılır. HTTP isteği için ters proxy olsa bile bu geçerlidir. |

## <a name="front-door-service-to-client"></a>İstemci hizmete ön kapısı

Ön kapısı arka ucundan gönderilen üst bilgileri de istemciye geçirilecek. Ön kapısı istemcilere gönderilen üst bilgiler verilmiştir.

| Üst bilgi  | Örnek |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Ön kapısı tarafından sunulan bir isteği tanımlayan benzersiz başvuru dize budur. Bu, erişim günlükleri aramak için kullanılan sorun giderme için önemlidir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Bir ön kapısı oluşturma](quickstart-create-front-door.md)
- [Ön kapısı işleyişi](front-door-routing-architecture.md)

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png