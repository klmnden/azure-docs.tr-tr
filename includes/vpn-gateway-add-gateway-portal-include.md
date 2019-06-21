---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/24/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 35f987f26ce47c19e3d5eb1ca5d2bb32d0c7ad1b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188228"
---
1. Portal sayfasının sol tarafına **+** simgesine tıklayın ve arama alanına ‘Sanal Ağ Geçidi’ yazın. **Sonuçlar** alanında **Sanal Ağ Geçidi**’ni bulup tıklayın.
2. ‘Sanal ağ geçidi’ sayfasının alt tarafındaki **Oluştur**'a tıklayın. Bu işlem **Sanal ağ geçidi oluşturma** sayfasını açar.
3. **Sanal ağ geçidi oluştur** sayfasında sanal ağ geçidinize ait değerleri belirtin.

    ![Sanal ağ geçidi oluştur sayfasının alanları](./media/vpn-gateway-add-gateway-portal-include/create-gateway.png "Yeni ağ geçidi")

   - **Ad**: Geçidinizi adlandırın. Bir ağ geçidi alt ağını adlandırmayla aynı değildir. Bu ad, oluşturduğunuz ağ geçidi nesnesinin adıdır.
   - **Ağ geçidi türü**: Seçin **VPN**. VPN ağ geçitleri, **VPN** sanal ağ geçidi türünü kullanır. 
   - **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma, Yol Tabanlı bir VPN türü gerektirir.
   - **SKU**: Açılan listeden ağ geçidi SKU'sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

     **Etkin-etkin modu etkinleştir** seçeneğini, yalnızca etkin-etkin ağ geçidi yapılandırması oluştururken kullanın. Aksi takdirde bu ayarı seçmeyin.
   - **Konum**: Konum görmek için kaydırmanız gerekebilir. **Konum** alanını, sanal ağınızın bulunduğu konumu işaret edecek şekilde ayarlayın. Örneğin, Batı ABD. Konum, sanal ağınızın bulunduğu bölgeye işaret etmiyorsa, sonraki adımda bir sanal ağ seçtiğinizde bu seçim açılır listede görünmez.
   - **Sanal ağ**: Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. **Sanal ağ**'a tıklayarak ‘Sanal ağ seçin’ sayfasını açın. VNet'i seçin. VNet'inizi görmüyorsanız Konum alanının, sanal ağınızın bulunduğu bölgeyi işaret ettiğinden emin olun.
   - **Ağ geçidi alt ağ adres aralığı**: Bu ayar, yalnızca bir ağ geçidi alt ağı, sanal ağınız için daha önce oluşturmadıysanız görürsünüz. Daha önce geçerli bir ağ geçidi alt ağı oluşturduysanız, bu ayar görünmez.
   - **Genel IP adresi**: Bu ayar, VPN ağ geçidi ile ilişkilendirilen genel bir IP adresi nesnesi belirtir. VPN ağ geçidi oluşturulduğunda genel IP adresi bu nesneye dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

     - **Yeni oluştur**'u seçili bırakın.
     - Metin kutusunda genel IP adresiniz için bir **Ad** yazın.

4. Yapılandırmanız için **BGP ASN yapılandır** ayarı özellikle gerekli değilse bu ayarı işaretlemeyin. Bu ayar gerekliyse varsayılan ASN değeri 65515’tir ancak bu değiştirilebilir.
5. Ayarları doğrulayın. Ağ geçidinizin panoda görünmesini istiyorsanız sayfanın en altında yer alan **Panoya sabitle** seçeneğini belirleyebilirsiniz. 
6. VPN ağ geçidi oluşturmaya başlamak için **Oluştur**'a tıklayın. Ayarlar doğrulandıktan sonra, panoda "Sanal ağ geçidi dağıtma" kutucuğunu görürsünüz. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.
   ağ geçidi.

Ağ geçidi oluşturulduktan sonra, portalda sanal ağa bakarak bu ağ geçidine atanmış IP adresini görüntüleyin. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı cihaza (sanal ağ geçidiniz) tıklayabilirsiniz.