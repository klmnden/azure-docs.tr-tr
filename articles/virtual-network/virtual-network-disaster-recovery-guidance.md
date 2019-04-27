---
title: Sanal ağ iş sürekliliği | Microsoft Docs
description: Azure sanal ağları etkileyen bir Azure hizmet kesintisi olması durumunda yapmanız gerekenler öğrenin.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: f8068524a33c6d29056af456a26b8bd87fc3e3d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60742724"
---
# <a name="virtual-network--business-continuity"></a>Sanal ağ – iş sürekliliği

## <a name="overview"></a>Genel Bakış
Bir sanal ağın (VNet) buluttaki ağınızın mantıksal bir gösterimidir. Kendi özel IP adres alanı tanımlayın ve ağ alt ağlar biçiminde segmentlere olanak tanır. Sanal ağlar, işlem kaynaklarınızı Azure Virtual Machines ve Cloud Services (web/çalışan rolleri) gibi barındırmak için bir güven sınırı görevi görür. Bir sanal ağ içinde bulunan kaynaklar arasında doğrudan özel IP iletişim sağlar. Bir şirket içi ağınıza bir VPN ağ geçidi veya ExpressRoute aracılığıyla bir sanal ağa bağlayabilirsiniz.

Bir sanal ağ bir bölge kapsamında oluşturulur. Sanal ağlar aynı adres alanında iki farklı bölge (örneğin, ABD Doğu ve Batı ABD) ile oluşturabilirsiniz, ancak birlikte bağlanamıyor. 

## <a name="business-continuity"></a>İş Sürekliliği

Uygulamanızı kesintiye birkaç farklı yol olabilir. Bir bölge doğal afet veya birden çok cihaz veya hizmet hatası nedeniyle kısmi bir olağanüstü durum nedeniyle tamamen kesilebilir. VNet hizmeti üzerindeki etkiyi bu durumların her farklıdır.

**S: Tüm bir bölge için bir kesinti oluşursa, ne yapmalıyım? Örneğin bir bölge tamamen doğal afet nedeniyle kesiliyor? Bölgesinde barındırılan sanal ağlara ne olur?**

Y: Sanal ağ kaynakları etkilenen bölgede kalır ve erişilemez hizmet kesintisi sırada.

![Basit bir sanal ağ diyagramı](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**S: Neler ekleyebilirim aynı sanal ağda farklı bir bölgede yeniden oluşturmak için?**

Y: Sanal ağlar oldukça basit kaynaklardır. Farklı bir bölgede aynı adres alanına sahip bir VNet oluşturmak için Azure API'leri çağırabilirsiniz. Etkilenen bölgede mevcut aynı ortama yeniden oluşturmak için Cloud Services web ve çalışan rolleri ve, olan sanal makineleri yeniden dağıtmak için API çağrıları olun. Şirket içi bağlantı varsa, örneğin karma bir dağıtımda, yeni bir VPN ağ geçidi dağıtımı ve şirket içi ağınıza bağlama sahip.

Bir sanal ağ oluşturmak için bkz [sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network).

**S: Sanal ağ içinde belirli bir bölgeye çoğaltmasını önceden başka bir bölgede yeniden oluşturulabilir mi?**

Y: Evet, önceden iki farklı bölgelerde aynı özel IP adres alanı ve kaynakları kullanarak iki sanal ağ oluşturabilirsiniz. İnternet'e yönelik VNet Hizmetleri'nde barındırıyorsanız, Traffic Manager'ı etkin olduğu bölgeye coğrafi olarak yönlendirin trafiği tarafından ayarlanmış. Ancak, yönlendirme sorunlarına neden gibi aynı adres alanıyla şirket içi ağınıza iki sanal ağ bağlanamıyor. Olağanüstü bir durum saati ve bir bölgedeki bir sanal ağ kaybı, şirket içi ağınıza eşleşen bir adres alanı ile kullanılabilir bölgede başka bir sanal ağ bağlanabilirsiniz.

Bir sanal ağ oluşturmak için bkz [sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network).

