---
title: 'Rota tabanlı VPN ağ geçidi oluşturmak: Azure portal | Microsoft Docs'
description: Hızlı rota tabanlı VPN Azure Portalı'nı kullanarak ağ geçidi oluşturma
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
ms.openlocfilehash: 550f655f6eac5a114636978255578eb3753e0d4b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir rota tabanlı VPN ağ geçidi oluşturma

Bu makalede Azure portalını kullanarak bir rota tabanlı Azure VPN ağ geçidi hızlı bir şekilde oluşturmanıza yardımcı olur.  Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Sanal ağlara bağlanmak için bir VPN ağ geçidi'ni de kullanabilirsiniz. 

Bu makaledeki adımları bir sanal ağ, bir alt ağ, bir ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantı oluşturabilirsiniz. Bu adımları bir Azure aboneliği gerektirir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="vnet"></a>Bir sanal ağ oluşturma

1. Bir tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve Azure hesabınızla oturum açın.
2. **Kaynak oluştur**’a tıklayın. **Markette ara** alanına 'sanal ağ' yazın. Döndürülen listeden **Sanal ağ**’ı bulun ve tıklayarak **Sanal Ağ** sayfasını açın.
3. Gelen sanal ağ sayfasının alt kısmındaki yakınında **dağıtım modeli seçin** listesinde, doğrulayın **Resource Manager** aşağı açılır listeden seçilen ve ardından **oluşturma**. Bu açılır **sanal ağ oluştur** sayfası.
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür. Aşağıdaki değerleri kullanın:

  - **Name**: TestVNet1
  - **Adres alanı**: 10.1.0.0/16
  - **Abonelik**: kullanmak istediğiniz listelenen abonelik olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
  - **Kaynak grubu**: TestRG1
  - **Konum**: Doğu ABD
  - **Alt ağ**: ön uç
  - **Adres aralığı**: 10.1.0.0/24

  ![Sanal ağ oluşturma sayfası](./media/create-routebased-vpn-gateway-portal/create-virtual-network.png "Sanal ağ oluşturma sayfası")
5. Değerleri girdikten sonra seçin **panoya Sabitle** Vnet'inizi Panoda bulun ve ardından kolaylaştırmak için **oluşturma**. ' I tıklattıktan sonra **oluşturma**, Panonuzda Vnet'inizin ilerleme durumunu yansıtır bir kutucuk görürsünüz. Sanal ağ oluşturulurken kutucuk değişir.

## <a name="gwsubnet"></a>Bir ağ geçidi alt ağı Ekle

Ağ geçidi alt ağı sanal ağ geçidi hizmetlerini kullanmak ayrılmış IP adresleri içerir. Bir ağ geçidi alt ağı oluşturun.

1. Portalda, sanal ağ geçidini oluşturmak istediğiniz sanal ağa gidin.
2. Sanal ağ sayfanızda tıklatın **alt ağlar** genişletmek için **VNet1 - alt ağlar** sayfası.
3. Tıklatın **+ ağ geçidi alt ağı** açmak için üst **alt ağ Ekle** sayfası.

  ![Ağ geçidi alt ağını ekleme](./media/create-routebased-vpn-gateway-portal/add-gateway-subnet.png "Ağ geçidi alt ağını ekleme")
4. **Adı** için alt ağınızı gerekli değeri 'GatewaySubnet' otomatik olarak doldurulur. Otomatik doldurulmuş ayarlamak **adres aralığı** aşağıdaki değerlerin eşleşmesi için değerler:

  **Adres aralığı (CIDR bloğu)**: 10.1.255.0/27

  ![Ağ geçidi alt ağını ekleme](./media/create-routebased-vpn-gateway-portal/gateway-subnet.png "Ağ geçidi alt ağını ekleme")
5. Ağ geçidi alt ağı oluşturmak için tıklatın **Tamam** sayfanın sonundaki.

