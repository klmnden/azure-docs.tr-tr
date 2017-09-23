---
title: "İnternet’e yönelik yük dengeleyicisi oluşturma - Klasik Azure portalı | Microsoft Docs"
description: "Klasik Azure portalını kullanarak klasik dağıtımda İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: a022154f5eca6de2d2dbfc1b9aa30d2ea0a7d650
ms.contentlocale: tr-tr
ms.lasthandoff: 01/24/2017

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Klasik Azure portalında İnternet’e yönelik yük dengeleyici (klasik) oluşturmaya başlama

> [!div class="op_single_selector"]
> * [Klasik Azure portalı](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure’da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır. [Azure Resource Manager kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) sayfasını da inceleyebilirsiniz.

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Sanal makineler için İnternet’e yönelik yük dengeleyici kurma

İnternet’ten gelen ağ trafiğine bir bulut hizmetindeki sanal makinelerde yük dengeleme gerçekleştirmek için yük dengeli küme oluşturmanız gerekir. Bu yordam, sanal makineleri önceden oluşturduğunuzu ve tümünün aynı bulut hizmetinde olduğunu varsaymaktadır.

**Sanal makineler için yük dengeli küme yapılandırma**

1. Klasik Azure portalında **Sanal Makineler**’e ve ardından yük dengeli kümedeki bir sanal makinenin adına tıklayın.
2. **Uç noktaları**’na ve ardından **Ekle**’ye tıklayın.
3. **Sanal makineye bir uç nokta ekleyin** sayfasında sağ oka tıklayın.
4. **Uç nokta ayrıntılarını belirtin** sayfasında:

   * **Ad** alanına uç nokta için bir ad yazın veya sık kullanılan protokoller için önceden tanımlı uç nokta listesinden bir ad seçin.
   * **Protokol** alanında uç nokta türüne göre gerekli olan TCP veya UDP protokolünü seçin.
   * **Genel Bağlantı Noktası ve Özel Bağlantı Noktası** alanlarına sanal makinenin kullanmasını istediğiniz bağlantı noktası numaralarını yazın. Trafiği uygulamanız için gereken şekilde yönlendirmek üzere sanal makinede özel bağlantı noktası ve güvenlik duvarı kuralları kullanabilirsiniz. Özel bağlantı noktası ile genel bağlantı noktası aynı olabilir. Örneğin, web (HTTP) trafiği uç noktası için 80 numaralı bağlantı noktasını hem genel hem de özel bağlantı noktası olarak atayabilirsiniz.

5. **Yük dengeli küme oluştur**’u seçip sağ oka tıklayın.
6. **Yük dengeli kümeyi yapılandır** sayfasında yük dengeli küme için bir ad girin ve Azure Load Balancer’ın araştırma davranışı değerini belirleyin. Azure Load Balancer, yük dengeli kümedeki sanal makinelerin gelen trafiği almaya uygun olup olmadığını belirlemek için araştırmaları kullanır.
7. Yük dengeli uç nokta oluşturmak için onay işaretine tıklayın. Sanal makineye ait **Uç noktaları** sayfasının **Yük dengeli küme adı** sütununda **Evet** görüntülenir.
8. Portalda **Sanal Makineler**’e tıklayın, yük dengeli kümedeki diğer sanal makinelerden birinin adına tıklayın, **Uç noktalar**’a tıklayın ve ardından **Ekle**’ye tıklayın.
9. **Sanal makineye bir uç nokta ekleyin** sayfasında **Var olan yük dengeli kümeye uç nokta ekleyin**’e tıklayın, yük dengeli kümenin adını seçin ve sağ oka tıklayın.
10. **Uç nokta ayrıntılarını belirtin** sayfasında uç noktası için bir ad girin ve onay işaretine tıklayın.

Yük dengeli kümedeki diğer sanal makineler için 8-10 arası adımları tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)

