---
title: "Olacak şekilde, Azure uygulama hizmeti ortamını yapılandırma zorlamalı tünel"
description: "Giden trafik zorlamalı tünel olduğunda çalışması uygulama hizmeti ortamınızı etkinleştir"
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
ms.openlocfilehash: f5f099042cefe666e22a9d561afeb4584db3d92c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Zorlamalı tünel ile uygulama hizmeti ortamınızı yapılandırma

Uygulama hizmeti ortamı Azure uygulama hizmeti Azure Virtual Network, Müşteri'nin örneğinde dağıtımını değil. Birçok müşteri sanal ağlarını VPN veya Azure ExpressRoute ile şirket içi ağlarını uzantılarını olacak şekilde yapılandırmak bağlantıları. Şirket ilkelerine veya diğer güvenlik kısıtlamaları nedeniyle, bunlar internet'e giderek, önce tüm giden trafiği şirket içi göndermek için bir yol yapılandırın. Giden trafiği sanal ağdan şirket içi VPN veya ExpressRoute bağlantısı üzerinden akmasını sanal ağı yönlendirme değiştirme zorlamalı tünel adı verilir. 

Zorlamalı tünel bir uygulama hizmeti ortamı için sorunlara neden olabilir. Uygulama hizmeti ortamı bölümünde açıklanan dış bağımlılıklar vardır [uygulama hizmeti ortamı ağ mimarisi] [ network] belge. Uygulama hizmeti ortamı, varsayılan olarak, tüm giden iletişim sağlandığında VIP uygulama hizmeti ortamı ile üzerinden gider gerektirir.

Hangi zorlamalı tünel olduğu ve onunla nasıl önemli bir özelliği yollardır. Bir Azure sanal ağında yönlendirmeyi en uzun ön ek eşleşmesi (LPM) göre yapılır. Aynı LPM eşleşmesine sahip birden fazla yol varsa, bir yol aşağıdaki sırayla ve kaynağına göre seçilir:

* Kullanıcı tanımlı yönlendirme (UDR)
* BGP yolu (ExpressRoute kullanıldığında)
* Sistem yolu

Bir sanal ağ içinde yönlendirme hakkında daha fazla bilgi için okuma [kullanıcı tanımlı yollar ve IP iletimini][routes]. 

Zorlamalı tünel sanal ağında çalışmak üzere uygulama hizmeti ortamınızı isterseniz, iki seçeneğiniz vardır:

* Doğrudan internet erişimi uygulama hizmeti ortamınızı etkinleştirin.
* Uygulama hizmeti ortamınız için çıkış uç noktası değiştirin.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Doğrudan internet erişimi uygulama hizmeti ortamınızı etkinleştir

App Service sanal ağınız ile bir ExpressRoute bağlantı yapılandırılırken çalışmaya ortamınız için şunları yapabilirsiniz:

* ExpressRoute 0.0.0.0/0 bildirmek için yapılandırın. Varsayılan olarak, zorla tünel giden tüm trafiği şirket içi.
* Bir UDR oluşturun. Uygulama hizmeti ortamı 0.0.0.0/0 ve bir sonraki atlama türü Internet bir adres ön ekine sahip içeren alt ağ için geçerlidir.

Bu iki değişiklik yaparsanız, uygulama hizmeti ortamı alt ağdan kaynaklanan Internet hedefleyen trafik ExpressRoute bağlantısı zorlanmaz ve uygulama hizmeti ortamı çalışır.

> [!IMPORTANT]
> Bir UDR tanımlanan rotalar ExpressRoute yapılandırma tarafından tanıtılan rotaları önceliklidir için belirli olması gerekir. Yukarıdaki örnek, geniş 0.0.0.0/0 adres aralığını kullanır. Bu büyük olasılıkla yanlışlıkla daha belirli adres aralıkları Yol tanıtımlarını tarafından geçersiz kılınabilir.
>
> Uygulama hizmeti ortamları ortak eşleme yolu yolları özel eşleme yoluna arası tanıtma ExpressRoute yapılandırmalarla desteklenmez. Microsoft'tan Yol tanıtımlarını ilgili yapılandırılmış ortak eşleme ile ExpressRoute yapılandırmaları. Reklam çok sayıda Microsoft Azure IP adres aralıklarını içerir. Özel eşliği yolda arası tanıtılan adres aralıklarını, uygulama hizmeti ortamı'nın alt ağdaki tüm giden ağ paketlerini bir müşterinin şirket içi ağ altyapısına tünelli zorla demektir. Bu ağ akışı, varsayılan olarak App Service ortamları şu anda desteklenmiyor. Bu sorun için bir çözüm, ortak eşleme yolu arası reklam yolları özel eşleme yoluna önlemektir. Başka bir çözüm zorlamalı tünel yapılandırmasında çalışmak uygulama hizmeti ortamınızı sağlamaktır.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Uygulama hizmeti ortamınız için çıkış uç noktası değiştirme ##

