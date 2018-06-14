---
title: App Service Ortamları ile Coğrafi Olarak Dağıtılmış Ölçek
description: Trafik Yöneticisi ve App Service ortamları coğrafi dağıtım kullanarak uygulamaları yatay ölçek öğrenin.
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
ms.openlocfilehash: 21f747239e565aba79a84c8e946a71e306b64968
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23836836"
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>App Service Ortamları ile Coğrafi Olarak Dağıtılmış Ölçek
## <a name="overview"></a>Genel Bakış
Çok büyük ölçekli gerektiren uygulama senaryoları tek bir uygulama dağıtım için kullanılabilir olan işlem kaynak kapasitesini aşamaz.  Uygulamaları oylama, spor olaylar ve televised eğlence olaylar tüm son derece yüksek ölçekli gerektiren senaryolara verilen örneklerdir. Yüksek ölçekli gereksinimleri, birden çok uygulama dağıtımı tek bir bölge içinde ve aynı zamanda bölgelere, aşırı yük gereksinimlerini karşılamak için yapılan uygulamalarını, yatay olarak ölçekleme tarafından karşılanabilir.

Uygulama hizmeti ortamları yatay ölçek genişletme için ideal bir platform ' dir.  Bir kez bir uygulama hizmeti yapılandırması seçili bilinen istek hızı destekleyen ortamı, geliştiricilerin ek App Service istenen yoğun yük kapasitesi elde etmek için ortamları "Tanımlama Bilgisi kesici" şekilde dağıtabilirsiniz.

Örneğin bir uygulama hizmeti ortamı yapılandırması üzerinde çalışan bir uygulama (RPS) saniyede 20 K isteklerini işlemek için test varsayalım.  İstenen yoğun yük kapasitesi 100 K RPS ise, sonra beş (5) App Service ortamları oluşturulabilir ve uygulamanın en fazla tahmini yükleme işleyebilir emin olmak için yapılandırılmış.

Müşteriler genellikle bir özel (ya da gösterim) etki alanını kullanan uygulamalara erişim olduğundan, geliştiricilerin uygulama isteklerini tüm uygulama hizmeti ortamı örnekleri arasında dağıtmak için bir yol gerekir.  Bunu gerçekleştirmek için harika bir özel etki alanı kullanarak çözümlemeye yoludur bir [Azure Traffic Manager profilini][AzureTrafficManagerProfile].  Trafik Yöneticisi profili, tek tek uygulama hizmeti ortamları hiç işaret edecek şekilde yapılandırılabilir.  Trafik Yöneticisi otomatik olarak dağıtma müşteriler tüm App Service ortamları Yük Dengeleme trafik Yöneticisi profili ayarlarında göre işler.  Bu yaklaşım olup tüm App Service ortamları tek bir Azure bölgesinde dağıtılan veya birçok Azure bölgesinde dünya çapında dağıtılan bağımsız olarak çalışır.

Ayrıca, müşteriler uygulamaları üzerinden gösterim etki alanına erişim bu yana müşteriler bir uygulamayı çalıştıran uygulama hizmeti ortamları sayısı farkında değildir.  Sonuç olarak geliştiricilerin hızla ve kolayca, ekleyip kaldırabilirsiniz, App Service ortamları gözlemlenen trafiği yüküne göre.

Aşağıdaki alanlara yönelik kavramsal diyagram yatay çıkışı tek bir bölge içinde üç uygulama hizmeti ortamları boyunca ölçeklendirilmiş bir uygulama gösterilmektedir.

![Kavramsal mimarisi][ConceptualArchitecture] 

Bu konunun geri kalanında birden fazla App Service ortamları kullanarak örnek uygulama için Dağıtılmış bir topolojiyi ayarlama ile ilgili adım adım anlatılmaktadır.

## <a name="planning-the-topology"></a>Topoloji planlama
Dağıtılmış uygulama ayak izini çıkışı yapılandırmadan önce önceden birkaç parça bilgiye sahip yardımcı olabilir.

