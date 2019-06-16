---
title: Oluşturma ve Azure App Service ortamı ile - iç yük dengeleyici kullanma | Microsoft Docs
description: Oluşturma ve bir ASE, ILB ile kullanma
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 88f100bc780d8df0202cfcce9b390085a71fc905
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130611"
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Bir App Service ortamı ile iç yük dengeleyici kullanma

> [!NOTE] 
> Bu makale, App Service ortamı v1 hakkında yöneliktir. App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).
>

App Service ortamı (ASE) özelliği, çok kiracılı Damgalar içinde kullanılabilir olmayan bir Gelişmiş Yapılandırma özelliği sunan Azure App Service Premium hizmet seçeneğidir. ASE özelliği temel olarak Azure App Service, Azure sanal ağ içinde dağıtır. Okuma App Service ortamları tarafından sunulan daha yüksek bir anlayış kazanmak için [bir App Service ortamı nedir] [ WhatisASE] belgeleri. Bir VNet okuma işletim avantajları bilmiyorsanız, [Azure Virtual Network SSS][virtualnetwork]. 

## <a name="overview"></a>Genel Bakış
Bir ASE'yi internet erişilebilir uç nokta veya sanal bir IP adresi ile dağıtılabilir. IP adresini bir sanal ağ adresine ayarlamak için bir iç yük Balancer(ILB) ASE'NİZLE dağıtmanız gerekebilir. ASE'nizi bir ILB ile yapılandırıldığında, sağladığınız:

* kendi etki alanı veya alt etki alanı. Bu belgede kolaylaştırmak için alt etki alanı kabul edilir, ancak her iki şekilde yapılandırabilirsiniz. 
* HTTPS için kullanılan sertifika
* DNS Yönetimi, alt etki alanı. 

Buna karşılık, şunun gibi şeyler yapabilirsiniz:

* güvenli bir şekilde bir siteye Site ya da ExpressRoute VPN aracılığıyla erişim buluttaki satır iş kolu uygulamaları gibi konak intranet uygulamaları
* Genel DNS sunucularında listelenmeyen ana bilgisayar uygulamalarını bulutta
* ile ön uç uygulamalarınızın güvenli bir şekilde tümleştirebilmeniz internet yalıtılmış arka uç uygulamaları oluşturma

#### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevler
ILB ASE'yi kullanırken bazı şeyler vardır. Bu noktalar şunlardır:

* IPSSL kullanma
* belirli uygulamalara IP adresleri atama
* satın alma ve portal üzerinden bir uygulama ile bir sertifika kullanıyor. Elbette, yine de bir sertifika yetkilisi sertifikalarıyla doğrudan alabilir ve uygulamalarınızla ancak değil Azure portalı üzerinden kullanmak.

## <a name="creating-an-ilb-ase"></a>ILB ASE oluşturma
ILB ASE oluşturma, normalde bir ASE oluşturma çok farklı değildir. ASE oluşturma hakkında daha ayrıntılı bilgi için bkz [bir App Service ortamı oluşturma][HowtoCreateASE]. ILB ASE oluşturma işlemini ASE oluşturma sırasında bir sanal ağ oluşturma veya önceden mevcut olan bir sanal ağ seçme arasında aynıdır. ILB ASE oluşturmak için: 

