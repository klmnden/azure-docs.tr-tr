---
title: Birden çok ön uçlar Azure için yük dengeleyici | Microsoft Docs
description: Azure yük dengeleyici birden çok ön Uçlarda genel bakış
services: load-balancer
documentationcenter: na
author: chkuhtz
manager: narayan
editor: ''
ms.assetid: 748e50cd-3087-4c2e-a9e1-ac0ecce4f869
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: chkuhtz
ms.openlocfilehash: cf8fa396e0518e1c847225dfc1d8f91c3421bd11
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30181498"
---
# <a name="multiple-frontends-for-azure-load-balancer"></a>Azure yük dengeleyici için birden çok ön Uçlar

Azure yük dengeleyici, birden çok bağlantı noktası, birden çok IP adresi veya her ikisi de Hizmetleri yüklemenizi sağlar. Genel ve iç yük dengeleyici tanımları Bakiye akışları VM'ler kümesi boyunca yüklemek için kullanabilirsiniz.

Bu makalede, bu özelliği, önemli kavramlar ve kısıtlamaları temelleri açıklanır. Yalnızca bir IP adresi hizmetlerinde kullanıma sunmak istiyorsanız, için Basitleştirilmiş yönergelerini bulabilirsiniz [ortak](load-balancer-get-started-internet-portal.md) veya [iç](load-balancer-get-started-ilb-arm-portal.md) yük dengeleyici yapılandırmalarının. Birden çok ön uçlar eklemek için tek bir ön uç yapılandırma artımlı olur. Bu makalede kavramları kullanarak herhangi bir zamanda Basitleştirilmiş bir yapılandırma genişletebilirsiniz.

Bir Azure yük dengeleyici tanımladığınızda, bir ön uç ve arka uç havuzunu yapılandırma kuralları ile bağlanır. Kuralı tarafından başvurulan durumu araştırması nasıl yeni akışları belirlemek için kullanılan bir düğüme arka uç havuzundaki gönderilir. Ön uç (diğer adıyla VIP), bir IP adresi (ortak veya dahili), Aktarım Protokolü (UDP veya TCP) ve Yük Dengeleme kuralı bağlantı noktası numarasından oluşan 3-tanımlama grubu tarafından tanımlanır. Arka uç havuzu, yük dengeleyici arka uç havuzu başvuran sanal makine IP yapılandırmaları (NIC kaynak parçası) koleksiyonudur.

Aşağıdaki tabloda bazı örnek ön uç yapılandırmaları içerir:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

Tablo dört farklı ön uçlar gösterir. Ön Uçlar #1, #2 ve #3 birden çok kural ile tek bir ön uç değerlerdir. Aynı IP adresi kullanılır ancak protokolü ve bağlantı noktası her ön uç için farklıdır. Ön Uçlar #1 ve #4 bir birden çok ön uçlar, aynı ön uç protokolü ve bağlantı noktası birden çok ön uçlar burada yeniden örnektir.

Azure yük dengeleyici Yük Dengeleme kuralları tanımlama esneklik sağlar. Nasıl bir adresi ve bağlantı noktası ön uç üzerinde eşlendiği arka uç bağlantı noktası ve hedef adresi için bir kural bildirir. Arka uç bağlantı noktaları kuralları yeniden kural türüne bağlıdır. Her kural türünün, ana bilgisayar yapılandırması ve araştırma tasarım etkileyebilir belirli gereksinimleri vardır. İki kural türü vardır:

1. Hiçbir arka uç bağlantı noktasını yeniden varsayılan kural
2. Arka uç bağlantı noktaları nerede tekrar kayan IP kuralı

Azure yük dengeleyici aynı yük dengeleyici yapılandırmasında her iki kural türlerini karma olarak sağlar. Kural kısıtlamalar tarafından uymanız sürece yük dengeleyici bunları aynı anda belirli bir VM veya herhangi bir birleşimini kullanabilirsiniz. Seçtiğiniz hangi kural türü, uygulamanızın gereksinimlerine ve bu yapılandırmayı destekleyen karmaşıklığına bağlıdır. Hangi kural türlerinin senaryonuz için en iyi olduğunu değerlendirmelidir.

