---
title: "Azure Site Recovery ile azure'a yük devretme sonrasında VM bağlantı için ağ | Microsoft Docs"
description: "Şirket içi Azure Site Recovery ile yük devretme sonrasında Azure Vm'lerine bağlanmak için kılavuz ağ oluşturma"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: f02cdbea-0940-48bf-9fa5-f38d9e584fae
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/12/2017
ms.author: raynew
ms.openlocfilehash: 01c8e664465350b9dd382502c65cc3fda350797c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="networking-for-vm-connectivity-after-failover"></a>VM bağlantı için yük devretme sonrasında ağ oluşturma

Bu makalede kullandıktan sonra Azure Vm'lerine bağlanmak için ağ gereksinimleri açıklanmıştır [Azure Site Recovery](site-recovery-overview.md) çoğaltma ve yük devretme Azure hizmet.

Bu makalede hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Kullanabileceğiniz bağlantı yöntemleri
> * Çoğaltılan Azure Vm'leri için farklı bir IP adresi kullanma
> * Yük devretme sonrasında Azure VM'ler için IP adreslerini korumak nasıl

## <a name="connecting-to-replica-vms"></a>Çoğaltma sanal makineleri bağlanma

Çoğaltma ve yük devretme stratejinizi planlarken, önemli sorular yük devretme sonrasında Azure VM'ye bağlanma biridir. Çoğaltma Azure VM'ler için ağ stratejinizi tasarlarken, birkaç seçeneğiniz vardır:

- **Farklı bir IP adresi kullanmak**: çoğaltılan Azure VM ağı için farklı bir IP adresi aralığı kullanmayı seçebilirsiniz. Bu senaryoda VM yük devretme sonrasında yeni bir IP adresi alır ve DNS güncelleştirme gereklidir.
- **Aynı IP adresini korumak**: aynı IP adresi aralığı olarak birincil şirket içi sitenizdeki yük devretme sonrasında Azure ağı için kullanmak isteyebilirsiniz. Tutma aynı IP adreslerini basitleştirir kurtarma azaltarak ağ ile ilgili sorunları yük devretme sonrasında. Ancak, Azure'a çoğaltma yapıyorsanız, yolları yeni konumunu IP adresleri ile yük devretme sonrasında güncelleştirmeniz gerekir. 

## <a name="retaining-ip-addresses"></a>IP adreslerini korur

Site Recovery sabit IP korumak özelliği üzerinden Azure için bir alt ağ yük devretme kümelemesiyle başarısız olduğunda adresleri sağlar.

- Alt ağ yük devretmesi ile belirli bir alt aynı anda Site 1 veya her iki sitede ancak hiçbir Site 2 konumunda bulunur.
- IP adres alanı bir yük devretme durumunda korumak için program aracılığıyla alt ağların bir siteden diğerine taşımak yönlendirici altyapı düzenleyin.
- Yük devretme sırasında alt ağlar ile ilişkili korumalı sanal makineleri taşıyın. Bir arıza olması durumunda olan asıl sakıncası, tüm alt ağı taşımanız gerekir.


### <a name="failover-example"></a>Yük devretme örneği

Azure usng ficticious şirket Woodgrove Bank yük devretme için bir örneğe bakalım.

- Woodgrove Bank, bu iş uygulamalarını şirket içi sitesi barındırır. Bunlar mobil uygulamalarını Azure ile ilgili ana bilgisayar.
- Kendi şirket içi uç ağ ve Azure sanal ağı arasında VPN siteden siteye bağlantı yok. VPN bağlantısı nedeniyle Azure sanal ağında bir şirket içi ağ uzantısı olarak görünür.
- Şirket içi iş yüklerini Azure Site Recovery ile çoğaltmak Woodgrove istemektedir.
 - Woodgrove azure'a yük devretme sonrasında, uygulamalar için IP adreslerini korumak ihtiyaç duydukları şekilde sabit kodlanmış IP adreslerini, bağlı olan uygulamalar vardır.
 - Azure'da çalışan kaynaklarını kullanan IP adresi aralığı 172.16.1.0/24 172.16.2.0/24.

![Alt ağ yük devretme önce](./media/site-recovery-network-design/network-design7.png)

**Yük devretme önce altyapısı**


Woodgrove olması için IP adresleri, burada 's ne korurken Vm'lerini Azure'a çoğaltmak için şirket yapması gerekir:


1. Yük devretme içi makineler sonra Azure sanal makinelerini oluşturulacak Azure sanal ağı oluşturun. Böylece uygulamaları sorunsuz bir şekilde yük devredebildiğini şirket içi ağ uzantısı olmalıdır.
2. Site kurtarma hizmetinde yük devretme önce bunlar makine özelliklerinde aynı IP adresi atayın. Yük devretme sonrasında Azure VM'ye bu adresi Site Recovery atar.
3. Yük devretme çalışır ve aynı IP adresi ile Azure VM'ler oluşturulduktan sonra ağ kullanmaya bağlandıklarında bir [Vnet'e bağlantı](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Bu eylem betiği yazılabilir.
4. Bunlar, 192.168.1.0/24 artık Azure'a taşındı yansıtacak şekilde rotaları da değiştirmeniz gerekir.


**Yük devretme sonrasında altyapısı**

![Alt ağ yük devretme sonrasında](./media/site-recovery-network-design/network-design9.png)

#### <a name="site-to-site-connection"></a>Siteden siteye bağlantı

Yük devretme sonrasında vnet-vnet bağlantısı yanı sıra Woodgrove siteden siteye VPN bağlantısı ayarlayabilirsiniz:
- Siteden siteye bağlantı kurma ayarladığınızda, IP adresi aralığı yalnızca yönlendirebilir Azure ağ trafiği şirket içi konumuna (yerel-ntwork) şirket içi IP adresi aralığından farklıdır. Azure esnetilen alt ağları desteklemiyor olmasıdır. Bu nedenle, alt ağ 192.168.1.0/24 şirket içi varsa, Azure ağında bir yerel ağ 192.168.1.0/24 ekleyemezsiniz. Azure alt ağda etkin VM yok ve yalnızca olağanüstü durum kurtarma için alt ağ oluşturulmaktadır bilmiyor çünkü bu beklenir.
- Doğru bir Azure ağı dışında ağ trafiğini yönlendirmek için ağ ve yerel ağ alt çakışma gerekir.




## <a name="assigning-new-ip-addresses"></a>Yeni IP adresleri atama

Bu [blog gönderisi](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) IP adreslerini yük devretme sonrasında korumak gerekmediğinde Azure ağ altyapısını ayarlamak açıklanmaktadır. Uygulama açıklaması ile başlayan, şirket içi ağ yukarı ve Azure nasıl ayarlanacağını bakar ve yük devretme işlemleri çalıştırma hakkında bilgi ile sonlanır. 

## <a name="next-steps"></a>Sonraki adımlar
[Yük devretme gerçekleştirme](site-recovery-failover.md)




