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
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: 6819cf96eb968e8faad5441cf3f3dd672653f1cd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971442"
---
# <a name="azure-monitor-for-containers-overview"></a>Kapsayıcılara genel bakış için Azure İzleyici

Kapsayıcılar için Azure İzleyici, yönetilen Azure Kubernetes Service (AKS) barındırılan Kubernetes kümelerini dağıtılan kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir. Özellikle birden çok uygulama ile ölçekli olarak bir üretim kümesi çalıştırırken, kapsayıcılarınızın izlenmesi kritik önem taşır.

Kapsayıcılar için Azure İzleyici denetleyicileri, düğümleri ve Kubernetes ölçümler API aracılığıyla kullanılabilir olan kapsayıcıları bellekten toplanması ve işlemci ölçümleri performans görünürlük sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerdeki izleme etkinleştirdikten sonra bu ölçüm ve günlükleri otomatik olarak sizin için Linux için Log Analytics aracısını kapsayıcı bir sürümü aracılığıyla toplanan ve depolanan, [Log Analytics](../log-analytics/log-analytics-overview.md) çalışma. 
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>Kapsayıcıları sağlamak için Azure İzleyici yapar?

Kapsayıcılar için Azure İzleyici kaynaklarınızda bulunan kapsayıcı iş yüklerinin ve destesinin hangi izlenen bir Kubernetes kümesi performans durumunun etkiler gösteren birkaç önceden tanımlanmış görünümleri içerir:  

* Düğüm ve bunların ortalama işlemci ve bellek kullanımı çalışan AKS kapsayıcı belirleyin. Bu bilgi, kaynak darboğazları belirlemenize yardımcı olabilir.
* Kapsayıcı bir denetleyici veya bir pod içinde bulunduğu belirleyin. Bu bilgi, denetleyicinin ya da pod'ın genel performansını görüntülemenize yardımcı olabilir. 
* Pod destekleyen standart işlemlere ilgisiz, ana bilgisayarda çalışan iş yüklerini kaynak kullanımını gözden geçirin.
* Ortalama ve en yoğun iş yükü altında kümeye davranışını anlayın. Bu bilgi, kapasite gereksinimlerini tanımlama ve kümenin karşılayabileceği en fazla yükü belirlemek yardımcı olabilir. 

## <a name="how-do-i-access-this-feature"></a>Bu özelliği nasıl erişim sağlanır?
İki yolu, seçilen AKS kümesini Azure İzleyici ya da doğrudan kapsayıcılar için Azure İzleyici erişebilirsiniz. Azure sahip olduğunuz İzleyici'deki tüm kapsayıcıları genel bir bakış açısını dağıtılan, hangi izlenen ve Abonelikleriniz ve kaynak grupları arasında arama ve filtreleme için verme kullanmıyorsanız ve ardından seçili kapsayıcılar için Azure İzleyici ile detaya gidin kapsayıcı.  Aksi takdirde, seçilen bir AKS kapsayıcı AKS sayfasından doğrudan yalnızca özellik erişebilirsiniz.  

![Kapsayıcılar için Azure İzleyici erişim yöntemlerine genel bakış](./media/monitoring-container-insights-overview/azmon-containers-views.png)

Docker ve Windows yönetme ve izleme de ilgileniyorsanız kapsayıcı konakları görünümü yapılandırma, denetleme ve kaynak kullanımını görmek [kapsayıcı izleme çözümü](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Sonraki adımlar
AKS kümenizi izlemeye başlamak için gözden [nasıl eklenirim Azure izlemek için kapsayıcılar](monitoring-container-insights-onboard.md) izlemeyi etkinleştirmek için gereksinimleri ve kullanılabilir yöntemlerin anlaşılması.  