* **Uygulama için özel etki alanı:** müşteriler uygulamaya erişmek için kullanacağı özel etki alanı adı nedir?  İçin örnek uygulama özel etki alanı adıdır *www.scalableasedemo.com*
* **Trafik yöneticisi etki alanı:** bir etki alanı adı oluştururken seçilmesi gerekir bir [Azure Traffic Manager profilini][AzureTrafficManagerProfile].  Bu ad ile birleştirilmiş *trafficmanager.net* soneki trafik Yöneticisi tarafından yönetilen bir etki alanı girişi kaydeder.  Örnek uygulama için ad seçilmiş olan *ana demo ölçeklenebilir*.  Sonuç olarak, trafik Yöneticisi tarafından yönetilen tam etki alanı adıdır *ana demo.trafficmanager.net ölçeklenebilir*.
* **Uygulama ayak ölçeklendirmeye yönelik stratejisi:** uygulama ayak tek bir bölge içinde birden fazla App Service ortamları arasında dağıtılmış?  Birden çok bölgeye?  Bir karışımını-ve-eşleşme her iki yaklaşımın?  Karar ne kadar iyi bir uygulamanın arka uç altyapısı destekleme rest ölçeklendirebilirsiniz yanı sıra burada müşteri trafiği kaynaklanan beklentilerini bağlı olmalıdır.  Örneğin, durum bilgisi olmayan bir % 100 uygulaması ile bir uygulama yüksek düzeyde birçok Azure bölgesinde dağıtılan uygulama hizmeti ortamları çarpılmasıyla Azure bölgesi başına birden fazla App Service ortamları birleşimini kullanarak genişletilebilir.  15 + ortak Azure bölgeler ile seçim yapabileceğiniz, müşterilerin gerçekten dünya çapında hiper ölçekli uygulama ayak izini oluşturabilirsiniz.  Bu makale için kullanılan örnek uygulama için üç uygulama hizmeti ortamları bir tek Azure bölgesinde (Orta Güney ABD) oluşturuldu.
* **Uygulama hizmeti ortamları için adlandırma:** her uygulama hizmeti ortamı benzersiz bir ad gerektirir.  Bir veya iki uygulama hizmeti ortamları her uygulama hizmeti ortamı belirlemenize yardımcı olması için bir adlandırma kuralı yararlıdır.  Örnek uygulama için basit bir adlandırma kuralı kullanıldı.  Üç uygulama hizmeti ortamları adlarının *fe1ase*, *fe2ase*, ve *fe3ase*.
* **Uygulamalar için adlandırma:** uygulama birden çok örneğini dağıtılacak olmadığından dağıtılan uygulama her örneği için bir ad gerekli.  Bir bilinen az, ancak çok kullanışlı uygulama hizmeti ortamları aynı uygulama adı birden fazla App Service ortamları kullanılabilir özelliğidir.  Her uygulama hizmeti ortamı benzersiz etki alanı soneki olduğundan, geliştiricilerin her ortamda tam aynı uygulama adı yeniden kullanmayı seçebilirsiniz.  Örneğin, bir geliştirici uygulamalar gibi adlı sahip olabilir: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, vb..  Örnek uygulama için yine de her bir uygulama örneği de benzersiz bir ada sahip.  Kullanılan uygulama örneği adları *webfrontend1*, *webfrontend2*, ve *webfrontend3*.

## <a name="setting-up-the-traffic-manager-profile"></a>Trafik Yöneticisi profili ayarlama
Bir uygulama birden çok örneği üzerinde birden fazla App Service ortamları dağıtıldığında, tek tek uygulama örnekleri ile trafik Yöneticisi kaydedilebilir.  Örnek uygulama için bir trafik Yöneticisi profili için gerekli *ana demo.trafficmanager.net ölçeklenebilir* , yönlendirebilirsiniz müşteriler herhangi şu dağıtılmış bir uygulamayı örnekleri:

* **webfrontend1.fe1ase.p.azurewebsites.NET:** ilk uygulama hizmeti ortamı üzerinde dağıtılan örnek uygulama örneği.
* **webfrontend2.fe2ase.p.azurewebsites.NET:** ikinci uygulama hizmeti ortamı üzerinde dağıtılan örnek uygulama örneği.
* **webfrontend3.fe3ase.p.azurewebsites.NET:** üçüncü uygulama hizmeti ortamı üzerinde dağıtılan örnek uygulama örneği.

Birden fazla Azure Uygulama Hizmeti uç noktası, içindeki tüm çalışan kaydetmek için en kolay yolu **aynı** Azure bölgesi Powershell ile olan [Azure Kaynak Yöneticisi trafik Yöneticisi Destek][ARMTrafficManager].  

İlk adım, bir Azure Traffic Manager profilini oluşturmaktır.  Aşağıdaki kod, profil için örnek uygulama nasıl oluşturulduğuna gösterir:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Bildirim nasıl *RelativeDnsName* parametresi ayarlandığı *ana demo ölçeklenebilir*.  Bunun nasıl etki alanı adı *ana demo.trafficmanager.net ölçeklenebilir* oluşturulur ve bir trafik Yöneticisi profili ile ilişkilendirilmiş.

*TrafficRoutingMethod* parametre tanımlar Yük Dengeleme İlkesi trafik Yöneticisi nasıl müşteri yük tüm kullanılabilir uç noktaları arasında yayılan belirlemek için kullanır.  Bu örnekte *Weighted* yöntemi seçildi.  Bu tüm her bitiş noktasıyla ilişkili göreli ağırlığı göre kayıtlı uygulama uç noktaları yayılmasını müşteri isteklerine neden olur. 

