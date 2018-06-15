---
title: Trafik Yöneticisi uç nokta türleri | Microsoft Docs
description: Bu makalede, Azure Traffic Manager ile kullanılan uç noktalarını farklı türleri açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 792712e3e529d77ff20a7603b5fbf028ca60f8c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23877520"
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager uç noktaları
Microsoft Azure trafik Yöneticisi, ağ trafiğini farklı veri merkezlerinde çalışan uygulama dağıtımları için nasıl dağıtıldığını denetlemenize olanak sağlar. Trafik Yöneticisi'nde her uygulama dağıtımı 'Bitiş' olarak yapılandırın. Trafik Yöneticisi DNS isteği aldığında, DNS yanıtında döndürmek için kullanılabilir uç nokta seçer. Trafik Yöneticisi seçimi, geçerli uç nokta durumu ve trafik yönlendirme yöntemini temel alır. Daha fazla bilgi için bkz: [nasıl trafik Yöneticisi çalışır](traffic-manager-how-traffic-manager-works.md).

Uç nokta Traffic Manager tarafından desteklenen üç tür vardır:
* **Azure uç noktaları** Azure içinde barındırılan hizmetler için kullanılır.
* **Dış uç noktalar** dış Azure, hem şirket içinde barındırılan hizmetlere veya farklı bir barındırma sağlayıcısı ile kullanılır.
* **İç içe uç noktaları** büyük, daha karmaşık dağıtımlar ihtiyaçlarını desteklemek için daha esnek trafik yönlendirme düzenleri oluşturmak için Traffic Manager profillerini birleştirmek için kullanılır.

Farklı türlerdeki uç noktaları içinde tek bir trafik Yöneticisi profili nasıl birleştirilir üzerinde herhangi bir kısıtlama yoktur. Her profil uç nokta türlerinin bir karışımını içerebilir.

Aşağıdaki bölümlerde daha derinlemesine her uç nokta türü açıklanmaktadır.

## <a name="azure-endpoints"></a>Azure uç noktaları

Azure uç noktaları Azure tabanlı hizmetler trafik Yöneticisi'nde için kullanılır. Aşağıdaki Azure kaynak türleri desteklenir:

