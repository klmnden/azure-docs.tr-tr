---
title: "Fiziksel sunucu çoğaltma Azure için ağ planı | Microsoft Docs"
description: "Bu makalede ağ fiziksel sunucuları Azure'a çoğaltırken gerekli planlama açıklanır"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 71db2435-b5ce-4263-83f6-093d10d1d4e1
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f8a20b45b50f71631122e574b634818c1912f12e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-4-plan-networking-for-physical-server-replication-to-azure"></a>4. adım: ağ fiziksel sunucu çoğaltma Azure için planlama

Bu makalede ağ çoğaltmaya Azure kullanarak fiziksel sunucuları şirket içi zaman planlama hususları özetlenmektedir [Azure Site Recovery](site-recovery-overview.md) hizmet.

Tüm yorumlarınızı bu makalenin alt kısmında paylaşabilir veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda soru sorabilirsiniz.


## <a name="connect-to-replica-azure-vms"></a>Azure VM'ler yenilemeye Bağlan

Çoğaltma ve yük devretme stratejinizi planlarken, önemli sorular yük devretme sonrasında Azure VM'ye bağlanma biridir. Çoğaltma Azure VM'ler için ağ stratejinizi tasarlarken, birkaç seçeneğiniz vardır:

- **Farklı bir IP adresi kullanmak**: çoğaltılan Azure VM ağı için farklı bir IP adresi aralığı kullanmayı seçebilirsiniz. Bu senaryoda, yük devretme ve DNS güncelleştirme tamamlandıktan sonra gerekli makine yeni bir IP adresi alır.
- **Aynı IP adresini kullanmak**: aynı IP adresi aralığı olarak birincil şirket içi sitenizdeki yük devretme sonrasında Azure ağı için kullanmak isteyebilirsiniz. Tutma aynı IP adreslerini basitleştirir kurtarma azaltarak ağ ile ilgili sorunları yük devretme sonrasında. Ancak, Azure'a çoğaltma yapıyorsanız, yolları yeni konumunu IP adresleri ile yük devretme sonrasında güncelleştirmeniz gerekir.

## <a name="retain-ip-addresses"></a>IP adreslerini korur

Site Recovery sabit IP korumak özelliği üzerinden Azure için bir alt ağ yük devretme kümelemesiyle başarısız olduğunda adresleri sağlar.
Alt ağ yük devretmesi ile belirli bir alt aynı anda Site 1 veya her iki sitede ancak hiçbir Site 2 konumunda bulunur. IP adres alanı bir yük devretme durumunda korumak için program aracılığıyla alt ağların bir siteden diğerine taşımak yönlendirici altyapı düzenleyin. Yük devretme sırasında alt ağlar ile ilişkili korumalı sanal makineleri taşıyın. Bir arıza olması durumunda olan asıl sakıncası, tüm alt ağı taşımanız gerekir.

### <a name="failover-example"></a>Yük devretme örneği

Azure'a yük devretme için bir örneğe bakalım.

- Ficticious şirket, Woodgrove Bank, iş uygulamalarını barındıran bir şirket içi altyapı sahiptir. Kullanıcıların mobil uygulamalar Azure üzerinde barındırılır.
- Azure ve şirket içi sunucularda Woodgrove Bank VMs arasındaki bağlantıyı, şirket içi uç ağ ve Azure sanal ağı arasında bir siteden siteye (VPN) bağlantısı tarafından sağlanır.
- Bu VPN Azure sanal ağında şirketin kendi şirket içi ağ bir uzantısı olarak görüneceği anlamına gelir.
- Woodgrove şirket içi iş yüklerini Azure'a çoğaltmak için Site Recovery kullanmak istiyor.
 - Uygulamalar ve IP adreslerini sabit kodlanmış bağlıdır ve bu nedenle Azure için yük devretme sonrasında uygulamaları için IP adreslerini bekletmeniz gerekir yapılandırmalarını uğraşmanız Woodgrove sahiptir.
 - Woodgrove atanmış IP adresleri aralığı 172.16.1.0/24, Azure'da çalışan kendi kaynaklarına 172.16.2.0/24 gelen.


Woodgrove olması için IP adresleri, burada 's ne korurken, sunucuları Azure'a çoğaltma için şirket yapması gerekir:

1. Bir Azure sanal ağı oluşturun. Böylece uygulamaları sorunsuz bir şekilde yük devredebildiğini şirket içi ağ uzantısı olmalıdır.
2. Azure siteden siteye VPN bağlantısı, Azure üzerinde oluşturulan sanal ağlar için noktadan siteye bağlantı yanı sıra eklemenize olanak sağlar.
3. Yalnızca şirket içi IP adres aralığından IP adresi aralığı farklıysa Azure ağında siteden siteye bağlantı kurarken şirket içi konumuna (yerel ağ) trafiği yönlendirebilir.
    - Azure esnetilen alt ağları desteklemiyor olmasıdır. Bu nedenle alt 192.168.1.0/24 şirket içi varsa, Azure ağında bir yerel ağ 192.168.1.0/24 ekleyemezsiniz.
    - Azure alt ağdaki hiçbir etkin makine olduğunu ve yalnızca olağanüstü durum kurtarma için alt ağ oluşturulmaktadır bilmiyor çünkü bu beklenir.
    - Doğru alt ağ ve yerel ağ çakışma dpm'nin bir Azure ağı dışında ağ trafiğini yönlendirmek için.

![Alt ağ yük devretme önce](./media/physical-walkthrough-network/network-design7.png)

#### <a name="before-failover"></a>Önce yük devretme

1. Ek bir ağa (örneğin kurtarma ağ) oluşturun. Bu ağdır VM'ler başarısız olarak oluşturulur.
2. Bir makine için IP adresi makine özelliklerinde bir yük devretme sonrasında tutulur emin olmak için > **yapılandırma**sunucu şirket içi sahip aynı IP adresi belirtin ve tıklatın **kaydetmek**.
3. Makine üzerinden başarısız olduğunda, Azure Site Recovery için sağlanan IP adresi atar.
4. Yük devretme tetikleyici tetiklenir ve gerekli IP adresi ile oluşturulan sanal makineleri Azure içinde olduktan sonra ağ kullanmaya bağlanabilir bir [Vnet'e bağlantı](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Bu eylem betiği yazılabilir.
5. Yollar uygun şekilde değiştirilmesi, bu 192.168.1.0/24 artık Azure'a taşındı yansıtacak şekilde gerekir.

    ![Alt ağ yük devretme sonrasında](./media/physical-walkthrough-network/network-design9.png)

#### <a name="after-failover"></a>Yük devretme sonrasında

Yukarıda gösterildiği gibi bir Azure ağı yoksa, yük devretme sonrasında birincil site ile Azure arasında siteden siteye VPN bağlantısı oluşturabilirsiniz.

## <a name="change-ip-addresses"></a>IP adreslerini değiştirin

Bu [blog gönderisi](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) IP adreslerini yük devretme sonrasında korumak gerekmediğinde Azure ağ altyapısını ayarlamak açıklanmaktadır. Uygulama açıklaması ile başlayan, şirket içi ağ yukarı ve Azure nasıl ayarlanacağını bakar ve yük devretme işlemleri çalıştırma hakkında bilgi ile sonlanır.  

## <a name="next-steps"></a>Sonraki adımlar

Git [5. adım: Azure hazırlama](physical-walkthrough-prepare-azure.md)
