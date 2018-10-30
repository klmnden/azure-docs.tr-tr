---
title: Oluşturma bir App Service ortamı v1
description: Bir app service ortamı v1 oluşturma akış açıklaması
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 1df3b790d0c6c0f597a8559551ff5e42c9f110e4
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230276"
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Oluşturma bir App Service ortamı v1 

> [!NOTE]
> Bu makale, App Service ortamı v1 hakkında yöneliktir. App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).
> 

### <a name="overview"></a>Genel Bakış
App Service ortamı (ASE), çok kiracılı Damgalar içinde kullanılabilir olmayan bir Gelişmiş Yapılandırma özelliği sunan Azure App Service Premium hizmet seçeneğidir. ASE özelliği, Azure App Service'in bir müşterinin sanal ağa temelde dağıtır. Okuma App Service ortamları tarafından sunulan daha yüksek bir anlayış kazanmak için [bir App Service ortamı nedir] [ WhatisASE] belgeleri.

### <a name="before-you-create-your-ase"></a>ASE'NİZİN oluşturmadan önce
Değiştiremezsiniz koca farkında olmak önemlidir. Oluşturulduktan sonra ASE'nizi hakkında değiştiremezsiniz. Bu yönleri şunlardır:

* Konum
* Abonelik
* Kaynak Grubu
* Kullanılan sanal ağ
* Kullanılan alt ağ 
* Alt ağ boyutu

Bir sanal ağ seçme ve bir alt ağ belirtme yaptığınızda emin gelecekteki büyümesine uyum sağlayacak kadar büyük. 

### <a name="creating-an-app-service-environment-v1"></a>Service ortamı v1 uygulama oluşturma
Bir App Service ortamı v1 oluşturmak için Azure Marketi için arama yapabilirsiniz ***App Service ortamı v1***, veya Git **kaynak Oluştur** -> **Web + mobil**  ->  **App Service ortamı**. Bir ASEv1 oluşturmak için:

1. ASE'NİZİN adını sağlayın. ASE için belirttiğiniz ad, ASE içinde oluşturulan uygulamalar için kullanılır. Ase'nin adı appsvcenvdemo ise, alt etki alanı adı şu şekilde olacaktır: *appsvcenvdemo.p.azurewebsites.net*. Bu nedenle adlı bir uygulama oluşturduysanız *mytestapp*, adresindeki adreslenebilir *mytestapp.appsvcenvdemo.p.azurewebsites.net*. Boşluk ASE'nizi adını kullanamaz. Etki alanı adı, adında büyük harf karakterler kullanırsanız, adı toplam küçük harfli sürümünü olacaktır. Bir ILB kullanırsanız, ASE adınız alt etki alanı içinde kullanılmaz, ancak bunun yerine ASE oluşturma sırasında açıkça belirtilir.
   
    ![][1]
2. Aboneliğinizi seçin. ASE'NİZİN kullandığınız abonelik, bu ASE içinde oluşturduğunuz tüm uygulamalar için de geçerli olacaktır. Başka bir abonelikte bir vnet'e ASE'nizi yerleştirilemiyor.
3. Seçin veya yeni bir kaynak grubu belirtin. ASE'niz için kullanılan kaynak grubunu, sanal ağ için kullanılan aynı olması gerekir. Önceden var olan bir sanal ağ'ı seçerseniz ASE'niz için kaynak grubu seçimi, ağınızın yansıtacak şekilde güncelleştirilir.
   
    ![][2]