Bu bölümde, uygulama hizmeti ortamı tarafından kullanılan çıkış uç noktası değiştirerek bir zorlamalı tünel yapılandırmasındaki çalışması bir uygulama hizmeti ortamı etkinleştirmeyi açıklar. Uygulama hizmeti ortamı giden trafiği için bir şirket içi ağınızda tünel zorla ise, uygulama hizmeti ortamı VIP adresi dışında IP adreslerinden kaynağına bu trafiğine izin vermesi gerekir.

Bir uygulama hizmeti ortamı yalnızca Dış bağımlılıklara sahiptir, ancak ayrıca gerekir gelen trafik için dinleme ve böyle trafiğine yanıt. TCP keser yanıtları geri başka bir adresinden gönderilemiyor. Uygulama hizmeti ortamı için çıkış uç noktası değiştirmek için gereken üç adım vardır:

1. Gelen yönetim trafiğinin aynı IP adresinden dönebilirsiniz emin olmak için bir yol tablosu ayarlayın.

2. Uygulama hizmeti ortamı Güvenlik Duvarı'nda çıkışı için kullanılacak olan, IP adreslerini ekleyin.

3. Yollar giden trafiği tünelli için uygulama hizmeti ortamı ayarlayın.

   ![Zorlamalı tünel ağ akışı][1]

Uygulama hizmeti ortamı zaten yukarı ve çalışır durumda ya da uygulama hizmeti ortamı dağıtımı sırasında ayarlanabilir sonra uygulama hizmeti ortamı farklı çıkış adresleriyle yapılandırabilirsiniz.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Uygulama hizmeti ortamı kazandıktan sonra çıkış adresini değiştirme ###
1. Uygulama hizmeti ortamınız için IP'leri çıkış kullanmak istediğiniz IP adreslerini alın. Zorlamalı tünel yaptığınız varsa, bu adresleri NAT ya da ağ geçidi IP gelir. Bir NVA aracılığıyla uygulama hizmeti ortamı giden trafiği yönlendirmek istiyorsanız, çıkış nva'nın genel IP adresidir.

2. Çıkış adresleri uygulama hizmeti ortamı yapılandırma bilgilerinizi ayarlayın. İçin Resource.Azure.com gidin ve aboneliği Git /<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Ardından, uygulama hizmeti ortamınızı tanımlayan JSON görebilirsiniz. Bunu seçili olduğundan emin olun **okuma/yazma** üstünde. Seçin **Düzenle**. Ekranı en alta kadar kaydırın ve değiştirme **userWhitelistedIpRanges** değeri **null** aşağıdaki gibi bir. Çıkış adres aralığı olarak ayarlamak istediğiniz adresleri kullanın. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Seçin **PUT** üstünde. Bu seçenek, uygulama hizmeti ortamınız üzerinde bir ölçeklendirme işlemi tetikler ve güvenlik duvarı ayarlar.
 
3. Oluşturma veya bir yol tablosu düzenleme ve doldurmak için/adreslerinden uygulama hizmeti ortamı konumunuza eşlenen yönetim erişimine izin vermek için kurallar. Yönetim adresleri bulmak için bkz: [uygulama hizmeti ortamı yönetim adreslerinin][management].

4. Uygulama hizmeti ortamı alt ağ bir yol tablosu ile uygulanan yolları veya BGP yollarını ayarlayın. 

Uygulama hizmeti ortamı portaldan yanıt vermeyen kalırsa, yaptığınız değişiklikleri ile ilgili bir sorun yoktur. Sorun çıkış adresleri listesi tamamlanmadı, trafiği kesildi veya trafiği engellenmiş olabilir. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Farklı çıkış adresine sahip yeni bir uygulama hizmeti ortamı oluşturun ###

Sanal ağınız zaten tünel tüm trafiği zorlamak için yapılandırılmışsa, uygulama hizmeti ortamınızı oluşturun, böylece başarıyla bulmalı için ek adımlar gerekir. Uygulama hizmeti ortamı oluşturma sırasında başka bir çıkış uç kullanımını etkinleştirmeniz gerekir. Bunu yapmak için izin verilen çıkış adresleri belirten bir şablon ile uygulama hizmeti ortamı oluşturmanız gerekir.

1. Uygulama hizmeti ortamınız için çıkış adresleri olarak kullanılacak IP adresleri alır.

2. Uygulama hizmeti ortamı tarafından kullanılacak alt ağ ön oluşturun. Yollar ayarlayabilirsiniz ihtiyacınız ve ayrıca, şablonu gerektiriyor.

3. Uygulama hizmeti ortamı konumunuza eşleme IP'leri yönetimi ile bir yol tablosu oluşturun. Uygulama hizmeti ortamınızı atayın.

4. İçindeki yönergeleri izleyin [sahip bir şablon bir uygulama hizmeti ortamı oluşturma][template]. Uygun şablonu çeker.

5. "Kaynaklar" bölümündeki azuredeploy.json dosyasını düzenleyin. İçin bir satır içeren **userWhitelistedIpRanges** değerlerinizi şöyle ile:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Bu bölümde düzgün şekilde yapılandırılmışsa, uygulama hizmeti ortamı herhangi bir sorun ile başlamanız gerekir. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
