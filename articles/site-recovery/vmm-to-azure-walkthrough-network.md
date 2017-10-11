---
title: "Azure Site Recovery ile Azure çoğaltma için Hyper-V (withSystem Center VMM) ağ planı | Microsoft Docs"
description: "Bu makalede ele Azure'a Hyper-V Vm'lerini (VMM ile) çoğaltırken ağ gerekli planlama"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ef87cd82b021e40f0da05142878daff245cd9c62
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2017
---
# <a name="step-4-plan-networking-for-hyper-v-with-vmm-to-azure-replication"></a>4. adım: Azure çoğaltma (VMM ile) Hyper-V için ağ planlama

Gerçekleştirdikten sonra [kapasite planlaması](vmm-to-azure-walkthrough-capacity.md) (tam dağıtımını yaptığınız varsa), ağ çoğaltma System Center Virtual Machine Manager (VMM) bulutlarında, Hyper-V sanal makineleri şirket içi zaman planlama konuları hakkında bilgi edinmek için bu makaleyi okuyun kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.

Tüm yorumlarınızı bu makalenin alt kısmında paylaşabilir veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda soru sorabilirsiniz.


## <a name="network-mapping-for-replication-to-azure"></a>Azure'a çoğaltma için Ağ eşlemesi

Ağ eşlemesi Hyper-V Vm'lerini (VMM yönetilen) çoğaltma Azure için kullanılır. Bir kaynak VMM sunucusunda VM ağları arasındaki eşlemeyi eşlemeleri ağ ve hedef Azure ağları. Eşleme şunları yapar:

- **Ağ bağlantısı**— yük devretme sonrasında, tüm çoğaltılan Azure Vm'lerinin eşlenen Azure ağına bağlanır. Bunlar farklı kurtarma planları devredilir olsa bile aynı ağda yük devri tüm makineler birbirine bağlanabilir.
- **Ağ geçidi**— bir ağ geçidi hedef Azure ağında ayarlanıp ayarlanmadığını VM'ler üzerinde premised eşlenen ağ diğer şirket içi sanal makinelere bağlanabilir.

### <a name="prepare-vmm-for-network-mapping"></a>VMM ağ eşlemesi için hazırlanma

VMM, ağ eşlemesi için aşağıdaki gibi hazırlayın:

