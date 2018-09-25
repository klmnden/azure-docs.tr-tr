---
title: Boşta kalma zaman aşımı üzerinde yük dengeleyici TCP sıfırlama | Microsoft Docs
description: Boşta kalma zaman aşımı çift yönlü TCP lk paketlere sahip yük dengeleyici
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/21/2018
ms.author: kumud
ms.openlocfilehash: b33c701bde082404ea86c9882dcb7bf50d1f1df9
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036193"
---
# <a name="load-balancer-with-tcp-reset-on-idle-timeout-public-preview"></a>Boşta kalma zaman aşımı (genel Önizleme) üzerinde yük dengeleyici ile TCP Sıfırla

Kullanabileceğiniz [Standard Load Balancer](load-balancer-standard-overview.md) her yapılandırılabilir boşta kalma zaman aşımı için çift yönlü TCP sıfırlar (TCP lk paket) ile senaryolarınız için daha öngörülebilir bir uygulama davranışı oluşturmak için.  Load Balancer'ın varsayılan davranışı, Akışlar bir akışın boşta kalma zaman aşımı ulaşıldığında sessizce bırak sağlamaktır.

![Yük Dengeleyici TCP Sıfırla](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>Şu anda genel önizleme olarak kullanılabilir ve sınırlı sayıda kullanılabilir TCP boşta kalma zaman aşımı işlevselliğini sıfırlama yük Dengeleyiciyle [bölgeleri](#regions). Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Belirli özellikler desteklenmiyor olabilir ya da kısıtlı yeteneklere sahip olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
Bu varsayılan davranış ve üzerinde bir gelen NAT kuralları, Yük Dengeleme kuralları, boşta kalma zaman aşımını TCP sıfırlar gönderme etkinleştir değiştirmek ve [giden kuralları](https://aka.ms/lboutboundrules).  Yük Dengeleyici kuralı başına etkin olduğunda, gönderir çift TCP Reset (TCP lk paketleri) yönlü istemci ve sunucu uç noktalarına eşleşen tüm akışlar için boşta kalma zaman aşımı, zamanında.

TCP lk paketleri alma uç noktaları, karşılık gelen yuva hemen kapatın. Bu bağlantının yayın oluştu ve gelecekteki tüm iletişimi aynı TCP bağlantısı başarısız olur Uç noktalara anında bildirim sağlar.  Yuva kapatır ve sonunda zaman aşımına TCP bağlantısı beklemeden gerektiğinde bağlantıları yeniden uygulamaların bağlantılarını temizleyebilirsiniz.

Birçok senaryo için bu akış, boşta kalma zaman aşımı yenilemek için gönderme TCP (veya uygulama katmanı) gerek canlı tutma azaltabilir. 

Boşta kalma süreleri, yapılandırma tarafından izin verilen en fazla veya etkin TCP sıfırlar ile uygulamanızı istenmeyen bir davranış gösterir, TCP bağlantılarının canlılık izlemek için TCP canlı tutma (veya uygulama katmanı canlı tutma) kullanmanız gerekebilir.  Ayrıca, canlı tutma ayrıca bağlantı herhangi bir yolu, özellikle uygulama katmanı canlı tutma proxy olduğunda için yararlı kalabilir.  

TCP boşta kalma zaman aşımını ayarlama sıfırlar, etkinleştirmenizi yararlı olup olmadığını ve istenen uygulamanın davranışı sağlamak için ek adımlar gerekebilir karar vermek için tüm uçtan uca senaryo dikkatle inceleyin.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Boşta kalma zaman aşımını TCP sıfırlama etkinleştirme

API sürümü 2018-07-01 kullanarak, iki TCP sıfırlar yönlü üzerinde bir kural başına temelinde boşta kalma zaman aşımı gönderimi etkinleştirebilirsiniz:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a> Bölge kullanılabilirliği

Bu parametre aşağıdaki bölgelerde şu anda etkilidir.  Burada listelenmeyen bölgelerde parametrenin etkisi yoktur.

| Bölge |
|---|
| ABD Doğu 2 |
| ABD Kuzey |
| ABD Batı |

Bu tablo, Önizleme, diğer bölgelere ayrıntılı olarak güncelleştirilecektir.  

## <a name="limitations"></a>Sınırlamalar

- Sınırlı [bölge kullanılabilirliği](#regions).
- Portal, yapılandırma veya TCP sıfırlama görüntülemek için kullanılamaz.  Bunun yerine şablonları, REST API, Az CLI 2. 0'ı veya PowerShell kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [standart Load Balancer](load-balancer-standard-overview.md).
- Hakkında bilgi edinin [giden kuralları](https://aka.ms/lboutboundrules).