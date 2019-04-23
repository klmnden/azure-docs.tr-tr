---
title: Traffic Manager uç nokta türleri | Microsoft Docs
description: Bu makalede, Azure Traffic Manager ile kullanılan uç noktalarını farklı türleri açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 3f41edef56b238d8789264d00d73998794fec7eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60188722"
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları
Microsoft Azure Traffic Manager, farklı veri merkezlerinde çalışan uygulama dağıtımları için ağ trafiğini nasıl dağıtıldığını denetlemenize olanak sağlar. Trafik Yöneticisi'nde ' de her uygulama dağıtımı 'endpoint' olarak yapılandırın. Traffic Manager DNS isteği aldığında, DNS yanıtında döndürmek için kullanılabilir uç nokta seçer. Traffic manager, geçerli uç nokta durumu ve trafik yönlendirme yöntemi seçimi alır. Daha fazla bilgi için [Traffic Manager nasıl çalışır](traffic-manager-how-it-works.md).

Uç nokta Traffic Manager tarafından desteklenen üç tür vardır:
* **Azure uç noktalarını** Azure'da barındırılan hizmetler için kullanılır.
* **Dış uç noktalar** IPv4/IPv6 adresleri veya, şirket içi ya da olabilir Azure dışında veya farklı bir barındırma sağlayıcısıyla barındırılan hizmetler için kullanılır.
* **İç içe uç noktalar** daha büyük, daha karmaşık dağıtımlar gereksinimlerini desteklemek için daha esnek trafik yönlendirme düzenleri oluşturmak için Traffic Manager profillerini birleştirmek için kullanılır.

Uç noktaları farklı türdeki tek bir Traffic Manager profilinde nasıl birleştirilir hiçbir sınırlama yoktur. Her profil, uç nokta türlerinin bir karışımını içerebilir.

Aşağıdaki bölümlerde her uç nokta türü daha derinlemesine açıklanmaktadır.

## <a name="azure-endpoints"></a>Azure uç noktaları

Azure uç noktaları, Azure tabanlı Hizmetleri Traffic Manager'da için kullanılır. Aşağıdaki Azure kaynak türleri desteklenir:

