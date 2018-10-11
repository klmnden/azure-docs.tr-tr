---
title: Azure Application Gateway özel hata sayfaları oluşturma
description: Bu makalede Application Gateway özel hata sayfaları oluşturma işlemini gösterir.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: article
ms.date: 10/11/2018
ms.author: victorh
ms.openlocfilehash: 2f76347105743538e9fc1d7588ecb949f2675696
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49071295"
---
# <a name="create-application-gateway-custom-error-pages"></a>Application Gateway özel hata sayfaları oluşturma

Uygulama ağ geçidi, varsayılan hata sayfalarını görüntüleme yerine özel hata sayfaları oluşturmanıza olanak sağlar. Kendi marka ve özel hata sayfasını kullanarak Düzen kullanabilirsiniz.

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

    ![AG genel bakış](media/custom-error/ag-overview.png)
2. Tıklayın **dinleyicileri** ve bir hata sayfası belirtmek istediğiniz belirli bir dinleyici gidin.

    ![Uygulama ağ geçidi dinleyicileri](media/custom-error/ag-listener.png)
3. Bir 403 WAF hatası için bir özel hata sayfası veya 502 Bakım Sayfası dinleyici düzeyinde yapılandırın.

    > [!NOTE]
    > Azure portalından genel düzeyi özel hata sayfaları oluşturma şu anda desteklenmiyor.

4. Belirtilen hata durum kodu Genel olarak erişilebilir blob URL'sini belirtin ve tıklayın **Kaydet**. Application Gateway, özel hata sayfası ile yapılandırılmıştır.

   ![Uygulama ağ geçidi hata kodları](media/custom-error/ag-error-codes.png)
## <a name="next-steps"></a>Sonraki adımlar
Application Gateway Tanılama hakkında daha fazla bilgi için bkz. [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md).