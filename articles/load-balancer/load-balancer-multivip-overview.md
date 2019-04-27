---
title: Azure Load Balancer için birden çok ön uç
titlesuffix: Azure Load Balancer
description: Azure yük dengeleyici ön Uçlarda birden çok genel bakış
services: load-balancer
documentationcenter: na
author: chkuhtz
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: chkuhtz
ms.openlocfilehash: b9a140314b8eba6386c37bdbcf2bb3de58589335
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60594168"
---
# <a name="multiple-frontends-for-azure-load-balancer"></a>Azure Load Balancer için birden çok ön uç

Azure Load Balancer, birden çok bağlantı noktası, birden çok IP adresi veya her ikisi de hizmetlerin yükünü dengelemeyi sağlar. Genel ve iç yük dengeleyici tanımları Bakiye akışlar bir VM'ler kümesi arasında yüklemek için kullanabilirsiniz.

Bu makalede, bu özelliği, önemli kavramları ve kısıtlamalar temelleri açıklanır. Yalnızca bir IP adresi Hizmetleri kullanıma sunmak istiyorsanız, için Basitleştirilmiş yönergelerini bulabilirsiniz [genel](load-balancer-get-started-internet-portal.md) veya [iç](load-balancer-get-started-ilb-arm-portal.md) yük dengeleyici yapılandırmalarının. Birden çok ön uç ekleme tek bir ön uç yapılandırmasına artımlıdır. Bu makaledeki kavramları kullanan basitleştirilmiş bir yapılandırma herhangi bir zamanda genişletebilirsiniz.

Azure Load Balancer tanımladığınızda, bir ön uç ve arka uç havuzu yapılandırma kuralları ile bağlanır. Kuralı tarafından başvurulan sistem durumu araştırması nasıl yeni akışlar belirlemek için kullanılan arka uç havuzundaki bir düğüme gönderilir. Ön uç (aynı zamanda VIP olarak da bilinir), bir IP adresi (public veya internal) Aktarım Protokolü (UDP veya TCP) ve Yük Dengeleme kuralını bir bağlantı noktası numarasından oluşan 3-tanımlama grubu tarafından tanımlanır. Arka uç havuzu, yük dengeleyici arka uç havuzu başvuran sanal makine IP yapılandırmaları (NIC kaynağı parçası) koleksiyonudur.

Aşağıdaki tabloda bazı örnek ön uç yapılandırmaları içerir:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

Tabloda dört farklı ön uçlar gösterilmiştir. Ön Uçlar #1, #2 ve #3 olan tek bir ön uç ile birden çok kural. Aynı IP adresi kullanılır ancak protokolü ve bağlantı noktası her ön uç için farklıdır. Ön Uçlar #1 ve #4 bir birden çok ön uç, aynı ön uç protokol ve bağlantı noktası birden çok ön uç burada yeniden örnektir.

Azure Load Balancer, Yük Dengeleme kuralları tanımlama esneklik sağlar. Bir kural, nasıl bir adresi ve bağlantı noktasına frontend eşlenmiş durumda arka uç bağlantı noktası ve hedef adresi bildirir. Arka uç bağlantı noktası kuralları arasında olup olmadığını tekrar kuralın türüne bağlıdır. Her kural türünün konak yapılandırması ve araştırma tasarım etkileyen belirli gereksinimleri vardır. Kuralları iki tür vardır:

1. Varsayılan kural herhangi bir arka uç bağlantı noktası yeniden ile
2. Arka uç bağlantı noktaları burada tekrar kayan IP kuralı

Azure Load Balancer, aynı yük dengeleyici yapılandırmasında iki kural türlerini karıştırmak olanak tanır. Kuralın kısıtlamaları uymayı sürece yük dengeleyici bunları aynı anda belirli bir sanal Makinenin veya herhangi bir birleşimini kullanabilirsiniz. Seçtiğiniz hangi kural türü, uygulamanızın gereksinimlerini ve bu yapılandırmayı destekleyen karmaşıklığına bağlıdır. Hangi kural türlerinin senaryonuz için en iyi olduğunu değerlendirmelidir.

