---
title: "Azure sanal ağlar etkileyen bir Azure hizmet kesintisi durumunda yapmanız gerekenler | Microsoft Docs"
description: "Azure sanal ağlar etkileyen bir Azure hizmet kesintisi durumunda yapmanız gerekenler hakkında bilgi edinin."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 4e125406d2e798138c45e3fbbf61a610afab69fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="virtual-network--business-continuity"></a>Sanal ağ – iş sürekliliği
## <a name="overview"></a>Genel Bakış
Bir sanal ağ (VNet) bulut ağınızdaki mantıksal bir gösterimidir. Ağ alt ağlara ayırabilir ve kendi özel IP adres alanını tanımlamak sağlar. Sanal ağlar, Azure sanal makineler ve bulut Hizmetleri (web/çalışan rolleri) gibi işlem kaynaklarınızı barındırmak için bir güven sınırı görevi görür. VNet içinde barındırılan kaynaklar arasında doğrudan özel IP iletişim sağlar. Bir sanal ağ, bir şirket içi ağınıza bir VPN ağ geçidi veya ExpressRoute gibi karma seçeneklerden birini aracılığıyla da bağlanabilir.

Bir sanal ağa bir bölge kapsamında oluşturulur. İki farklı bölgelerdeki aynı adres alanı ile sanal ağlar oluşturabilirsiniz (yani BİZE Doğu ve Batı ABD bunları başka bir girişe doğrudan bağlantı kurulamıyor ancak). 

## <a name="business-continuity"></a>İş Sürekliliği
Uygulamanızı kesintiye birkaç farklı yolu olabilir. Belirli bir bölgedeki doğal afet veya birden çok aygıt/hizmetlerini bir hata nedeniyle kısmi bir olağanüstü durum nedeniyle tamamen kesilebilir. VNet hizmeti üzerindeki etkiyi her bu durumlarda farklıdır.

**S: bir tüm bölgesine bir kesinti durumunda ne yapacaksınız? yani bir bölge doğal afet nedeniyle tamamen kesme ise? Bölgede barındırılan sanal ağlara ne olur?**

Y: sanal ağ ve etkilenen bölge içindeki kaynaklara kalır erişilemez hizmet kesintisi süre boyunca.

![Basit sanal ağ diyagramı](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**S: neler yapabilirim aynı sanal ağda farklı bir bölgede yeniden oluşturmak için?**

Y: sanal ağ (VNet) oldukça basit kaynaktır. Azure API'ları farklı bir bölgede aynı adres alanına sahip bir VNet oluşturmak için çalıştırabilirsiniz. Etkilenen bölgede yoktu aynı ortamı yeniden oluşturmak için sahip olduğunuz sanal makineler ve bulut Hizmetleri (web/çalışan rolleri) yeniden dağıtmak için API çağrıları yapmak zorunda. Bir VPN ağ geçidi Döndür ve şirket içi bağlantı (bir karma dağıtımı olduğu gibi) varsa, şirket içi ağınıza bağlanmak gerekir.

Bir sanal ağ oluşturma için yönergeler bulunur [burada](virtual-networks-create-vnet-arm-pportal.md). 

**S: sanal ağ içinde belirli bir bölgedeki çoğaltmasını önceden başka bir bölgede yeniden oluşturulabilir?**

A: Evet vaktinden iki farklı bölgelerde aynı özel IP adres alanı ve kaynakları kullanarak iki sanal ağlar oluşturabilirsiniz. Bir müşteri hizmetleri VNet içinde Internet'e barındırma, bunlar trafik Yöneticisi etkin bölge coğrafi yol trafiğini ayarladınız. Yönlendirme sorununa neden olur ancak, bir müşteri iki Vnet kendi şirket içi ağ için aynı adres alanı ile bağlantı kuramıyor. Bir olağanüstü durum süresi ve bir bölgede bulunan bir sanal ağ kaybı, bir müşteri kendi şirket içi ağ adresi alanını eşleşen ile kullanılabilir bölgede diğer Vnet'in bağlanabilir.

Bir sanal ağ oluşturma için yönergeler bulunur [burada](virtual-networks-create-vnet-arm-pportal.md).

