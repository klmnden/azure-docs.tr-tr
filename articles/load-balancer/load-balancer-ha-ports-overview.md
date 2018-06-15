---
title: Azure'da yüksek kullanılabilirlik bağlantı noktalarına genel bakış | Microsoft Docs
description: Yüksek oranda kullanılabilir bağlantı noktaları yük bir iç yük dengeleyici Dengeleme hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: kumud
ms.openlocfilehash: 53e09498324f802ce1839d262999657d37ee973b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32169127"
---
# <a name="high-availability-ports-overview"></a>Yüksek kullanılabilirlik bağlantı noktalarına genel bakış

Bir iç yük dengeleyici kullanırken azure standart yük dengeleyicisi, tüm bağlantı noktaları ağdaki Yük Dengeleme TCP ve UDP akışlar aynı anda yardımcı olur. 

Bir yüksek kullanılabilirlik (HA) bağlantı noktası kuralı bir bir iç standart yük dengeleyici üzerinde yapılandırılmış bir Yük Dengeleme kuralı çeşididir. Bir iç standart yük dengeleyici tüm bağlantı noktalarında gelen tüm TCP ve UDP akışlar yük dengelemek için tek bir kural sağlayarak bir yük dengeleyici kullanımını kolaylaştırabilir. Yük Dengeleme karar akış yapılır. Bu eylem aşağıdaki beş bölütlü bağlantısında dayanır: kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokolü.

HA bağlantı noktalarını özelliği ile yüksek kullanılabilirlik ve ağ sanal Gereçleri (NVAs) içinde sanal ağlar için ölçek gibi kritik senaryolarda yardımcı olur. Çok sayıda bağlantı noktaları yük dengeli olması gerektiğinde özelliği de yardımcı olabilir. 

Ön uç ve arka uç bağlantı noktaları kümesine HA bağlantı noktalarını özelliği yapılandırılır **0** ve protokol **tüm**. İç yük dengeleyici kaynak bağlantı noktası numarası bağımsız olarak tüm TCP ve UDP akışlar sonra dengeler.

## <a name="why-use-ha-ports"></a>HA bağlantı noktalarını neden kullanılır?

### <a name="nva"></a>Ağ sanal Gereçleri

Güvenlik tehditlerine birden çok tür Azure İş yükünüzün güvenliğini sağlamaya yardımcı olmak için NVAs kullanabilirsiniz. Bu senaryolarda NVAs kullandığınızda, güvenilir ve yüksek oranda kullanılabilir olması gerekir ve bunlar için isteğe bağlı ölçeğini gerekir.

İç yük dengeleyici arka uç havuzuna yalnızca NVA örnekleri ekleyerek bu hedefleri elde edebilirsiniz ve yük dengeleyici kuralı bir HA yapılandırma bağlantı noktaları.

HA bağlantı noktaları NVA HA senaryoları için aşağıdaki avantajları sağlar:
- Örnek sistem durumu araştırmalarının ile hızlı yük devretme sağlıklı örneklerine sağlayın
- Ölçek dışı ile daha yüksek bir performans sağlamak *n*-etkin örnekleri
- Sağlamak *n*-etkin ve Etkin-pasif senaryoları
- Uygulamaları izlemek için Apache ZooKeeper düğümleri gibi karmaşık çözümleri gereksinimini ortadan kaldırmak

Aşağıdaki diyagramda bir hub ve bağlı sanal ağ dağıtımı gösterir. Bağlı bileşen zorlamalı tünel hub sanal ağ ve güvenilir alanı çıkmadan önce NVA aracılığıyla kendi trafiği. Bir HA bağlantı noktası yapılandırması ile iç standart yük dengeleyici arkasında NVAs var. Tüm trafiği işlenen ve buna uygun olarak iletilir.