Varsayılan davranışı ile başlatarak bu senaryolar daha fazla inceleyeceğiz.

## <a name="rule-type-1-no-backend-port-reuse"></a>Kural türü #1: Herhangi bir arka uç bağlantı noktası yeniden

![Yeşil ve mor ön uç ile birden çok ön uç çizim](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Bu senaryoda, ön uçlar aşağıdaki gibi yapılandırılır:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| ![Yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![Mor ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

DIP gelen akışta hedefi olan. Arka uç havuzundaki her VM istenen hizmette bir DIP benzersiz bir bağlantı noktasını kullanıma sunar. Bu hizmet, bir kural tanımı ile ön uç ile ilişkilidir.

İki kural tanımlarız:

| Kural | Ön uç eşleme | Arka uç havuzu |
| --- | --- | --- |
| 1 |![Yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![Arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![Arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![Arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![Arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Azure Load Balancer tam eşlemede artık şu şekilde olur:

| Kural | Ön uç IP adresi | protokol | port | Hedef | port |
| --- | --- | --- | --- | --- | --- |
| ![Yeşil kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |DIP adresi |80 |
| ![Mor kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |DIP adresi |81 |

Her kural, benzersiz bir hedef IP adresine ve hedef bağlantı noktası bileşimi ile akış üretmesi gerekir. Hedef bağlantı noktası akışın değiştirerek birden çok kural, farklı bağlantı noktalarını aynı dıp'ye akışları sunabilirsiniz.

Sistem durumu araştırmaları, her zaman sanal makinenin DIP için yönlendirilir. Emin olmanız gerekir, araştırma VM durumunu yansıtır.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Kural türü #2: kayan IP kullanarak arka uç bağlantı noktası yeniden kullanma

Azure yük dengeleyici ön uç bağlantı noktası üzerinden kullanılan kural türünden bağımsız olarak birden çok ön uç yeniden kullanma esnekliği sağlar. Ayrıca, bazı uygulama senaryolarında tercih veya arka uç havuzundaki tek bir VM'de birden çok uygulama örnekleri tarafından kullanılmak üzere aynı bağlantı noktası gereklidir. Genel bağlantı noktası yeniden örnekleri ağ sanal Gereçleri ve yeniden şifreleme olmadan birden çok TLS uç noktaları kullanıma sunduğundan, daha yüksek kullanılabilirlik için kümeleme içerir.

Arka uç bağlantı noktası birden çok kural yeniden kullanmak istiyorsanız, kural tanımı kayan IP etkinleştirmeniz gerekir.

"Kayan IP" doğrudan sunucu döndürür (DSR olarak) bilinen, bir kısmını Azure'nın terminolojisi gerçekleştirilir. DSR iki bölümden oluşur: eşleme şemasını bir akış topolojisi ve bir IP adresi. Platform düzeyinde, Azure Load Balancer DSR akış topolojisinde kayan IP etkin olup olmadığının bağımsız olarak her zaman çalışır. Bu, doğrudan başlangıç noktasına geri akışı giden bir akışın parçası her zaman doğru bir şekilde yeniden anlamına gelir.

Varsayılan kural türüyle Azure geleneksel Yük Dengeleme IP adresi eşleme düzeni kullanım kolaylığı sunar. Kayan IP etkinleştirme, aşağıda açıklandığı gibi ek esneklik sağlamak amacıyla IP adresi eşleme şemasını değiştirir.

Aşağıdaki diyagramda bu yapılandırma gösterilmektedir:

![DSR ile yeşil ve mor ön uç ile birden çok ön uç çizim](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Bu senaryo için arka uç havuzundaki her VM, üç ağ arabirimi bulunur:

* DIP: bir sanal (Azure'nın NIC kaynak IP yapılandırması) VM ile ilişkili NIC
* Ön uç 1: 1 ön uç IP adresiyle yapılandırılması dışında bir işletim sistemi Konuk içinde geri döngü arabirimine bir
* Ön uç 2: bir döngü arabirimde Konuk 2 ön uç IP adresiyle yapılandırılması dışında bir işletim sistemi

> [!IMPORTANT]
> Geri döngü arabirimlerinin yapılandırmasını, konuk işletim sistemi içinde gerçekleştirilir. Bu yapılandırma değil gerçekleştirilen veya Azure tarafından yönetilir. Bu yapılandırma olmazsa, kuralları çalışmaz. Sistem durumu araştırması tanımları DSR ön uç temsil eden geri döngü arabirimine yerine sanal Makineyi DIP kullanın. Bu nedenle, hizmetinizi DSR ön uç temsil eden geri döngü arabirimine üzerinde sunulan hizmetin durumunu yansıtacak bir DIP bağlantı noktası üzerinde araştırma yanıtları sağlamanız gerekir.

Önceki senaryonun olduğu gibi aynı ön uç yapılandırması varsayalım:

| Ön uç | IP adresi | protokol | port |
| --- | --- | --- | --- |
| ![Yeşil ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![Mor ön uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

İki kural tanımlarız:

| Kural | Ön uç | Arka uç havuzuna eşleme |
| --- | --- | --- |
| 1 |![kural](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![Arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 (içinde VM1 ve VM2) |
| 2 |![kural](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![Arka uç](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 (içinde VM1 ve VM2) |

Aşağıdaki tabloda yük dengeleyicide tam eşleme gösterilmektedir:

| Kural | Ön uç IP adresi | protokol | port | Hedef | port |
| --- | --- | --- | --- | --- | --- |
| ![Yeşil kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |aynı ön uç (65.52.0.1) |aynı ön uç (80) |
| ![Mor kuralı](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |aynı ön uç (65.52.0.2) |aynı ön uç (80) |

Gelen akış hedef VM'de geri döngü arabirimde ön uç IP adresidir. Her kural, benzersiz bir hedef IP adresine ve hedef bağlantı noktası bileşimi ile akış üretmesi gerekir. Hedef IP adresi akışın değiştirerek bağlantı noktası yeniden aynı VM'de mümkündür. Hizmetinizin yük dengeleyiciye ön uç'ın IP adresi ve bağlantı noktası ilgili geri döngü arabirimine bağlama tarafından kullanıma sunulur.

Bu örnekte, hedef bağlantı noktasını değiştirmez dikkat edin. Bu bir kayan IP senaryo olsa da, Azure Load Balancer arka uç hedef bağlantı noktası yeniden yaz ve ön uç hedef bağlantı noktasından farklı hale getirmek için bir kural tanımlama da destekler.

Kayan IP kural türü birden fazla yük dengeleyici yapılandırması düzenleri oluşturmaktadır. Şu anda kullanılabilir bir örnektir [SQL AlwaysOn ile birden çok dinleyici](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) yapılandırma. Zamanla, size bu senaryoların birkaçı belgeler.

## <a name="limitations"></a>Sınırlamalar

* Birden çok ön uç yapılandırması yalnızca Iaas Vm'leri ile desteklenir.
* Kayan IP kural ile uygulamanızı giden akışlar için birincil IP yapılandırması kullanmanız gerekir. Uygulamanızı yapılandırılmış ön uç IP adresine bağlar, geri döngü üzerinde konuk işletim sistemi, Azure'nın SNAT arabiriminde giden akış yeniden kullanılabilir değil ve akışın başarısız olur.
* Genel IP adresleri, faturalandırma üzerinde bir etkisi yoktur. Daha fazla bilgi için [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Abonelik sınırlar geçerlidir. Daha fazla bilgi için [hizmet sınırları](../azure-subscription-service-limits.md#networking-limits) Ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [giden bağlantılar](load-balancer-outbound-connections.md) giden bağlantı davranışları birden çok ön Uçlarda etkisini anlamak için.
