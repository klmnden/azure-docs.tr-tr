---
title: 'Rota temelli VPN ağ geçidi oluşturun: Azure portalı | Microsoft Docs'
description: Azure portalını kullanarak rota temelli VPN Gateway oluşturma
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/18/2018
ms.author: cherylmc
ms.openlocfilehash: ddc42023bae3403e7778327a40316462c85222c0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60390079"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Azure portalını kullanarak rota temelli VPN ağ geçidi oluşturma

Bu makalede, Azure portalını kullanarak rota temelli Azure VPN ağ geçidi hızlı bir şekilde oluşturmanıza yardımcı olur.  Bir VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluşturulurken kullanılır. Vnet'leri bağlamak için bir VPN ağ geçidi'ni de kullanabilirsiniz. 

Bu makaledeki adımlarda, sanal ağ, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra ardından bağlantıları oluşturabilirsiniz. Bu adımlar, bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="vnet"></a>Sanal ağ oluşturma

1. Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.
2. **Kaynak oluştur**’a tıklayın. **Markette ara** alanına 'sanal ağ' yazın. Döndürülen listeden **Sanal ağ**’ı bulun ve tıklayarak **Sanal Ağ** sayfasını açın.
3. Gelen sanal ağ sayfasının en altına yakın **dağıtım modeli seçin** listesinde, doğrulayın **Resource Manager** açılan listeden seçildiğinden ve ardından **Oluştur**. Bu açılır **sanal ağ oluştur** sayfası.
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür. Aşağıdaki değerleri kullanın:

   - **Ad**: TestVNet1
   - **Adres alanı**: 10.1.0.0/16
   - **Abonelik**: Kullanmak istediğiniz aboneliği listede olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
   - **Kaynak grubu**: TestRG1
   - **Konum**: Doğu ABD
   - **Alt ağ**: Ön uç
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

   **Adres aralığı (CIDR bloğu)** : 10.1.255.0/27

   ![Ağ geçidi alt ağını ekleme](./media/create-routebased-vpn-gateway-portal/add-gateway-subnet.png "Ağ geçidi alt ağını ekleme")
5. Ağ geçidi alt ağı oluşturmak için tıklayın **Tamam** sayfanın alt kısmındaki.

## <a name="gwvalues"></a>Gateway ayarlarını yapılandırma

1. Portal sayfasının sol tarafında tıklayın **+ kaynak Oluştur** ve türü 'Sanal ağ geçidi' arama kutusuna basın **Enter**. **Sonuçlar** alanında **Sanal Ağ Geçidi**’ni bulup tıklayın.
2. 'Sanal ağ geçidi' sayfasının alt kısmında tıklayın **Oluştur** açmak için **sanal ağ geçidi Oluştur** sayfası.
3. **Sanal ağ geçidi oluştur** sayfasında sanal ağ geçidinize ait değerleri belirtin.

   - **Ad**: Vnet1GW
   - **Ağ geçidi türü**: VPN 
   - **VPN türü**: Rota tabanlı
   - **SKU**: VpnGw1
   - **Konum**: Doğu ABD
   - **Sanal ağ**: Tıklayın **sanal ağ/sanal ağ Seç** açmak için **bir sanal ağ seçin** sayfası. Seçin **VNet1**.
   - **Genel IP adresi**: Bu ayar, VPN ağ geçidi ile ilişkilendirilen genel bir IP adresi nesnesi belirtir. VPN ağ geçidi oluşturulduğunda genel IP adresi bu nesneye dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

     - **Yeni oluştur**'u seçili bırakın.
     - Metin kutusunda genel IP adresiniz için bir **Ad** yazın. Bu alıştırma için kullanmak **Vnet1gwıp**.<br>

     ![Gateway ayarlarını yapılandırma](./media/create-routebased-vpn-gateway-portal/gw.png "gateway ayarlarını yapılandırma")

## <a name="creategw"></a>VPN ağ geçidi oluşturma

1. Ayarları doğrulayın **sanal ağ geçidi Oluştur** sayfası. Gerekirse değerlerini ayarlayın.
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
