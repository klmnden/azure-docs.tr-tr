---
title: "Sanal ağlar Azure Site kurtarma iki Azure bölgeleri arasında eşleme | Microsoft Docs"
description: "Azure Site Recovery, çoğaltma, yük devretme ve sanal makinelerin ve fiziksel sunucuları kurtarma düzenler. Azure'a veya ikincil veri merkezine yük devretme hakkında bilgi edinin."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: d7dd35a8382f4a99ababbe804c5c71b29148c44a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Farklı Azure bölgelerinde sanal ağları Eşle


Bu makalede, Azure sanal ağının birbirleriyle farklı Azure bölgelerinde bulunan iki örneği eşlemek açıklar. Ağ eşlemesi, çoğaltılmış bir sanal makine hedef Azure bölgesi oluşturduğunuzda, sanal makine ayrıca kaynak sanal makinenin sanal ağa eşlenen sanal ağda oluşturulduğunu sağlar.  

## <a name="prerequisites"></a>Önkoşullar
Ağlarını eşlemek önce oluşturduğunuz olun bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) hem kaynak bölge, hem de hedef Azure bölgesi.

## <a name="map-virtual-networks"></a>Sanal ağlar eşleme

Bir Azure bölgesi (kaynak ağ) Azure sanal makineleri için başka bir bölgede (hedef ağ) bulunan bir sanal ağ içinde bulunan bir Azure sanal ağı eşlemek için Git **Site Recovery altyapısı**  >  **Ağ eşlemesi**. Ağ eşlemesi oluşturun.

![Eşlemeleri penceresi ağ - ağ eşlemesi oluşturma](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Aşağıdaki örnekte, Doğu Asya bölgesinde sanal makine çalışıyor. Sanal makine Güneydoğu Asya bölgeye çoğaltılır.

Ağ eşlemesi Güneydoğu Asya bölgesine Doğu Asya bölgesi oluşturmak için kaynak ağ konumu ve hedef ağ konumu seçin. Ardından, seçin **Tamam**.

![Ağ eşleme pencere - eklemek için kaynak ağ kaynak ve hedef konumları seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Ağ eşlemesi, Doğu Asya bölgesine Güneydoğu Asya bölgesi oluşturmak için yukarıdaki işlemi yineleyin.

![Ağ Eşleme bölmesi - ekleme hedef ağ için kaynak ve hedef konumları seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Çoğaltma etkinleştirdiğinizde, bir ağ eşleme

Ağ eşleme varsa, bir sanal makine bir Azure bölgesindeki başka bir bölgeye ilk kez çoğalttığınızda, çoğaltma ayarladığınızda, hedef ağ ayarlayabilirsiniz. Bu ayarda bağlı olarak, Azure Site Recovery ağ eşlemeleri hedef bölgeye kaynak bölgesinden ve kaynak bölge için hedef bölgesinden oluşturur.   

![Ayarlar bölmesini Yapılandır - hedef konumu seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Varsayılan olarak, Site Recovery hedef bölgede kaynak ağına aynı olan bir ağ oluşturur. Site Recovery ekleyerek bir ağ oluşturur **-asr** kaynak ağ adı için bir son eki olarak. Önceden oluşturulmuş bir ağ seçmek için Seç **Özelleştir**.

![Hedef ayarları bölmesi - kümesi hedef kaynak grubu adı ve hedef sanal ağ adı özelleştirme](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Ağ eşlemesi zaten olduysa, çoğaltma etkinleştirdiğinizde, hedef sanal ağ değiştiremezsiniz. Bu durumda, hedef sanal ağ değiştirmek için mevcut ağ eşlemesini değiştirin.  

![Hedef özelleştirme ayarları bölmesi - hedef kaynak grubu adını ayarla](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Ağ eşlemesi bölmesinde değiştirilemiyor - varolan bir hedef sanal ağ adı değiştir](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Bölge B A bölgesinden bir ağ eşlemesini değiştirirseniz, ayrıca B bölgesinden ağ eşlemesi bölgeye A. değişiklik emin olun
>
>


## <a name="subnet-selection"></a>Alt ağ seçimi
Hedef sanal makinenin, kaynak sanal makinenin adını temel alarak seçilir. Hedef ağ kaynak sanal makinenin aynı ada sahip bir alt ağ varsa, o alt hedef sanal makine için ayarlanır. Alfabetik olarak ilk alt ağ, aynı ada sahip bir alt ağ hedef ağ yoksa, hedef alt ağ ayarlanır. 

Alt ağ değiştirmek için Git **işlem ve ağ** sanal makine için ayarları.

![İşlem ve ağ işlem Özellikler penceresi](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP adresi

Hedef sanal makinenin her ağ arabirimi için IP adresi, aşağıdaki bölümlerde açıklandığı şekilde ayarlanır.

### <a name="dhcp"></a>DHCP
Kaynak sanal makinenin Ağ arabiriminin DHCP kullanıyorsa, hedef sanal makine ağ arabiriminin Ayrıca DHCP kullanmak üzere ayarlanmış.

### <a name="static-ip-address"></a>Statik IP adresi
Ağ arabirimi kaynak sanal makinenin bir statik IP adresi kullanıyorsa, hedef sanal makine ağ arabiriminin de statik IP adresi kullanmak üzere ayarlanmış. Aşağıdaki bölümlerde, bir statik IP adresi ayarlanma açıklanmaktadır.

#### <a name="same-address-space"></a>Aynı adres alanı

Kaynak alt ağı ve hedef alt aynı adres alanı varsa, kaynak sanal makinenin Ağ arabiriminin IP adresini hedef IP adresi olarak ayarlanır. Aynı IP adresi kullanılabilir durumda değilse, bir sonraki kullanılabilir IP adresi hedef IP adresi olarak ayarlanır.

#### <a name="different-address-spaces"></a>Farklı bir adres alanları

Kaynak alt ağı ve hedef alt farklı adres alanları varsa, hedef alt ağdaki bir sonraki kullanılabilir IP adresi hedef IP adresi olarak ayarlanır.

Her ağ arabiriminin hedef IP değiştirmek için Git **işlem ve ağ** sanal makine için ayarları.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [Azure sanal makineleri çoğaltmak için kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md).
