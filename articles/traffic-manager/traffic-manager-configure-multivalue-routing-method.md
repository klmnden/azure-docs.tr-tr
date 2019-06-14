---
title: Azure Traffic Manager'da çok değerli trafik yönlendirme yöntemini yapılandırma
description: Bu makalede, trafiği yönlendirmek için A/AAAA uç noktaları Traffic Manager yapılandırma açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: allensu
ms.openlocfilehash: 5db8e2932a43a2d6c6cb8a99c4f32b37a4a5a3f8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050913"
---
# <a name="configure-multivalue-routing-method-in-traffic-manager"></a>Trafik Yöneticisi'nde çoklu değer yönlendirme yöntemini yapılandırma

Bu makalede, çoklu değer trafik yönlendirme yöntemini yapılandırma açıklar. **Birden çok değerli** trafiği yönlendirme yöntemini birden fazla sağlıklı bir uç nokta döndürmenizi sağlar ve istemcilerin başka bir DNS arama yapmak zorunda kalmadan yeniden denemek için daha fazla seçeneğe sahip olduğundan, uygulama güvenilirliğini artırmaya yardımcı olur. Çok değerli yönlendirme yalnızca IPv4 veya IPv6 adresleri kullanarak belirtilen tüm uç noktaları olan profiller için etkinleştirilir. Bu profil için bir sorgu alındığında, tüm sağlıklı uç noktalar belirtilen yapılandırılabilir maksimum dönüş sayısına göre döndürülür. 

>[!NOTE]
> Uç noktaları ekleme şu anda kullanarak IPv4 veya IPv6 adresleri yalnızca türündeki uç noktalar için desteklenen **dış** ve bu nedenle çok değerli yönlendirme de yalnızca bu uç noktaları için desteklenir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Traffic Manager profili için bir kaynak grubu oluşturun.
1. Azure portalında sol bölmesinde seçin **kaynak grupları**.
2. İçinde **kaynak grupları**, sayfanın üst kısmındaki seçin **Ekle**.
3. İçinde **kaynak grubu adı**, bir ad yazın *myResourceGroupTM1*. İçin **kaynak grubu konumu**seçin **Doğu ABD**ve ardından **Tamam**.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
En düşük gecikme süresine uç noktaya göndererek kullanıcı trafiği yönlendiren bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur**'u seçin.
2. İçinde **Traffic Manager profili oluştur**girin veya seçin, aşağıdaki bilgileri, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **Oluştur**:
    
    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ad                   | Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı ile sonuçlanması gerekir.                                   |
    | Yönlendirme yöntemi          | Seçin **birden çok değerli** yönlendirme yöntemi.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | Seçin *myResourceGroupTM1*. |
    | Location                | Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.                              |
   |        |           | 
  
   ![Traffic Manager profili oluşturma](./media/traffic-manager-multivalue-routing-method/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

İki IP adresi önceki adımda oluşturduğunuz birden çok değerli Traffic Manager profili dış uç noktalar olarak ekleyin.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Dış uç noktası                                   |
    | Ad           | myEndpoint1                                        |
    | Tam etki alanı adı (FQDN) veya IP           | Bu Traffic Manager profiline eklemek istediğiniz uç noktaya genel IP adresini yazın                         |
    |        |           |

4. 2 ve 3 adlı başka bir uç nokta ekleme *myEndpoint2*, için **tam etki alanı adı (FQDN) veya IP**, ikinci uç nokta genel IP adresini girin.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

   ![Traffic Manager uç noktası ekleme](./media/traffic-manager-multivalue-routing-method/add-endpoint.png)
 
## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- Daha fazla bilgi edinin [performans yönlendirme yöntemi](traffic-manager-configure-performance-routing-method.md)
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.


