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
ms.openlocfilehash: 09c51441d393de5d801e7a4c259b711a527349d8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="high-availability-ports-overview"></a>Yüksek kullanılabilirlik bağlantı noktalarına genel bakış

Standart Azure yük dengeleyici, bir iç yük dengeleyici kullanırken tüm bağlantı noktalarındaki Bakiye TCP ve UDP akışları aynı anda yükleme yardımcı olur. 

Bir HA bağlantı noktası kuralı bir bir Yük Dengeleme kuralı, bir iç standart yük dengeleyici üzerinde yapılandırılmış çeşididir. Bir iç standart yük dengeleyici tüm bağlantı noktalarında gelen tüm TCP ve UDP akışları yükünü dengelemek için tek bir kural sağlayarak, yük dengeleyici kullanımınız basitleştirebilirsiniz. Yük Dengeleme karar akış yapılır. Bu aşağıdaki beş bölütlü bağlantısında dayanır: kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokolü.

HA bağlantı noktalarını özelliği ile yüksek kullanılabilirlik ve ağ sanal Gereçleri (NVA) içinde sanal ağlar için ölçek gibi kritik senaryolarda yardımcı olur. Çok sayıda bağlantı noktaları yük dengeli olması gerektiğinde de yardımcı olabilir. 

Ön uç ve arka uç bağlantı noktaları kümesine HA bağlantı noktalarını özelliği yapılandırılır **0**ve protokol **tüm**. İç yük dengeleyici kaynak bağlantı noktası numarası bağımsız olarak tüm TCP ve UDP akışlar sonra dengeler.

## <a name="why-use-ha-ports"></a>HA bağlantı noktalarını neden kullanılır?

### <a name="nva"></a>Ağ sanal Gereçleri

NVAs Azure İş yükünüzün birden çok tür güvenlik tehditlerine karşı güvenliğini sağlamak için kullanabilirsiniz. Bu senaryolarda NVAs kullanıldığında, güvenilir ve yüksek oranda kullanılabilir olması gerekir ve bunlar için isteğe bağlı ölçeğini gerekir.

Yalnızca NVA örnekleri Azure iç yük dengeleyici arka uç havuzuna ekleme ve bir HA bağlantı noktalarını yük dengeleyici kuralı yapılandırarak bu hedefleri elde edebilirsiniz.

HA bağlantı noktaları gibi çeşitli avantajları NVA HA senaryoları için sağlar:
- Örnek sistem durumu araştırmalarının ile sağlıklı örneklerine hızlı yük devretme
- Genişleme için daha yüksek performansla *n*-etkin örnekleri
- *N*-etkin ve Etkin-pasif senaryoları
- Uygulamaları izlemek için Apache ZooKeeper düğümleri gibi karmaşık çözümleri gereksinimini

Aşağıdaki diyagramda bir hub ve bağlı sanal ağ dağıtımı gösterir. Bağlı bileşen zorlamalı tünel hub sanal ağ ve güvenilir alanı çıkmadan önce NVA aracılığıyla kendi trafiği. Bir HA bağlantı noktası yapılandırması ile iç standart yük dengeleyici arkasında NVAs var. Tüm trafiği işlenen ve buna uygun olarak iletilir.

