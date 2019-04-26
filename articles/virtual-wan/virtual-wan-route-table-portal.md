---
title: Bir Azure sanal WAN sanal hub yönlendirme tablosu - Azure portalı oluşturma | Microsoft Docs
description: Portalı kullanarak bir ağ sanal gerecinin trafik faaliyetidir için rota tablosu sanal WAN sanal hub.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a route table using the portal.
ms.openlocfilehash: 2c8b3b4671fd14f9b10b8491861ae2c652f0188b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461663"
---
# <a name="create-a-virtual-wan-hub-route-table-for-nvas-azure-portal"></a>Nva'ları için bir sanal WAN hub yol tablosu oluşturun: Azure portal

Bu makalede trafiği ağ sanal Gereci (NVA) için bir hub'ından faaliyetidir gösterilmektedir.

![Sanal WAN diyagramı](./media/virtual-wan-route-table/vwanroute.png)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki ölçütleri karşıladığınızı doğrulayın:

*  Bir ağ sanal Gereci (NVA) var. Ağ sanal Gereci, Azure Marketi'nden bir sanal ağ içinde sağlanan genellikle kendi tercih ettiğiniz bir üçüncü taraf yazılımdır.

    * Özel bir IP adresi NVA ağ arabirimine atanmış olmalıdır.

    * NVA sanal hub'ı dağıtılmaz. Ayrı bir Vnet'te dağıtılması gerekir.

    *  Bir NVA VNet olabilir veya birçok sanal ağa bağlı. Bu makalede, bir 'dolaylı bağlı sanal ağ' NVA Vnet'e diyoruz. Bu sanal ağlar, VNet eşlemesi kullanarak, NVA sanal ağa bağlanabilir.
*  2 sanal ağ oluşturmuş oldunuz. Uç sanal ağları kullanılır.

    * Bu alıştırma için sanal ağ uç adres alanları şunlardır: VNet1: 10.0.2.0/24 ve VNet2: 10.0.3.0/24. Sanal ağ oluşturma hakkında bilgi gerekirse bkz [sanal ağ oluşturma](../virtual-network/quick-create-portal.md).

    * Hiçbir sanal ağ geçitleri herhangi birinde Vnet'ler emin olun.
    * Bu yapılandırma için bu sanal ağlar, ağ geçidi alt ağı gerektirmez.

## <a name="signin"></a>1. Oturum aç

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

## <a name="vwan"></a>2. Sanal WAN oluşturma

Sanal WAN oluşturun. Bu alıştırmanın amacı doğrultusunda, aşağıdaki değerleri kullanabilirsiniz:

* **Sanal WAN adı:** myVirtualWAN
* **Kaynak grubu:** testRG
* **Konum:** Batı ABD

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. Hub oluşturma

Hub'ı oluşturun. Bu alıştırmanın amacı doğrultusunda, aşağıdaki değerleri kullanabilirsiniz:

* **Konum:** Batı ABD
* **Ad:** westushub
* **Hub özel adres alanı:** 10.0.1.0/24

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="route"></a>4. Oluşturma ve bir hub yol tablosu uygulama

Hub, hub yol tablosu ile güncelleştirin. Bu alıştırmanın amacı doğrultusunda, aşağıdaki değerleri kullanabilirsiniz:

* **Dolaylı uç sanal ağ adres alanları:** (VNet1 ve vnet2'den) 10.0.2.0/24 ve 10.0.3.0/24
* **DMZ NVA ağ arabiriminin özel IP adresi:** 10.0.4.5

1. Sanal WAN'ınıza gidin.
2. Bir yol tablosu oluşturmak istediğiniz hub'ı tıklatın.
3. Tıklayın **...** ve ardından **düzenleme sanal hub**.
4. Üzerinde **düzenleme sanal hub** sayfasında, aşağı kaydırın ve onay kutusunu işaretleyin **Yönlendirme kullanımı tablo**.
5. İçinde **hedef ön eki ise** sütun, adres alanlarını ekleyin. İçinde **göndermek için sonraki atlama** sütun DMZ NVA ağ arabiriminin özel IP adresini ekleyin.
6. Tıklayın **Onayla** hub kaynak ile rota tablosu ayarları güncellenemedi.

## <a name="connections"></a>5. Sanal ağ bağlantıları oluşturma

Bağlantı hub'ına her dolaylı uç (VNet1 ve vnet2'den) sanal ağ oluşturun. Ardından, bir bağlantı hub'ına NVA sanal ağdan oluşturun.

 Bu adım için aşağıdaki değerleri kullanabilirsiniz:

| Sanal ağ adı| Bağlantı adı|
| --- | --- |
| VNet1 | testconnection1 |
| VNet2 | testconnection2 |
| NVAVNet | testconnection3 |

Aşağıdaki yordamı, bağlanmak istediğiniz her sanal ağ için yineleyin.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantıları**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Tıklayın **Tamam** bağlantı oluşturmak için.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.