## <a name="gwvalues"></a>Ağ Geçidi ayarlarını yapılandırma

1. Portal sayfasının sol tarafındadır tıklatın **+ kaynak oluşturma** ve basın türü 'Sanal ağ geçidi' arama kutusuna **Enter**. **Sonuçlar** alanında **Sanal Ağ Geçidi**’ni bulup tıklayın.
2. 'Sanal ağ geçidi' sayfasının en altında tıklatın **oluşturma** açmak için **sanal ağ geçidi Oluştur** sayfası.
3. **Sanal ağ geçidi oluştur** sayfasında sanal ağ geçidinize ait değerleri belirtin.

  - **Ad**: Vnet1GW
  - **Ağ geçidi türü**: VPN 
  - **VPN türü**: rota tabanlı
  - **SKU**: VpnGw1
  - **Konum**: Doğu ABD
  - **Sanal ağ**: tıklatın **sanal ağ/Seç bir sanal ağ** açmak için **sanal ağ seçin** sayfası. Seçin **VNet1**.

  ![Ağ Geçidi ayarlarını yapılandırma](./media/create-routebased-vpn-gateway-portal/configure-gateway.png "ağ geçidi ayarlarını yapılandırma")

## <a name="pip"></a>Bir ortak IP adresi oluşturun

Bir VPN ağ geçidi dinamik olarak ayrılan bir ortak IP adresi olmalıdır. Bir VPN ağ geçidi bağlantı oluşturduğunuzda, bu şirket içi Cihazınızı bağlanan IP adresidir.

1. Seçin **ilk IP yapılandırması oluşturma ağ geçidi IP yapılandırması** genel bir IP adresi istemek için.

  ![İlk IP yapılandırması](./media/create-routebased-vpn-gateway-portal/add-public-ip-address.png "ilk IP yapılandırması")
2. Üzerinde **Seç ortak IP sayfasına**, tıklatın **+ Yeni Oluştur** açmak için **ortak IP adresi oluştur** sayfası.
3. Ayarları ile aşağıdaki değerleri yapılandırın:

  - **Name**: **VNet1GWIP**
  - **SKU**: **temel**

  ![Genel IP oluşturma](./media/create-routebased-vpn-gateway-portal/public-ip-address-name.png "PIP oluşturma")
4. Tıklatın **Tamam** yaptığınız değişiklikleri kaydetmek için bu sayfanın sonundaki.

## <a name="creategw"></a>VPN ağ geçidi oluşturma

1. Ayarları doğrulayın **sanal ağ geçidi Oluştur** sayfası. Gerekirse değerlerini ayarlayın.

  ![VPN ağ geçidini Oluştur](./media/create-routebased-vpn-gateway-portal/create-vpn-gateway.png "oluşturma VPN ağ geçidi")
2. Tıklatın **oluşturma** sayfanın sonundaki.

Tıklattıktan sonra **oluşturma**, ayarları doğrulanır ve **dağıtma sanal ağ geçidi** döşeme panosunda görüntülenir. Bir VPN ağ geçidi 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

## <a name="viewgw"></a>VPN ağ geçidi görüntüleyin

1. Ağ geçidi oluşturulduktan sonra portalda VNet1'gidin. VPN ağ geçidi genel bakış sayfasında bağlı bir aygıt görüntülenir.

  ![Bağlı cihazları](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "bağlı cihazları")

2. Aygıt listesinde tıklatın **VNet1GW** daha fazla bilgi görüntülemek için.

  ![Görünüm VPN ağ geçidi](./media/create-routebased-vpn-gateway-portal/view-gateway.png "görünüm VPN ağ geçidi")

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturmayı tamamlandıktan sonra sanal ağınız ve başka bir VNet arasında bir bağlantı oluşturabilirsiniz. Veya, sanal ağ ve bir şirket içi konum arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Bir noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Başka bir VNet bağlantı oluşturun.](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)