![NVAs HA modunda dağıtılmış olan hub ve bağlı sanal ağ diyagramı](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> NVAs kullanıyorsanız, ilgili sağlayıcı ile en iyi HA bağlantı noktalarını kullanacak biçimde nasıl ve hangi senaryolar desteklenir onaylayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Yük Dengeleme çok sayıda bağlantı noktaları

HA bağlantı noktalarını bağlantı noktalarının çok sayıda Yük Dengeleme gerektiren uygulamalar için de kullanabilirsiniz. Bir iç kullanarak bu senaryoları basitleştirebilirsiniz [standart yük dengeleyici](load-balancer-standard-overview.md) HA bağlantı noktasına sahip. Tek bir Yük Dengeleme kuralı birden çok ayrı Yük Dengeleme kuralları, her bağlantı noktası için bir tane değiştirir.

## <a name="region-availability"></a>Bölge kullanılabilirliği

HA bağlantı noktalarını özelliği tüm genel Azure bölgelerde kullanılabilir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

### <a name="one-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Tek tek olmayan kayan IP (olmayan - doğrudan sunucu dönüşü) HA bağlantı noktalarını yapılandırmasına iç standart yük dengeleyici

Temel bir HA bağlantı noktalarını yapılandırma budur. Aşağıdaki yapılandırma, tek bir ön uç IP adresi HA bağlantı noktalarını dengelemesini yapılandırmanıza olanak sağlar-
- Standart yük dengeleyici yapılandırılırken seçin **HA bağlantı noktaları** yük dengeleyici kuralı yapılandırmasında, onay kutusu 
- Unutmadan **kayan IP** ayarlanır **devre dışı**.

Bu yapılandırma, tüm diğer Yük Dengeleme kuralı yapılandırmasına geçerli yük dengeleyici kaynak yanı sıra, iç yük dengeleyici kaynak için başka bir yapılandırma arka uç örnekleri verilen kümesine izin vermiyor.

Ancak, bu HA bağlantı noktası kuralı yanı sıra arka uç örnekleri için ortak bir standart yük dengeleyici yapılandırabilirsiniz.

## <a name="one-single-floating-ip-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Tek tek kayan IP (doğrudan sunucu dönüşü) HA bağlantı yapılandırmasına iç standart yük dengeleyici

Benzer şekilde bir Yük Dengeleme kuralı ile kullanmak için yük dengeleyici yapılandırabilirsiniz **HA bağlantı noktası** tek bir ön uç ile ve **kayan IP** kümesine **etkin**. 

Bu yapılandırma, kurallar ve / veya ortak bir yük dengeleyici daha kayan IP dengelemesini eklemenizi sağlar. Ancak, bu yapılandırma üzerinde olmayan - kayan IP HA bağlantı noktası boad karşı yapılandırma kullanamazsınız.

## <a name="multiple-ha-ports-configurations-on-the-internal-standard-load-balancer"></a>İç standart yük dengeleyici üzerinde birden çok HA bağlantı noktası yapılandırmaları

Birden fazla HA bağlantı noktası ön uçlar için aynı arka uç havuzunu yapılandırma senaryonuz gerektiriyorsa, bu tarafından elde edebilirsiniz: 
- tek bir iç standart yük dengeleyici kaynak için özel IP adreslerini birden çok ön uç yapılandırma.
- birden çok Yük Dengeleme kuralları, her kural, tek bir bulunduğu yapılandırma benzersiz ön uç IP adresi seçilidir.
- Seçin **HA bağlantı noktaları** seçeneği ve ayarlayın **kayan IP** için **etkin** tüm Yük Dengeleme kuralları.

## <a name="internal-load-balancer-with-ha-ports--public-load-balancer-on-the-same-backend-instances"></a>İç yük dengeleyici HA çıkışı ve aynı arka uç örneklerinde ortak yük dengeleyici ile

Yapılandırabileceğiniz **bir** bir tek iç standart yük dengeleyici HA bağlantı noktaları ile birlikte arka uç kaynaklarına genel standart yük dengeleyici kaynağıdır.

>[!NOTE]
>Bu özellik kullanılabilir bugün Azure Resource Manager şablonları aracılığıyla ancak Azure Portalı aracılığıyla değil.

## <a name="limitations"></a>Sınırlamalar

- HA bağlantı noktalarını yapılandırma yalnızca iç yük dengeleyici için kullanılabilir, ortak bir yük dengeleyici için kullanılabilir değildir.

- HA bağlantı noktalarını Yük Dengeleme kuralı ve olmayan HA bağlantı noktalarını Yük Dengeleme kuralı birleşimi desteklenmez.

- HA bağlantı noktalarını özellik IPv6 için kullanılamıyor.

- Akış simetrisi NVA senaryoları için yalnızca tek bir NIC ile desteklenir. Açıklamaya bakın ve için diyagram [sanal gereçler ağ](#nva). Ancak, bir hedef NAT senaryonuz için çalışıyorsanız, iç yük dengeleyici dönüş trafiği aynı nva'nın gönderir emin olmak için kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- [Bir iç standart yük Dengeleyicide HA bağlantı noktalarını yapılandırma](load-balancer-configure-ha-ports.md)
- [Standart yük dengeleyici hakkında bilgi edinin](load-balancer-standard-overview.md)
