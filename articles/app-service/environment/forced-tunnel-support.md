---
title: "Azure App Service Ortamınızı zorlamalı tünel için yapılandırma"
description: "App Service Ortamınızın giden trafiğe zorlamalı tünel uygulandığında çalışmasını etkinleştirme"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 4caaf0df3f1dd4b2cb9b76283a6beed897531c1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>App Service Ortamınızı zorlamalı tünel ile yapılandırma

App Service Ortamı, Azure App Service’in bir Azure Sanal Ağı müşteri örneği içindeki dağıtımıdır. Birçok müşteri, sanal ağlarını VPN veya Azure ExpressRoute bağlantıları ile şirket içi ağlarının uzantıları olacak şekilde yapılandırır. Şirket ilkeleri veya diğer güvenlik kısıtlamaları nedeniyle, bu müşteriler yolları internet’e ulaşmadan önce tüm şirket içi giden trafiği gönderecek şekilde yapılandırır. Sanal ağ yönlendirmesini, giden trafiğin VPN veya ExpressRoute bağlantısı üzerinden şirket içine geçebilmesi için değiştirme işlemine zorlamalı tünel adı verilir. 

Zorlamalı tünel bir App Service Ortamı için sorunlara neden olabilir. App Service Ortamında, [App Service Ortamı ağ mimarisi][network] belgesinde listelenen birkaç dış bağımlılık bulunur. App Service Ortamı varsayılan olarak tün giden iletişimin App Service Ortamı ile sağlanan VIP üzerinden gerçekleşmesini gerektirir.

Yollar, zorlamalı tünelin ne olduğu ve nasıl ele alınması gerektiğiyle ilgili önemli bir unsurdur. Bir Azure sanal ağında yönlendirme, en uzun ön ek eşleşmesi (LPM) temel alınarak yapılır. Aynı LPM eşleşmesine sahip birden fazla yol bulunuyorsa yol aşağıdaki sırayla ve kaynağına göre seçilir:

* Kullanıcı tanımlı yol (UDR)
* BGP yolu (ExpressRoute kullanıldığında)
* Sistem yolu

Sanal ağ içinde yönlendirme hakkında daha fazla bilgi için [Kullanıcı tanımlı yollar ve IP iletme][routes] makalesini okuyun. 

App Service Ortamınızın bir zorlamalı tünel sanal ağında çalışmasını istiyorsanız iki seçeneğiniz vardır:

* App Service Ortamınızın doğrudan internet erişimine sahip olmasını sağlama.
* App Service Ortamınız için çıkış uç noktasını değiştirme.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>App Service Ortamınızın doğrudan internet erişimine sahip olmasını sağlama

Sanal ağınız bir ExpressRoute bağlantısı ile yapılandırıldığında App Service Ortamınızın çalışması için şunları yapabilirsiniz:

* ExpressRoute’u 0.0.0.0/0 tanıtacak şekilde yapılandırın. Varsayılan olarak, giden tüm şirket içi trafiğe zorlamalı tünel uygular.
* UDR oluşturun. App Service Ortamını içeren alt ağa, 0.0.0.0/0 adres ön eki ve İnternet sonraki atlama türü ile uygulayın.

Bu iki değişikliği yaparsanız, App Service Ortamı alt ağından çıkıp internet’i hedefleyen trafik ExpressRoute bağlantısına zorlanmaz ve App Service Ortamı çalışır.

> [!IMPORTANT]
> Bir UDR’de tanımlanan yollar, ExpressRoute yapılandırması tarafından tanıtılan herhangi bir yoldan öncelikli olacak kadar spesifik olmalıdır. Önceki örnekte geniş 0.0.0.0/0 adres aralığı kullanılır. Bu aralık, daha spesifik adres aralıkları kullanan yol tanıtımları tarafından yanlışlıkla geçersiz kılınabilir.
>
> App Service Ortamları, genel eşleme yolundan özel eşleme yoluna çapraz olarak tanıtılan ExpressRoute yapılandırmaları ile desteklenmez. Genel eşlemesi yapılandırılmış ExpressRoute yapılandırmaları, Microsoft’tan yol tanıtımları almaz. Reklamlar çok sayıda Microsoft Azure IP adresi aralığı içerir. Adres aralıkları özel eşleme yolunda çapraz tanıtılırsa, App Service Ortamının alt ağından giden tüm ağ paketlerine bir müşterinin şirket içi ağ altyapısı için zorlamalı tünel uygulanır. Bu ağ akışı şu anda App Service Ortamları ile varsayılan olarak desteklenmemektedir. Bu sorunun bir çözümü, genel eşleme yolundan özel eşleme yoluna yolların çapraz tanıtımını durdurmaktır. Başka bir çözüm ise App Service Ortamınızı bir zorlamalı tünel yapılandırmasında çalışacak şekilde etkinleştirmektir.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>App Service Ortamınız için çıkış uç noktasını değiştirme ##

