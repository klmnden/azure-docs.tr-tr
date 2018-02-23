---
title: "Oluşturma ve bir iç yük dengeleyici bir uygulama hizmeti ortamı ile kullanma | Microsoft Docs"
description: "Oluşturma ve bir ana bir ILB ile kullanma"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: f7c94b790c6aa7c75c62fd05671f016b7185b2a2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Bir iç yük dengeleyici ile uygulama hizmeti ortamı kullanma

> [!NOTE] 
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır. Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
>

Uygulama hizmeti ortamı (ana) özelliği, çok kiracılı Damgalar kullanılamaz bir Gelişmiş Yapılandırma özelliği sunan Azure App Service'in Premium hizmet seçeneğidir. ANA özelliği, Azure sanal Network(VNet) Azure App Service'te temelde dağıtır. Okuma App Service ortamları tarafından sunulan yetenekleri büyük anlamak için [bir uygulama hizmeti ortamı nedir] [ WhatisASE] belgeleri. VNet okunur işletim avantajlarını bilmiyorsanız, [Azure Virtual Network SSS][virtualnetwork]. 

## <a name="overview"></a>Genel Bakış
Bir ana sanal ağınızı bir IP adresi veya bir internet erişilebilir uç nokta ile dağıtılabilir. IP adresi için bir sanal ağ adresi ayarlamak için bir iç yük Balancer(ILB) ile ana dağıtmanız gerekir. Ana bir ILB ile yapılandırıldığında, sağladığınız:

* kendi etki alanı veya alt etki alanı. Bu belge kolaylaştırmak için alt etki alanı olduğunu varsayar ancak her iki durumda da yapılandırabilirsiniz. 
* HTTPS için kullanılan sertifika
* Alt etki alanı için DNS yönetimi. 

Buna karşılık, şunun gibi şeyler yapabilirsiniz:

* güvenli bir şekilde bir siteye Site veya ExpressRoute VPN üzerinden erişim bulutta satır iş kolu uygulamaları gibi ana bilgisayar intranet uygulamaları
* Genel DNS sunucuları listelenmeyen ana bilgisayar uygulamalarını bulutta
* hangi ön uç uygulamalarınızı güvenli bir şekilde tümleştirebilirsiniz yalıtılmış Internet arka uç uygulamaları oluşturma

#### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevler
Bir ILB ana kullanırken yapamayacağınız bazı şeyler vardır. Bu konuları içerir:

* IPSSL kullanma
* belirli uygulamalar için IP adresleri atama
* satın alma ve portal üzerinden bir uygulama ile bir sertifika kullanıyor. Doğal olarak, hala bir sertifika yetkilisi sertifikalarıyla doğrudan edinebilir ve uygulamalarınızı, ancak Azure portalı üzerinden değil kullanın.

## <a name="creating-an-ilb-ase"></a>Bir ILB ana oluşturma
Bir ILB ana oluşturma bir ana normalde oluşturma çok farklı değildir. Daha derin bir tartışma bir ana oluşturma hakkında bilgi için bkz: [bir uygulama hizmeti ortamı oluşturmak nasıl][HowtoCreateASE]. Bir ILB ana oluşturma işlemi ana oluşturma sırasında bir VNet oluşturma veya varolan bir sanal ağ seçme arasında aynıdır. ILB ASE oluşturmak için: 

1. Azure portalında seçin **kaynak Oluştur -> Web + mobil -> uygulama hizmeti ortamı**.
2. Aboneliğinizi seçin.
3. Kaynak grubunu seçin veya oluşturun.
4. Bir VNet seçin veya oluşturun.
5. Bir sanal ağı seçerek bir alt ağ oluşturun.
6. Seçin **sanal ağ konumu sanal ağ yapılandırması ->** ve VIP türü için dahili olarak ayarlayın.
7. (Bu bu ana içinde oluşturulan uygulamaları için kullanılan alt etki alanı adıdır) bir alt etki alanı adı sağlayın.
8. Seçin **Tamam** ve ardından **oluşturma**.

![][1]

Sanal ağ bölmesinde olanak sağlayan seçeneğini iç VIP veya bir dış VIP arasında bir sanal ağ yapılandırması yok. Dış varsayılandır. Dış ayarlanmalıdır varsa, ana Internet erişilebilir VIP kullanır. Dahili seçerseniz, ana sanal ağınızın içinde bir IP adresinde bir ILB ile yapılandırılır. 

