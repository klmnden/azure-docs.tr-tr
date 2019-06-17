---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 940306f79aa48567e3da943fe752a6acdf206c27
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66157208"
---
Azure portalını kullanarak Resource Manager dağıtımında bir VNet oluşturmak için aşağıdaki adımları izleyin. Ekran görüntüleri örnek olarak verilmiştir. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Virtual Network’e Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Bu VNet’i şirket içi konuma bağlamak (P2S yapılandırması oluşturmaya ek olarak) istiyorsanız şirket içi ağ yöneticinizle bu ağ için özellikle kullanabileceğiniz bir IP adresi aralığı ayırma işlemini koordine etmeniz gerekir. VPN bağlantısının her iki tarafında bir yinelenen adres aralığı varsa, trafik beklediğiniz şekilde yönlendirilmez. Ayrıca, bu sanal ağı başka bir sanal ağa bağlamak isterseniz adres alanı diğer sanal ağ ile örtüşemez. Ağ yapılandırmanızı uygun şekilde planlamaya dikkat edin.
>
>

1. Tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. **+** öğesine tıklayın. **Market’te ara** alanına "Sanal Ağ" yazın. Döndürülen listeden **Sanal Ağ**’ı bulun ve tıklayarak **Sanal Ağ** sayfasını açın.

   ![Sanal Ağ kaynağı sayfasını bulma](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/newvnetportal700.png "Sanal Ağ kaynağı sayfasını bulma")
3. Sanal Ağ sayfasının en altına doğru, **Bir dağıtım modeli seçin** listesinden **Resource Manager**’ı seçip **Oluştur**’a tıklayın.

   ![Resource Manager’ı seçin](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Resource Manager’ı seçin")
4. **Sanal ağ oluştur** sayfasında sanal ağ ayarlarını yapılandırın. Alanları doldururken, alana girilen karakterler geçerliyse kırmızı ünlem işareti yeşil onay işaretine dönüşür. Otomatik olarak doldurulmuş değerler olabilir. Böyle değerler varsa, değerleri kendi değerlerinizle değiştirin. **Sanal ağ oluştur** sayfası aşağıdaki örneğe benzer şekilde görünür.

   ![Alan doğrulama](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/vnetp2s.png "Alan doğrulama")
5. **Ad**: Sanal ağınızın adını girin.
6. **Adres alanı**: Adres alanını girin. Eklenecek birden fazla adres alanınız varsa birinci adres alanınızı ekleyin. Sanal ağı oluşturduktan sonra başka adres alanları ekleyebilirsiniz.
7. **Abonelik**: Listelenen aboneliğin doğru olduğunu doğrulayın. Açılan listeyi kullanarak abonelikleri değiştirebilirsiniz.
8. **Kaynak grubu**: Mevcut bir kaynak grubunu seçin veya yeni kaynak grubunuz için bir ad yazarak yeni bir tane oluşturun. Yeni bir grup oluşturuyorsanız, planlanan yapılandırma değerlerinize göre kaynak grubunu adlandırın. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)’ı ziyaret edin.
9. **Konum**: Ağınızın konumunu seçin. Konum bu sanal ağa dağıttığınız kaynakların nerede olacağını belirler.
10. **Alt ağ**: Alt ağ adını ve alt ağ adres aralığını ekleyin. Sanal ağı oluşturduktan sonra başka alt ağlar ekleyebilirsiniz.
11. Sanal ağınızı panoda kolay bulmak istiyorsanız **Panoya sabitle**’yi seçin ve ardından **Oluştur**’a tıklayın.

    ![Panoya sabitleme](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "Panoya sabitleme")
12. **Oluştur**’a tıkladıktan sonra panonuzda sanal ağınızın ilerleme durumunu yansıtacak bir kutucuk göreceksiniz. Sanal ağ oluşturulurken kutucuk değişir.

    ![Sanal ağ oluşturma kutucuğu](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Creating virtual network tile")