1. Yoksa, hazırlama bir [VMM mantıksal ağı](https://docs.microsoft.com/system-center/vmm/network-logical) , Hyper-V konaklarının bulunduğu bulutla ilişkili.
2. Yoksa, oluşturulan bir [VM ağı](https://docs.microsoft.com/system-center/vmm/network-virtual) üzerinde hazırlanmış mantıksal ağ ile bağlantılı.
3. Sanal makineleri Hyper-V konak kümesinde sunucu/VMM bulutta VM ağına bağlayın.

 
Şunlara dikkat edin: 
- Kaynak VM ağı'na yeni VM'ler eklendiğinde Çoğaltma gerçekleştiğinde eşlenen Azure ağına bağlanır.
- Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa, çoğaltılan sanal makine yük devretme işleminin ardından hedef alt ağa bağlanır.
- Eşleşen ada sahip bir hedef alt ağ yoksa sanal makine ağdaki ilk alt ağa bağlanır.
- Bir Azure ağı hazırlamak ve bu senaryoyu dağıtmak gibi ağ eşlemeleri ayarlayın.

## <a name="connecting-to-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerinin bağlanma

Çoğaltma ve yük devretme stratejinizi planlarken, önemli sorular yük devretme sonrasında Azure VM'ye bağlanma biridir. Çoğaltma Azure VM'ler için ağ stratejinizi tasarlarken, birkaç seçeneğiniz vardır:

- **Farklı bir IP adresi kullanmak**: çoğaltılan Azure VM ağı için farklı bir IP adresi aralığı kullanmayı seçebilirsiniz. Bu senaryoda VM yük devretme sonrasında yeni bir IP adresi alır ve DNS güncelleştirme gereklidir.
- **Aynı IP adresini korumak**: aynı IP adresi aralığı olarak birincil şirket içi ağınızda yük devretme sonrasında Azure ağı için kullanmak isteyebilirsiniz.  Tutma aynı IP adreslerini basitleştirir kurtarma azaltarak ağ ile ilgili sorunları yük devretme sonrasında. Ancak, Azure'a çoğaltma yapıyorsanız, yolları yeni konumunu IP adresleri ile yük devretme sonrasında güncelleştirmeniz gerekir.


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


Woodgrove olması için IP adresleri, burada 's ne korurken Vm'lerini Azure'a çoğaltmak için şirket yapması gerekir:

1. Bir Azure sanal ağı oluşturun. Böylece uygulamaları sorunsuz bir şekilde yük devredebildiğini şirket içi ağ uzantısı olmalıdır.
2. Azure siteden siteye VPN bağlantısı, Azure üzerinde oluşturulan sanal ağlar için noktadan siteye bağlantı yanı sıra eklemenize olanak sağlar.
3. Yalnızca şirket içi IP adres aralığından IP adresi aralığı farklıysa Azure ağında siteden siteye bağlantı kurarken şirket içi konumuna (yerel ağ) trafiği yönlendirebilir.
    - Azure esnetilen alt ağları desteklemiyor olmasıdır. Bu nedenle alt 192.168.1.0/24 şirket içi varsa, Azure ağında bir yerel ağ 192.168.1.0/24 ekleyemezsiniz.
    - Azure alt ağda etkin VM yok ve yalnızca olağanüstü durum kurtarma için alt ağ oluşturulmaktadır bilmiyor çünkü bu beklenir.
    - Doğru alt ağ ve yerel ağ çakışma dpm'nin bir Azure ağı dışında ağ trafiğini yönlendirmek için.

![Alt ağ yük devretme önce](./media/vmm-to-azure-walkthrough-network/network-design7.png)

### <a name="before-failover"></a>Önce yük devretme

1. Ek bir ağa (örneğin kurtarma ağ) oluşturun. Bu ağdır VM'ler başarısız olarak oluşturulur.
2. Bir VM için IP adresi VM Özellikleri'nde, yük devretme sonrasında korunur emin olmak için > **yapılandırma**VM içi sahip aynı IP adresi belirtin ve tıklatın **kaydetmek**.
3. VM üzerinden başarısız olduğunda, Azure Site Recovery için sağlanan IP adresi atar.

    ![Ağ Özellikleri](./media/vmm-to-azure-walkthrough-network/network-design8.png)

4. Yük devretme tetikleyici tetiklenir ve gerekli IP adresi ile oluşturulan sanal makineleri Azure içinde olduktan sonra ağ kullanmaya bağlanabilir bir [Vnet vonnection Vnet'e](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Bu eylem betiği yazılabilir.
5. Yollar uygun şekilde değiştirilmesi, bu 192.168.1.0/24 artık Azure'a taşındı yansıtacak şekilde gerekir.

    ![Alt ağ yük devretme sonrasında](./media/vmm-to-azure-walkthrough-network/network-design9.png)

### <a name="after-failover"></a>Yük devretme sonrasında

Yukarıda gösterildiği gibi bir Azure ağı yoksa, failvoer sonra birincil site ile Azure arasında siteden siteye VPN bağlantısı oluşturabilirsiniz.

## <a name="change-ip-addresses"></a>IP adreslerini değiştirin

Bu [blog gönderisi](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) IP adreslerini yük devretme sonrasında korumak gerekmediğinde Azure ağ altyapısını ayarlamak açıklanmaktadır. Uygulama açıklaması ile başlayan, şirket içi ağ yukarı ve Azure nasıl ayarlanacağını bakar ve yük devretme işlemleri çalıştırma hakkında bilgi ile sonlanır.  

## <a name="next-steps"></a>Sonraki adımlar

Git [5. adım: Azure hazırlama](vmm-to-azure-walkthrough-prepare-azure.md)
