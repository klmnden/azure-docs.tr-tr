---
title: Sorun giderme - Azure ön kapısı hizmeti yapılandırmanız ile ilgili sorunları giderme | Microsoft Docs
description: Bu öğreticide, şirket içinde ön kapısı karşılaşabilecekleri yaygın sorunlardan bazılarını sorunlarını giderme konusunda bilgi edinin.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2018
ms.author: sharadag
ms.openlocfilehash: 7a261d65a7bd3eea150dd764c65b94ddd47466b3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736131"
---
# <a name="troubleshooting-common-routing-issues"></a>Genel yönlendirme sorunlarını giderme
Bu makalede bazı Azure ön kapısı hizmet yapılandırmanızı karşılaşabilecekleri yönlendirme yaygın sorunların nasıl giderileceği açıklanmaktadır. 

## <a name="hostname-not-routing-to-backend-and-returns-400-status-code"></a>Ana bilgisayar adı için arka uç ve döndürür 400 durum kodu yönlendirme değil


### <a name="symptom"></a>Belirti
- Bir ön kapısı oluşturduysanız ancak ön uç barındırmak için bir istek bir HTTP 400 durum kodu döndürüyor.

  - Bir DNS oluşturduğunuz yapılandırdığınız özel bir etki alanından ön uç ana bilgisayara eşleme. Ancak, özel etki alanı konak adı için bir istek, bir HTTP 400 durum kodu döndürür ve sizin için backend(s) yönlendirmek için görünmez gönderme yapılandırdığınızdan.

### <a name="cause"></a>Nedeni
- Frontend ana bilgisayar olarak eklemiş olduğunuz özel bir etki alanı için bir yönlendirme kuralı yapılandırmadıysanız Bu belirti oluşabilir. Yönlendirme kuralı bir ön kapısı alt etki alanı altında frontend ana bilgisayar için zaten yapılandırılmış olsa bile, frontend ana bilgisayar için açıkça eklenmesi gerekir (*. azurefd.net) özel etki alanınız için bir DNS eşlemesi sahiptir.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları
- Yönlendirme kuralı özel etki alanından istenen arka uç havuzuna ekleyin.

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>Frontend ana bilgisayar adı için döndürür 404 durum kodu istek

### <a name="symptom"></a>Belirti
- Bir ön kapısı oluşturduğunuza ve frontend ana bilgisayar, bir arka uç havuzu ile da en az bir arka uç ve arka uç havuzuna frontend ana bağlayan yönlendirme kuralı yapılandırılmış. Bir HTTP 404 durum kodu döndürdüğünden yapılandırılmış ön uç ana bilgisayara bir isteği gönderirken kullanılabilir olması için içeriğinizi görünmüyor.

### <a name="cause"></a>Nedeni
Bu belirti birkaç olası nedeni vardır:
 - Arka uç, genel kullanıma yönelik bir arka uç değil ve ön kapısı hizmete görünür değil.

- Arka uç, yanlış isteği göndermek ön kapı hizmetin neden yanlış yapılandırılmış (diğer bir deyişle, arka uç HTTP yalnızca kabul eder ancak sahip olmayan ön kapısı HTTPS iletmeye çalışıyor için HTTPS isteklerini verme denetlenmeyen).
- Arka uç ile isteği arka uca iletildi ana bilgisayar üst bilgisini reddediyor.
- Arka uç yapılandırmasını henüz tam olarak dağıtılmadı.

### <a name="troubleshooting-steps"></a>Sorun giderme adımları
1. Dağıtım süresi
    - Dağıtılacak yapılandırma için yaklaşık 10 dakika süre bekledi olun.

2. Arka uç ayarlarını kontrol edin
   - (Bağlıdır için yapılandırılmış bir yönlendirme kuralını nasıl olan) istek yönlendirme arka uç havuzu gidin ve doğrulayın _arka uç ana bilgisayar türü_ ve arka uç ana bilgisayar adı doğru. Arka uç, özel bir ana bilgisayar ise, doğru yazdığınızdan emin olun. 

   - HTTP ve HTTPS bağlantı noktaları kontrol edin. Çoğu durumda, 80 ve 443 (sırasıyla) doğru olduğundan ve hiçbir değişiklik gerekli olacaktır. Ancak, arka ucunuza bu şekilde yapılandırılmamış ve farklı bir bağlantı noktasında dinleme imkanı yoktur.

     - Denetleme _arka uç ana bilgisayar üstbilgisi_ Frontend ana bilgisayar yönlendirme arka uçları için yapılandırılmış. Çoğu durumda, bu üst bilgisi ile aynı olmalıdır _arka uç ana bilgisayar adı_. Ancak, arka uç farklı bir şey bekliyorsa yanlış bir değere çeşitli HTTP 4xx durum kodları neden olabilir. Arka uç IP adresini girin, ayarlamanız gerekebilir _arka uç ana bilgisayar üstbilgisi_ arka uç ana bilgisayar adı için.


3. Yönlendirme kuralı ayarlarını kontrol edin
     - Frontend ana bilgisayar adını söz konusu bir arka uç havuzuna yönlendirmek yönlendirme kuralı gidin. Kabul edilen protokollerden doğru şekilde yapılandırıldığından emin olun ya da aksi takdirde, iletilmeden ön kapısı kullanacaksınız Protokolü doğru şekilde yapılandırıldığından emin olun. _Kabul protokolleri_ , ön kapısı kabul isteyeceğini belirler ve _Protokolü iletme_ altında _Gelişmiş_ sekmesi, hangi ön kapısı Protokolü belirler arka uç isteği iletmek için kullanmanız gerekir.
          - Arka uç yalnızca HTTP isteklerini kabul ediyorsa bir örnek olarak, aşağıdaki yapılandırmalar geçerli olacaktır:
               - _Kabul protokolleri_ HTTP ve HTTPS. _Protokol iletme_ HTTP'dir. Eşleşen istek, izin verilen bir protokolü HTTPS olduğundan ve bir istek HTTPS geliyorsa, HTTPS kullanılarak iletmek ön kapı isteriz çalışmaz.

               - _Kabul protokolleri_ HTTP. _Protokol iletme_ isteği veya HTTPS ya da eşleşen olduğu.

   - Tıklayarak _Gelişmiş_ yönlendirme kuralı yapılandırma bölmesinde üst kısmındaki sekme. _URL yeniden yazma_ varsayılan olarak devre dışıdır ve arka uç tarafından barındırılan ve kullanılabilir hale getirmek istediğiniz kaynakları kapsamını daraltmak isterseniz bu alan yalnızca kullanmanız gerekir. Devre dışı bırakıldığında, ön kapısı aldığı aynı istek yolu iletir. Bu alan hatalı yapılandırıldı ve ön kapısı böylece bir HTTP 404 durum kodu döndürerek bir kaynak kullanılabilir değil, arka uç isteyen mümkündür.

