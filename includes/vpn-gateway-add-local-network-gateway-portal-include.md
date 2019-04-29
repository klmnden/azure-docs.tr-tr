---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 12/19/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 03a4005da7794a989166f914e4ba4d03f93ae8b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60407785"
---
1. Portalda **+Kaynak oluştur**’a tıklayın.
2. Arama kutusuna **Yerel ağ geçidi** yazıp aramak için **Enter** tuşuna basın. Böylece sonuçların bir listesi döndürülür. **Yerel ağ geçidi**’ne ve sonra **Oluştur** düğmesine tıklayarak **Yerel ağ geçidi oluştur** sayfasını açın.

   ![Yerel ağ geçidi oluşturma](./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway.png "Yerel ağ geçidi oluşturma")

3. **Yerel ağ geçidi oluştur** sayfasında, yerel ağ geçidiniz için değerlerleri belirtin.

   - **Adı:** Yerel ağ geçidi nesneniz için bir ad belirtin.
   - **IP adresi:** Bu, Azure bağlanmak istediğiniz VPN cihazının genel IP adresidir. Geçerli bir genel IP adresi belirtin. IP adresini şu anda bilmiyorsanız örnekte gösterilen değerleri kullanabilirsiniz ancak geri dönüp yer tutucu IP adresinizi VPN cihazınızın genel IP adresiyle değiştirmeniz gerekir. Aksi halde Azure bağlantı kuramaz.
   - **Adres Alanı**, bu yerel ağın temsil ettiği ağa ilişkin adres aralıkları anlamına gelir. Birden fazla adres alanı aralığı ekleyebilirsiniz. Burada belirttiğiniz aralıkların, bağlanmak istediğiniz diğer ağların aralıklarıyla çakışmadığından emin olun. Azure, belirttiğiniz adres aralığını şirket içi VPN cihazının IP adresine yönlendirir. *Şirket içi sitenize bağlanmak istiyorsanız, burada, örnekte gösterilen değerleri değil, kendi değerlerinizi kullanın*.
   - **BGP ayarları yapılandırın:** Yalnızca BGP'yi yapılandırırken kullanın. Aksi takdirde, bu seçeneği işaretlemeyin.
   - **Abonelik:** Doğru aboneliğin gösterildiğini doğrulayın.
   - **Kaynak grubu:** Kullanmak istediğiniz kaynak grubunu seçin. Yeni bir kaynak grubu oluşturabilir veya önceden oluşturduğunuz birini seçebilirsiniz.
   - **Konum:** Bu nesnenin oluşturulacağı konumu seçin. VNet'inizin bulunduğu konumu seçebilirsiniz ancak bu zorunlu değildir.

4. Tüm değerleri belirttikten sonra yerel ağ geçidini oluşturmak için sayfanın alt tarafındaki **Oluştur** düğmesine tıklayın.
