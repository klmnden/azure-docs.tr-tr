---
title: Azure yüksek kullanılabilirlik bağlantı noktaları genel bakış
titlesuffix: Azure Load Balancer
description: İç yük dengeleyici üzerinde yüksek kullanılabilirlik bağlantı noktaları yük dengelemeyi hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2018
ms.author: kumud
ms.openlocfilehash: 328471292ea6cbe07e96cc18af7f9c524407de3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60930293"
---
# <a name="high-availability-ports-overview"></a>Yüksek kullanılabilirlik bağlantı noktaları genel bakış

İç yük dengeleyici kullanıldığında azure standart Load Balancer, tüm bağlantı noktalarındaki Yük Dengeleme TCP ve UDP akışlar aynı anda yardımcı olur. 

Yüksek kullanılabilirlik (HA) bağlantı noktalarını Yük Dengeleme kuralı bir Yük Dengeleme kuralı, bir iç standart Load Balancer üzerinde yapılandırılmış çeşididir. Tek bir kural yük dengelemek için bir iç standart Load Balancer'ın tüm bağlantı noktalarında gelen tüm TCP ve UDP akışlar sağlayarak bir yük dengeleyicinin kullanılmasını kolaylaştırabilirsiniz. Yük Dengeleme karar akış yapılır. Bu eylem aşağıdaki beş bölütlü bağlantısında bağlıdır: IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokol kaynağı

HA bağlantı noktaları, Yük Dengeleme kuralları, yüksek kullanılabilirlik ve ölçek için sanal ağ içindeki ağ sanal Gereçleri (Nva) gibi kritik senaryolarda yardımcı. Özellik bağlantı noktaları, çok sayıda yük dengeli olması gerektiğinde de yardımcı olabilir. 

Ön uç ve arka uç bağlantı noktalarını ayarlayın, HA bağlantı noktaları Yük Dengeleme kuralları yapılandırılır **0** ve protokole **tüm**. İç yük dengeleyici kaynak bağlantı noktası numarası bağımsız olarak tüm TCP ve UDP akışlar ardından dengeler.

## <a name="why-use-ha-ports"></a>HA bağlantı noktaları neden kullanmalısınız?

### <a name="nva"></a>Ağ sanal Gereçleri

Azure İş yükünüzün güvenlik tehditlerini birden çok tür güvenliğini sağlamaya yardımcı olmak için nva'ları kullanabilirsiniz. Bu senaryolarda nva'ları kullandığınızda, güvenilir ve yüksek oranda kullanılabilir olması gerekir ve bunlar için isteğe bağlı ölçeği gerekir.

İç load balancer arka uç havuzuna yalnızca NVA örneklerinin ekleyerek bu hedeflere elde edebileceğiniz ve yük dengeleyici kuralı yapılandırma bir HA bağlantı noktaları.

NVA HA senaryoları için HA bağlantı noktaları aşağıdaki avantajları sağlar:
- Hızlı yük devretmeyi sağlıklı örnekleri için örnek başına sistem durumu araştırmaları ile sağlayın
- Ölçek genişletme ile daha yüksek performans sağlamak *n*-etkin örnekler
- Sağlamak *n*-etkin ve Aktif-Pasif senaryoları
- Cihazları izleme için Apache ZooKeeper düğümleri gibi karmaşık çözümleri gerekmemesi

Aşağıdaki diyagramda bir merkez ve uç sanal ağ dağıtımı sunar. Uçlar zorlamalı tünel hub sanal ağa ve güvenilen alanı çıkmadan önce NVA aracılığıyla trafiği. HA bağlantı noktaları yapılandırmaya sahip bir iç standart yük dengeleyici arkasında nva'ları var. Tüm trafiği, işlenen ve buna göre iletilir. Aşağıdaki diyagramda Göster olarak yapılandırıldığında, bir yüksek kullanılabilirlik bağlantı noktaları Yük Dengeleme kuralı ayrıca giriş ve çıkış trafiği için akış Simetri sağlar.

