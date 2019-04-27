---
title: Azure Application Gateway özel hata sayfaları oluşturma
description: Bu makalede Application Gateway özel hata sayfaları oluşturma işlemini gösterir.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
origin.date: 02/14/2019
ms.date: 02/26/2019
ms.author: v-junlch
ms.openlocfilehash: abfe33ff679bef125d9bf5b78e1790a1a4c64863
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832054"
---
# <a name="create-application-gateway-custom-error-pages"></a>Application Gateway özel hata sayfaları oluşturma

Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

Örneğin, web uygulamanızı ulaşılabilir değilse, kendi Bakım Sayfası tanımlayabilirsiniz. Veya bir web uygulaması için kötü amaçlı bir istek gönderirse, yetkisiz erişim sayfası oluşturabilirsiniz.

Özel hata sayfaları için aşağıdaki iki senaryoda desteklenir:

- **Bakım Sayfası** -502 hatalı ağ geçidi sayfası yerine bu özel hata sayfası gönderilir. Application Gateway, trafiği yönlendirmek için arka uç olduğunda gösterilir. Örneğin, Bakım veya beklenmedik bir sorun arka uç havuzuna erişimi olduğunda efektler zamanlanan olduğunda.
- **Yetkisiz erişim sayfası** -bir 403 yetkisiz erişim sayfası yerine bu özel hata sayfası gönderilir. Application Gateway WAF, kötü amaçlı trafik algılar ve onu engellediğinde gösterilir.

Daha sonra bir hata arka uç sunuculardan kaynaklanıyorsa, çağırana değiştirilmemiş arka geçirilir. Özel hata sayfası görüntülenmez. Bir isteği arka uç ulaştığında uygulama ağ geçidi özel hata sayfası görüntüleyebilirsiniz.

Özel hata sayfaları, genel düzeyde ve dinleyici düzeyinde tanımlanabilir:

- **Genel düzeyde** -trafik, application gateway üzerinde dağıtılan tüm web uygulamaları için hata sayfasının uygulandığı.
- **Dinleyici düzeyi** -hata sayfası, bu Dinleyicide alınan trafik uygulanır.
- **Her ikisi de** -dinleyici düzeyinde tanımlanan özel hata sayfası ayarlanan genel düzeyde geçersiz kılar.

Özel hata sayfası oluşturmak için şunlara sahip olmalısınız:
- HTTP yanıtı durum kodu.
- hata sayfası için karşılık gelen konum. 
- Genel olarak erişilebilen Azure depolama blobu konumu için.
- bir *.htm veya *.html uzantı türü. 

Hata sayfasının boyutu 1 MB'tan az olmalıdır. Hata sayfasında bağlantılı görüntü varsa, genel olarak erişilebilir mutlak URL'ler veya base64 ile kodlanmış görüntü satır özel hata sayfası içinde olmalıdır. Aynı blob konumda görüntülerle göreli bağlantıları şu anda desteklenmemektedir. 

Bir hata sayfası belirttikten sonra uygulama ağ geçidi depolama blob konumdan indirir ve yerel uygulama ağ geçidi önbelleğe kaydeder. Ardından hata sayfası doğrudan uygulama ağ geçidinden sunulur. Mevcut bir özel hata sayfasını değiştirmek için bir uygulama ağ geçidi yapılandırması farklı blob konumuna işaret etmelidir. Uygulama ağ geçidi, düzenli aralıklarla yeni sürümler getirmek için blob konumu denetlemez.

## <a name="portal-configuration"></a>Portal Yapılandırması

1. Portalda uygulama ağ geçidine gidin ve bir uygulama ağ geçidi seçin.

    ![AG genel bakış](./media/custom-error/ag-overview.png)
2. Tıklayın **dinleyicileri** ve bir hata sayfası belirtmek istediğiniz belirli bir dinleyici gidin.

    ![Uygulama ağ geçidi dinleyicileri](./media/custom-error/ag-listener.png)
3. Bir 403 WAF hatası için bir özel hata sayfası veya 502 Bakım Sayfası dinleyici düzeyinde yapılandırın.

    > [!NOTE]
    > Azure portalından genel düzeyi özel hata sayfaları oluşturma şu anda desteklenmiyor.

4. Belirtilen hata durum kodu Genel olarak erişilebilir blob URL'sini belirtin ve tıklayın **Kaydet**. Application Gateway, özel hata sayfası ile yapılandırılmıştır.

   ![Uygulama ağ geçidi hata kodları](./media/custom-error/ag-error-codes.png)

## <a name="azure-powershell-configuration"></a>Azure PowerShell yapılandırması

Özel hata sayfasını yapılandırmak için Azure PowerShell kullanabilirsiniz. Örneğin, genel bir özel hata sayfası:

`$updatedgateway = Add-AzApplicationGatewayCustomError -ApplicationGateway $appgw -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url`

Veya bir dinleyici düzeyi hata sayfası:

`$updatedlistener = Add-AzApplicationGatewayHttpListenerCustomError -HttpListener $listener01 -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url`

Daha fazla bilgi için [Ekle AzApplicationGatewayCustomError](https://docs.microsoft.com/powershell/module/az.network/add-azapplicationgatewaycustomerror?view=azps-1.2.0) ve [Ekle AzApplicationGatewayHttpListenerCustomError](https://docs.microsoft.com/powershell/module/az.network/add-azapplicationgatewayhttplistenercustomerror?view=azps-1.3.0).

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway Tanılama hakkında daha fazla bilgi için bkz. [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).

<!-- Update_Description: wording update -->
