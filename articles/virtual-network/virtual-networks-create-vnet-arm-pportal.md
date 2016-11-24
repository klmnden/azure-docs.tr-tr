---
title: "Azure portalını kullanarak bir sanal ağ oluşturma | Microsoft Belgeleri"
description: "Azure portalını kullanarak bir sanal ağ oluşturmayı öğrenin | Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/8/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 6e96471c4f61e1ebe15c23f87ac646001d8e30ee
ms.openlocfilehash: d68f6a7e935f530630ee33f48cfad1b9e01e66a8


---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal ağ oluşturma

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure'un iki dağıtım modeli vardır: Azure Resource Manager ve klasik. Microsoft, kaynakların Resource Manager dağıtım modeliyle oluşturulmasını önerir. İki model arasındaki farkları öğrenmek için [Azure dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesini okuyun.
 
Bu makalede, Azure portalını kullanarak Resource Manager dağıtım modelinde bir sanal ağ oluşturma adımları açıklanmaktadır. Resource Manager’da farklı araçlar kullanarak da sanal ağ kurabilir veya aşağıdaki listeden farklı bir seçenek belirleyerek klasik dağıtım modeliyle sanal ağ oluşturabilirsiniz:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [CLI](virtual-networks-create-vnet-arm-cli.md)
- [Şablon](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (Klasik)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (Klasik)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (Klasik)](virtual-networks-create-vnet-classic-cli.md)


[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Azure portalını kullanarak sanal ağ oluşturmak için aşağıdaki adımları uygulayın:

1. Tarayıcıdan http://portal.azure.com adresine gidin ve gerekiyorsa Azure hesabınıza giriş yapın.
2. Aşağıdaki resimde gösterildiği şekilde **Yeni** > **Ağ** > **Sanal ağ** yolunu izleyin:

    ![Yeni Sanal Ağ](./media/virtual-network-create-vnet-arm-pportal/1.png)

3. Açılan **Sanal ağ** dikey penceresinde *Resource Manager*’ın seçili olduğundan emin olun ve **Oluştur**’a tıklayın (aşağıdaki resme bakın):

    ![Sanal Ağ](./media/virtual-network-create-vnet-arm-pportal/2.png)
    
4. Açılan **Sanal ağ oluştur** dikey penceresinde *TestVNet* bilgisini **Ad** alanına, *192.168.0.0/16* adresini **Adres alanı** satırına, *FrontEnd* bilgisini **Alt ağ adı** alanına, *192.168.1.0/24* bilgisini **Alt ağ adres aralığı** alanına, *TestRG* girişini **Kaynak grubu** alanına yazın, **Abonelik** ve **Konum** seçin, ardından **Oluştur** düğmesine tıklayın (aşağıdaki resme bakın):

    ![Sanal ağ oluşturma](./media/virtual-network-create-vnet-arm-pportal/3.png)

    Alternatif olarak var olan bir kaynak grubunu seçebilirsiniz. Kaynak grupları hakkında daha fazla bilgi için [Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups) makalesini okuyun. Farklı bir konum da seçebilirsiniz. Azure konumları ve bölgeleri hakkında daha fazla bilgi için [Azure bölgeleri](https://azure.microsoft.com/regions) makalesini okuyun.

5. Portaldan sanal ağ oluştururken yalnızca bir alt ağ oluşturabilirsiniz. Bu senaryoda sanal ağ oluşturulduktan sonra ikinci bir alt ağ oluşturulması gerekmektedir. İkinci ağı oluşturmak için **Tüm kaynaklar**’a ve ardından **Tüm kaynaklar** dikey penceresinden **TestVNet**’e tıklayın (aşağıdaki resme bakın):

    ![Sanal ağ oluşturuldu](./media/virtual-network-create-vnet-arm-pportal/4.png)

6. Açılan **TestVNet** dikey penceresinde **Alt ağ**’a ve ardından **+Alt ağ**’a tıklayıp *BackEnd* bilgisini **Ad** alanına, *192.168.2.0/24* bilgisini **Adres aralığı** alanına (**Alt ağ ekle** dikey penceresinde) girin ve **Tamam**’a tıklayın (aşağıdaki resme bakın):

    ![Alt ağ ekleme](./media/virtual-network-create-vnet-arm-pportal/5.png)

7. Aşağıdaki resimde gösterilen iki alt ağ listelenir:
    
    ![VNet’te alt ağlar listesi](./media/virtual-network-create-vnet-arm-pportal/6.png)

Bu makalede, test amaçlı olarak iki alt ağa sahip bir sanal ağ oluşturma adımları açıklanmıştır. Üretim kullanımı için bir sanal ağ oluşturmadan önce [Sanal ağa genel bakış](virtual-networks-overview.md) ve [Sanal ağ planlama ve tasarım](virtual-network-vnet-plan-design-arm.md) makalelerini okuyarak sanal ağları ve tüm ayarları tam olarak kavramanızı öneririz. 

## <a name="next-steps"></a>Sonraki adımlar

Nasıl bağlayacağınızı öğrenin:

- Sanal ağ ile sanal makine (VM) bağlantısı için [Windows VM oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md) veya [Linux VM oluşturma](../virtual-machines/virtual-machines-linux-quick-create-portal.md) makalelerini okuyun. Makalelerde yer alan adımlarda sanal ağ ve alt ağ oluşturmak yerine var olan sanal ağı ve alt ağı seçerek VM bağlantısı yapabilirsiniz.
- Bir sanal ağı diğer sanal ağlara bağlamak için [Sanal ağları bağlama](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesini okuyun.
- Bir sanal ağı şirket içi ağa bağlamak için siteden siteye sanal özel ağ (VPN) veya ExpressRoute devresi kullanın. Nasıl yapacağınızı öğrenmek için [Siteden siteye VPN kullanarak sanal ağ ile şirket içi ağ arasında bağlantı kurma](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) ve [Bir sanal ağı ExpressRoute devresine bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md) makalelerini okuyun.


<!--HONumber=Nov16_HO3-->