Dahili seçtikten sonra ana daha fazla IP adresleri ekleme yeteneği kaldırılır ve bunun yerine, ana etki alanının alt sağlamanız gerekir. ASE'de bir dış VIP ile o ana oluşturulan uygulamalar için alt etki alanı içinde ana adı kullanılır. Ana adlandırılmışsa ***contosotest*** ve o ana uygulamanızda adlı ***mytest***, alt etki alanı biçimidir ***contosotest.p.azurewebsites.net*** ve bu uygulama için URL ***mytest.contosotest.p.azurewebsites.net***. Ana adınızı Internal VIP türü ayarlarsanız, alt etki alanı içinde ana için kullanılmaz. Alt etki alanı açıkça belirtin. Alt etki alanı ise ***contoso.corp.net*** ve ana adlı içeren bir uygulama tarafından ***timereporting***, bu uygulama için URL ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Bir ILB ana uygulamalar
ILB ASE'de bir uygulama oluşturma ASE'de normalde bir uygulama oluşturma aynıdır. 

1. Azure portalında seçin **kaynak Oluştur -> Web + mobil -> Web** veya **mobil** veya **API uygulaması**.
2. Uygulamanın adını girin.
3. Aboneliğinizi seçin.
4. Kaynak grubunu seçin veya oluşturun.
5. Bir uygulama hizmeti Plan(ASP) oluşturun veya seçin. Yeni bir ASP oluşturuyorsanız, ana konumu olarak seçin ve ASP oluşturulmasını istediğiniz çalışan havuzu seçin. ASP oluşturduğunuzda, konum ve çalışan havuzu, ana seçin. Uygulama adı belirttiğinizde, uygulama adı altında bir alt etki alanı için ana alt etki alanına değiştirilir görürsünüz. 
6. **Oluştur**’u seçin. Seçtiğinizden emin olun **panoya Sabitle** Panonuzda göstermeyi uygulama istiyorsanız onay kutusunu. 

![][2]

Uygulama adı altında alt etki alanı adı, ana etki alanının alt yansıtacak şekilde güncelleştirilir. 

## <a name="post-ilb-ase-creation-validation"></a>POST ILB ana oluşturma doğrulama
Bir ILB ASE, ILB olmayan ASE’den biraz farklıdır. Zaten kendi DNS yönetmeniz gerekmez ve aynı zamanda HTTPS bağlantıları için kendi sertifikanızı sağlamak zorunda not almıştınız. 

Ana oluşturduktan sonra belirttiğiniz ve yeni bir öğe yok ve alt etki alanının alt etki alanı gösterir fark edeceksiniz **ayarı** adlı menüsü **ILB sertifika**. Ana HTTPS test kolay hale getiren otomatik olarak imzalanan bir sertifika ile oluşturulur. Portalı kullanarak kendi sertifikanızı HTTPS için sağlamanız gereken size bildirir ancak bu, alt etki alanı ile gider sertifikasına sahip olmasını öneririz. 

![][3]

Öğeleri yalnızca çalıştığınız ve bir sertifika oluşturmak nasıl bilmiyorsanız, otomatik olarak imzalanan bir sertifika oluşturmak için IIS MMC konsol uygulamasını kullanabilirsiniz. Bir kez oluşturulduktan sonra bir .pfx dosyası olarak dışarı aktarma ve ILB sertifika Arabiriminde yükleyin. Tarayıcınız otomatik olarak imzalanan sertifikası ile güvenli bir siteye eriştiğinizde, erişmeye çalıştığınız site sertifikayı doğrulamak için sorunları nedeniyle güvenli olmadığını bildiren bir uyarı verir. Bu uyarıyı önlemek istiyorsanız, tarayıcınız tarafından kabul edilen güven zinciri, alt etki alanı ile eşleşen ve doğru şekilde imzalanmış bir sertifika gerekir.

![][6]

Kendi sertifikaları ile akışını deneyin ve, ana hem HTTP hem de HTTPS erişimi test etmek istiyorsanız:

