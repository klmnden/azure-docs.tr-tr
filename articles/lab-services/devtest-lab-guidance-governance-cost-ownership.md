---
title: Azure DevTest Labs altyapı İdaresi
description: Bu makalede, Azure DevTest Labs altyapısının İdaresi için yönergeler sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 7832691812d8f10342dc7df20a7cfab7265f2d9d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60775721"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---manage-cost-and-ownership"></a>İdare maliyet ve sahipliğinin - Azure DevTest Labs altyapısını yönetme
Geliştirme sürecinizi oluşturmayı düşünün ve test ortamları maliyet ve sahiplik birincil edilir. Bu bölümde, maliyetini en iyi duruma getirmek ve ortamınız genelinde sahipliği hizalama yardımcı olan bilgiler bulabilirsiniz.

## <a name="optimize-for-cost"></a>Maliyetini en iyi duruma getirme

### <a name="question"></a>Soru
DevTest Labs ortamımın içinde nasıl maliyetini iyileştirebilirim?

### <a name="answer"></a>Yanıt
Bir dizi yerleşik özellik yardımcı DevTest Labs, maliyetini en iyi duruma vardır. Bkz: [maliyet yönetimi, eşikleri](devtest-lab-configure-cost-management.md) [ve ilkeleri](devtest-lab-set-lab-policy.md) kullanıcılarınızın etkinlikleri sınırlamak için makaleler. 

Geliştirme ve test iş yükleri için DevTest Labs yazılımınız olarak kullanan düşünebilirsiniz [Kurumsal geliştirme ve Test aboneliği teklifi](https://azure.microsoft.com/offers/ms-azr-0148p/), Kurumsal anlaşmanız kapsamında. Alternatif olarak, bir Kullandıkça Öde müşterisiyseniz göz önünde bulundurun isteyebilirsiniz [ödeme-olarak-, go geliştirme ve test teklifi](https://azure.microsoft.com/offers/ms-azr-0023p/).

Bu yaklaşım ile çok sayıda avantaj sağlar:

- Özel, indirimli geliştirme ve Test fiyatları Windows sanal makineleri, bulut Hizmetleri, HDInsight, App Service ve Logic Apps hakkında
- Diğer Azure hizmetlerinde mükemmel Kurumsal Anlaşma (EA) oranları
- Windows 8.1 ve Windows 10 görüntüleri dahil olmak üzere Galeri'deki özel Geliştirme ve Test görüntülerinin tamamına erişme olanağı
 
Yalnızca etkin Visual Studio aboneleri (standart abonelikler, yıllık bulut aboneliklerine ve aylık bulut abonelikleri) bir kurumsal geliştirme ve Test aboneliği içinde çalışan Azure kaynakları kullanabilirsiniz. Bununla birlikte, son kullanıcılar geri bildirim sağlamak veya onay testleri gerçekleştirmek için uygulamayı erişebilir. Bu abonelikteki kaynakların kullanımı geliştirme ve test uygulamalarıyla kısıtlanmıştır ve çalışma zamanı garantisi sunulmaz.

Geliştirme ve test teklifi kullanmaya karar verirseniz, bu avantaj yalnızca geliştirme ve test uygulamalarınız için olduğunu unutmayın. Aboneliği dahilinde kullanım, Azure DevOps ve HockeyApp kullanımı dışında finansal destekli bir SLA içermez.

## <a name="define-a-role-based-access-across-your-organization"></a>Kuruluşunuzda bir rol tabanlı erişim tanımlayın
### <a name="question"></a>Soru
Ne miyim tanımlamak rol tabanlı erişim denetimi geliştiriciler ve test çalışmalarını yapabilseniz benim için DevTest Labs, BT'nin emin olmak için ortamlar yöneten? 

### <a name="answer"></a>Yanıt
Kuruluşunuzu ayrıntı bağlıdır ancak geniş bir düzeni vardır.

Merkezi BT yalnızca gerekli olanla sahip ve gereken düzeyde bir denetiminiz proje ve uygulama ekiplerin olanak sağlar. Genellikle, bu Orta anlamına gelir BT aboneliğin sahibi ve tanıtıcıları çekirdek ağ yapılandırmaları gibi BT işlevleri. Dizi **sahipleri** abonelik küçük olmalıdır. Bu sahipleri, ihtiyaç olduğunda ek sahipleri aday gösterin veya örneğin "ortak IP" abonelik düzeyinde ilkeler uygulayabilirsiniz.

Tier1 veya Katman 2 destek gibi bir abonelik üzerinden erişim gerektiren kullanıcılar bir alt kümesi olabilir. Bu durumda, bu kullanıcılar size öneririz **katkıda bulunan** kullanıcıların kaynaklarını yönetmek, ancak değil kullanıcı erişim sağlamak veya ilkeleri ayarlama böylece erişim.

DevTest Labs kaynak proje/uygulama ekibine yakın çeken sahipler tarafından sahip olunan. Makineleri ve gerekli yazılım gereksinimlerine anladıkları olmasıdır. Birçok kuruluşta bu DevTest Labs kaynak yaygın olarak proje/Geliştirme lideri sahibidir. Bu sahibi kullanıcı ve laboratuvar ortamında ilkelerini yönetebilir ve DevTest Labs ortamdaki tüm sanal makineleri yönetebilir.

Proje/uygulama takım üyeleri DevTest Labs kullanıcı rolüne eklenmesi gerekir. Bu kullanıcılar, sanal makineler (satır içi Laboratuvar ve abonelik düzeyinde ilkeleri) oluşturabilir. Bunlar, kendi sanal makinelerini de yönetebilirsiniz. Bunlar, diğer kullanıcılara ait sanal makineleri yönetemez.

Daha fazla bilgi için [Azure Kurumsal iskelesi: öngörücü abonelik İdaresi](/azure/architecture/cloud-adoption/appendix/azure-scaffold) belgeleri.


## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Kurumsal ilke ve Uyumluluk](devtest-lab-guidance-governance-policy-compliance.md).