Bu bölümde, App Service Ortamı tarafından kullanılan çıkış noktasını değiştirerek bir App Service Ortamını zorlamalı tünel yapılandırmasında çalıştırma işlemi açıklanmaktadır. App Service Ortamından giden trafiğe bir şirket içi ağ için zorlamalı tünel uygulanırsa, bu trafiğin App Service Ortamı VIP adresi dışındaki IP adreslerinden çıkmasına izin vermeniz gerekir.

Bir App Service Ortamı yalnızca dış bağımlılıklar içermez, aynı zamanda gelen trafiği dinlemeli ve bu tür bir trafiğe yanıt vermelidir. Yanıtlar TCP’yi keseceği için başka bir adresten geri gönderilemez. App Service Ortamı’na ilişkin çıkış uç noktasını değiştirmek için üç adım gereklidir:

1. Gelen yönetim trafiğinin aynı IP adresinden geri gidebildiğinden emin olmak için bir yol tablosu ayarlayın.

2. Çıkış için kullanılacak IP adreslerinizi App Service Ortamı güvenlik duvarına ekleyin.

3. Tünel uygulanacak App Service Ortamından giden trafiğin yollarını ayarlayın.

   ![Zorlamalı tünel ağ akışı][1]

App Service Ortamını, App Service Ortamı çalışır duruma geldikten sonra farklı çıkış adresleriyle ayarlayabilirsiniz veya App Service Ortamı dağıtımı sırasında ayarlayabilirsiniz.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>App Service Ortamı çalışır duruma geldikten sonra çıkış adresini değiştirme ###
1. App Service Ortamınız için çıkış IP’si olarak kullanmak istediğiniz IP adreslerini alın. Zorlamalı tünel uyguluyorsanız, bu adresler NAT ya da ağ geçidi IP’lerinizden gelir. App Service Ortamı giden trafiğini bir NVA üzerinden yönlendirmek istiyorsanız, çıkış adresi NVA’nın genel IP’sidir.

2. Çıkış adreslerini, App Service Ortamınızın yapılandırma bilgilerinde ayarlayın. resource.azure.com sayfasına ve Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name> menüsüne gidin. Bundan sonra App Service Ortamınızı tanımlayan JSON dosyasını görebilirsiniz. Üst kısımda **read/write** ifadesinin gösterildiğinden emin olun. **Düzenle**’yi seçin. Ekranı en alta kadar kaydırın ve **null** olan **userWhitelistedIpRanges** değerini aşağıdakine benzer bir değerle değiştirin. Çıkış adres aralığı olarak ayarlamak istediğiniz adresleri kullanın. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Üst kısımdaki **PUT** öğesini seçin. Bu seçenek, App Service Ortamınızda bir ölçeklendirme işlemi başlatır ve güvenlik duvarını ayarlar.
 
3. Bir yol tablosu oluşturun ya da düzenleyin ve App Service Ortamınızın konumuyla eşleşen yönetim adreslerine/adreslerinden erişime izin vermek için kuralları doldurun. Yönetim adreslerini bulmak için bkz. [App Service Ortamı yönetim adresleri][management].

4. App Service Ortamı alt ağına bir yol tablosu veya BGP yolları ile uygulanan yolları ayarlayın. 

App Service Ortamı portalda yanıt vermemeye başlarsa, değişikliklerinizle ilgili bir sorun vardır. Sorun, çıkış adresleri listenizin eksik olması, trafiğin kaybedilmesi veya trafiğin engellenmesi olabilir. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Farklı bir çıkış adresi ile yeni bir App Service Ortamı oluşturma ###

Sanal ağınız zaten tüm trafiğe zorlamalı tünel uygulayacak şekilde yapılandırılmışsa, başarıyla ortaya çıkabilmesi için App Service Ortamınızı oluşturmaya yönelik ek adımlar uygulamanız gerekir. App Service Environment oluşturulurken başka bir çıkış uç noktasının kullanılmasını etkinleştirmeniz gerekir. Bunu yapmak için, izin verilen çıkış adreslerini belirten bir şablonla App Service Ortamını oluşturmalısınız.

1. App Service Ortamınız için çıkış adresleri olarak kullanılacak IP adreslerini alın.

2. App Service Ortamı tarafından kullanılacak alt ağı önceden oluşturun. Yolları ayarlayabilmek için ve ayrıca şablon gerektirdiği için bu alt ağa ihtiyacınız olacaktır.

3. App Service Ortamınızın konumuyla eşleşen yönetim IP’leri ile bir yol tablosu oluşturun. Bu tabloyu App Service Ortamınıza atayın.

4. [ŞPablon ile App Service Ortamı oluşturma][template] bölümündeki yönergeleri izleyin. Uygun şablonu çekin.

5. azuredeploy.json dosyasının "kaynaklar" bölümünü düzenleyin. Değerleri aşağıdaki gibi olan bir **userWhitelistedIpRanges** satırı ekleyin:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Bu bölüm düzgün şekilde yapılandırılırsa, App Service Ortamı sorunsuz bir şekilde başlar. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
