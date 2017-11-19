---
title: "Olacak şekilde, Azure uygulama hizmeti ortamını yapılandırma zorlamalı tünel"
description: "Giden trafik zorlamalı tünel olduğunda çalışması, ana etkinleştir"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: ccompy
ms.openlocfilehash: cd89bd23074dec1de6fa0e8784d42a5c539938b1
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Zorlamalı tünel ile uygulama hizmeti ortamınızı yapılandırma

Uygulama hizmeti ortamı (ana), müşterinin Azure sanal ağ (VNet) Azure App Service'te bir dağıtımıdır. Birçok müşteri VPN'ler ya da ExpressRoute ile şirket içi ağlarını uzantılarını olacak şekilde kendi sanal ağları yapılandırma bağlantıları. Şirket ilkelerine veya diğer güvenlik kısıtlamaları nedeniyle, bunlar internet'e giderek, önce tüm giden trafiği şirket içi göndermek için bir yol yapılandırın. Şirket içi VPN veya ExpressRoute bağlantısı üzerinden VNet giden trafiği akmasını VNet yönlendirme değiştirme zorlamalı tünel adı verilir.  

Zorlamalı tünel için bir ana sorunlara neden olabilir. Ana bu numaralandırılır dış bağımlılıklar vardır [ana ağ mimarisi] [ network] belge. Ana varsayılan olarak, tüm giden iletişim sağlandığında VIP ana ile üzerinden gider gerektirir.

Zorlamalı tünel nedir önemli bir özelliği ve onunla nasıl yolların edilir. Bir Azure sanal ağında yönlendirme üzerinde en uzun ön ek eşleşmesi (LPM) göre yapılır.  Aynı LPM eşleşmesine sahip birden fazla yol varsa, bir yol aşağıdaki sırayla ve kaynağına göre seçilir:

1. Kullanıcı tanımlı yol
1. BGP yolu (ExpressRoute kullanıldığında)
1. Sistem yolu

Sanal ağ içinde yönlendirme hakkında daha fazla bilgi için okuma [kullanıcı tanımlı yollar ve IP iletimini][routes]. 

VNet zorlanan tünel içinde çalışması için ana isterseniz, iki seçeneğiniz vardır:

1. Doğrudan internet erişimini sağlamak, ana etkinleştir
1. Çıkış uç noktası için ana değiştirme

## <a name="enable-your-ase-to-have-direct-internet-access"></a>Doğrudan internet erişimini sağlamak, ana etkinleştir

Ana sanal ağınızı bir ExpressRoute ile yapılandırılırken çalışması, şunları yapabilirsiniz:

* ExpressRoute 0.0.0.0/0 bildirmek için yapılandırın. Varsayılan olarak, zorla tünel giden tüm trafiği şirket içi.
* Bir UDR oluşturun. Bir adres öneki 0.0.0.0/0 ve bir sonraki atlama türü Internet ile ana içeren alt ağ için geçerlidir.

Bu iki değişiklik yaparsanız, ana alt ağdan kaynaklanan Internet hedefleyen trafiğe works ExpressRoute ve ana aşağı zorlanmaz.

> [!IMPORTANT]
> Bir UDR tanımlanan rotalar ExpressRoute yapılandırma tarafından tanıtılan rotaları önceliklidir için belirli olması gerekir. Yukarıdaki örnek, geniş 0.0.0.0/0 adres aralığını kullanır. Bu büyük olasılıkla yanlışlıkla daha belirli adres aralıkları Yol tanıtımlarını tarafından geçersiz kılınabilir.
>
> ASEs ortak eşleme yolu yolları özel eşleme yoluna arası tanıtma ExpressRoute yapılandırmalarla desteklenmez. Microsoft'tan Yol tanıtımlarını ilgili yapılandırılmış ortak eşleme ile ExpressRoute yapılandırmaları. Reklam çok sayıda Microsoft Azure IP adres aralıklarını içerir. Özel eşliği yolda arası tanıtılan adres aralıklarını, tüm giden ağ paketlerinin ana'nın alt ağdan bir müşterinin şirket içi ağ altyapısına tünelli zorla demektir. Bu ağ akışı ASEs varsayılan olarak şu anda desteklenmiyor. Bu sorun için bir çözüm, ortak eşleme yolu arası reklam yolları özel eşleme yoluna önlemektir.  Diğer bir zorlamalı tünel yapılandırmasındaki çalışması, ana etkinleştirmek için çözümüdür.

## <a name="change-the-egress-endpoint-for-your-ase"></a>Çıkış uç noktası için ana değiştirme ##

