---
title: Yük Dengeleyici TCP sıfırlama Azure'da boştayken
titlesuffix: Azure Load Balancer
description: Boşta kalma zaman aşımı çift yönlü TCP lk paketlere sahip yük dengeleyici
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2019
ms.author: kumud
ms.openlocfilehash: 4a09492fcb8a7985fa27b6daae89aa5dec0fa6e0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413852"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer ile TCP boşta kalma (genel Önizleme) Sıfırla

Kullanabileceğiniz [Standard Load Balancer](load-balancer-standard-overview.md) için belirli bir kural sıfırlama TCP boşta üzerinde'ı etkinleştirerek senaryolarınız için daha öngörülebilir bir uygulama davranışı oluşturmak için. Load Balancer'ın varsayılan davranışı, Akışlar bir akışın boşta kalma zaman aşımı ulaşıldığında sessizce bırak sağlamaktır.  Bu özelliği etkinleştirmek, Load Balancer'ı çift yönlü TCP sıfırlar (TCP lk paket) boşta kalma zaman aşımını göndermeye neden olur.  Bu bağlantı zaman aşımına uğradı ve artık kullanılamaz, uygulama uç noktalarını bildirir.  Uç noktaları, hemen gerekirse yeni bir bağlantı kurabilirsiniz.

![Yük Dengeleyici TCP Sıfırla](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>TCP boşta kalma zaman aşımı işlevselliğini sıfırlama yük Dengeleyiciyle şu anda genel önizleme olarak kullanılabilir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
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

Tüm bölgelerde kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

- Portal, yapılandırma veya TCP sıfırlama görüntülemek için kullanılamaz.  Bunun yerine şablonları, REST API, Az CLI 2. 0'ı veya PowerShell kullanın.
- TCP lk ESTABLISHED durumdaki TCP bağlantısı sırasında yalnızca gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [standart Load Balancer](load-balancer-standard-overview.md).
- Hakkında bilgi edinin [giden kuralları](load-balancer-outbound-rules-overview.md).
