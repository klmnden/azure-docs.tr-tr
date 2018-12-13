---
title: Yük Dengeleyici TCP Sıfırla boşta | Microsoft Docs
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
ms.date: 12/03/2018
ms.author: kumud
ms.openlocfilehash: 3cc4db387c33388d5692fa25ac26f2b6399cd185
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879851"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer ile TCP boşta kalma (genel Önizleme) Sıfırla

Kullanabileceğiniz [Standard Load Balancer](load-balancer-standard-overview.md) her yapılandırılabilir boşta kalma zaman aşımı için çift yönlü TCP sıfırlar (TCP lk paket) ile senaryolarınız için daha öngörülebilir bir uygulama davranışı oluşturmak için.  Load Balancer'ın varsayılan davranışı, Akışlar bir akışın boşta kalma zaman aşımı ulaşıldığında sessizce bırak sağlamaktır.

![Yük Dengeleyici TCP Sıfırla](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>Şu anda genel önizleme olarak kullanılabilir ve sınırlı sayıda kullanılabilir TCP boşta kalma zaman aşımı işlevselliğini sıfırlama yük Dengeleyiciyle [bölgeleri](#regions). Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
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
| Güneydoğu Asya |
| Güney Brezilya |
| Orta Kanada |
| Batı Avrupa |
| Hindistan Orta |
| Hindistan Batı |
| Japonya Batı |
| Kore Orta |
| Kore Güney |
| UK Kuzey |
| UK Güney 2 |
| ABD Doğu |
| ABD Doğu 2 |
| ABD Kuzey |
| ABD Batı |

Bu tablo, Önizleme, diğer bölgelere ayrıntılı olarak güncelleştirilecektir.  

## <a name="limitations"></a>Sınırlamalar

- Sınırlı [bölge kullanılabilirliği](#regions).
- Portal, yapılandırma veya TCP sıfırlama görüntülemek için kullanılamaz.  Bunun yerine şablonları, REST API, Az CLI 2. 0'ı veya PowerShell kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [standart Load Balancer](load-balancer-standard-overview.md).
- Hakkında bilgi edinin [giden kuralları](load-balancer-outbound-rules-overview.md).
