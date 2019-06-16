---
title: Azure Application Gateway - Portal App service web apps gibi çok kiracılı uygulamalara yönelik trafiği yönetme
description: Bu makalede, Azure App service web Apps'e mevcut veya yeni bir application Gateway, arka uç havuzu üyeleri olarak yapılandırma hakkında yönergeler sağlanır.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/11/2019
ms.author: absha
ms.openlocfilehash: 4dae04c14f9132c54dcc0575ccb2841a4742a626
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831222"
---
# <a name="configure-app-service-with-application-gateway"></a>Application Gateway ile App Service'ı yapılandırma

Uygulama ağ geçidi arka uç havuzu üyesi olarak bir Azure App service webapp veya diğer çok kiracılı hizmetlere sahip olmanızı sağlar. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> - Arka uç havuzu oluşturun ve App Service ekleyin
> - Etkin "Konak adı seçin" anahtarlarla HTTP ayarları ve özel araştırma oluşturma

## <a name="prerequisites"></a>Önkoşullar

- Uygulama ağ geçidi: Mevcut uygulama ağ geçidine sahip değilseniz, bkz. nasıl [bir uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
- Uygulama Hizmeti: Mevcut bir App service yoksa bkz [App service belgeleri](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Uygulama hizmet arka uç havuzu Ekle

1. Azure portalında, uygulama ağ geçidi yapılandırma görünümünü açın.

2. Altında **arka uç havuzları**, tıklayarak **Ekle** yeni bir arka uç havuzu oluşturmak için.

3. Arka uç havuzu için uygun bir ad sağlayın. 

4. Altında **hedefleri**üzerindeki açılır menüye tıklayın ve seçin **uygulama hizmetleri** seçeneği olarak.

5. Bir açılan hemen altındaki **hedefleri** açılan görünür olan App Service'leriniz listesini içerir. Bu açılan listeden, Ekle ve arka uç havuzu üyesi olarak eklemek istediğiniz App Service'ı seçin.

   ![App service arka ucu](./media/configure-web-app-portal/backendpool.png)

## <a name="create-http-settings-for-app-service"></a>App service için HTTP ayarları oluşturma

1. Altında **HTTP ayarları**, tıklayın **Ekle** yeni bir HTTP ayarı oluşturmak için.

2. HTTP ayarı için bir ad girin ve etkinleştirebilir veya tanımlama bilgisi tabanlı benzeşim ihtiyacınıza göre devre dışı bırakın.

3. Kullanım Örneğinize ilişkin olarak HTTP veya HTTPS protokolünü seçin. 

4. İçin kutuyu işaretleyin **kullanılmak üzere App Service** ve kapatma **arka uç adresi çekme ana bilgisayar adına sahip bir yoklama oluşturma** ve **arka uç adresi çekme konak adı** seçenekleri. Bu seçenek, aynı zamanda bir araştırma etkin anahtarı ile otomatik olarak oluşturun ve bu HTTP ayarıyla ilişkilendirmek.

5. Tıklayın **Tamam** HTTP ayarı oluşturmak için.

   ![HTTP setting1](./media/configure-web-app-portal/http-setting1.png)

   ![HTTP-setting2](./media/configure-web-app-portal/http-setting2.png)

## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Dinleyiciyi ve arka uç havuzu HTTP ayarı bağlamak için kural oluşturma

1. Altında **kuralları**, tıklayın **temel** yeni bir temel kuralı oluşturun.

2. Uygun bir ad sağlayın ve App service için gelen istekleri kabul eden dinleyici seçin.

3. İçinde **arka uç havuzu** açılır listesinde, yukarıda oluşturduğunuz arka uç havuzu seçin.

4. İçinde **HTTP ayarı** açılır listesinde, yukarıda oluşturulan ayarı HTTP seçin.

5. Tıklayın **Tamam** bu kuralı kaydedin.

   ![Kural](./media/configure-web-app-portal/rule.png)

## <a name="restrict-access"></a>Erişimi kısıtlama

Bu örneklerde dağıtılan web uygulamaları doğrudan Internet'ten erişilebilen genel IP adresleri kullanın. Bu, yeni bir özellik hakkında öğrenirken sorun giderme ve yeni şeyler çalışırken yardımcı olur. Ancak, bir özellik üretim ortamında dağıtmak istiyorsanız, daha fazla kısıtlama eklemek isteyeceksiniz.

Web uygulamalarınız için erişimi kısıtlayabilir tek bir yolu [Azure App Service statik IP kısıtlamaları](../app-service/app-service-ip-restrictions.md). Örneğin, yalnızca uygulama ağ geçidinden trafiği alır, böylece web uygulaması kısıtlayabilirsiniz. Uygulama ağ geçidi VIP erişimi olan tek adres olarak listelemek için app service IP kısıtlama özelliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

App service ve diğer uygulama ağ geçidi ile çok kiracılı desteği hakkında daha fazla bilgi için bkz: [application gateway ile çok kiracılı hizmet desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

App service'inizin yanıttan App service'nın URL'ye yeniden yönlendirerek durumda görmek için nasıl [yeniden yönlendirme için App service'nın URL sorun giderme](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).