1. Azure portalında **kaynak Oluştur -> Web + mobil -> App Service ortamı**.
2. Aboneliğinizi seçin.
3. Kaynak grubunu seçin veya oluşturun.
4. Bir VNet seçin veya oluşturun.
5. Bir sanal ağ'ı seçerek bir alt ağ oluşturun.
6. Seçin **sanal ağ/konum, sanal ağ yapılandırması ->** ve VIP türü için dahili olarak ayarlayın.
7. (Bu adı bu ASE'de oluşturulan uygulamaları için kullanılan alt etki alanı) bir alt etki alanı adını sağlayın.
8. Seçin **Tamam** ardından **oluşturma**.

![][1]

Sanal ağ bölmesinde sağlayan seçeneğini bir dış VIP veya iç VIP arasında bir VNet yapılandırma yoktur. Dış varsayılandır. Dış ayarlar varsa, ASE'niz internet ile erişilebilen bir VIP kullanır. Dahili seçerseniz ASE'niz, sanal ağ içindeki bir IP adresi üzerindeki bir ILB ile yapılandırılır. 

Dahili seçtikten sonra ase'nize daha fazla IP adresi özelliği kaldırılır ve bunun yerine ase'nin alt etki alanı belirtmeniz gerekir. Dış VIP bir ASE'de, ase'nin adı bu ASE'de oluşturulan uygulamaların içinde alt etki alanı kullanılır. ASE'NİZİN adlandırılmışsa ***contosotest*** ve uygulamanızda bu ASE olarak adlandırılan ***mytest***, alt etki alanı biçimindedir ***contosotest.p.azurewebsites.net*** ve bu uygulamanın URL'si ***mytest.contosotest.p.azurewebsites.net***. VIP türü için dahili olarak ayarlarsanız, ASE adınız ASE alt etki alanı içinde kullanılmaz. Alt etki alanını açıkça belirtin. Varsa, alt etki alanı ***contoso.corp.net*** ve ASE adlı bir uygulama yapılan ***timereporting***, bu uygulama için URL ***timereporting.contoso.corp.NET şeklinde***.

## <a name="apps-in-an-ilb-ase"></a>Bir ILB ase'deki uygulamalar
ILB ASE'de uygulama oluşturma bir ASE'de uygulama oluşturma, normalde aynıdır. 

1. Azure portalında **kaynak Oluştur -> Web + mobil -> Web** veya **mobil** veya **API uygulaması**.
2. Uygulamanın adını girin.
3. Aboneliğinizi seçin.
4. Kaynak grubunu seçin veya oluşturun.
5. Bir App Service Plan(ASP) oluşturun veya seçin. Yeni bir ASP oluşturuluyorsa, konum olarak ASE'nizi seçin ve ASP oluşturulmasını istediğiniz çalışan havuzunu seçin. ASP oluşturduğunuzda, konumu ve çalışan havuzunu ASE'nizi seçin. Uygulamanın adını belirttiğinizde, uygulama adının altındaki alt etki alanı tarafından bir alt etki alanı ASE'NİZİN değiştirilir görürsünüz. 
6. **Oluştur**’u seçin. Seçtiğinizden emin olun **panoya Sabitle** , Panonuzda görünmesini istiyorsanız onay kutusunu. 

![][2]

Uygulama adı altında alt etki alanı adı ase'nizin alt etki alanını yansıtacak şekilde güncelleştirilir. 

## <a name="post-ilb-ase-creation-validation"></a>ILB ASE oluşturmayı doğrulama sonrası
Bir ILB ASE, ILB olmayan ASE’den biraz farklıdır. Zaten kendi DNS'İNİZİ yönetmeniz gerekir ve HTTPS bağlantıları için kendi sertifikanızı sağlamak de not. 

ASE'yi oluşturduktan sonra belirttiğiniz ve içinde yeni bir öğe ve alt etki alanının alt etki alanı gösterir fark edeceksiniz **ayarı** adlı menüsü **ILB sertifikası**. ASE HTTPS'yi test etmeyi kolaylaştırır otomatik olarak imzalanan bir sertifika ile oluşturulur. Portal, HTTPS için kendi sertifikanızı sağlamanız gereken bildirir ancak bu alt etki alanı ile giden bir sertifikasına sahip olmasını öneririz. 

![][3]

Öğeleri sadece çalışıyorsunuz ve bir sertifika oluşturmak nasıl saptayacağımı bilmiyorum, otomatik olarak imzalanan bir sertifika oluşturmak için IIS MMC konsol uygulamasını kullanabilirsiniz. Oluşturulduktan sonra bir .pfx dosyası olarak dışarı aktarmak ve ILB sertifikası Arabiriminde karşıya yükleyin. Tarayıcınız otomatik olarak imzalanan bir sertifika ile güvenli bir siteye eriştiğinizde, erişmeye çalıştığınız site bağlanamama nedeniyle sertifikayı doğrulamak için güvenli olmadığını bildiren bir uyarı verir. Bu uyarıyı önlemek istiyorsanız, alt etki alanı ile eşleşen ve tarayıcınız tarafından tanınan bir güven zincirine sahip doğru imzalı bir sertifika gerekir.

![][6]

Kendi sertifikalarınızı akışı deneyin ve ASE'niz için hem HTTP hem de HTTPS erişimi test etmek istiyorsanız:

1. ASE ASE oluşturulduktan sonra kullanıcı Arabirimine gidin **ASE -> Ayarlar -> ILB sertifikaları**.
2. Sertifika pfx dosyasını seçerek ILB sertifikasını ayarlamak ve parola belirtin. Bu adım işlemek için kısa bir süre alır ve bir ölçeklendirme işlemi devam ediyor bir ileti görüntülenir.
3. ASE'NİZİN ILB adresini alın (**ASE -> Özellikler -> sanal IP adresi**).
4. Bir web uygulaması oluşturulduktan sonra ASE oluşturma. 
5. (Değil, aynı alt ağda ASE veya şeyler kesme) bir sanal ağ içinde yoksa, bir VM oluşturun.
6. DNS alt etki alanı için ayarlayın. Joker karakter, DNS'de alt etki alanı ile kullanın veya bazı basit testler yapmak istiyorsanız, web uygulaması adını VIP IP adresine ayarlamak için VM üzerindeki konak dosyalarını düzenleyin. Alt etki alanı adı ASE'NİZİN olsaydı. ilbase.com ve web uygulaması mytestapp mytestapp.ilbase.com ele alınması, böylece yaptığınız kümesi, hosts dosyasında. (Windows üzerinde konakları C:\Windows\System32\drivers\etc dosyasıdır\)
7. Bu VM'de bir tarayıcı kullanın ve Git https://mytestapp.ilbase.com (veya alt etki alanı ile web uygulamanızın adına ne olursa olsun).
8. Veya bu sanal makinedeki bir tarayıcıyı kullanıp https://mytestapp.ilbase.com sayfasına gidin. Kendinden imzalı bir sertifika kullanıyorsanız güvenlik eksikliğini kabul etmeniz gerekir. 

Ilb'nizin IP adresi, sanal IP adresi, özelliklerinde listelenir.

![][4]

## <a name="using-an-ilb-ase"></a>ILB ASE kullanır
#### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
ILB ASE uygulamalarınız için ağ yalıtımı sağlar. Uygulamaları erişilebilir veya internet tarafından bilinen bile değildir. Bu yaklaşım satır iş kolu uygulamaları gibi intranet sitelerini barındırmak için uygundur. Erişimi daha da kısıtlamak gerektiğinde ağ düzeyinde erişimi denetlemek için ağ güvenlik Groups(NSGs) yine de kullanabilirsiniz. 

Daha fazla erişimi kısıtlamak için Nsg kullanmak istiyorsanız, ASE çalışması için gereken iletişimi bölmediğinizden emin olmanız gerekir. HTTP/HTTPS erişim yalnızca ILB ASE tarafından kullanılan aracılığıyla olsa da, ASE yine de sanal ağ dışında kaynakları bağlıdır. Hangi ağ erişimi hala gerekli olduğunu görmek için bkz. [bir App Service ortamına gelen trafiği denetleme] [ ControlInbound] ve [ile App Service ortamları için ağ yapılandırma ayrıntıları ExpressRoute][ExpressRoute]. 

Nsg'lerinizi yapılandırmak için ASE'nizi yönetmek için Azure tarafından kullanılan IP adresini bilmeniz gerekir. İnternet isteklerini yaparsa bu IP adresi de giden IP gidenler adresidir. Giden IP adresi ASE'nizi ASE'nizi süresince statik kalır. ASE'NİZİN silip yeni bir IP adresi alırsınız. IP adresini bulmak için Git **ayarlar -> Özellikler** ve bulma **giden IP adresi**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Genel ILB ASE Yönetimi
ILB ASE yönetme büyük ölçüde normalde bir ASE'de yönetme aynıdır. Daha fazla ASP Örnekleri barındırmak ve artan miktarlarda HTTP/HTTPS trafiğini işlemek için ön uç sunucuların ölçeğini artırmak için çalışan havuzları ölçeğini gerekir. Bir ASE yapılandırmasını yönetme ile ilgili genel bilgiler için bkz. [bir App Service ortamını yapılandırma][ASEConfig]. 

Ek yönetim sertifikası ve DNS Yönetimi öğelerdir. Edinmeli ve ILB ASE oluşturulduktan sonra HTTPS için kullanılan sertifikayı karşıya yüklemek ve süresi dolmadan önce değiştirin. Azure temel etki alanı sahibi olduğu için dış VIP Ase'ler için sertifikalar sağlayabilir. ILB ASE tarafından kullanılan alt etki alanı her şey olabilir. bu yana, HTTPS için kendi sertifikanızı sağlamanız gerekir. 

#### <a name="dns-configuration"></a>DNS yapılandırması
Dış VIP kullanırken, DNS, Azure tarafından yönetilir. ASE’nizde oluşturulan herhangi bir uygulama, genel bir DNS olan Azure DNS'e otomatik olarak eklenir. ILB ASE'de kendi DNS’inizi yönetmeniz gerekir. Contoso.corp.net gibi belirli bir alt kayıtları için ILB adresini işaret eden DNS A oluşturmanız gerekir:

    * 
    *.scm ftp publish 


## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz: [App Service ortamlarına giriş][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
