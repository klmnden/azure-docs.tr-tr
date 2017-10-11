---
title: "ExpressRoute için bir sanal ağa bir sanal ağ geçidi eklemek: Portal: Azure | Microsoft Docs"
description: "Bu makalede önceden oluşturulmuş bir Resource Manager Vnet'i ExpressRoute için bir sanal ağ geçidine eklerken size yol gösterilir."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: cherylmc
ms.openlocfilehash: 2bd0cf8be87937044ad515a2c6f253b1711bb2bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-azure-portal"></a>Bir sanal ağ geçidi Azure portalını kullanarak ExpressRoute için yapılandırma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasik - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Bu makalede, önceden var olan bir VNet için bir sanal ağ geçidi eklemek için adım adım anlatılmaktadır. Bu makalede, eklemek, yeniden boyutlandırma ve önceden var olan bir VNet için bir sanal ağ (VNet) ağ geçidini kaldırmak için adım adım anlatılmaktadır. Bu yapılandırma için özellikle bir expressroute bağlantı yapılandırmasında kullanılacak Resource Manager dağıtım modeli kullanılarak oluşturulan sanal ağlar için adımlardır. Sanal ağ geçitleri ve ExpressRoute ağ geçidi yapılandırma ayarları hakkında daha fazla bilgi için bkz: [ExpressRoute için sanal ağ geçitleri hakkında](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Başlamadan önce

Bu görev için adımlar aşağıdaki yapılandırma başvuru listesinde değerlere dayalı bir sanal ağ kullanın. Biz bizim örnek adımlarda bu listeyi kullanın. Değerleri kendinizinkilerle değiştirerek bir başvuru olarak kullanılacak listesini kopyalayabilirsiniz.

**Yapılandırma başvuru listesi**

* Sanal ağ adı "TestVNet" =
* Sanal ağ adres alanı 192.168.0.0/16 =
* Alt ağ adı "Ön uç" = 
    * Alt ağ adres alanının "192.168.1.0/24" =
* Kaynak grubu "TestRG" =
* Konum, "Doğu ABD" =
* Ağ geçidi alt ağ adı: "GatewaySubnet gerekir her zaman adını bir ağ geçidi alt ağı" *GatewaySubnet*.
    * Ağ geçidi alt ağ adres alanının "192.168.200.0/26" =
* Ağ geçidi adı "ERGW" =
* Ağ geçidi IP adı "MyERGWVIP" =
* Ağ geçidi türü = "ExpressRoute" Bu tür bir ExpressRoute yapılandırma için gereklidir.
* Ağ geçidi genel IP adı "MyERGWVIP" =

Görüntüleyebileceğiniz bir [Video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) yapılandırmanıza başlamadan önce bu adımları.

## <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma

1. [Portal](http://portal.azure.com)’da, sanal ağ geçidini oluşturmak istediğiniz Resource Manager sanal ağına gidin.
2. VNet dikey pencerenizin **Ayarlar** bölümünde, Alt Ağlar dikey penceresini genişletmek için **Alt Ağlar**'a tıklayın.
3. **Alt ağlar** dikey penceresinde, **+Alt ağ**’a tıklayarak **Alt ağ ekle** dikey penceresini açın. 
   
    ![Ağ geçidi alt ağını ekleme](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "Ağ geçidi alt ağını ekleme")


4. Alt ağınız için **Ad** alanı otomatik olarak ‘GatewaySubnet’ değeriyle doldurulur. Alt ağın Azure tarafından ağ geçidi alt ağı olarak tanınması için bu değer gereklidir. Otomatik olarak doldurulmuş **Adres aralığı** değerlerini, yapılandırma gereksinimlerinize uyacak şekilde ayarlayın. / 27 veya daha büyük bir ağ geçidi alt ağı oluşturmanızı öneririz (/ 26, / 25 vb..). Ardından **Tamam** değerleri kaydetmek ve ağ geçidi alt ağı oluşturmak için.

    ![Alt ağı ekleme](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "Alt ağı ekleme")

## <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma

1. Portalda, sol taraftaki **+** simgesine tıklayın ve arama alanına ‘Sanal Ağ Geçidi’ yazın. Arama sonuçlarında **Sanal ağ geçidi** seçeneğini bulun ve girişe tıklayın. **Sanal ağ geçidi** dikey penceresinin alt kısmındaki **Oluştur**’a tıklayın. Bu işlem **Sanal ağ geçidi oluştur** dikey penceresini açar.
2. **Sanal ağ geçidi oluştur** dikey penceresinde, sanal ağ geçidinize ait değerleri girin.

    ![Sanal ağ geçidi oluştur dikey penceresinin alanları](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Sanal ağ geçidi oluştur dikey penceresinin alanları")
3. **Ad**: Ağ geçidinizi adlandırın. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
4. **Ağ geçidi türü**: seçin **ExpressRoute**.
5. **SKU**: Açılır listeden ağ geçidi SKU’sunu seçin.
6. **Konum**: **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Konum, sanal ağınızın bulunduğu bölgeye işaret etmiyorsa, 'Sanal ağ seçin' açılır menüsünde sanal ağ görüntülenmez.
7. Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. **Sanal ağ**'a tıklayarak **Sanal ağ seçin** dikey penceresini açın. VNet'i seçin. Sanal ağınızı görmüyorsanız **Konum** alanının, sanal ağınızın bulunduğu bölgeyi işaret ettiğinden emin olun.
9. Genel bir IP adresi seçin. **Genel IP adresi**'ne tıklayarak **Genel IP adresi seçin** dikey penceresini açın. **+Yeni Oluştur**'a tıklayarak **Genel IP adresi oluştur** dikey penceresini açın. Genel IP adresiniz için bir ad girin. Bu dikey pencere bir genel IP adresi nesnesi oluşturur ve daha sonra bu nesneye dinamik olarak bir genel IP adresi atanır. Bu dikey penceredeki değişiklikleri kaydetmek için **Tamam**’a tıklayın.
10. **Abonelik**: Doğru aboneliğin seçildiğini doğrulayın.
11. **Kaynak grubu**: Bu ayar, seçtiğiniz Sanal Ağ tarafından belirlenir.
12. Önceki ayarları belirttikten sonra **Konum**'u ayarlamayın.
13. Ayarları doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız dikey pencerenin altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz.
14. Ağ geçidi oluşturmaya başlamak için **Oluştur**’a tıklayın. Ayarlar doğrulanır ve ağ geçidi dağıtılır. Sanal ağ geçidi oluşturma tamamlamak 45 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
VNet ağ geçidini oluşturduktan sonra bir expressroute bağlantı hattı ağınıza bağlayabilirsiniz. Bkz: [bir expressroute bağlantı hattı için bir sanal ağ bağlantı](expressroute-howto-linkvnet-portal-resource-manager.md).