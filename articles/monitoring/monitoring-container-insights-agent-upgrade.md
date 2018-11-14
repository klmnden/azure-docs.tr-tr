---
title: Azure İzleyici (Önizleme) kapsayıcı Aracısı yükseltme | Microsoft Docs
description: Bu makalede, Azure İzleyici tarafından kullanılan kapsayıcıları için Log Analytics aracısını nasıl yükseltme açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: fb45002d284bc28dfb7093f69cfac7aae0681e8d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629192"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Azure İzleyici (Önizleme) kapsayıcı Aracısı yükseltme
Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. Aracıyı yeni bir sürümü yayımlandığında, aracıyı Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilmez.

Bu makalede, aracı yükseltme işlemi açıklanmaktadır.

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>İzlenen bir Kubernetes kümesinde aracı yükseltme
Aracı yükseltme işlemi, iki anlaşılır adımdan oluşur. İlk adım, Azure İzleyici ile Azure CLI kullanarak kapsayıcıları için izleme devre dışı bırakmaktır.  İçinde açıklanan adımları izleyin [izlemeyi devre dışı](monitoring-container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) makalesi. Azure CLI kullanarak çözüm ve çalışma alanında depolanan karşılık gelen verileri etkilemeden Kümedeki düğümlerden aracıyı kaldırmak sağlıyor. 

>[!NOTE]
>Bu bakım etkinliği gerçekleştiriyorsanız, kümedeki düğümler, toplanan verileri ilettiğiniz değil ve performans görünümlerine veri gösterilmez arasındaki zaman aracıyı kaldırın ve ardından yeni sürümü yükleyin. 
>

Aracısı'nın yeni sürümünü yüklemek için açıklanan adımları izleyin. [yerleşik izleme](monitoring-container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) bu işlemi tamamlamak için Azure CLI kullanarak makalesi.  

İzleme yeniden etkinleştirdikten sonra bu küme için güncelleştirilmiş sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="next-steps"></a>Sonraki adımlar
Aracı yükseltme sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](monitoring-container-insights-troubleshoot.md) desteği.