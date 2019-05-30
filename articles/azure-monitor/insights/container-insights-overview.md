---
title: Kapsayıcılar için Azure İzleyicisi'nin genel bakış | Microsoft Docs
description: Bu makalede, AKS kapsayıcı öngörüleri çözümüdür ve değer AKS kümeleri ve Azure Container Instances'da durumunu izleyerek sunar izleyen kapsayıcılar için Azure İzleyici açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: d6321564672097fbf901d1d33afac9f606fcb63a
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65521830"
---
# <a name="azure-monitor-for-containers-overview"></a>Kapsayıcılara genel bakış için Azure İzleyici

Kapsayıcılar için Azure İzleyici, Azure Container Instances'ı veya Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri için Dağıtılmış bir kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir. Özellikle birden çok uygulama ile ölçekli olarak bir üretim kümesi çalıştırırken, kapsayıcılarınızın izlenmesi kritik önem taşır.

Kapsayıcılar için Azure İzleyici denetleyicileri, düğümleri ve Kubernetes ölçümler API aracılığıyla kullanılabilir olan kapsayıcıları bellekten toplanması ve işlemci ölçümleri performans görünürlük sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerdeki izleme etkinleştirdikten sonra ölçüm ve günlükleri otomatik olarak sizin için Linux için Log Analytics aracısını kapsayıcı bir sürümü aracılığıyla toplanır. Ölçüm ölçümleri deposuna yazılır ve günlük veriler ile ilişkili günlükleri deposuna yazılır, [Log Analytics](../log-query/log-query-overview.md) çalışma. 

![Kapsayıcıları mimarisi için Azure İzleyici](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Kapsayıcıları sağlamak için Azure İzleyici yapar?

Kapsayıcılar için Azure İzleyici performans ve Kubernetes kümenizi ve kapsayıcı iş yüklerinin durumunu anlamanıza olanak sağlayan Azure İzleyicisi'nin farklı özelliklerini kullanarak kapsamlı bir izleme deneyimi sunar. Kapsayıcılar için Azure İzleyici ile şunları yapabilirsiniz:

* Düğüm ve bunların ortalama işlemci ve bellek kullanımı çalışan AKS kapsayıcı belirleyin. Bu bilgi, kaynak darboğazları belirlemenize yardımcı olabilir.
* Kapsayıcı grupları ve Azure Container Instances'da barındırılan kapsayıcılarına işlemci ve bellek kullanımı belirleyin.  
* Kapsayıcı bir denetleyici veya bir pod içinde bulunduğu belirleyin. Bu bilgi, denetleyicinin ya da pod'ın genel performansını görüntülemenize yardımcı olabilir.
* Pod destekleyen standart işlemlere ilgisiz, ana bilgisayarda çalışan iş yüklerini kaynak kullanımını gözden geçirin.
* Ortalama ve en yoğun iş yükü altında kümeye davranışını anlayın. Bu bilgi, kapasite gereksinimlerini tanımlama ve kümenin karşılayabileceği en fazla yükü belirlemek yardımcı olabilir. 
* Proaktif olarak size bildirim veya düğümlerin veya kapsayıcılar CPU ve bellek kullanımı, eşikleri aştığında bunu kaydetmek için uyarıları yapılandırın.  

## <a name="how-do-i-access-this-feature"></a>Bu özelliği nasıl erişim sağlanır?
İki yolu, seçilen AKS kümesini Azure İzleyici ya da doğrudan kapsayıcılar için Azure İzleyici erişebilirsiniz. Azure İzleyici'deki genel bakış açısı tüm kapsayıcılar, izlenen ve Abonelikleriniz ve kaynak grupları arasında arama ve filtreleme izin olduğu değil, dağıtılan ve ardından kapsayıcılar için Azure İzleyici ile ayrıntıya sahip Seçilen kapsayıcı.  Aksi takdirde, seçilen bir AKS kapsayıcı AKS sayfasından doğrudan özellik erişebilirsiniz.  

![Kapsayıcılar için Azure İzleyici erişim yöntemlerine genel bakış](./media/container-insights-overview/azmon-containers-experience.png)

Docker ve Windows yönetme ve izleme de ilgileniyorsanız kapsayıcı konakları görünümü yapılandırma, denetleme ve kaynak kullanımını görmek [kapsayıcı izleme çözümü](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Sonraki adımlar
AKS kümenizi izlemeye başlamak için gözden [kapsayıcılar için Azure İzleyici etkinleştirme](container-insights-onboard.md) izlemeyi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemlerin anlaşılması.  
