---
title: HTTP üst bilgilerini Azure ön kapısı hizmeti - destek | Microsoft Docs
description: Bu makale ön kapısı tarafından desteklenen HTTP üst bilgisi protokoller anlamanıza yardımcı olur.
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
ms.openlocfilehash: 40bfdfc3837da12f62864433508482a65def291c
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407326"
---
# <a name="azure-front-door-service---http-headers-support"></a>Azure ön kapısı hizmeti - HTTP üstbilgileri desteği
Bu belge, aşağıdaki görüntüde belirtildiği gibi arama yolu çeşitli bölümleriyle Azure ön kapısı hizmetinin desteklediği protokolü özetlenmektedir. Aşağıdaki bölümlerde, ön kapısı desteklediği HTTP üstbilgileri hakkında daha fazla öngörüye verilmektedir.

![Azure ön kapısı hizmet HTTP üstbilgileri Protokolü][1]

>[!WARNING]
>Azure ön kapısı hizmet burada belgelenmemiş bir HTTP üstbilgileri garanti sağlamaz.

## <a name="1-client-to-front-door"></a>1. İstemci ön kapısı
(Bunları değiştirmeden) ön kapısı çoğu üst bilgiye gelen istek kabul eder, ancak bunlar gönderiliyorsa, gelen istekte kaldırılacak bazı ayrılmış üstbilgiler vardır. Bu, aşağıdaki ön ekleri ile üst bilgileri içerir:
 - X-FD-*

## <a name="2-front-door-to-backend"></a>2. Arka uca ön kapısı

Yukarıda bahsedilen kısıtlamaları nedeniyle kaldırıldı sürece ön kapısı gelen istek üstbilgileri dahil edilir. Ön kapısı ayrıca aşağıdaki üst bilgileri ekler:

| Üst bilgi  | Örnek ve açıklaması |
| ------------- | ------------- |
| Şunun aracılığıyla: |  *Aracılığıyla: 1.1 azure* </br> İstemcinin HTTP sürümü üst bilgi değeri olarak 'Azure' arkasından ön kapısı ekler. İstemcinin HTTP sürümü belirtmek için eklenen ve isteği arka uç ile istemci arasında bir ara alıcısı, Azure ön kapısı idi.  |
| X-Azure-ClientIP | *X-Azure-Clientıp: 127.0.0.1* </br> İşlenmekte olan istek ile ilişkili "istemci" Internet Protokol adresi temsil eder. Örneğin, bir proxy sunucudan gelen bir istek özgün arayan IP adresini belirtmek için X-iletilen-için üst bilgisi ekleyebilir. |
| X-Azure-SocketIP | *X-Azure-SocketIP: 127.0.0.1* </br> Geçerli İsteğin kaynaklandığı TCP bağlantısıyla ilişkili yuva Internet Protokol adresi temsil eder. Bir isteğin istemci IP adresi, rasgele bir son kullanıcı tarafından kılınabilir çünkü yuva IP adresine eşit olmayabilir.|
| X-Azure-Ref | *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Ön kapısı tarafından sunulan bir isteği tanımlayan benzersiz başvuru dize budur. Erişim günlükleri aramak için kullanılan sorun giderme için önemlidir.|
| X Azure RequestChain |  *X-Azure-RequestChain: atlama sayısı = 1* </br> Bu istek döngü algılamak için ön kapı kullanan bir üst bilgi ve kullanıcılar üzerinde bir bağımlılık almamalıdır. |
| X-iletilen-için | *X-iletilen-için: 127.0.0.1* </br> X-Forwarded-For (XFF) HTTP üstbilgi alanı, isteğin kaynak IP adresini bir web sunucusuna HTTP Ara sunucu veya yük dengeleyici üzerinden bağlanan bir istemci tanımlamaya yönelik yaygın bir yöntemdir. Varsa mevcut bir XFF üst bilgisi ön kapısı istemci yuvası IP başka ona ekler sonra istemci yuvası IP XFF üstbilgiyle ekler. |
| X iletilen konak | *X iletilen konak: contoso.azurefd.net* </br> X iletilen konak HTTP üstbilgisi alanını olduğundan istek işleme arka uç sunucusu için ön kapı ana bilgisayar adından farklı olabilir, ana bilgisayar HTTP istek üst bilgisinde istemci tarafından istenen özgün ana bilgisayar tanımlamaya yönelik yaygın bir yöntemdir. |
| X iletilen Proto | *X iletilen Proto: http* </br> X iletilen Proto HTTP üstbilgisi alanını yapılandırmasına bağlı olarak ön kapısı HTTP isteği için ters proxy olsa bile, HTTPS kullanarak arka uç ile iletişim kurmaya devam edebileceğini beri kaynak protokolü bir HTTP isteğinin tanımlamak için yaygın bir yöntemdir. |

## <a name="3-front-door-to-client"></a>3. İstemci ön kapısı

Ön kapısı istemcilere gönderilen üst bilgiler aşağıda verilmiştir. Arka ucunuzdan ön kapısı gönderilen tüm üstbilgileri istemciye üzerinden geçirilir.

| Üst bilgi  | Örnek |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Ön kapısı tarafından sunulan bir isteği tanımlayan benzersiz başvuru dize budur. Erişim günlükleri aramak için kullanılan sorun giderme için önemlidir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png