<a node="diagram"></a>
![Yüksek kullanılabilirlik modunda dağıtılabilir nva'ları ile merkez-uç sanal ağ diyagramı](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Nva'ları kullanıyorsanız sağlayıcıları ile HA bağlantı noktaları en iyi şekilde kullanmak ve hangi senaryolar desteklenir öğrenmek için onaylayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Bağlantı noktalarını çok sayıda Yük Dengeleme

HA bağlantı noktalarını bağlantı noktalarının çok sayıda Yük Dengeleme gerektiren uygulamalar için de kullanabilirsiniz. Bir iç kullanarak bu senaryolar basit hale getirebileceğinizi [Standard Load Balancer](load-balancer-standard-overview.md) HA bağlantı noktaları. Tek bir yük dengeleyici kuralı, birden fazla bireysel Yük Dengeleme kuralları, her bağlantı noktası için bir tane yerine geçer.

## <a name="region-availability"></a>Bölge kullanılabilirliği

HA bağlantı noktaları özelliğini tüm genel Azure bölgelerinde kullanılabilir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerindeki tek, değişken olmayan bir IP (olmayan - doğrudan sunucu dönüşü) HA bağlantı noktaları yapılandırma

Bu yapılandırma bir temel HA bağlantı noktaları yapılandırmadır. Yapılandırabileceğiniz bir HA bağlantı noktaları Yük Dengeleme kuralı üzerinde tek bir ön uç IP adresi aşağıdakileri yaparak:
1. Standard Load Balancer'ı yapılandırırken, seçin **HA bağlantı noktaları** onay kutusuna yük dengeleyici kuralı yapılandırması.
2. İçin **kayan IP**seçin **devre dışı bırakılmış**.

Bu yapılandırma diğer Yük Dengeleme kuralı yapılandırma geçerli yük dengeleyici kaynağına izin vermez. İç yük dengeleyici kaynağı için başka bir yapılandırma arka uç örneklerinin verilen kümesine izin verir.

Ancak, genel bir Standard Load Balancer arka uç örneklerinin HA bağlantı noktaları kuralın ek olarak yapılandırabilirsiniz.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerindeki bir tek, kayan IP (doğrudan sunucu dönüşü) HA bağlantı noktaları yapılandırma

Benzer şekilde, bir Yük Dengeleme kuralı ile kullanmak için yük dengeleyici yapılandırabilirsiniz **HA bağlantı noktası** ayarlayarak, tek bir ön uç ile **kayan IP** için **etkin**. 

Bu yapılandırmayı kullanarak kayan IP yük dengeleyici kuralları ve/veya herkese açık yük dengeleyici ekleyebilirsiniz. Ancak, HA bağlantı noktaları bir değişken olmayan IP kullanamazsınız. Bu yapılandırma üzerinde Yük Dengeleme yapılandırma.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici birden çok yapılandırmada HA bağlantı noktaları

Birden fazla HA bağlantı noktası ön uç için aynı arka uç havuzunu yapılandırma senaryonuz gerektiriyorsa, bunu yapabilirsiniz: 
- Birden fazla ön uç özel IP adresi tek bir iç standart yük dengeleyici kaynağı için yapılandırın.
- Her kural seçilen tek benzersiz bir ön uç IP adresine sahip olduğu birden çok Yük Dengeleme kuralları yapılandırın.
- Seçin **HA bağlantı noktaları** seçeneğini ve ardından **kayan IP** için **etkin** tüm Yük Dengeleme kuralları.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>HA bağlantı noktaları ve aynı arka uç örneğinde bir genel yük dengeleyici ile iç yük dengeleyici

Yapılandırabileceğiniz *bir* tek iç standart yük dengeleyici HA bağlantı noktaları ile birlikte arka uç kaynakları için genel bir Standard Load Balancer kaynağı.

>[!NOTE]
>Bu özellik şu anda Azure Resource Manager şablonları kullanılabilir, ancak Azure portalı üzerinden kullanılabilir değil.

## <a name="limitations"></a>Sınırlamalar

- HA bağlantı noktaları yapılandırma yalnızca dahili yük Dengeleyiciler için kullanılabilir. Genel load balancer'ları için kullanılamıyor.

- Birleştirme, bir HA bağlantı noktaları Yük Dengeleme kuralı ve bir HA olmayan bağlantı noktaları Yük Dengeleme kuralı desteklenmiyor.

- HA bağlantı noktaları özelliği IPv6 için kullanılamıyor.

- Yukarıdaki ve kullanarak diyagramda gösterildiği gibi kullanıldığında yüksek kullanılabilirlik bağlantı noktaları Yük Dengeleme kuralları yalnızca akış Simetri (öncelikle için NVA senaryoları) arka uç örneği ve bir tek NIC (ve tek bir IP yapılandırması ile) desteklenir. Diğer bir senaryoda sağlanmadı. Bu, iki veya daha fazla yük dengeleyici kaynakları ve bunların ilgili kuralları bağımsız kararlar ve hiçbir zaman Eşgüdümlü anlamına gelir. Açıklamasına bakın ve için diyagram [ağ sanal Gereçleri](#nva). Birden çok NIC kullanma veya bir genel ve iç yük dengeleyici arasındaki NVA sandwiching akış Simetri kullanılamıyor.  Giriş akışı aynı NVA gelmesi yanıtları izin vermek için gerecin IP NAT'ing kaynak tarafından bu sorunu çözmek mümkün olabilir.  Ancak, tek bir NIC'ye ve yukarıdaki Diyagramda gösterilen başvuru mimarisini kullanarak öneririz.


## <a name="next-steps"></a>Sonraki adımlar

- [HA bağlantı noktaları üzerinde iç standart Load Balancer yapılandırma](load-balancer-configure-ha-ports.md)
- [Standart Load Balancer hakkında bilgi edinin](load-balancer-standard-overview.md)