1. Ana ana oluşturulduktan sonra UI gidin **ana ayarları -> ILB sertifikalar ->**.
2. Sertifika pfx dosyasını seçerek ILB sertifika ayarlayın ve parola belirtin. Bu adım işlemek için işlem biraz zaman alır ve bir ölçeklendirme işlemi devam ediyor ileti görüntülenir.
3. ILB adresi almak için ana (**ana -> Özellikler sanal IP adresi ->**).
4. Bir web uygulaması oluşturulduktan sonra ana oluşturun. 
5. Bu VNET birinde (değil, aynı alt ağda ana veya şeyler sonu) yoksa, bir VM oluşturun.
6. DNS alt etki alanı için ayarlayın. Joker karakter ile alt etki alanı DNS sunucunuzun kullanın veya bazı basit testler yapmak istiyorsanız VIP IP adresi için web uygulaması adı ayarlamak için VM üzerinde hosts dosyasını düzenleyin. Alt etki alanı adı, ana olsaydı. ilbase.com ve mytestapp.ilbase.com ele alınması böylece web uygulama mytestapp yapılan kümesinin hosts dosyasında. (Windows, konaklar C:\Windows\System32\drivers\etc dosyasıdır\)
7. Bu VM üzerinde bir tarayıcı kullanın ve http://mytestapp.ilbase.com (veya web uygulaması adı, alt etki alanı ile ne olursa olsun) gidin.
8. Bu VM’de bir tarayıcı kullanın ve https://mytestapp.ilbase.com adresine gidin. Kendinden imzalı bir sertifika kullanıyorsanız, güvenlik eksikliği kabul etmeniz gerekir. 

IP adresi, ILB için özelliklerinizi sanal IP adresi olarak listelenir.

![][4]

## <a name="using-an-ilb-ase"></a>ILB ASE kullanır
#### <a name="network-security-groups"></a>Ağ Güvenlik Grupları
Bir ILB ana uygulamalarınız için ağ yalıtımı sağlar. Uygulamaları erişilebilir veya internet tarafından bilinen bile olmayan. Bu satırı iş kolu uygulamaları gibi intranet siteleri barındırmak için mükemmel bir yaklaşımdır. Daha erişimi kısıtlamak gerektiğinde, ağ düzeyinde erişimi denetlemek için ağ güvenlik Groups(NSGs) hala kullanabilirsiniz. 

Daha fazla erişimi kısıtlamak için Nsg'ler kullanmak istiyorsanız, ana çalışması için gereken iletişim bozmadığını emin olmanız gerekir. HTTP/HTTPS erişim yalnızca ana tarafından kullanılan ILB aracılığıyla olsa da, ana kaynaklar sanal ağ dışında hala bağlıdır. Hangi ağ erişimi hala gerekli olduğunu görmek için bkz: [bir uygulama hizmeti ortamına gelen trafiği denetleme] [ ControlInbound] ve [App Service ortamları için ağ yapılandırma ayrıntıları ExpressRoute][ExpressRoute]. 

Nsg'lerinizi yapılandırmak için Azure tarafından ana yönetmek için kullanılan IP adresini bilmeniz gerekir. Internet istekleri yapıyorsa bu IP adresi, ana giden IP adresinden de değil. Ana giden IP adresini, ana süresince statik kalır. Ana silip yeni bir IP adresi alırsınız. IP adresini bulmak için şu adrese gidin **ayarlar -> Özellikler** ve Bul **giden IP adresi**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Genel ILB ana Yönetimi
Bir ILB ana yönetme büyük ölçüde bir ana normalde yönetme ile aynıdır. Daha fazla ASP Örnekleri barındırmak ve daha yüksek miktarlarda HTTP/HTTPS trafiğini işlemek için ön uç sunucularınızı ölçeklendirmek için çalışan havuzlarınızı ölçeklendirin gerekir. Bir ana yapılandırma yönetme ile ilgili genel bilgiler için bkz: [uygulama hizmeti ortamını yapılandırma][ASEConfig]. 

Ek yönetim sertifikası ve DNS Yönetimi öğelerdir. Edinmeli ve ILB ana oluşturulduktan sonra HTTPS için kullanılan sertifikayı karşıya yüklemek ve süresi dolmadan önce değiştirin. Azure temel etki alanına sahip olduğundan, bir dış VIP ile ASEs için sertifikalar sağlayabilir. Bir ILB ana tarafından kullanılan alt etki alanı herhangi bir şey olabileceği için HTTPS için kendi sertifikanızı sağlamanız gerekir. 

#### <a name="dns-configuration"></a>DNS yapılandırması
Bir dış VIP kullanırken, DNS Azure tarafından yönetilir. ASE’nizde oluşturulan herhangi bir uygulama, genel bir DNS olan Azure DNS'e otomatik olarak eklenir. ILB ASE'de kendi DNS’inizi yönetmeniz gerekir. Contoso.corp.net gibi belirli bir alt DNS A ILB adresinizi işaret kayıtları oluşturmanız gerekir:

    * 
    *.SCM ftp yayımlama 


## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [App Service ortamları giriş][WhatisASE]

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
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
