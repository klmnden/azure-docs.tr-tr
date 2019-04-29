---
title: Coğrafi olarak dağıtılmış ölçek App Service ortamları - Azure ile
description: Traffic Manager ve App Service ortamları ile coğrafi dağıtım kullanarak uygulamaları yatay olarak ölçeklendirmeyi öğrenin.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 769e6b9936ad6d3cb963e208cec4c49813f2b6d3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130730"
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>App Service Ortamları ile Coğrafi Olarak Dağıtılmış Ölçek
## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerektiren çok büyük ölçekli uygulama senaryoları, tek bir uygulama dağıtımı için kullanılabilir işlem kaynak kapasitesini aşamaz.  Uygulamaları oylama, spor ve eğlence televised olayları tüm son derece büyük ölçekli gerektiren senaryolar örnekleridir. Yüksek ölçek gereksinimlerine uygulamalarına göz aşırı yük gereksinimlerini karşılamak için tek bir bölgede yanı sıra bölgeler genelinde yapılan birden çok uygulama dağıtımları ile yatay ölçeklendirme tarafından karşılanabilir.

App Service ortamları yatay ölçek genişletme için ideal bir platform olan.  Bir kez bir App Service yapılandırma seçili, bilinen istek hızı destekleyebilir ortamı, geliştiricilerin ek App Service istenen yoğun yük kapasitesi elde etmek için ortamları "Tanımlama Bilgisi kesici" bir şekilde dağıtabilirsiniz.

