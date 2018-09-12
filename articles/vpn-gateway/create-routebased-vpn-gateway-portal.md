---
title: 'Rota tabanlı VPN gateway oluşturma: Azure portal | Microsoft Docs'
description: Bir rota tabanlı VPN Gateway Azure portalını kullanarak hızlıca oluşturun
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2018
ms.author: cherylmc
ms.openlocfilehash: fe05ab36f971105cf72342b8df5e2a82de7fc2b8
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391802"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Azure portalını kullanarak rota temelli VPN ağ geçidi oluşturma

Bu makalede, Azure portalını kullanarak rota temelli Azure VPN ağ geçidi hızlı bir şekilde oluşturmanıza yardımcı olur.  Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Vnet'leri bağlamak için bir VPN ağ geçidi'ni de kullanabilirsiniz. 

Bu makaledeki adımlarda, sanal ağ, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantıları oluşturabilirsiniz. Bu adımlar, bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="vnet"></a>Sanal ağ oluşturma

1. Bir tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve Azure hesabınızla oturum açın.
2. **Kaynak oluştur**’a tıklayın. **Markette ara** alanına 'sanal ağ' yazın. Döndürülen listeden **Sanal ağ**’ı bulun ve tıklayarak **Sanal Ağ** sayfasını açın.
3. Gelen sanal ağ sayfasının en altına yakın **dağıtım modeli seçin** listesinde, doğrulayın **Resource Manager** açılan listeden seçildiğinden ve ardından **Oluştur**. Bu açılır **sanal ağ oluştur** sayfası.
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür. Aşağıdaki değerleri kullanın:

  - **Ad**: TestVNet1
  - **Adres alanı**: 10.1.0.0/16
  - **Abonelik**: kullanmak istediğiniz aboneliği listede olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
  - **Kaynak grubu**: TestRG1
  - **Konum**: Doğu ABD
  - **Alt ağ**: ön uç
  - **Adres aralığı**: 10.1.0.0/24

  ![Sanal ağ oluşturma sayfası](./media/create-routebased-vpn-gateway-portal/create-virtual-network.png "Sanal ağ oluşturma sayfası")
5. Değerleri girdikten sonra seçin **panoya Sabitle** Vnet'inizi Panoda bulun ve ardından daha kolay hale getirmek için **Oluştur**. ' I tıklattıktan sonra **Oluştur**, Panonuzda, sanal ağınızın ilerleme durumunu yansıtan bir kutucuk görürsünüz. Sanal ağ oluşturulurken kutucuk değişir.

## <a name="gwsubnet"></a>Bir ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Bir ağ geçidi alt ağı oluşturun.

1. Portalda, sanal ağ geçidini oluşturmak istediğiniz sanal ağa gidin.
2. Sanal ağ sayfanızda tıklayın **alt ağlar** genişletmek için **VNet1 - alt ağlar** sayfası.
3. Tıklayın **+ ağ geçidi alt ağı** açmak için üstteki **alt ağ Ekle** sayfası.

  ![Ağ geçidi alt ağını ekleme](./media/create-routebased-vpn-gateway-portal/gateway-subnet.png "Ağ geçidi alt ağını ekleme")
4. **Adı** alt ağınızın gereken 'GatewaySubnet' değeriyle otomatik olarak doldurulur. Otomatik olarak doldurulmuş ayarlamak **adres aralığı** değerleri aşağıdaki değerleri eşleşecek şekilde:

  **Adres aralığı (CIDR bloğu)**: 10.1.255.0/27

  ![Ağ geçidi alt ağını ekleme](./media/create-routebased-vpn-gateway-portal/add-gateway-subnet.png "Ağ geçidi alt ağını ekleme")
5. Ağ geçidi alt ağı oluşturmak için tıklayın **Tamam** sayfanın alt kısmındaki.

## <a name="gwvalues"></a>Gateway ayarlarını yapılandırma

1. Portal sayfasının sol tarafında tıklayın **+ kaynak Oluştur** ve türü 'Sanal ağ geçidi' arama kutusuna basın **Enter**. **Sonuçlar** alanında **Sanal Ağ Geçidi**’ni bulup tıklayın.
2. 'Sanal ağ geçidi' sayfasının alt kısmında tıklayın **Oluştur** açmak için **sanal ağ geçidi Oluştur** sayfası.
3. **Sanal ağ geçidi oluştur** sayfasında sanal ağ geçidinize ait değerleri belirtin.

  - **Ad**: Vnet1GW
  - **Ağ geçidi türü**: VPN 
  - **VPN türü**: rota tabanlı
  - **SKU**: VpnGw1
  - **Konum**: Doğu ABD
  - **Sanal ağ**: tıklayın **sanal ağ/sanal ağ Seç** açmak için **bir sanal ağ seçin** sayfası. Seçin **VNet1**.

  ![Gateway ayarlarını yapılandırma](./media/create-routebased-vpn-gateway-portal/configure-gateway.png "gateway ayarlarını yapılandırma")

## <a name="pip"></a>Genel IP adresi oluşturma

Bir VPN ağ geçidi, dinamik olarak ayrılan bir genel IP adresi olmalıdır. Bir VPN ağ geçidine bir bağlantı oluşturduğunuzda, bu, şirket içi cihaz için bağlanan IP adresidir.

1. Seçin **ilk IP yapılandırması ağ geçidi IP yapılandırması oluştur** genel bir IP adresi istemek için.

  ![İlk IP yapılandırması](./media/create-routebased-vpn-gateway-portal/add-public-ip-address.png "ilk IP yapılandırması")
2. Üzerinde **Seç genel IP sayfasına**, tıklayın **+ Yeni Oluştur** açmak için **genel IP adresi oluşturma** sayfası.
3. Ayarları aşağıdaki değerleri yapılandırın:

  - **Adı**: **Vnet1gwıp**
  - **SKU**: **temel**

  ![Genel IP oluşturma](./media/create-routebased-vpn-gateway-portal/public-ip-address-name.png "PIP oluşturma")
4. Tıklayın **Tamam** yaptığınız değişiklikleri kaydetmek için bu sayfanın alt kısmındaki.

## <a name="creategw"></a>VPN ağ geçidi oluşturma

1. Ayarları doğrulayın **sanal ağ geçidi Oluştur** sayfası. Gerekirse değerlerini ayarlayın.

  ![VPN ağ geçidi Oluştur](./media/create-routebased-vpn-gateway-portal/create-vpn-gateway.png "oluşturma VPN ağ geçidi")
2. Tıklayın **Oluştur** sayfanın alt kısmındaki.

Tıkladıktan sonra **Oluştur**, ayarlar doğrulanır ve **sanal ağ geçidini dağıtma** kutucuk Panoda görünür. Bir VPN ağ geçidi 45 dakikaya kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

## <a name="viewgw"></a>VPN ağ geçidi görüntüleyin

1. Ağ geçidi oluşturulduktan sonra portalda VNet1'e gidin. VPN ağ geçidi bağlı bir cihaz gibi genel bakış sayfasında görüntülenir.

  ![Bağlı cihazlar](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "bağlı cihazlar")

2. Cihaz listesinde **VNet1GW** daha fazla bilgi görüntülemek için.

  ![Görünüm VPN ağ geçidi](./media/create-routebased-vpn-gateway-portal/view-gateway.png "görünümü VPN ağ geçidi")

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturma tamamlandıktan sonra başka bir sanal ağ ve sanal ağınız arasında bir bağlantı oluşturabilirsiniz. Veya sanal ağınız ile şirket içi konumunuz arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Başka bir sanal ağa bağlantı oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