![NVAs HA modunda dağıtılmış olan hub ve bağlı sanal ağ diyagramı](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> NVAs kullanıyorsanız, bunların sağlayıcıları ile nasıl en iyi HA bağlantı noktalarını kullanacak biçimde ve hangi senaryoları desteklenir öğrenmek için onaylayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Bağlantı noktaları çok sayıda Yük Dengeleme

HA bağlantı noktalarını bağlantı noktalarının çok sayıda Yük Dengeleme gerektiren uygulamalar için de kullanabilirsiniz. Bir iç kullanarak bu senaryoları basitleştirebilirsiniz [standart yük dengeleyici](load-balancer-standard-overview.md) HA bağlantı noktasına sahip. Tek bir Yük Dengeleme kuralı birden çok ayrı ayrı Yük Dengeleme kuralları, her bağlantı noktası için bir tane değiştirir.

## <a name="region-availability"></a>Bölge kullanılabilirliği

HA bağlantı noktalarını özelliği tüm genel Azure bölgelerde kullanılabilir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerinde tek, değişken olmayan IP (olmayan - doğrudan sunucu dönüşü) HA-bağlantı noktalarını yapılandırma

Bu yapılandırma bir temel HA bağlantı noktalarını yapılandırmadır. Bir HA yapılandırabilirsiniz Yük Dengeleme bağlantı noktaları, aşağıdakileri yaparak tek bir ön uç IP adresinde kural:
1. Standart yük dengeleyici yapılandırılırken seçin **HA bağlantı noktaları** onay kutusuna yük dengeleyici kuralı yapılandırması.
2. İçin **kayan IP**seçin **devre dışı**.

Bu yapılandırma tüm diğer Yük Dengeleme kuralı yapılandırması geçerli yük dengeleyici kaynak izin vermiyor. İç yük dengeleyici kaynak için başka bir yapılandırma arka uç örnekleri verilen kümesine izin verir.

Ancak, bu HA bağlantı noktalarını kural yanı sıra arka uç örnekleri için ortak bir standart yük dengeleyici yapılandırabilirsiniz.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerinde tek, kayan IP (doğrudan sunucu dönüşü) HA-bağlantı noktalarını yapılandırma

Benzer şekilde, yük dengeleyici Yük Dengeleme kuralı ile kullanmak için yapılandırabilirsiniz **HA bağlantı noktası** ayarlayarak tek bir ön uç ile **kayan IP** için **etkin**. 

Bu yapılandırmayı kullanarak kayan IP Yük Dengeleme kuralları ve/veya bir genel yük dengeleyiciye ekleyebilirsiniz. Ancak, bağlantı noktalarını HA bir değişken olmayan IP kullanamazsınız bu yapılandırma üzerinde yük dengeleyici yapılandırması.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerinde birden çok HA-bağlantı noktası yapılandırmaları

Aynı arka uç havuzu için birden fazla HA bağlantı noktası ön uç yapılandırma senaryonuz gerektiriyorsa, aşağıdakileri yapabilirsiniz: 
- Birden çok ön uç özel IP adresi tek iç standart yük dengeleyici kaynak için yapılandırın.
- Her kural seçilen tek benzersiz bir ön uç IP adresine sahip olduğu birden çok Yük Dengeleme kuralları yapılandırın.
- Seçin **HA bağlantı noktaları** seçeneğini ve ardından **kayan IP** için **etkin** tüm Yük Dengeleme kuralları.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>HA bağlantı noktalarını ve bir genel yük dengeleyiciye aynı arka uç örneğinde olan bir iç yük dengeleyici

Yapılandırabileceğiniz *bir* bir tek iç standart yük dengeleyici HA bağlantı noktaları ile birlikte arka uç kaynaklar için ortak standart yük dengeleyici kaynak.

>[!NOTE]
>Bu özellik şu anda Azure Resource Manager şablonları kullanılabilir, ancak Azure portalı üzerinden kullanılabilir değil.

## <a name="limitations"></a>Sınırlamalar

- HA bağlantı noktalarını yapılandırma yalnızca iç yük dengeleyicileri için kullanılabilir. Ortak yük Dengeleyiciler için kullanılabilir değil.

- Bir HA birleştirme bağlantı noktalarını Yük Dengeleme kuralı ve bir HA olmayan bağlantı noktaları Yük Dengeleme kuralı desteklenmiyor.

- HA bağlantı noktalarını özellik IPv6 için kullanılamıyor.

- Akış simetrisi NVA senaryoları için yalnızca tek bir NIC ile desteklenir. Açıklamaya bakın ve için diyagram [sanal gereçler ağ](#nva). Ancak, bir hedef senaryonuz için NAT işe iç yük dengeleyicisi dönüş trafiği aynı nva'nın gönderir emin olmak için kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- [Bir iç standart yük Dengeleyicide HA bağlantı noktalarını yapılandırma](load-balancer-configure-ha-ports.md)
- [Standart yük dengeleyici hakkında bilgi edinin](load-balancer-standard-overview.md)