Örneğin, bir App Service ortamı yapılandırması üzerinde çalışan bir uygulamanın (RP'ler) saniye başına 20 bin isteklerini işlemek üzere test edilmiştir varsayalım.  100 bin RPS istenen yoğun yük kapasite ise daha sonra beş (5) App Service ortamları oluşturulabilir ve uygulamanın en fazla öngörülen yük işleyebilir emin olmak için yapılandırılmış.

Müşteriler genellikle bir özel (veya gösterim) etki alanı'nı kullanarak uygulamalara erişmesine, geliştiricilerin uygulama istekleri tüm App Service ortamı örnekleri arasında dağıtmak için bir yol gerekir.  Özel etki alanı kullanarak çözümlemek için bunu gerçekleştirmek için harika bir yoludur bir [Azure Traffic Manager profilini][AzureTrafficManagerProfile].  Traffic Manager profili, ayrı App Service ortamları tümünün işaret edecek şekilde yapılandırılabilir.  Traffic Manager otomatik olarak dağıtılmasını müşteriler tüm App Service ortamları Traffic Manager profili ayarları yük dengelemeyi göre işler.  Bu yaklaşım olup tüm App Service ortamları tek bir Azure bölgesinde dağıtılmış veya Azure bölgelerinde dünya çapında dağıtılan bağımsız olarak çalışır.

Ayrıca, müşteriler uygulamalarının gösterim etki alanı erişip olduğundan, müşteriler çalışan bir uygulamayı App Service ortamları sayısı farkında değildir.  Sonuç olarak geliştiriciler hızlı ve kolay bir şekilde, ekleyip kaldırabilirsiniz, App Service ortamlarında gözlemlenen trafiği yüküne göre.

Aşağıdaki kavramsal diyagram yatay olarak tek bir bölgede üç App Service ortamları arasında ölçeği bir uygulama gösterilmektedir.

![Kavramsal mimari][ConceptualArchitecture] 

Bu konunun geri kalanı için örnek uygulamayı birden fazla App Service ortamları kullanarak dağıtılmış bir tipoloji ayarlama ile ilgili adım adım anlatılmaktadır.

## <a name="planning-the-topology"></a>Topoloji planlama
Bir dağıtılmış uygulama Ayak izi kullanıma yapılandırmadan önce önceden birkaç parça bilgi sağlamak için yardımcı olur.

* **Uygulama için özel etki alanı:**  Müşteriler, uygulamaya erişmek için kullanacağı özel etki alanı adı nedir?  Örnek uygulama için özel etki alanı adıdır `www.scalableasedemo.com`
* **Traffic Manager etki alanı:**  Bir etki alanı adı oluşturulurken seçilmesi gerekir bir [Azure Traffic Manager profilini][AzureTrafficManagerProfile].  Bu ad ile birlikte *trafficmanager.net* soneki Traffic Manager tarafından yönetilen bir etki alanı girişi kaydetmek için kullanılır.  Örnek uygulama için adı seçilen olduğu *ase tanıtım ölçeklenebilir*.  Sonuç olarak, Traffic Manager tarafından yönetilen tam etki alanı adıdır *ase demo.trafficmanager.net ölçeklenebilir*.
* **Uygulama Ayak izi ölçeklendirme stratejisi:**  Uygulama Ayak izi, tek bir bölgede birden fazla App Service ortamları arasında dağıtılır?  Birden çok bölgede?  Bir karışımı ve eşleştirme her iki yaklaşımın?  Karar ne kadar iyi bir uygulamanın arka uç altyapısı destekleme rest ölçeklendirebilirsiniz yanı sıra burada müşteri trafiğinden kaynaklanan beklentilerini bağlı olmalıdır.  Örneğin, durum bilgisi olmayan bir % 100 uygulama ile birlikte uygulama yüksek düzeyde Azure bölgelerinde dağıtılan App Service ortamları ile çarpılır Azure bölgesi başına birden fazla App Service ortamları oluşan birleşimlerin kullanıldığı ölçeklendirilebilir.  15 + ortak olan Azure bölgeleri seçim yapabileceğiniz, müşterilerin gerçek anlamda bir dünya çapında hiper ölçekli uygulama Ayak izi oluşturabilirsiniz.  Bu makalede kullanılan örnek uygulama için bir tek bir Azure bölgesinde (Güney Orta ABD) üç App Service ortamları oluşturuldu.
* **App Service ortamları için adlandırma kuralı:**  Her App Service ortamı, benzersiz bir ad gerektirir.  Bir veya iki App Service ortamları, her bir App Service ortamı belirlemenize yardımcı olması için bir adlandırma kuralınızın bulunduğundan yardımcı olur.  Örnek uygulama için basit bir adlandırma kuralı kullanıldı.  Üç App Service ortamları adlarıdır *fe1ase*, *fe2ase*, ve *fe3ase*.
* **Uygulamalar için adlandırma kuralı:**  Uygulama birden çok örneğini dağıtılacak olduğundan, dağıtılan uygulamanın her örneği için bir ad gereklidir.  Bir bilinen küçük ama çok kullanışlı App Service ortamları aynı uygulama adı birden fazla App Service ortamları kullanılabilir özelliğidir.  Her App Service ortamı benzersiz etki alanı soneki olduğundan, geliştiricilerin tam aynı uygulama adı her ortamda yeniden kullanmayı da seçebilirsiniz.  Örneğin, bir geliştirici uygulamaları gibi adlı sahip olabilirsiniz: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*vb.  Örnek uygulama için yine de her uygulama örneği de benzersiz bir adı vardır.  Kullanılan uygulama örneği adları *webfrontend1*, *webfrontend2*, ve *webfrontend3*.

## <a name="setting-up-the-traffic-manager-profile"></a>Traffic Manager profili ayarlama
Bir uygulama birden çok örneğini birden fazla App Service ortamlarında uygulama dağıtıldıktan sonra Traffic Manager ile tek tek uygulama örnekleri kaydedilebilir.  Örnek uygulama için bir Traffic Manager profili için gerekli *ase demo.trafficmanager.net ölçeklenebilir* , yönlendirebilir müşteriler herhangi şu dağıtılan uygulama örnekleri:

* **webfrontend1.fe1ase.p.azurewebsites.NET:**  İlk App Service ortamında dağıtılan örnek uygulama örneği.
* **webfrontend2.fe2ase.p.azurewebsites.NET:**  İkinci App Service ortamında dağıtılan örnek uygulama örneği.
* **webfrontend3.fe3ase.p.azurewebsites.NET:**  Üçüncü App Service ortamında dağıtılan örnek uygulama örneği.

Birden fazla Azure App Service uç noktası, tüm çalışan kaydetmek için en kolay yolu **aynı** Powershell ile Azure bölgesi olan [Azure Resource Manager Traffic Manager desteği] [ ARMTrafficManager].  

İlk adım, bir Azure Traffic Manager profilini oluşturmaktır.  Aşağıdaki kod, profil için örnek uygulamayı nasıl oluşturulduğu gösterilmektedir:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Bildirim nasıl *RelativeDnsName* parametresi ayarlanmıştır *ase tanıtım ölçeklenebilir*.  Bu, nasıl etki alanı adı *ase demo.trafficmanager.net ölçeklenebilir* oluşturulur ve Traffic Manager profili ile ilişkilendirilmiş.

*TrafficRoutingMethod* parametre Yük Dengeleme İlkesi Traffic Manager, müşteri yükü tüm kullanılabilir uç noktalar arasında dağıtmak nasıl belirlemek için kullanacağı tanımlar.  Bu örnekte *ağırlıklı* yöntemi seçildi.  Bu, müşteri isteklerinde tüm her uç noktasıyla ilişkili göreli ağırlıklara göre kayıtlı uygulama uç noktaları yayılmasını neden olur. 

Profili oluşturulan, her uygulama örneği, bir yerel Azure uç noktası olarak profiline eklenir.  Aşağıdaki kod her ön uç web uygulaması başvuru getirir ve ardından bir Traffic Manager uç noktası sunar olarak her bir uygulama ekler *Targetresourceıd* parametresi.

    $webapp1 = Get-AzWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Nasıl bir çağrı fark *Ekle AzureTrafficManagerEndpointConfig* her tek tek uygulama örneği.  *Targetresourceıd* her Powershell komutunu parametresinde üç dağıtılan uygulama örneklerinden birine başvuruyor.  Traffic Manager profilini yük profili kayıtlı tüm üç uç noktalar arasında yayılır.

Üç bitiş noktalarının tümü için aynı değeri (10) kullanması *ağırlık* parametresi.  Bu Traffic Manager arasına yayarak müşteri isteklerindeki tüm üç uygulama örnekleri arasında oldukça eşit bir şekilde sonuçlanır. 

## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Traffic Manager etki alanına özel etki alanında uygulamanın işaret eden
Özel etki alanını Traffic Manager etki alanındaki uygulamanın işaret edecek şekilde son adım gerekli olduğu.  Örnek uygulama için işaret eden başka bir deyişle `www.scalableasedemo.com` adresindeki `scalable-ase-demo.trafficmanager.net`.  Bu adım, özel etki alanını yöneten etki alanı kayıt şirketinde şunların tamamlanmış gerekir.  

Kayıt şirketinizin etki alanı yönetim araçlarını kullanarak, özel etki alanını Traffic Manager etki alanına işaret ettiği oluşturulması gereken bir CNAME kaydeder.  Aşağıdaki resimde bu CNAME yapılandırma benzer bir örnek gösterilmektedir:

![Özel etki alanı için CNAME][CNAMEforCustomDomain] 

Bu konu başlığında ele alınmamaktadır olsa da, her bir tek tek uygulama örneği ile de kayıtlı özel etki alanınız olması gerektiğini unutmayın.  Aksi takdirde uygulama örneğine bir istek kolaylaştırır ve uygulama ile uygulamanın kayıtlı özel etki alanı yok ise isteği başarısız olur.  

Bu örnekte özel etki alanı olan `www.scalableasedemo.com`, ve her uygulama örneği ile ilişkili özel etki alanı vardır.

![Özel Etki Alanı][CustomDomain] 

Özel bir etki alanı ile Azure App Service uygulamaları kaydetme bir özeti için aşağıdaki makaleye bakın [özel etki alanlarını kaydetme][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Dağıtılmış topoloji çalışıyor
İstekleri için Traffic Manager ve DNS yapılandırmasını nihai sonucu olan `www.scalableasedemo.com` aşağıdakiler akar:

1. DNS arama yapmak tarayıcı veya cihaz `www.scalableasedemo.com`
2. Etki alanı kayıt şirketinde CNAME girişi DNS araması için Azure Traffic Manager yönlendirilmesi neden olur.
3. DNS arama yaptığınız *ase demo.trafficmanager.net ölçeklenebilir* karşı Azure Traffic Manager DNS sunucularından biri.
4. Yük Dengeleme İlkesi tabanlı ( *TrafficRoutingMethod* Traffic Manager profili oluştururken daha önce kullanılan parametre), Traffic Manager yapılandırılmış uç noktalardan biri seçin ve döndürmek için bu endpoint FQDN'si Tarayıcı veya cihaz.
5. Uç nokta FQDN'sini URL'si bir App Service ortamında çalışan bir uygulama örneğinin olduğundan, tarayıcı veya cihaz için bir IP adresi FQDN'yi çözümlemek için bir Microsoft Azure DNS sunucusu sorar. 
6. Tarayıcı veya cihaz, HTTP/S isteği IP adresine gönderir.  
7. İstek, App Service ortamları biri üzerinde çalışan uygulama örnekleri birinde ulaşırsınız.

Konsol resimde, üç örnek App Service ortamları (Bu durumda ikinci üç App Service ortamları) biri üzerinde çalışan bir uygulama örneği için başarılı bir şekilde çözme örnek uygulamanın özel etki alanı için DNS araması gösterilmektedir:

![DNS araması][DNSLookup] 

## <a name="additional-links-and-information"></a>Ek bağlantıları ve bilgileri
PowerShell belgeleri [Azure Resource Manager Traffic Manager desteği][ARMTrafficManager].  

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: ../app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
