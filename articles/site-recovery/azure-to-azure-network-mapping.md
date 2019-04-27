---
title: Azure Site recovery'de iki Azure bölgeleri arasında sanal ağları eşleme | Microsoft Docs
description: Çoğaltma, yük devretme ve kurtarma sanal makinelerin ve fiziksel sunucuları Azure Site Recovery düzenler. Azure'a veya ikincil veri merkezine yük devretme hakkında bilgi edinin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/9/2019
ms.author: mayg
ms.openlocfilehash: d08715b1b3e0db4dfcf31bb4c020ab44ed3916e1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60791107"
---
# <a name="set-up-network-mapping-and-ip-addressing-for-vnets"></a>Ağ eşlemesini ve sanal ağlar için IP adresini ayarlama

Bu makalede iki farklı Azure bölgelerinde bulunan Azure sanal ağlar (Vnet'ler) örneğini eşlemeyle ilgili bilgi ve ağlar arasında IP adresini ayarlama konusunda açıklanmaktadır. Ağ eşlemesini hedef ağ seçimi çoğaltmayı etkinleştirme sırasında kaynak ağı temel alarak varsayılan davranış sağlar.

## <a name="prerequisites"></a>Önkoşullar

Ağları Eşle önce olması [Azure sanal ağları](../virtual-network/virtual-networks-overview.md) kaynak ve hedef Azure bölgesi. 

## <a name="set-up-network-mapping-manually-optional"></a>Ağı ayarlama (isteğe bağlı), el ile eşleme

Ağları aşağıdaki şekilde eşlenir:

1. İçinde **Site Recovery altyapısı**, tıklayın **+ ağ eşlemesi**.

    ![ Ağ eşlemesi oluşturma](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)

3. İçinde **ağ eşlemesi Ekle**kaynağını seçin ve hedef konumları. Örneğimizde, kaynak VM Doğu Asya bölgesinde çalıştığından ve Güneydoğu Asya bölgeye çoğaltır.

    ![Kaynak ve hedef seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)
3. Şimdi bir ağ eşlemesini karşı dizin oluşturun. Örneğimizde, kaynak artık Güneydoğu Asya olacaktır ve hedef Doğu Asya olacaktır.

    ![Ağ Eşleme bölmesi - ekleme kaynak ve hedef konumların hedef ağ seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-networks-when-you-enable-replication"></a>Çoğaltmayı etkinleştirdiğinizde ağları Eşle

Azure Vm'leri için olağanüstü durum kurtarma yapılandırmadan önce Ağ eşlemesi hazırlandı yapmadıysanız, bir hedef belirtebilirsiniz ağ, [ayarlama ve çoğaltmayı etkinleştirme](azure-to-azure-how-to-enable-replication.md). Aşağıdaki bunu yaptığınızda olur:

- Seçtiğiniz hedefe bağlı olarak, Site Recovery otomatik olarak ağ eşlemeleri kaynaktan hedef bölge ve kaynak bölgeye hedeften oluşturur.
- Varsayılan olarak, Site Recovery, hedef bölgedeki kaynak ağına aynı olan bir ağ oluşturur. Site Recovery ekler **-asr** kaynak ağı adı soneki olarak. Hedef ağ özelleştirebilirsiniz.
- Kaynak ağ için Ağ eşlemesi zaten oluştuysa, eşlenmiş hedef ağ daha fazla sanal makine için çoğaltma etkinleştirme zaman her zaman varsayılan olur. Hedef sanal ağ, diğer kullanılabilir seçenekler açılır listeden seçerek değiştirmek seçebilirsiniz. 
- Varsayılan hedef sanal ağ için yeni çoğaltmalar değiştirmek için mevcut ağ eşlemesini değiştirmeniz gerekir.
- Bir bölgeden bölgeye B bir ağ eşlemesini değiştirmek isterseniz emin olun, ağ eşlemesini B bölgeden bölgeye A. silin Ters eşleme hesabınız silindikten sonra bir bölgeden bölgeye B ağ eşlemesini değiştirmek ve ardından ilgili ters eşleme oluşturun.

>[!NOTE]
>* Ağ eşlemesini değiştirmek, yalnızca yeni VM çoğaltma için varsayılanları değiştirir. Mevcut çoğaltma için hedef sanal ağ seçimler etkilemez. 
>* Var olan bir çoğaltma için hedef ağ değiştirmek istiyorsanız, işlem ve ağ ayarlarını çoğaltılan öğenin geçelim.

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
Yük devretme sanal ağı hedef ağ değil | -Hedef IP adresi, statik yük devretme için ayrılmış aynı IP adresine sahip olacaktır.<br/><br/>  -Aynı IP adresi zaten atanmış ise, IP adresini bir sonraki alt aralığın sonunda kullanılabilir olduğunu.<br/><br/> Örneğin: Kaynak statik IP adresi 10.0.0.19 ve yük devretme yük devretme ağı olmayan bir ağ üzerinde aralığı 10.0.0.0/24 hedef statik IP adresi varsa 10.0.0.0.19 olacaktır ve aksi durumda 10.0.0.254 olur.

- Yük devretme VNet olağanüstü durum kurtarma işlemini ayarladığınız olurken seçtiğiniz hedef ağdır.
- Üretim dışı ağ her zaman test yük devretmesi için kullanmanızı öneririz.
- Hedef IP adresini değiştirebileceğiniz **işlem ve ağ** VM ayarları.


## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md) Azure VM'LERİNDE olağanüstü durum kurtarma için.
- [Daha fazla bilgi edinin](site-recovery-retain-ip-azure-vm-failover.md) yük devretmeden sonra IP adresleri koruma hakkında.

Seçilen hedef ağı yük devretme vnet'se"ve"Seçili hedef ağ yük devretme sanal ağdan farklı ancak yük devretme sanal ağ ile aynı alt ağ aralığında ise"söylemek 2 noktası
