---
title: "Azure Site kurtarma iki Azure bölgeleri arasında ağ eşlemesi | Microsoft Docs"
description: "Azure Site Recovery, çoğaltma, yük devretme ve sanal makinelerin ve fiziksel sunucuları kurtarma düzenler. Azure veya ikincil veri merkezine yük devretme hakkında bilgi edinin."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 9d6a806ec533259797080fbfee2c38f918ebd8a2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>İki Azure bölgeleri arasında ağ eşlemesi


Bu makalede, Azure sanal ağlar birbirlerine iki Azure bölgelerinin eşlemek açıklar. Ağ eşlemesi hedef Azure bölgesi çoğaltılmış sanal makine oluşturulduğunda, kaynak sanal makinenin sanal ağa eşlenen sanal ağda oluşturulduğunu sağlar.  

## <a name="prerequisites"></a>Ön koşullar
Eşledikten önce ağları oluşturduğunuzdan emin olun [Azure sanal ağlar](../virtual-network/virtual-networks-overview.md) hem de kaynak ve hedef Azure bölgeleri.

## <a name="map-networks"></a>Ağ eşleme

Bir Azure bölgesindeki bir Azure sanal ağı başka bir bölgede başka bir sanal ağ eşlemek için bir ağ eşlemesi oluşturun ve Site Recovery altyapısı için Ağ eşlemesi (Azure Virtual Machines için) -> gidin.

![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Aşağıdaki örnekte sanal Makinem Doğu Asya bölgesinde çalıştığından ve Güneydoğu Asya çoğaltılır.

Kaynak ve hedef ağ seçin ve ardından Güneydoğu Asya Doğu Asya ağ eşlemesi oluşturmak için Tamam'ı tıklatın.

![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Ağ eşlemesi Güneydoğu Asya Doğu Asya oluşturmak için aynı işlevi görür.  
![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Çoğaltma etkinleştirirken eşleme ağ

Başka bir sanal makine için ilk zaman form bir Azure bölgesi çoğaltma yapıyorsanız, ağ eşlemesi yapılmazsa, hedef ağ aynı işleminin bir parçası olarak seçebilirsiniz. Site Recovery, hedef bölgeye kaynak bölgesinden ve bu seçime dayalı kaynak bölge için hedef bölgesinden ağ eşlemeleri oluşturur.   

![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Varsayılan olarak, Site Recovery hedef bölgede kaynak ağa ve ekleyerek aynı olan bir ağ oluşturur '-asr' kaynak ağ adı için bir son eki olarak. Önceden oluşturulmuş bir ağ Özelleştir'i tıklatarak seçebilirsiniz.

![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Ağ eşlemesi zaten yapıldığında, çoğaltmayı etkinleştirirken hedef sanal ağ değiştiremezsiniz. Değiştirmek için mevcut ağ eşlemesini değiştirin.  

![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Ağ eşlemesi](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Bölge 2 bölge-1 arasında bir ağ eşlemesini değiştirirseniz, bölge 2 ağ eşlemesi bölge-1 de değiştirdiğinizden emin olun.
>
>


## <a name="subnet-selection"></a>Alt ağ seçimi
Hedef sanal makinenin, kaynak sanal makinenin adını temel alarak seçilir. Hedef ağ kullanılabilir, kaynak sanal makine olarak aynı ada sahip bir alt ağ ise, hedef sanal makine için seçilir. Varsa hedef ağdaki aynı ada sahip bir alt ağ sonra alfabetik olarak ilk alt ağ hedef alt ağ seçilir. Bu alt ağ, işlem ve ağ sanal makinenin ayarlarını giderek değiştirebilirsiniz.

![Alt ağ değiştirme](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP adresi

Her hedef sanal makinenin ağ arabirimi için IP adresi gibi seçilir:

### <a name="dhcp"></a>DHCP
Kaynak sanal makinenin Ağ arabiriminin DHCP kullanıyorsanız, hedef sanal makinenin ağ arabirimi Ayrıca DHCP ayarlanır.

### <a name="static-ip"></a>Statik IP
Kaynak sanal makinenin Ağ arabiriminin statik IP kullanıyorsanız, ardından hedef sanal makinenin ağ arabirimi de statik IP kullanmak üzere ayarlanmış. Statik IP şu şekilde seçilir:

#### <a name="same-address-space"></a>Aynı adres alanı

Kaynak alt ağı ve hedef alt aynı adres alanı varsa, hedef IP aynı kaynak sanal makinenin Ağ arabiriminin IP ayarlanır. Aynı IP kullanılabilir durumda değilse, başka bir kullanılabilir IP hedef IP olarak ayarlanır.

#### <a name="different-address-space"></a>Farklı bir adres alanı

Kaynak alt ağı ve hedef alt farklı bir adres alanı varsa, hedef IP hedef alt ağdaki kullanılabilir herhangi bir IP'yi olarak ayarlanır.

Her ağ arabiriminin hedef IP, sanal makinenin işlem ve ağ ayarlarına giderek değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure Vm'lerini çoğaltma kılavuz ağ](site-recovery-azure-to-azure-networking-guidance.md).