* Bulut Hizmetleri 'Klasik' Iaas Vm'leri ve PaaS.
* Web Apps
* Publicıpaddress kaynakları (hangi bağlanması VM'ler için doğrudan veya bir Azure yük dengeleyici üzerinden). Publicıpaddress bir Traffic Manager profilini kullanılmak üzere atanmış bir DNS adı olmalıdır.

Publicıpaddress, Azure Resource Manager kaynaklarını kaynaklardır. Klasik dağıtım modelinde mevcut değil. Bu nedenle yalnızca desteklenir trafik Yöneticisi'nin Azure Resource Manager deneyimleri oldukları. Diğer uç nokta türleri, Resource Manager ve klasik dağıtım modeli aracılığıyla desteklenir.

'Klasik' Iaas VM, bulut hizmeti veya bir Web uygulaması durdurulduğunda ve başlatıldığında Azure uç noktaları kullanırken, trafik Yöneticisi algılar. Bu durum uç nokta durumu yansıtılır. Bkz: [trafik Yöneticisi uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status) Ayrıntılar için. Temel alınan hizmet durdurulduğunda, trafik Yöneticisi uç noktası durumu denetimleri veya uç noktasına doğrudan trafik gerçekleştirmez. Hiçbir trafik Yöneticisi fatura olaylar durdurulmuş örneği için oluşur. Hizmet yeniden başlatıldığında faturalama sürdürür ve uç nokta trafiği almak uygun. Bu algılama Publicıpaddress uç noktaları için geçerli değildir.

## <a name="external-endpoints"></a>Dış uç noktalar

Dış uç noktalar dışında Azure Hizmetleri için kullanılır. Örneğin, bir hizmeti şirket içi barındırılan veya farklı bir sağlayıcı ile. Dış uç noktalar ayrı ayrı kullanılabilir veya aynı trafik Yöneticisi profili Azure uç noktaları ile birlikte. Azure uç noktaları dış uç noktalar ile birleştirerek çeşitli senaryolara olanak sağlar:

* Ya da bir yük devretme etkin-etkin veya etkin-pasif modelinde, Azure mevcut bir artıklığı içi uygulama sağlamak için kullanın.
* Tüm dünyada kullanıcılar için uygulama gecikme süresini azaltmak için mevcut şirket içi uygulamaya Azure ek coğrafi konumlarda genişletir. Daha fazla bilgi için bkz: ['Performans' Traffic Manager trafik yönlendirme](traffic-manager-routing-methods.md#performance).
* Mevcut bir ek kapasite sağlamak için kullanım Azure uygulaması, sürekli olarak veya bir ani talep karşılamak üzere 'veri bloğu-bulut' bir çözüm olarak şirket içi.

Bazı durumlarda, Azure hizmetlerine başvurmak için dış uç noktalar kullanmak yararlı olur (örnekler için bkz: [SSS](traffic-manager-faqs.md#traffic-manager-endpoints)). Bu durumda, sistem durumu denetimlerinin Azure uç noktaları oranı, dış uç noktalar oranı faturalandırılır. Ancak, Azure uç noktaları, durdurmak veya temel alınan hizmet silerseniz sistem durumu denetimi faturalama devre dışı bırakır veya trafik Yöneticisi'nde uç noktasını silmek kadar devam eder.

## <a name="nested-endpoints"></a>İç içe geçmiş uç noktaları

İç içe geçmiş uç noktaları esnek trafik yönlendirme planları oluşturun ve daha büyük ve karmaşık dağıtımlar ihtiyaçlarını desteklemek üzere birden çok trafik Yöneticisi profili birleştirin. İç içe uç ile bir 'alt' profili 'parent' profili için bir uç noktası olarak eklenir. Alt ve üst profilleri diğer uç noktaları diğer iç içe profil de dahil olmak üzere, her tür içerebilir. Daha fazla bilgi için bkz: [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web uygulamaları uç noktalar olarak

Web Apps trafiği Manager'da uç noktalar olarak yapılandırırken bazı hususlar geçerlidir:

1. Yalnızca Web uygulamaları 'Standart' SKU veya üzerinde trafik Yöneticisi ile kullanılması için uygundur. Bir Web uygulaması alt sku'sunun ekleme girişimleri başarısız olur. Var olan bir Web uygulamasının SKU'su eski sürüme düşürmeyi trafiği artık bu Web uygulaması için trafiği göndermeye Yöneticisi'nde sonuçlanır.
2. Bir uç nokta bir HTTP isteği aldığında, istek 'ana' üstbilgisinde belirlemek için hangi Web uygulaması isteğe hizmet kullanır. Ana bilgisayar üstbilgisi isteği, örneğin 'contosoapp.azurewebsites.net' başlatmak için kullanılan DNS adı içeriyor. Farklı bir DNS adı ile Web uygulamanızı kullanmak için DNS adı uygulama için özel etki alanı adı olarak kaydedilmelidir. Azure uç noktası olarak bir Web uygulaması uç noktası eklerken, trafik Yöneticisi profili DNS adı uygulama için otomatik olarak kaydedilir. Uç nokta silindiğinde, bu kayıt otomatik olarak kaldırılır.
3. Her trafik Yöneticisi profili en çok bir Web uygulaması uç nokta her Azure bölgesinden olabilir. Bu kısıtlamanın için çözmek için bir Web uygulaması bir dış uç noktası olarak yapılandırabilirsiniz. Daha fazla bilgi için bkz: [SSS](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Etkinleştirme ve uç noktaları devre dışı bırakma

Bir uç nokta trafik Yöneticisi'nde devre dışı bırakma Bakım modu veya yeniden dağıtılmakta olan bir uç nokta geçici olarak trafik kaldırmak yararlı olabilir. Uç nokta yeniden çalışır duruma geldiğinde yeniden etkinleştirilebilir.

Uç noktaları etkin ve trafik Yöneticisi portal, PowerShell, CLI ya da Resource Manager ve klasik dağıtım modeli desteklenen REST API aracılığıyla devre dışı.

> [!NOTE]
> Azure uç noktası devre dışı bırakılması azure'daki dağıtım durumu ile ilgisi vardır. Bir Azure hizmetini (gibi bir VM veya Web uygulamasının çalışıp çalışmadığını ve hatta trafik Yöneticisi'nde devre dışı bırakıldığında trafiği almaya kalır. Trafik, hizmet örneği için doğrudan yerine trafik Yöneticisi profili DNS adı aracılığıyla çözülebilir. Daha fazla bilgi için bkz: [trafik Yöneticisi nasıl çalıştığını](traffic-manager-how-traffic-manager-works.md).

Trafiği almak için her uç nokta geçerli uygunluğunu aşağıdaki etkenlere bağlıdır:

* Profil durumu (etkin/devre dışı)
* Uç nokta durumu (etkin/devre dışı)
* Bu uç noktası için sistem sonuçlarını denetler

Ayrıntılar için bkz [trafik Yöneticisi uç nokta izleme](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Trafik Yöneticisi DNS düzeyinde çalışır olduğundan, herhangi bir uç nokta var olan bağlantılara etkilemek alamıyor. Bir uç nokta kullanılamaz duruma geldiğinde, trafik Yöneticisi başka bir kullanılabilir uç nokta için yeni bağlantı yönlendirir. Ancak, devre dışı bırakılmış veya sağlıksız bitiş noktasının ardındaki ana Bu oturumlar durduruluncaya kadar mevcut bağlantıları üzerinden trafiği almaya devam edebilir. Uygulamaların mevcut bağlantılarından boşaltmak trafiğe izin verecek şekilde oturum süresi sınırlamanız gerekir.

Bir profildeki tüm uç noktaları devre dışı bırakılmışsa veya profili devre dışı bırakılırsa, trafik Yöneticisi yeni bir DNS sorgusu 'NXDOMAIN' yanıta gönderir.


## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [trafik Yöneticisi nasıl çalıştığını](traffic-manager-how-traffic-manager-works.md).
* Trafik Yöneticisi hakkında bilgi edinin [uç nokta izleme ve otomatik yük devretme](traffic-manager-monitoring.md).
* Trafik Yöneticisi hakkında bilgi edinin [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).
