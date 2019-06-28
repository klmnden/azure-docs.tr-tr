---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 72e61a36b58c0bc666f3e19b71fb1abe842208f5
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188216"
---
1. Azure portal ve Seç'i açın **kaynak Oluştur**. **Yeni** sayfası açılır.

2. İçinde **arama Market alanı**, girin *sanal ağ geçidi*seçip **sanal ağ geçidi** arama listeden. 

3. Üzerinde **sanal ağ geçidi** sayfasında **Oluştur** açmak için **sanal ağ geçidi Oluştur** sayfası.

   ![Sanal ağ geçidi oluştur sayfasının alanları](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Sanal ağ geçidi oluştur sayfasının alanları")

4. Üzerinde **sanal ağ geçidi Oluştur** sayfasında, sanal ağ geçidinize ait değerleri girin:

   - **Ad**: Oluşturmakta olduğunuz ağ geçidi nesnesi için bir ad girin. Bu ad, ağ geçidi alt ağı adından farklıdır. 

   - **Ağ geçidi türü**: Seçin **VPN** VPN ağ geçitleri için. 

   - **VPN türü**: Yapılandırmanızla ilgili belirtilen VPN türünü seçin. Çoğu yapılandırma gerektiren bir **rota tabanlı** VPN türü.

   - **SKU**: Açılan listeden ağ geçidi SKU'sunu seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

      Yalnızca belirli **etkinleştir aktif / aktif modu** bir etkin-etkin ağ geçidi yapılandırması oluşturuyorsanız. Aksi takdirde bu ayarı seçmeyin.
  
   - **Konum**: Görmek için kaydırmanız gerekebilir **konumu**. Ayarlama **konumu** için sanal ağınızın bulunduğu konumu. Örneğin, **Batı ABD**. Konum, sanal ağınızın bulunduğu bölgeye ayarlamazsanız, bir sanal ağ'ı seçtiğinizde açılan listede görünmez.

   - **Sanal ağ**: Bu ağ geçidini eklemek istediğiniz sanal ağı seçin. Seçin **sanal ağ** açmak için **sanal ağ Seç** sayfasında ve sanal ağ'ı seçin. Vnet'inizi görmüyorsanız emin **konumu** alanını, sanal ağınızın bulunduğu bölgeye ayarlayın.

   - **Ağ geçidi alt ağ adres aralığı**: Bu ayar, yalnızca sanal ağınız için daha önce bir ağ geçidi alt ağı oluşturmadıysanız görürsünüz. Daha önce geçerli bir ağ geçidi alt ağı oluşturduysanız, bu ayar görünmez.

   - **Genel IP adresi**: Bu ayar, VPN ağ geçidi ile ilişkili genel IP adresi nesnesi belirtir. VPN ağ geçidi oluşturulduğunda genel IP adresi bu nesneye dinamik olarak atanır. VPN ağ geçidi şu anda yalnızca destekler *dinamik* genel IP adresi ayırma. Ancak, dinamik ayırma IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Yalnızca bir kez ortak IP adresi değişiklikleri olduğunda ağ geçidi silinip yeniden oluşturulması. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.
    
      - **Yeni oluştur**'u seçili bırakın.

      - Metin kutusu içinde genel IP adresiniz için bir ad girin.

   - **BGP ASN'sini Yapılandır**: Bu ayar seçili, yapılandırmanızı özellikle gerektirmediği sürece bırakın. Bu ayar gerektiriyorsa, ASN varsayılandır *65515*, değiştirebilirsiniz.
     
5. Ayarları doğrulayın ve seçin **Oluştur** VPN ağ geçidi oluşturmaya başlamak için. Ayarlar doğrulanır ve gördüğünüz **sanal ağ geçidini dağıtma** Panoda kutucuk. Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

6. Ağ geçidi oluşturduktan sonra portalda sanal ağı görüntüleyerek atanan IP adresi doğrulayın. Ağ geçidi bağlı bir cihaz gibi görüntülenir. Daha fazla bilgi görüntülemek için bağlı cihaza (sanal ağ geçidiniz) seçebilirsiniz.