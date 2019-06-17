---
title: IP adresi olağanüstü durum kurtarma ve Azure Site Recovery ile azure'a yük devredildikten sonra bağlanmak için ayarlama | Microsoft Docs
description: IP adresi şirket içi Azure Site Recovery ile olağanüstü durum kurtarma ve yük devretme sonrasında Azure Vm'lerine bağlanmak için ayarlama açıklanmaktadır
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/15/2019
ms.author: mayg
ms.openlocfilehash: 2e1cbb2446501d0afda29eba179e388b5a22e6a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60772272"
---
# <a name="set-up-ip-addressing-to-connect-to-azure-vms-after-failover"></a>IP adresi yük devretmeden sonra Azure Vm'lerine bağlanmak için ayarlama

Bu makalede kullandıktan sonra Azure Vm'lerine bağlanmak için ağ gereksinimleri açıklanır [Azure Site Recovery](site-recovery-overview.md) çoğaltma ve azure'a yük devretme için hizmet.

Bu makalede hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Kullanabileceğiniz bağlantı yöntemleri
> * Çoğaltılan Azure Vm'leri için farklı bir IP adresi kullanma
> * IP adresleri, yük devretme sonrasında Azure Vm'leri için koruma

## <a name="connecting-to-replica-vms"></a>Çoğalma VM'ler için bağlanma

Çoğaltma ve yük devretme stratejisi planlama yaparken, önemli sorular yük devretme sonrasında Azure VM'ye bağlanma biridir. Azure Vm'lerini çoğaltma için ağ stratejinizi tasarlarken birkaç seçeneğiniz vardır:

- **Farklı bir IP adresi kullanmak**: Çoğaltılan Azure VM ağı için farklı bir IP adresi aralığı kullanmayı seçebilirsiniz. Bu senaryoda, sanal makine yük devretmeden sonra yeni bir IP adresi alır ve DNS güncelleştirme gerekli değildir.
- **Aynı IP adresini koruma**: Aynı IP adresi aralığı, birincil şirket içi sitenizdeki yük devretme sonrasında Azure ağı için kullanmak istediğiniz. Tutma aynı IP adreslerini basitleştirir kurtarma azaltarak yük devretme sonrasında ağa ilişkin sorunları. Ancak, Azure'a çoğaltırken yollar yük devretmeden sonra IP adreslerini yeni konumu ile güncelleştirmeniz gerekir.

## <a name="retaining-ip-addresses"></a>IP adresleri korunuyor

Site Recovery, sabit IP korumak için özelliği üzerinden Azure'a bir alt ağ yük devretme başarısız olduğunda adresleri sağlar.

- Alt ağ ile yük devretme, belirli bir alt ağ aynı anda Site 1 veya ancak hiçbir zaman hem sitelerdeki Site 2 sırasında mevcuttur.
- Yük devretme durumunda IP adres alanı sürdürmek için program aracılığıyla alt ağlar bir konumdan diğerine taşımak yönlendirici altyapı düzenleyin.
- Yük devretme sırasında alt ağlar ile ilişkili korumalı Vm'leri taşıyın. Bir hata olması durumunda olan asıl sakıncası, tüm alt taşıma gerekir.


### <a name="failover-example"></a>Yük devretme örneği

Kurgusal bir şirkette, Woodgrove Bank'ı kullanarak azure'a yük devretme için bir örneğe bakalım.

- Woodgrove Bankası'nda iş uygulamalarını şirket içi sitede barındırır. Bunlar mobil uygulamalarını azure'da barındırın.
- Kendi şirket içi uç ağ ve Azure sanal ağı arasında VPN siteden siteye bağlantı yok. VPN bağlantısı nedeniyle Azure sanal ağı şirket içi ağınıza uzantısı görünür.
- Azure Site Recovery ile şirket içi iş yüklerini çoğaltmak Woodgrove istiyor.
  - Woodgrove azure'a yük devredildikten sonra uygulamalar için IP adresleri saklamak ihtiyaç duydukları bırakmayarak sabit kodlanmış IP adreslerinde bağımlı uygulamaları vardır.
  - Azure'da çalışan kaynakları kullandığınız IP adresi aralığı 172.16.1.0/24 172.16.2.0/24.

![Önce alt ağ yük devretme](./media/site-recovery-network-design/network-design7.png)

**Yük devretmeden önce altyapı**


Kullanabilmek Woodgrove için hangi IP adresleri, burada 's korurken Vm'lerini Azure'a çoğaltmak için şirket yapması gerekir:


1. Yük devretme şirket içi makineleri sonra Azure Vm'leri oluşturulur Azure sanal ağı oluşturun. Böylece uygulamaları sorunsuz bir şekilde yük devredebildiğini şirket içi ağınıza uzantısı olması gerekir.
2. Site recovery'de yük devretmeden önce bunlar makine özellikleri aynı IP adresi atayın. Yük devretme işleminden sonra Site Recovery Azure VM'sine bu adresi atar.
3. Yük devretme çalıştırır ve aynı IP adresi ile oluşturulan Azure Vm'lerinin sonra bunlar kullanarak ağ bağlantı bir [Vnet'ten Vnet'e bağlantı](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Bu eylem yazılabilir.
4. Bunlar, yollar, o 192.168.1.0/24 artık Azure'a taşıdı yansıtacak şekilde değiştirmeniz gerekir.


**Yük devretmeden sonra altyapı**

![Alt ağ yük devretmeden sonra](./media/site-recovery-network-design/network-design9.png)

#### <a name="site-to-site-connection"></a>Siteden siteye bağlantı

Ek yük devretme sonrasında vnet-vnet bağlantısı olarak siteden siteye VPN bağlantısı Woodgrove ayarlayabilirsiniz:
- Siteden siteye bağlantı kurma ayarladığınızda, IP adres aralığı varsa yalnızca rota Azure ağ trafiği şirket içi konuma (yerel ağı) şirket içi IP adresi aralığından farklıdır. Azure esnetilen alt ağları desteklemiyor olmasıdır. Bu nedenle, şirket 192.168.1.0/24 alt ağ varsa, Azure ağında bir yerel ağı 192.168.1.0/24 ekleyemezsiniz. Azure alt ağdaki hiçbir etkin Vm'leri olduğunu ve yalnızca olağanüstü durum kurtarma için alt ağ oluşturulmaktadır bilmediği beklenmektedir.
- Bir Azure ağı ağ trafik doğru şekilde yönlendirmek için alt ağların ağ ve yerel ağ çakışma gerekmez.




## <a name="assigning-new-ip-addresses"></a>Yeni IP adresleri atama

Bu [blog gönderisi](https://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) yük devretmeden sonra IP adresleri saklamak ihtiyacınız kalmadığında, Azure ağ altyapısını ayarlamak açıklanmaktadır. Bir uygulama açıklaması ile başlayan, şirket içi ağı ayarlama ve azure'da nasıl ayarlanacağını bakar ve yük devretme işlemleri çalıştırma hakkında bilgi ile burada sona eriyor.

## <a name="next-steps"></a>Sonraki adımlar
[Yük devretme çalıştırma](site-recovery-failover.md)
