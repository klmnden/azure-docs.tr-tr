---
title: Internet'e yönelik Yük Dengeleyici genel bakış | Microsoft Docs
description: Internet'e yönelik Yük Dengeleyici ve özellikleri için genel bakış. Nasıl bir yük dengeleyici sanal makineler ve bulut hizmetlerini kullanarak Azure için çalışır.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 33a63538e3b96c5a37ddda6be31a0d972884b9b7
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="internet-facing-load-balancer-overview"></a>Internet kullanıma yönelik Yük Dengeleyici genel bakış


Azure yük dengeleyici gelen trafiği ortak IP adresi ve bağlantı noktası numarasını özel IP adresi ve bağlantı noktası numarası sanal makinenin ve tersi yanıt trafiği sanal makineden eşler. Yük Dengeleme kurallarında belirli birden çok sanal makineler veya hizmetler arasındaki trafik türlerinin dağıtmak olanak sağlar. Örneğin, birden çok web sunucuları veya web rolleri web isteği trafik yükünü yayılabilir.

Web rolleri veya çalışan rolleri örneklerini içeren bir bulut hizmeti için hizmet açıklaması (.csdef) dosyasında genel bir uç nokta tanımlayabilirsiniz.

*Servicedefinition.csdef* dosyası uç nokta yapılandırması içerir ve bir web veya çalışan rolü dağıtımı için birden çok rol örneği varsa, yük dengeleyici ayarları için olacaktır. Bulut dağıtımınız için örnek eklemek için örnek sayısı hizmet yapılandırma dosyası (.csfg) üzerinde değiştirmektedir.

## <a name="example-of-an-internet-facing-load-balancer"></a>Internet'e yönelik Yük Dengeleyici örneği

Yük dengeli bir uç nokta için genel ve özel TCP bağlantı noktası 80 üç sanal makineler arasında paylaşılan web trafiği için aşağıdaki şekilde gösterilmiştir. Bu üç sanal makine bir yük dengeli kümesi yok.

![Ortak yük dengeleyici örneği](./media/load-balancer-internet-overview/IC727496.png)

Şekil 1 - web trafiği için yük dengeli uç nokta

Internet istemcilerinin, TCP bağlantı noktası 80'bulut hizmetinin genel IP adresine web sayfası istekleri gönderdiğinizde, Azure yük dengeleyici istekleri için yük dengeli kümesi içinde üç sanal makine arasında dağıtır. Yük Dengeleyici algoritmalar hakkında daha fazla bilgi için bkz: [temel yük dengeleyici genel bakış sayfasında](load-balancer-overview.md#fundamental-load-balancer-features).

Varsayılan olarak, Azure yük dengeleyici ağ trafiği birden çok sanal makine örnekleri arasında eşit olarak dağıtır. Oturum benzeşimi de yapılandırabilirsiniz daha fazla bilgi için bkz: [yük dengeleyici dağıtım modu](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [iç yük dengeleyici](load-balancer-internal-overview.md) daha iyi bir uyum bulut dağıtımınız için hangi yük dengeleyici olduğundan daha iyi anlamak için.

Ayrıca [Internet'e yönelik Yük Dengeleyici oluşturmaya başlamak](load-balancer-get-started-internet-arm-ps.md) ve ne tür yapılandırma [dağıtım modu](load-balancer-distribution-mode.md) bir özel yük dengeleyici ağ trafiği davranışı için.

Uygulamanızın yük dengeleyici arkasındaki sunucular için bağlantıları canlı tutması gerekiyorsa [yük dengeleyici için boşta kalma TCP zaman aşımı ayarları](load-balancer-tcp-idle-timeout.md) hakkında daha fazla bilgi edinebilirsiniz. Bu, Azure Load Balancer kullanırken boşta kalan bağlantı davranışları hakkında bilgi edinmenizi sağlayacaktır.
