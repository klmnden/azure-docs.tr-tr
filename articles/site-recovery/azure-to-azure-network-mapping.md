---
title: Azure Site recovery'de iki Azure bölgeleri arasında sanal ağları eşleme | Microsoft Docs
description: Çoğaltma, yük devretme ve kurtarma sanal makinelerin ve fiziksel sunucuları Azure Site Recovery düzenler. Azure'a veya ikincil veri merkezine yük devretme hakkında bilgi edinin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: fccc7379794b4b75ff53e517eddd95ff0f7db0e9
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223791"
---
# <a name="set-up-network-mapping-and-ip-addressing-for-vnets"></a>Ağ eşlemesini ve sanal ağlar için IP adresini ayarlama

Bu makalede iki farklı Azure bölgelerinde bulunan Azure sanal ağlar (Vnet'ler) örneğini eşlemeyle ilgili bilgi ve ağlar arasında IP adresini ayarlama konusunda açıklanmaktadır. Ağ eşlemesini sağlar çoğaltılmış bir VM oluşturulur kaynak VM sanal ağa eşlenmiş vnet'teki hedef Azure bölgesi oluşturulur.

## <a name="prerequisites"></a>Önkoşullar

Ağları Eşle önce olması [Azure sanal ağları](../virtual-network/virtual-networks-overview.md) kaynak ve hedef Azure bölgesi. 

## <a name="set-up-network-mapping"></a>Ağ eşlemesini ayarlama

Ağları aşağıdaki şekilde eşlenir:

1. İçinde **Site Recovery altyapısı**, tıklayın **+ ağ eşlemesi**.

    ![ Ağ eşlemesi oluşturma](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)

3. İçinde **ağ eşlemesi Ekle**kaynağını seçin ve hedef konumları. Örneğimizde, kaynak VM Doğu Asya bölgesinde çalıştığından ve Güneydoğu Asya bölgeye çoğaltır.

    ![Kaynak ve hedef seçin ](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)
3. Şimdi bir ağ eşlemesini karşı dizin oluşturun. Örneğimizde, kaynak artık Güneydoğu Asya olacaktır ve hedef Doğu Asya olacaktır.

    ![Ağ Eşleme bölmesi - ekleme kaynak ve hedef konumların hedef ağ seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-networks-when-you-enable-replication"></a>Çoğaltmayı etkinleştirdiğinizde ağları Eşle

Azure Vm'leri için olağanüstü durum kurtarma yapılandırmadan önce Ağ eşlemesi hazırlandı yapmadıysanız, bir hedef belirtebilirsiniz ağ, [ayarlama ve çoğaltmayı etkinleştirme](azure-to-azure-how-to-enable-replication.md). Aşağıdaki bunu yaptığınızda olur:

- Seçtiğiniz hedefe bağlı olarak, Site Recovery otomatik olarak ağ eşlemeleri kaynaktan hedef bölge ve kaynak bölgeye hedeften oluşturur.
- Varsayılan olarak, Site Recovery, hedef bölgedeki kaynak ağına aynı olan bir ağ oluşturur. Site Recovery ekler **-asr** kaynak ağı adı soneki olarak. Hedef ağ özelleştirebilirsiniz.
- Ağ eşlemesi zaten oluştuysa, çoğaltmayı etkinleştirdiğinizde, hedef sanal ağ değiştiremezsiniz. Hedef sanal ağ değiştirmek için mevcut ağ eşlemesini değiştirmeniz gerekir.
- Bir bölgeden bölgeye B bir ağ eşlemesini değiştirirseniz, ayrıca bölgeye A. B bölgeden ağ eşlemesini değiştirmek olun]

## <a name="specify-a-subnet"></a>Bir alt ağ belirtin

VM Seçilen hedefin alt ağ kaynak VM alt ağ adını temel alarak.

- Hedef ağ ile kaynak VM alt ağı ile aynı ada sahip bir alt ağ varsa, bu alt hedef sanal makine için ayarlanır.
- Alfabetik sırada ilk alt ağ, aynı ada sahip bir alt ağı hedef ağ mevcut değilse hedef alt ağ ayarlanır.
- Değiştirebileceğiniz içinde **işlem ve ağ** VM için ayarlar.

    ![İşlem ve ağ işlem Özellikler penceresi](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="set-up-ip-addressing-for-target-vms"></a>Hedef sanal makineler için IP adresini ayarlama

Hedef sanal makinedeki her NIC için IP adresini aşağıdaki gibi yapılandırılır:

- **DHCP**: Kaynak VM NIC'i DHCP kullanıyorsa, hedef VM NIC de DHCP kullanacak şekilde ayarlanır.
- **Statik IP adresi**: Kaynak NIC VM statik IP adresleme kullanıyorsa, hedef VM NIC de statik IP adresi kullanır.


## <a name="ip-address-assignment-during-failover"></a>Yük devretme sırasında IP adresi ataması

**Kaynak ve hedef alt ağlar** | **Ayrıntılar**
--- | ---
Aynı adres alanı | Kaynak VM NIC IP adresi, VM'nin NIC IP adresi hedef olarak ayarlanır.<br/><br/> Adresi kullanılabilir değilse, bir sonraki kullanılabilir IP adresi hedef olarak ayarlanır.
Farklı bir adres alanı<br/><br/> Hedef alt ağdaki bir sonraki kullanılabilir IP adresi, hedef VM NIC adresi olarak ayarlanır.



## <a name="ip-address-assignment-during-test-failover"></a>Yük devretme testi sırasında IP adresi ataması

**Hedef ağ** | **Ayrıntılar**
--- | ---
Hedef VNet yük devretme ağdır | -Hedef IP adresi, statik, ancak aynı IP adresi, yük devretme için ayrılmış olarak değil değildir.<br/><br/>  -Alt ağ aralığı sonundan sonraki kullanılabilir adresi atanan adresidir.<br/><br/> Örneğin: Kaynak IP adresini 10.0.0.19 ve Yük Devretme ağ aralığı 10.0.0.0/24 kullanılıyorsa, hedef VM'ye atanmış sonraki IP adresi 10.0.0.254 olduğu.
Yük devretme sanal ağı hedef ağ değil | -Hedef IP adresi, statik yük devretme için ayrılmış aynı IP adresine sahip olacaktır.<br/><br/>  -Aynı IP adresi zaten atanmış ise, IP adresini bir sonraki her bir alt ağ aralığı kullanılabilir olduğunu.<br/><br/> Örneğin: Kaynak statik IP adresi 10.0.0.19 ve yük devretme yük devretme ağı olmayan bir ağ üzerinde aralığı 10.0.0.0/24 hedef statik IP adresi varsa 10.0.0.0.19 olacaktır ve aksi durumda 10.0.0.254 olur.

- Yük devretme VNet olağanüstü durum kurtarma işlemini ayarladığınız olurken seçtiğiniz hedef ağdır.
- Üretim dışı ağ her zaman test yük devretmesi için kullanmanızı öneririz.
- Hedef IP adresini değiştirebileceğiniz **işlem ve ağ** VM ayarları.


## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md) Azure VM'LERİNDE olağanüstü durum kurtarma için.
- [Daha fazla bilgi edinin](site-recovery-retain-ip-azure-vm-failover.md) yük devretmeden sonra IP adresleri koruma hakkında.

Seçilen hedef ağı yük devretme vnet'se"ve"Seçili hedef ağ yük devretme sanal ağdan farklı ancak yük devretme sanal ağ ile aynı alt ağ aralığında ise"söylemek 2 noktası