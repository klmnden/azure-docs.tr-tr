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
ms.openlocfilehash: 0270fa207b782d2d89f2408a380b766f58fa05cb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
1. Portalda **Tüm kaynaklar** menüsündeki **+Ekle**’ye tıklayın. 
2. **Her Şey** sayfasının arama kutusuna **Yerel ağ geçidi** yazıp aramak için tıklayın. Bunun yapılması bir liste döndürür. **Yerel ağ geçidi**’ne tıklayarak sayfayı açın, ardından **Oluştur**’a tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

  ![yerel ağ geçidi oluşturma](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)

3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

  - **Ad:** Yerel ağ geçidi nesneniz için bir ad belirtin.
  - **IP adresi:** Bu değer, Azure'ın bağlanmasını istediğiniz VPN cihazının genel IP adresidir. Geçerli bir genel IP adresi belirtin. IP adresi, NAT'nin ardında olamaz ve Azure tarafından erişilebilir olması gerekir. IP adresini şu anda bilmiyorsanız ekran görüntüsünde gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
  - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. *Burada, ekran görüntüsünde gösterilen değerler yerine kendi değerlerinizi kullanın*.
  - **BGP ayarları yapılandır:** Yalnızca BGP’yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
  - **Abonelik:** Doğru aboneliğin gösterildiğinden emin olun.
  - **Kaynak Grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
  - **Konum:** Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

4. Tüm değerleri belirttikten sonra yerel ağ geçidini oluşturmak için sayfanın alt tarafındaki **Oluştur** düğmesine tıklayın.