Biz bu senaryolar daha fazla varsayılan davranışı ile başlatarak keşfedin.

## <a name="rule-type-1-no-backend-port-reuse"></a>Kural türü #1: hiçbir arka uç bağlantı noktasını yeniden kullanma

![Yeşil ve mor ön uç ile birden çok ön uç çizim](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Bu senaryoda, ön uçlar aşağıdaki gibi yapılandırılır:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| ![Yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![Mor ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

DIP gelen akışının hedefi değil. Arka uç havuzundaki her VM benzersiz bir bağlantı noktası bir DIP üzerinde istenen hizmeti kullanıma sunar. Bu hizmet, bir kural tanımı aracılığıyla ön uç ile ilişkilidir.

Şu iki kural tanımlayın:

| Kural | Ön uç eşleme | Arka uç havuzuna |
| --- | --- | --- |
| 1 |![Yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Azure yük dengeleyici tam eşlemesindeki şimdi aşağıdaki gibidir:

| Kural | Ön uç IP adresi | protokol | port | Hedef | port |
| --- | --- | --- | --- | --- | --- |
| ![Yeşil kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |DIP IP adresi |80 |
| ![Mor kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |DIP IP adresi |81 |

Her kural benzersiz bir hedef IP adresi ve hedef bağlantı noktası birleşimi ile akışını üretmek gerekir. Hedef bağlantı noktası akışının değiştirerek, birden çok kural akışları farklı bağlantı noktalarını aynı DIP ulaştırabilirsiniz.

Sistem durumu araştırmalarının her zaman bir VM DIP için yönlendirilirsiniz. Sağlamanız gerekir, araştırma VM durumunu yansıtır.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Kural türü #2: kayan IP kullanarak arka uç bağlantı noktasını yeniden kullanma

Azure yük dengeleyici ön uç bağlantı noktası üzerinden birden çok ön uçlar kullanılan kural türü ne olursa olsun yeniden kullanma esnekliği sağlar. Ayrıca, bazı uygulama senaryoları tercih veya arka uç havuzundaki tek bir VM üzerindeki birden çok uygulama örneği tarafından kullanılmak üzere aynı bağlantı noktası gerektirir. Bağlantı noktası yeniden ortak örnekleri için yüksek kullanılabilirliği kümeleme içerir, ağ sanal Gereçleri ve birden çok TLS uç noktaları yeniden şifreleme olmadan gösterme.

Arka uç bağlantı noktası birden çok kural yeniden kullanmak istiyorsanız, kural tanımı kayan IP etkinleştirmeniz gerekir.

"Kayan IP" doğrudan sunucu Döndür (DSR olarak) bilinen, bir kısmını Azure'nın terminolojisi gerçekleştirilir. DSR iki bölümden oluşur: Eşleme Şeması bir akış topolojisi ve bir IP adresi. Bir platform düzeyde Azure yük dengeleyici DSR akış topolojisinde kayan IP etkin olup olmadığı bağımsız olarak her zaman çalışır. Bu, doğrudan kaynağa geri akışı için bir akış giden parçası her zaman doğru yeniden anlamına gelir.

Varsayılan kural türüyle Azure geleneksel Yük Dengeleme kullanım kolaylığı için IP adresi eşleme şeması kullanıma sunar. Kayan IP etkinleştirme aşağıda açıklandığı gibi ek esneklik sağlamak amacıyla IP adresi eşleme şeması değiştirir.

Aşağıdaki diyagramda bu yapılandırma gösterilmektedir:

![Yeşil ve mor ön uç DSR ile birlikte birden çok ön uç çizim](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Bu senaryo için arka uç havuzundaki her VM üç ağ arabirimi bulunur:

* DIP: sanal (Azure'un NIC kaynak IP yapılandırması) VM ile ilişkili NIC
* Ön uç 1: bir geri döngü arabirimde Konuk ön uç 1 IP adresiyle yapılandırılmış işletim sistemi
* Ön uç 2: bir geri döngü arabirimde Konuk ön uç 2 IP adresiyle yapılandırılmış işletim sistemi

> [!IMPORTANT]
> Geri döngü arabirimlerinin yapılandırmasını konuk işletim sistemi içinde gerçekleştirilir. Bu yapılandırma değil gerçekleştirilen veya Azure tarafından yönetiliyor. Bu yapılandırma olmazsa, kurallar çalışmayacaktır. Sistem durumu araştırma tanımları DSR ön uç temsil eden döngü arabirimine yerine VM DIP kullanın. Bu nedenle, hizmetiniz DSR ön uç temsil eden geri döngü arabirimine sunulan hizmetinin durumunu yansıtacak bir DIP bağlantı noktasında araştırma yanıtları sağlamanız gerekir.

Önceki senaryoda olduğu gibi aynı ön uç yapılandırma varsayalım:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| ![Yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![Mor ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Şu iki kural tanımlayın:

| Kural | Ön uç | Arka uç havuzuna eşleme |
| --- | --- | --- |
| 1 |![kural](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 (içinde VM1 ve VM2) |
| 2 |![kural](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 (içinde VM1 ve VM2) |

Aşağıdaki tabloda yük dengeleyicide tam eşleme gösterilmektedir:

| Kural | Ön uç IP adresi | protokol | port | Hedef | port |
| --- | --- | --- | --- | --- | --- |
| ![Yeşil kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |aynı ön uç (65.52.0.1) |aynı ön uç (80) |
| ![Mor kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |aynı ön uç (65.52.0.2) |aynı ön uç (80) |

Gelen akış hedef VM geri döngü arabirimindeki ön uç IP adresidir. Her kural benzersiz bir hedef IP adresi ve hedef bağlantı noktası birleşimi ile akışını üretmek gerekir. Akış hedef IP adresini değiştirerek, bağlantı noktası yeniden aynı VM'de mümkündür. Hizmetiniz için yük dengeleyici ön uç'ın IP adresi ve bağlantı noktası ilgili geri döngü arabiriminin bağlayarak açıktır.

Bu örnek hedef bağlantı noktası değiştirmez dikkat edin. Bu bir kayan IP senaryo olsa da, Azure yük dengeleyici arka uç hedef bağlantı noktası yeniden yazma ve ön uç hedef bağlantı noktası farklı sağlamak için kural tanımlama de destekler.

Kayan IP kural türü birden fazla yük dengeleyici yapılandırması desenlerini oluşturmaktadır. Şu anda kullanılabilir bir örnektir [SQL AlwaysOn ile birden çok dinleyici](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) yapılandırma. Zamanla, biz bu senaryoların birkaçı belge.

## <a name="limitations"></a>Sınırlamalar

* Birden çok ön uç yapılandırmaları yalnızca Iaas VM'ler ile desteklenir.
* Kayan IP kuralla uygulamanız için giden trafik akışları birincil IP yapılandırması kullanmanız gerekir. Uygulamanızı yapılandırılmış ön uç IP adresine bağlar, geri döngü üzerinde konuk işletim sistemi, Azure'nın SNAT arabiriminde giden akış yeniden kullanılabilir değil ve akış başarısız olur.
* Genel IP adresleri faturalama üzerinde bir etkisi yoktur. Daha fazla bilgi için bkz: [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Abonelik sınırları uygulayın. Daha fazla bilgi için bkz: [hizmet sınırları](../azure-subscription-service-limits.md#networking-limits) Ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [giden bağlantılar](load-balancer-outbound-connections.md) giden bağlantı davranışını birden çok ön Uçlarda etkisini anlamak için.