Bu bölümde, ana tarafından kullanılan çıkış uç noktası değiştirerek bir zorlamalı tünel yapılandırmasındaki çalışması bir ana etkinleştirmeyi açıklar. Ana giden trafiği zorlanması durumunda, bu trafiğin ana VIP adresi dışında IP adreslerinden kaynağına izin vermeniz gereken sonra bir şirket içi ağına tünelli.

Bir ana yalnızca Dış bağımlılıklara sahiptir, ancak ayrıca ana yönetmek gelen trafik için dinleme gerekir. Ana bu tür trafik yanıt verebilir ve TCP sonları gibi başka bir adresinden yanıtları gönderilemiyor.  Böylece çıkış uç noktası için ana değiştirmek için gereken üç adım vardır.

1. Gelen yönetim trafiğinin aynı IP adresinden dönebilirsiniz emin olmak için bir yol tablosu ayarlama
1. Ana Güvenlik Duvarı'nda çıkışı için kullanılacak IP adresleri ekleme
1. Yollar giden trafiği tünel oluşturulmasını ana ayarlayın.

![Zorlamalı tünel ağ akışı][1]

Ana zaten yukarı ve çalışır durumda veya ana dağıtımı sırasında ayarlanabilir sonra farklı çıkış adresleriyle ana yapılandırabilirsiniz.  

### <a name="changing-the-egress-address-after-the-ase-is-operational"></a>Ana kazandıktan sonra çıkış adresini değiştirme ###
1. İçin ana IP'leri çıkış kullanmak istediğiniz IP adresleri alır. Yapıyorsanız bu NAT veya ağ geçidi IP olacaktır sonra zorlanan tünel.  Bir NVA aracılığıyla ana giden trafiği yönlendirmek istiyorsanız, çıkış adresi nva'nın genel IP olacaktır.
2. Çıkış adresleri ana yapılandırma bilgilerinizi ayarlayın. İçin Resource.Azure.com gidin ve gidin: Abonelik /<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name> sonra ana tanımlayan json görebilirsiniz.  Okuma/yazma üst seçili olduğundan emin olun.  En Alta kadar kaydırın kaydırma Düzenle'yi tıklatın ve gelen userWhitelistedIpRanges değiştirin  

       "userWhitelistedIpRanges": null 
      
  aşağıdaki gibi şeyler için. Çıkış adres aralığı olarak ayarlamak istediğiniz adresleri kullanın. 

      "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

  PUT üstündeki'ı tıklatın. Bu, ana üzerinde bir ölçeklendirme işlemi tetikler ve Güvenlik Duvarı'nı ayarlayın.
   
3. Oluşturma veya bir yol tablosu düzenleyebilir ve doldurmak için/adreslerinden ana konumunuza eşlenen yönetim erişimine izin vermek için kurallar.  Yönetim adreslerinin işte [uygulama hizmeti ortamı yönetimi adresleri][management] 

4. Bir yol tablosu ile ana alt ağa uygulanan yolları veya BGP yollarını ayarlayın.  

Ana portaldan yanıt vermeyen kalırsa, yaptığınız değişiklikleri ile ilgili bir sorun yoktur.  Çıkış adresleri listesi tamamlanmadı, trafiği kesildi veya trafiği engellenmiş olabilir.  

### <a name="create-a-new-ase-with-a-different-egress-address"></a>Farklı çıkış adresine sahip yeni bir ana oluşturun  ###

Sanal ağınızı tünel tüm trafiği zorlamak için zaten yapılandırıldı gelmesi durumunda, başarılı bir şekilde bulmalı şekilde, ana oluşturmak için bazı ek adımlar gerekir. Başka bir deyişle, ana oluşturma sırasında başka bir çıkış uç kullanımını etkinleştirmeniz gerekir.  Bunu yapmak için izin verilen çıkış adresleri belirten bir şablon ile ana oluşturmanız gerekir.

1. İçin ana çıkış adresleri olarak kullanılacak IP adresleri alır.
1. Ana tarafından kullanılacak alt ağ ön oluşturun. Bu yollar ayarlamanıza izin vermek için gereklidir ve ayrıca, şablonu gerektiriyor.  
1. Ana konumunuza harita ve, ana Ata IP'leri yönetimi ile bir yol tablosu oluşturma
1. Burada, yönergeleri izleyin [bir ana sahip bir şablon oluşturma][template]ve uygun şablon isteme
1. Azuredeploy.json dosyasını "Kaynaklar" bölümü düzenleyin. İçin bir satır içeren **userWhitelistedIpRanges** gibi değerlerle:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Bu düzgün şekilde yapılandırılmışsa, ana herhangi bir sorun ile başlamanız gerekir.  


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
