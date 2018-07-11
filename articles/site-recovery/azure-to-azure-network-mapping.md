---
title: Azure Site recovery'de iki Azure bölgeleri arasında sanal ağları eşleme | Microsoft Docs
description: Çoğaltma, yük devretme ve kurtarma sanal makinelerin ve fiziksel sunucuları Azure Site Recovery düzenler. Azure'a veya ikincil veri merkezine yük devretme hakkında bilgi edinin.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 7b7f9c079a1fc9d74fed4cc4d94d37f336ca5dc7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916749"
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Farklı Azure bölgelerindeki sanal ağları eşleme


Bu makalede, Azure sanal ağ birbiriyle farklı Azure bölgelerinde bulunan iki örneğini eşlemek açıklar. Ağ eşlemesini çoğaltılan bir sanal makinenin hedef Azure bölgeniz oluşturulduğunda, sanal makine de kaynak sanal makinenin sanal ağa eşlenen sanal ağda oluşturulmasını sağlar.  

## <a name="prerequisites"></a>Önkoşullar
Ağları Eşle önce siz oluşturduğunuzdan emin olun bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md) Kaynak bölgesi hem de hedef Azure bölgesi.

## <a name="map-virtual-networks"></a>Sanal ağları eşleme

Bir Azure bölgesinde (kaynak ağı) Azure sanal makineleri için başka bir bölgede (hedef ağı) bulunan bir sanal ağda bulunan bir Azure sanal ağı eşleme için şuraya gidin: **Site Recovery altyapısı**  >  **Ağ eşleme**. Ağ eşlemesi oluşturun.

![Ağ eşlemeleri penceresi - ağ eşlemesi oluşturma](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Aşağıdaki örnekte, Doğu Asya bölgesinde sanal makinenin çalışıyor. Sanal makine Güneydoğu Asya bölgeye çoğaltılır.

Ağ eşlemesi için Güneydoğu Asya bölgesi Doğu Asya bölgesi oluşturmak için kaynak ağ konumunu ve hedef ağ konumunu seçin. Sonra **Tamam**’ı seçin.

![Ağ eşleme pencere - eklemek kaynak ağı için kaynak ve hedef konumları seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Ağ eşlemesi, Doğu Asya bölgesi için Güneydoğu Asya bölgesi oluşturmak için önceki işlemi tekrarlayın.

![Ağ Eşleme bölmesi - ekleme kaynak ve hedef konumların hedef ağ seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Çoğaltmayı etkinleştirdiğinizde, bir ağ eşleme

Hiçbir ağ eşlemesi varsa bir sanal makine bir Azure bölgesinden başka bir bölgeye ilk kez çoğalttığınızda, çoğaltma işlemini ayarladığınız zaman hedef ağ ayarlayabilirsiniz. Bu ayarda bağlı olarak, Azure Site Recovery ağ eşlemeleri kaynak bölgesinden hedef bölge ve kaynak bölgeye hedef bölge oluşturur.   

![Ayarlar bölmesini yapılandırma - hedef konumu seçin](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Varsayılan olarak, Site Recovery, hedef bölgedeki kaynak ağına aynı olan bir ağ oluşturur. Site Recovery ağ ekleyerek oluşturur **-asr** kaynak ağı adı soneki olarak. Önceden oluşturulmuş bir ağ seçmek için Seç **Özelleştir**.

![Hedef ayarları bölmesi - kümesi hedef kaynak grubu adı ve hedef sanal ağ adı özelleştirme](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Ağ eşlemesi zaten oluştuysa, çoğaltmayı etkinleştirdiğinizde, hedef sanal ağ değiştiremezsiniz. Bu durumda, hedef sanal ağ değiştirmek için mevcut ağ eşlemesini değiştirin.  

![Hedef özelleştirme ayarları bölmesi - hedef kaynak grubu adını ayarla](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Ağ eşleme bölmesini değiştirme - mevcut bir hedef sanal ağ adını değiştirme](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Bir bölgeden bölgeye B bir ağ eşlemesini değiştirirseniz, aynı zamanda bölgeye A. B bölgeden ağ eşlemesini değiştirmek emin olun
>
>


## <a name="subnet-selection"></a>Alt ağ seçimi
Hedef sanal makinenin alt ağ, alt ağının kaynak sanal makinenin adına göre seçilir. Bu alt ağ, kaynak sanal makinenin aynı ada sahip bir alt ağ hedef ağdaki mevcutsa hedef sanal makine için ayarlanır. Alfabetik olarak ilk alt ağ, aynı ada sahip bir alt ağı hedef ağ mevcut değilse hedef alt ağ ayarlanır.

Alt ağ değiştirmek için Git **işlem ve ağ** sanal makine için ayarları.

![İşlem ve ağ işlem Özellikler penceresi](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP adresi

Aşağıdaki bölümlerde açıklandığı gibi hedef sanal makinenin her ağ arabirimi için IP adresi ayarlanır.

### <a name="dhcp"></a>DHCP
Kaynak sanal makinenin ağ arabiriminde DHCP kullanıyorsa, hedef sanal makinenin ağ arabiriminde de DHCP kullanacak şekilde ayarlanır.

### <a name="static-ip-address"></a>Statik IP adresi
Kaynak sanal makinenin ağ arabiriminde bir statik IP adresi kullanıyorsa, hedef sanal makinenin ağ arabiriminde de statik bir IP adresi kullanacak şekilde ayarlanır. Statik bir IP adresi nasıl belirlendiğini aşağıdaki bölümlerde açıklanmaktadır.

#### <a name="same-address-space"></a>Aynı adres alanı

Kaynak alt ağ ve hedef alt ağ aynı adres alanı varsa, kaynak sanal makinenin Ağ arabiriminin IP adresini hedef IP adresi olarak ayarlanır. Aynı IP adresi kullanılabilir değilse, bir sonraki kullanılabilir IP adresi hedef IP adresi olarak ayarlanır.

#### <a name="different-address-spaces"></a>Farklı bir adres alanları

Kaynak alt ağ ve hedef alt ağ farklı adres alanları varsa, hedef alt ağdaki bir sonraki kullanılabilir IP adresi hedef IP adresi olarak ayarlanır.

Her ağ arabirimi hedef IP değiştirmek için Git **işlem ve ağ** sanal makine için ayarları.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [Ağ Kılavuzu, Azure sanal makineleri çoğaltma](site-recovery-azure-to-azure-networking-guidance.md).
