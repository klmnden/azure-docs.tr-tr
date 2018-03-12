---
title: "Sanal ağ iş sürekliliği | Microsoft Docs"
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
ms.openlocfilehash: d993144006d1fb17d79ffee4f2da538264a309a4
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="virtual-network--business-continuity"></a>Sanal ağ – iş sürekliliği

## <a name="overview"></a>Genel Bakış
Bir sanal ağ (VNet) bulut ağınızdaki mantıksal bir gösterimidir. Ağ alt ağlara ayırabilir ve kendi özel IP adres alanını tanımlamak sağlar. Sanal ağlar, Azure sanal makineler ve bulut Hizmetleri (web/çalışan rolleri) gibi işlem kaynaklarınızı barındırmak için bir güven sınırı görevi görür. VNet içinde barındırılan kaynaklar arasında doğrudan özel IP iletişim sağlar. Bir şirket içi ağınıza bir VPN ağ geçidi veya ExpressRoute aracılığıyla bir sanal ağa bağlayabilirsiniz.

Bir sanal ağa bir bölge kapsamında oluşturulur. Sanal ağlar ile iki farklı bölgelerde (örneğin, ABD Doğu ve Batı ABD) aynı adres alanında oluşturabilirsiniz, ancak bunları birlikte bağlanamıyor. 

## <a name="business-continuity"></a>İş Sürekliliği

Uygulamanızı kesintiye birkaç farklı yolu olabilir. Bir bölge doğal afet ya da birden fazla cihazda veya hizmetleri bir hata nedeniyle kısmi bir olağanüstü durum nedeniyle tamamen kesilebilir. VNet hizmeti üzerindeki etkiyi her bu durumlarda farklıdır.

**S: bir kesinti tüm bölge için oluşursa, ne yapmalıyım? Örneğin bir bölge tamamen doğal afet nedeniyle kesiliyor? Bölgede barındırılan sanal ağlara ne olur?**

Y: sanal ağ ve etkilenen bölge içindeki kaynaklara kalır erişilemez hizmet kesintisi süre boyunca.

![Basit sanal ağ diyagramı](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**S: neler yapabilirim aynı sanal ağda farklı bir bölgede yeniden oluşturmak için?**

Y: sanal ağlar oldukça basit kaynaklardır. Azure API'ları farklı bir bölgede aynı adres alanına sahip bir VNet oluşturmak için çalıştırabilirsiniz. Etkilenen bölgede yoktu aynı ortamını yeniden oluşturmak için bulut Hizmetleri web ve çalışan rolleri ve sahip olduğunuz sanal makineleri dağıtmak için API çağrıları yapma. Şirket içi bağlantınız gibi bir karma dağıtımında, yeni bir VPN ağ geçidi dağıtmak ve şirket içi ağınıza bağlanmak varsa.

Bir sanal ağ oluşturmak için bkz: [bir sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network).

**S: sanal ağ içinde belirli bir bölgedeki çoğaltmasını önceden başka bir bölgede yeniden oluşturulabilir?**

A: Evet vaktinden iki farklı bölgelerde aynı özel IP adres alanı ve kaynakları kullanarak iki sanal ağlar oluşturabilirsiniz. Sanal ağ internet'e yönelik Hizmetleri'nde barındırıyorsanız, trafik Yöneticisi etkin bölge coğrafi yol trafiğini tarafından ayarlayın. Ancak, yönlendirme sorununa neden olur ile aynı adres alanı, şirket içi ağınıza iki Vnet bağlanamıyor. Bir olağanüstü durum süresi ve bir bölgede bulunan bir sanal ağ kaybı, şirket içi ağınıza eşleşen adres alanına sahip bir VNet kullanılabilir bölge içinde bağlanabilir.

Bir sanal ağ oluşturmak için bkz: [bir sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network).

