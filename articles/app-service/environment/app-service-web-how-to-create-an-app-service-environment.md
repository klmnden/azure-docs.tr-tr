---
title: "Bir uygulama hizmeti oluşturma ortamını v1"
description: "Bir uygulama hizmeti ortamı v1 oluşturma akış açıklaması"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: ef0dc1b820f42b73af3af3882085729ecc21230c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Bir uygulama hizmeti oluşturma ortamını v1 

> [!NOTE]
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır. Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
> 

### <a name="overview"></a>Genel Bakış
Uygulama hizmeti ortamı (ana) bir Premium hizmeti çok kiracılı Damgalar kullanılamaz bir Gelişmiş Yapılandırma özelliği sunan Azure App Service seçeneğidir. ANA özelliği, Azure uygulama hizmeti bir müşterinin sanal ağınıza temelde dağıtır. Okuma App Service ortamları tarafından sunulan yetenekleri büyük anlamak için [bir uygulama hizmeti ortamı nedir] [ WhatisASE] belgeleri.

### <a name="before-you-create-your-ase"></a>Ana oluşturmadan önce
Değiştiremezsiniz şey haberdar olmanız önemlidir. Oluşturulduktan sonra ana hakkında değiştiremezsiniz bu yönleri şunlardır:

* Konum
* Abonelik
* Kaynak Grubu
* Kullanılan sanal ağ
* Kullanılan alt ağ 
* Alt ağ boyutu

Bir VNet çekme ve bir alt ağ belirterek yaptığınızda emin gelecekteki büyümesine uyum sağlayacak kadar büyük. 

### <a name="creating-an-app-service-environment-v1"></a>Hizmet ortamı v1 bir uygulama oluşturma
Gereken Azure Market aramak için bir uygulama hizmeti ortamı v1 oluşturmak için ***uygulama hizmeti ortamı v1***, veya yeni giderek -> Web + mobil -> uygulama hizmeti ortamı. Bir ASEv1 oluşturmak için:

1. ANA adını sağlayın. Ana için belirtilen ad ana oluşturulan uygulamalar için kullanılır. ANA adını appsvcenvdemo ise alt etki alanı adı olması. *appsvcenvdemo.p.azurewebsites.net*. Bu nedenle adlı bir uygulama oluşturduysanız *mytestapp* adresindeki adreslenebilir sonra *mytestapp.appsvcenvdemo.p.azurewebsites.net*. Beyaz alan adına, ana kullanamazsınız. Büyük harf karakterler adı kullanırsanız, etki alanı adı bu adı toplam küçük harfli sürümünü olacaktır. Bir ILB kullanırsanız, ardından ana adınızı alt etki alanı içinde kullanılmaz, ancak bunun yerine ana oluşturma sırasında açıkça belirtilmiştir
   
    ![][1]
2. Aboneliğinizi seçin. Ana için kullanılan abonelikle o ana tüm uygulamaları ile oluşturulan bir yetenektir. Başka bir abonelikte bir VNet içinde ana yerleştirilemiyor
3. Seçin veya yeni bir kaynak grubu belirtin. Kaynak grubu için ana kullanılan ağınız için kullanılan aynı olması gerekir. Önceden var olan bir sanal ağ seçerseniz, ana için kaynak grubu seçimi, sanal ağınızı yansıtacak şekilde güncelleştirilir.
   
    ![][2]
4. Sanal ağ ve konum seçimlerinizi yapın. Yeni bir VNet oluşturun veya varolan bir sanal ağ seçin seçebilirsiniz. Ardından, yeni bir sanal ağ seçerseniz bir ad ve konum belirtebilirsiniz. Yeni sanal ağ adres aralığı 192.168.250.0/23 adlı bir alt ağ olacaktır **varsayılan** 192.168.250.0/24 tanımlanır. Önceden varolan Klasik veya Resource Manager Vnet'i kısaca seçebilirsiniz. VIP türü seçimi, ana doğrudan (harici) Internet üzerinden erişilebiliyorsa veya bir iç yük dengeleyici (ILB) kullanıyorsa, belirler. Bunları okuyun hakkında daha fazla bilgi edinmek için [bir uygulama hizmeti ortamı ile bir iç yük dengeleyici kullanarak][ILBASE]. Dış VIP türü seçerseniz, sistem IPSSL amacıyla oluşturulur kaç dış IP adreslerini seçebilirsiniz. Dahili seçerseniz, ana kullanacağı alt etki alanı belirtmeniz gerekir. ASEs kullanan sanal ağları içinde dağıtılabilir *ya da* ortak adres aralıklarını *veya* RFC1918 adres alanları (özel adresleri). Bir sanal ağ ortak adres aralığı ile kullanabilmeniz için önceden VNet oluşturmanız gerekir. Önceden var olan bir VNet seçtiğinizde ana oluşturma sırasında yeni bir alt ağı oluşturmanız gerekir. **Portalda önceden oluşturulmuş bir alt ağ kullanılamıyor. Resource manager şablonu kullanarak, ana oluşturursanız, önceden var olan bir alt ağ ile bir ana oluşturabilirsiniz.** Bir ana şablon kullanımdan bilgiyi burada oluşturmak için [şablonundan bir uygulama hizmeti ortamı oluşturma] [ ILBAseTemplate] ve burada [şablonundanbirILBuygulamahizmetiortamıoluşturma] [ASEfromTemplate].

### <a name="details"></a>Ayrıntılar
Bir ana 2 ön uçlar ve 2 çalışan ile oluşturulur. Ön uçlar HTTP/HTTPS uç noktalar olarak davranmasına ve uygulamalarınızı barındırmak rolleri olan çalışanlar için trafiği gönderebilir. Ana oluşturulduktan sonra miktarı ayarlayabilir ve hatta ayarlama Bu kaynak havuzlarının otomatik ölçeklendirme kurallarını kullanabilirsiniz. Yönetim ve izleme bir uygulama hizmeti ortamı el ile ölçeklendirme geçici daha fazla ayrıntı için buraya gidin: [bir uygulama hizmeti ortamını yapılandırma][ASEConfig] 

Yalnızca bir ana ana tarafından kullanılan alt ağ içinde bulunabilir. Alt ağ ana dışında her şey için kullanılamaz

### <a name="after-app-service-environment-v1-creation"></a>Uygulama hizmeti ortamı v1 oluşturulduktan sonra
Ana oluşturulduktan sonra ayarlayabilirsiniz:

* Ön uçlar miktarını (en az: 2)
* Çalışanlar miktarını (en az: 2)
* IP SSL için kullanılabilir IP adresleri miktarı
* Ön uçlar veya çalışanlar tarafından kullanılan kaynak boyutları işlem (ön uç minimum boyut olan P2)

El ile ölçeklendirme, yönetim ve burada App Service ortamları izleme etrafında daha fazla ayrıntı: [bir uygulama hizmeti ortamını yapılandırma][ASEConfig] 

Otomatik ölçeklendirme hakkında bilgi için yoktur burada bir kılavuz: [otomatik ölçeklendirme bir uygulama hizmeti ortamı için yapılandırma][ASEAutoscale]

Depolama ve veritabanı gibi özelleştirme için kullanılabilir değil ek bağımlılıklar vardır. Bunlar Azure tarafından işlenen ve sistemi ile gelir. Tüm uygulama hizmeti ortamı için 500 GB'a kadar sistem depolama destekler ve veritabanı sistem ölçek tarafından gerektiği şekilde Azure tarafından ayarlanır.

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamı v1 ile çalışmaya başlamak için bkz: [uygulama hizmeti ortamı v1 giriş][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