Oluşturulan profiliyle her uygulama örneği profiline bir yerel Azure uç noktası olarak eklenir.  Aşağıdaki kodu her ön uç web uygulaması için bir başvuru getirir ve bir trafik Yöneticisi uç noktası tarafından yolu olarak her uygulama ekler *uç noktası Targetresourceıd* parametresi.

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Nasıl yapılan bir çağrı fark *Ekle AzureTrafficManagerEndpointConfig* her tek tek uygulama örneği.  *Uç noktası Targetresourceıd* her Powershell komutunu parametresinde üç dağıtılmış bir uygulamayı örneklerden birini başvuruyor.  Trafik Yöneticisi profili yükleme profilinde kayıtlı tüm üç uç noktaları arasında yayılır.

Üç bitiş noktalarının tümü için aynı değeri (10) kullanması *ağırlık* parametresi.  Bu trafik Yöneticisi yayılmasından müşteri isteklerine tüm üç uygulama örneklerde görece eşit olur. 

## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Uygulamanın özel etki alanı trafik yöneticisi etki alanına işaret eden
Son gerekli özel etki alanı trafik yöneticisi etki alanındaki uygulamanın işaret edecek şekilde adımdır.  Örnek uygulama için bu işaret eden anlamına gelir *www.scalableasedemo.com* adresindeki *ana demo.trafficmanager.net ölçeklenebilir*.  Bu adım, özel etki alanı yöneten etki alanı kayıt şirketi ile tamamlanması gerekiyor.  

Kayıt şirketinizin etki alanı Yönetim Araçları'nı kullanarak bir CNAME özel etki alanı trafik yöneticisi etki alanına işaret ettiği oluşturulacak gereksinimlerini kaydeder.  Aşağıdaki resimde bu CNAME yapılandırma benzer bir örnek gösterilmektedir:

![Özel etki alanı için CNAME][CNAMEforCustomDomain] 

Bu konuda ele değil de, her tek tek uygulama örneği ile de kayıtlı özel etki alanı sahip olması gerektiğini unutmayın.  Aksi takdirde, uygulama örneğini istekte bulunur ve uygulamanın uygulamayla kayıtlı özel etki alanı yok, isteği başarısız olur.  

Bu örnekte özel etki alanıdır *www.scalableasedemo.com*, ve ilişkili özel etki alanı her uygulama örneği vardır.

![Özel Etki Alanı][CustomDomain] 

Azure uygulama hizmeti uygulamaları ile özel bir etki alanı kaydı bir özeti için aşağıdaki makaleye bakın [özel etki alanlarını kaydetme][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Dağıtılmış topoloji çalışıyor
Trafik Yöneticisi ve DNS yapılandırması nihai sonucu isteklerine için olan *www.scalableasedemo.com* aşağıdakiler akar:

1. DNS araması yapmak bir tarayıcı veya aygıt *www.scalableasedemo.com*
2. Etki alanı kayıt CNAME girdisi Azure trafik Yöneticisi için yeniden yönlendirilmesi için DNS araması neden olur.
3. DNS araması yapılan *ana demo.trafficmanager.net ölçeklenebilir* Azure trafik Yöneticisi DNS sunucularını birini karşı.
4. Yük Dengeleme İlkesi göre ( *TrafficRoutingMethod* trafik Yöneticisi profili oluştururken daha önce kullanılan parametresi), trafik Yöneticisi yapılandırılmış uç birini seçin ve bu uç FQDN'sini tarayıcı veya aygıt geri dönün.
5. Uç nokta FQDN'sini bir uygulama hizmeti ortamı üzerinde çalışan bir uygulama örneğine URL'sini olduğundan, tarayıcı veya aygıt IP adresine FQDN'yi çözümlemek için bir Microsoft Azure DNS sunucusu sorar. 
6. HTTP/S isteği, tarayıcı veya aygıt IP adresine gönderir.  
7. İstek, App Service ortamları biri üzerinde çalışan uygulama örnekleri birinde ulaşırsınız.

Konsol resimde üç örnek uygulama hizmeti ortamları (Bu durumda ikinci üç uygulama hizmeti ortamları,) biri üzerinde çalışan bir uygulama örneği başarıyla çözümleme örnek uygulamanın özel etki alanı için DNS araması gösterilmektedir:

![DNS araması][DNSLookup] 

## <a name="additional-links-and-information"></a>Ek bağlantıları ve bilgileri
Belgeleri, Powershell [Azure Kaynak Yöneticisi trafik Yöneticisi Destek][ARMTrafficManager].  

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
