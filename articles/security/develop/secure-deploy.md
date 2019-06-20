---
title: Microsoft Azure üzerinde güvenli uygulamalar dağıtma
description: Bu makalede, web uygulaması projenizin sürüm ve yanıtlama aşamaları sırasında dikkate alınması gereken en iyi uygulamalar açıklanmaktadır.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: e8249113ee65c28414c79f00c53d11596673434b
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144457"
---
# <a name="deploy-secure-applications-on-azure"></a>Azure'da güvenli uygulamalar dağıtma
Bu makalede güvenlik etkinliklerini ve bulut için uygulama dağıtırken göz önünde bulundurmanız denetimler sunar. Güvenlik sorularını ve Microsoft sürüm ve yanıtlama aşamaları sırasında dikkate alınması gereken kavramlar [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) ele alınmaktadır. Etkinlikleri ve daha güvenli bir uygulama dağıtmak için kullanabileceğiniz Azure Hizmetleri tanımlamanıza yardımcı olmaktır.

Aşağıdaki SDL aşamaları, bu makalede ele alınmaktadır:

- Yayınla
- Yanıt

## <a name="release"></a>Yayınla
Yayın aşaması'nin odak noktası, genel sürüm için bir proje readying olduğu.
Bu yolları daha sonra oluşabilecek güvenlik açıklarına ve etkili bir şekilde yayın sonrası bakım görevleri gerçekleştirmek için planlama içerir.

### <a name="check-your-applications-performance-before-you-launch"></a>Başlatma, önce uygulamanızın performansını kontrol edin.

Başlatın veya güncelleştirmeleri üretim ortamına dağıtmak için önce uygulamanızın performansını kontrol edin. Bulut tabanlı çalıştırma [yük testleri](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) dağıtım kalitesini artırmak, uygulamanızda performans sorunlarını bulmak için Visual Studio kullanarak, uygulamanızın her zaman yukarı ya da mevcut olduğunu ve uygulamanızın trafiği işleyebilir emin olun için başlatma.

### <a name="install-a-web-application-firewall"></a>Bir web uygulaması Güvenlik Duvarı'nı yükleme

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları ve siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu saldırılarını önleme zor olabilir. Bu ayrıntılı bakım, düzeltme eki uygulama ve uygulama topolojisinin birçok katmanına izleme gerektirebilir. Merkezi bir WAF, güvenlik yönetimini daha kolay hale getirir. Bir WAF çözümü bilinen bir güvenlik açığı her tek tek web uygulamasını güvenli hale getirir ve merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine ayrıca tepki verebilir.

[Azure Application Gateway WAF](https://docs.microsoft.com/azure/application-gateway/waf-overview) web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları merkezi koruma sağlar. WAF, kurallara göre [OWASP çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9'daki.

### <a name="create-an-incident-response-plan"></a>Bir olay yanıtlama planı oluşturma

Bir olay yanıtlama planı hazırlama, zaman içinde ortaya çıkmaya başladı yeni tehditlere yardımcı olması önemlidir. Bir olay yanıtlama planı hazırlama, uygun güvenlik Acil kişileri tanımlama ve bakım planları lisanslı bir üçüncü taraf kodu ve kuruluştaki diğer gruplardan devralınan kod için güvenlik oluşturma içerir.

### <a name="conduct-a-final-security-review"></a>Son güvenlik incelemesi gerçekleştirme

Kasıtlı olarak gerçekleştirilen tüm güvenlik etkinlikleri gözden geçirme, yazılım yayın veya uygulamanız için hazırlık sağlamaya yardımcı olur. Tehdit modelleri, Araçlar çıkışları ve performans gereksinimlerini aşamasında tanımlanan hata çubukları ve kalite kapıları karşı İnceleme (FSR) son güvenlik incelemesi genellikle içerir.

### <a name="certify-release-and-archive"></a>Yayın ve Arşiv Onayla

Bir yayın güvenlik ve gizlilik gereksinimlerinin karşılandığından emin olun yardımcı olur. önce yazılım sertifika. Tüm ilgili veri arşivleme, yayın sonrası bakım görevlerini gerçekleştirmek için gereklidir. Ayrıca yardımcı olur, Sürdürülen yazılım Mühendisliği ile ilişkili uzun vadeli maliyetleri düşük arşivleme.

## <a name="response"></a>Yanıt
Mümkün ve ortaya çıkan yazılım tehditleri ve güvenlik açıkları herhangi bir rapor için uygun şekilde yanıt vermek kullanılabilir olan geliştirme ekibi yanıt yayın sonrası aşaması ortalar.

### <a name="execute-the-incident-response-plan"></a>Olay yanıtlama planı çalıştırma

Yayın aşamasında instituted olay yanıtlama planı uygulamak için müşterilerin, ortaya çıkan yazılım güvenlik veya gizlilik güvenlik açıklarına karşı korunmasına yardımcı olmak için gereklidir.

### <a name="monitor-application-performance"></a>Uygulama performansını izleme

Potansiyel olarak dağıtıldıktan sonra uygulamanızı sürekli izleme, performans sorunlarını ve bunun yanı sıra güvenlik açıklarını algılama yardımcı olur.
Uygulama izleme ile yardımcı olan azure hizmetleri şunlardır:

  - Azure Application Insights
  - Azure Güvenlik Merkezi

#### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformlardaki web geliştiricilerine yönelik genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Application Insights performans anormalliklerini otomatik olarak algılar. Bu sorunları tanılamak ve ne kullanıcıların uygulamanızla aslında yaptığını anlamanıza yardımcı olacak güçlü analiz araçlarına içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

#### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) , önlemenize, algılamanıza ve Artırılmış görünürlük ile tehditleri (ve üzerinde denetim) yardımcı olan web uygulamaları gibi Azure kaynaklarınızın güvenlik. Azure Güvenlik Merkezi, aksi takdirde gözden kaçan geçebilir tehditleri algılamanıza yardımcı olur. Bu, çeşitli güvenlik çözümleri ile çalışır.

Güvenlik Merkezi'nin ücretsiz katmanı, yalnızca Azure kaynaklarınız için sınırlı bir güvenlik sunar. [Güvenlik Merkezi standart katmanı](https://docs.microsoft.com/azure/security-center/security-center-onboarding) şirket içi kaynaklara ve diğer bulutlarda bu özelliklerini genişletir.
Güvenlik Merkezi standart size yardımcı olur:

  - Güvenlik güvenlik açıklarını bulup düzeltin.
  - Kötü amaçlı etkinliği engellemek için erişim ve uygulama denetimleri uygulayın.
  - Analiz ve zeka kullanarak tehditleri algılayın.
  - Saldırı altındayken hızlıca yanıt.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenlik denetimleri öneririz ve yardımcı olabilecek etkinlikleri tasarım ve güvenli uygulamalar geliştirin.

- [Güvenli uygulamalar tasarlama](secure-design.md)
- [Güvenli uygulamalar geliştirin](secure-develop.md)