4. Sanal ağ ve konum seçimlerinizi yapın. Önceden var olan bir VNet seçin veya yeni bir VNet oluşturun seçebilirsiniz. Ardından yeni bir sanal ağ seçerseniz bir ad ve konum belirtin. Yeni sanal ağ adres aralığı 192.168.250.0/23 ve adlı bir alt ağ gerekir **varsayılan** 192.168.250.0/24 tanımlanır. Ayrıca, önceden mevcut olan bir Klasik veya Resource Manager sanal ağı seçebilirsiniz. VIP türü seçimi, ASE'NİZİN doğrudan internet'ten (Dış) erişilebilen bir iç yük dengeleyici (ILB) kullanıyorsa veya belirler. Bunları okuyun hakkında daha fazla bilgi edinmek için [bir App Service ortamı ile bir iç Load Balancer'ı kullanarak][ILBASE]. Dış VIP türü seçerseniz, sistem IPSSL amacıyla oluşturulur kaç dış IP adreslerini seçebilirsiniz. Dahili seçerseniz ASE'niz kullanan alt etki alanı belirtmek gerekir. Ase'ler kullanan sanal ağlara dağıtılabilir *ya da* genel adres aralıklarının *veya* RFC1918 adres alanları (yani özel adresler). Ortak adres aralığı ile bir sanal ağ kullanmak için önceden sanal ağ oluşturmanız gerekir. Önceden var olan bir sanal ağ seçtiğinizde ASE oluşturma sırasında yeni bir alt ağ oluşturmanız gerekir. **Portalda, önceden oluşturulmuş bir alt ağ kullanamazsınız. Resource manager şablonu kullanarak ASE'nizi oluşturmak istiyorsanız, önceden mevcut olan bir alt ağ ile bir ASE oluşturabilirsiniz.** Bir ASE şablon kullanımdan buradaki bilgileri oluşturmak için [şablondan bir App Service ortamı oluşturma] [ ILBAseTemplate] ve burada [şablondanbirILBAppServiceortamıoluşturma] [ASEfromTemplate].

### <a name="details"></a>Ayrıntılar
Bir ASE 2 ön uçlar ve 2 çalışan ile oluşturulur. Ön uçlar HTTP/HTTPS uç noktaları olarak davranır ve uygulamalarınızı rolleri olan çalışanlar için trafiği göndermek. ASE oluşturulduktan sonra miktarı ayarlamak ve bu kaynak havuzlarının otomatik ölçeklendirme kurallarını bile ayarlama yapabilirsiniz. Yönetim ve izleme bir App Service ortamı el ile ölçeklendirme etrafında daha fazla ayrıntı için buraya gidin: [bir App Service ortamını yapılandırma][ASEConfig] 

Bir ASE yalnızca ASE tarafından kullanılan alt ağ içinde bulunabilir. Alt ağ, ASE'nin dışında her şey için kullanılamaz

### <a name="after-app-service-environment-v1-creation"></a>App Service ortamı v1 oluşturulduktan sonra
ASE oluşturulduktan sonra ayarlayabilirsiniz:

* Ön uçlar miktarı (en az: 2)
* Çalışanları miktarı (en az: 2)
* IP SSL için kullanılabilir IP adreslerinin miktar
* Ön uçlar veya çalışanlar tarafından kullanılan kaynak boyutları işlem (P2 ön uç en küçük boyut'dır)

El ile ölçeklendirme, yönetim ve burada App Service ortamları izleme ile ilgili daha fazla ayrıntı: [bir App Service ortamını yapılandırma][ASEConfig] 

Otomatik ölçeklendirme hakkında bilgi için yok burada bir kılavuz: [bir App Service ortamı için otomatik ölçeklendirmeyi yapılandırma][ASEAutoscale]

Özelleştirme veritabanı ve depolama gibi kullanılabilir olmayan ek bağımlılıklar vardır. Bunlar Azure tarafından işlenen ve sistemiyle birlikte gelir. Sistem deposu tüm App Service ortamı için 500 GB'a kadar destekler ve veritabanı tarafından sistem ölçeği gerektiği gibi Azure tarafından ayarlanır.

## <a name="getting-started"></a>Başlarken
App Service ortamı v1 ile çalışmaya başlamak için bkz: [App Service ortamı v1'e giriş][WhatisASE]

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