* PaaS bulut Hizmetleri.
* Web Apps
* Web uygulaması yuvaları
* PublicIPAddress kaynaklarını (hangi bağlanabilir Vm'lere doğrudan ya da Azure Load Balancer üzerinden). Publicıpaddress bir Traffic Manager profilinde kullanılmak üzere atanmış bir DNS adı olmalıdır.

PublicIPAddress kaynaklarını Azure Resource Manager kaynaklarıdır. Klasik dağıtım modelinde bulunmaz. Bu nedenle yalnızca desteklenen Traffic Manager'ın Azure Resource Manager deneyimleri değildirler. Bir uç nokta türleri, Resource Manager ve klasik dağıtım modeli desteklenir.

'Klasik' Iaas VM, bulut hizmeti veya Web uygulaması durduruldu ve başlatılan Azure uç noktaları kullanırken, Traffic Manager algılar. Bu durum uç nokta durumu yansıtılır. Bkz: [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status) Ayrıntılar için. Temel alınan hizmet durdurulduğunda, Traffic Manager uç nokta durum denetimlerinin veya doğrudan trafiği uç noktasına gerçekleştirmez. Traffic Manager faturalama olayı yok, durdurulan bir örnek için oluşur. Hizmet yeniden başlatıldığında, faturalandırma devam eder ve uç nokta trafiği almak uygun. Bu algılama yöntemi, Publicıpaddress uç noktaları için geçerli değildir.

## <a name="external-endpoints"></a>Dış uç noktaları

Dış uç noktaları ya da IPv4/IPv6 adresleri veya, Azure dışındaki hizmetleri için kullanılır. IPv4/IPv6 adresi uç noktalarını traffic manager, bunlar için bir DNS adı gerek kalmadan uç noktaları durumunu denetlemek izin verir. Sonuç olarak, Traffic Manager A/AAAA kayıt sorgularla uç noktanın yanıt olarak döndürürken yanıt verebilir. Azure dışındaki hizmetleri, bir barındırılan hizmet şirket içi içerebilir veya farklı bir sağlayıcı. Dış uç noktalar ayrı ayrı kullanılabilir veya dış uç noktalar yalnızca olabilen IPv4 veya IPv6 adresleri olarak belirtilen uç noktaları dışında aynı Traffic Manager profilindeki Azure uç noktaları ile birlikte. Azure uç noktalarını dış uç noktaları ile birleştirerek, çeşitli senaryolara olanak tanır:

* Artıklığı ya da mevcut bir şirket içi uygulama için Azure'ı kullanarak bir aktif-aktif veya Aktif-Pasif yük devretme modeli sağlar. 
* İlişkili bir DNS adı olmayan uç noktaları için trafiği yönlendirme. Döndürülen bir DNS adı bir IP adresini almak için ikinci bir DNS sorgusu çalıştırmak amacıyla gereksinimini ortadan kaldırarak Ayrıca, Genel DNS Arama gecikme süresini azaltın. 
* Dünyanın dört bir yanındaki kullanıcılara uygulama gecikme süresini azaltmak, mevcut şirket içi uygulamaya ek coğrafi konumlarda Azure'ı genişletin. Daha fazla bilgi için ['Performans' Traffic Manager trafik yönlendirme](traffic-manager-routing-methods.md#performance).
* Ek kapasite için mevcut bir uygulama, sürekli olarak veya bir 'patlama buluta' çözümü bir ani değişiklik Azure'ı kullanarak isteğe bağlı olarak şirket içinde sağlayın.

Bazı durumlarda, dış uç noktalar Azure Hizmetleri başvurmak yararlı olur (örnekler için bkz [SSS](traffic-manager-faqs.md#traffic-manager-endpoints)). Bu durumda, sistem durumu denetimleri, dış uç noktaları oranı Azure uç noktalarını ücreti üzerinden faturalandırılır. Ancak, Azure uç noktası, durdurmak veya silmek, temel alınan hizmete sistem durumunu denetleyin faturalandırma devre dışı bırakın veya Traffic Manager'da uç noktasını silmek kadar devam eder.

## <a name="nested-endpoints"></a>İç içe uç noktaları

İç içe uç noktalar esnek trafik yönlendirme planları oluşturun ve daha büyük ve karmaşık dağıtımları gereksinimlerini desteklemek için birden çok Traffic Manager profili birleştirin. İç içe uç noktaları ile 'alt' profili, 'parent' profiline bir uç noktası olarak eklenir. Diğer iç içe profiller de dahil olmak üzere herhangi bir tür diğer uç noktalar alt ve üst profilleri içerebilir. Daha fazla bilgi için [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps uç noktalar olarak

Web Apps, Traffic Manager'da uç noktalar olarak yapılandırırken bazı ek hususlar geçerlidir:

1. Yalnızca 'Standart' SKU veya üzeri Web Apps, Traffic Manager ile kullanım için uygundur. Daha düşük bir SKU'ya sahip Web App ekleme girişimleri başarısız olur. Mevcut bir Web App'in SKU eski sürüme düşürme, Traffic artık bu Web App'e trafik gönderen Manager'da sonuçlanır. Desteklenen planları hakkında daha fazla bilgi için bkz [App Service planları](https://azure.microsoft.com/en-us/pricing/details/app-service/plans/)
2. Bir uç nokta bir HTTP isteği aldığında, istekteki 'host' üst bilgisi belirlemek için hangi Web uygulaması isteğin karşılanması kullanır. Barındırma üst bilgisi, örneğin 'contosoapp.azurewebsites.net' isteği başlatmak için kullanılan DNS adını içerir. Farklı bir DNS adı ile Web uygulamanızı kullanmak için DNS adını uygulama için bir özel etki alanı adı olarak kaydedilmesi gerekir. Bir Web uygulaması uç noktası Azure uç noktası eklerken, Traffic Manager profili DNS adı uygulama için otomatik olarak kaydedilir. Bu kayıt, uç nokta silindiğinde otomatik olarak kaldırılır.
3. Her bir Traffic Manager profili, her bir Azure bölgesinden en fazla bir Web uygulaması uç nokta olabilir. Bu kısıtlama için geçici olarak çözmek için bir Web uygulaması bir dış uç noktası olarak yapılandırabilirsiniz. Daha fazla bilgi için [SSS](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Uç noktaları devre dışı bırakma ve etkinleştirme

Bir uç nokta Traffic Manager'da devre dışı bırakma, Bakım modunda veya yeniden dağıtılmakta olan bir uç noktanın geçici olarak trafik kaldırmak yararlı olabilir. Uç nokta yeniden çalışır duruma geçtikten sonra yeniden etkinleştirilebilir.

Uç noktaları etkinleştirilmiş ve PowerShell, CLI veya REST API'yi Traffic Manager portal devre dışı.

> [!NOTE]
> Bir Azure uç noktası devre dışı bırakma, azure'daki dağıtım durumu ile yapmak için hiçbir şey vardır. Bir Azure hizmeti (gibi bir VM veya Web uygulaması çalıştıran ve Traffic Manager'da devre dışı bırakıldığında bile trafik alabilir kalır. Trafik Traffic Manager profili DNS adı aracılığıyla değil, doğrudan hizmet örneği için çözülebilir. Daha fazla bilgi için [Traffic Manager nasıl çalışır](traffic-manager-how-it-works.md).

Trafiği almak için her uç noktanın geçerli uygunluk aşağıdaki etkenlere bağlıdır:

* Profil durumu (etkin/devre dışı)
* Uç nokta durumu (etkin/devre dışı)
* Sistem sonuçları için bu endpoint denetler

Ayrıntılar için bkz [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Traffic Manager DNS düzeyinde çalışır olduğundan, herhangi bir uç nokta için varolan bağlantılar etkilemek silemiyor. Bir uç nokta kullanılamadığında, Traffic Manager kullanılabilir başka bir uç nokta için yeni bağlantılar yönlendirir. Ancak, konak devre dışı bırakılmış veya iyi durumda olmayan uç noktayı arkasında bu oturum sonlandırılana kadar mevcut bağlantıları üzerinden trafiği almak devam edebilir. Uygulamalar, mevcut bağlantılardan boşaltma trafiğe izin verecek şekilde oturum süresi sınırlamanız gerekir.

Bir profildeki tüm uç noktalar devre dışıysa veya profili devre dışı bırakılırsa Traffic Manager bir 'NXDOMAIN' yanıt olarak yeni bir DNS sorgusu gönderir.


## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [Traffic Manager nasıl çalışır](traffic-manager-how-it-works.md).
* Traffic Manager hakkında bilgi edinin [uç nokta izleme ve otomatik yük devretme](traffic-manager-monitoring.md).
* Traffic Manager hakkında bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md).
