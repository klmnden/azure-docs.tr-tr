---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9ab0dfac298ab7246935649010100c14dbe9c76a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
1. Portalda **+Kaynak oluştur**’a tıklayın.
2. Arama kutusuna **Yerel ağ geçidi** yazıp aramak için **Enter** tuşuna basın. Böylece sonuçların bir listesi döndürülür. **Yerel ağ geçidi**’ne ve sonra **Oluştur** düğmesine tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

  ![Yerel ağ geçidi oluşturma](./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway.png "Yerel ağ geçidi oluşturma")

3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

  - **Ad:** Yerel ağ geçidi nesneniz için bir ad belirtin.
  - **IP adresi:** Bu değer, Azure'ın bağlanmasını istediğiniz VPN cihazının genel IP adresidir. Geçerli bir genel IP adresi belirtin. IP adresi, NAT'nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir. IP adresini şu anda bilmiyorsanız örnekte gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
  - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. *Şirket içi sitenize bağlanmak istiyorsanız, burada, örnekte gösterilen değerleri değil, kendi değerlerinizi kullanın*.
  - **BGP ayarları yapılandır:** Yalnızca BGP’yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
  - **Abonelik:** Doğru aboneliğin gösterildiğinden emin olun.
  - **Kaynak Grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
  - **Konum:** Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

4. Tüm değerleri belirttikten sonra yerel ağ geçidini oluşturmak için sayfanın alt tarafındaki **Oluştur** düğmesine tıklayın.