---
title: "Kapsayıcıları için Azure sanal ağı | Microsoft Docs"
description: "Kubernetes kümeler için eklenti birbirine ve diğer kaynakların bir sanal ağ ile iletişim kurmak kapsayıcı sağlar CNI hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: jdial
ms.openlocfilehash: f70afa8ff69b6c79363313c0f2df3b6785da8d81
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="container-networking"></a>Kapsayıcı ağ iletişimi

Azure sağlayan bir [kapsayıcı ağ arabirimi (CNI) eklentisi](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md) dağıtmak ve Azure yerel ağ yetenek kendi Kubernetes kümeyi yönetmek sağlar. Kubernetes dağıtma ile kümeleri, eklenti, varsayılan olarak, etkin [Azure kapsayıcı Hizmeti altyapısının](https://github.com/Azure/acs-engine) (veya ACS altyapısı).

## <a name="networking-capabilities"></a>Ağ Özellikleri

Kapsayıcıları zengin özellik kümesi sağlayan bir sanal ağ sunan, gibi kullanabilir:
-   Kümeniz için ayrı bir sanal ağ oluşturun veya varolan bir sanal ağı kümenizdeki dağıtabilirsiniz. 
-   Kümedeki her pod sanal ağda bir IP adresi alır ve kümedeki diğer pod'ları ve sanal ağda herhangi bir sanal makine ile doğrudan iletişim kurabilir. 
-   Bir pod ExpressRoute ve siteden siteye VPN bağlantıları diğer pod'ları ve eşlenen sanal ağlardaki sanal makineler ve şirket içi ağlara bağlayabilirsiniz. Şirket içi kaynakları pod'ları için iletişim kurabilir. 
-   Azure yük dengeleyici üzerinden internet Kubernetes hizmet getirebilir.  
-   Hizmet uç noktaları etkin olan bir alt ağdaki pod'ları bağlanabilir güvenli bir şekilde Azure hizmetlerine (depolama ve örnek için SQL veritabanı).
-   Bir ağ sanal gereç pod'ları trafiği yönlendirmek için kullanıcı tanımlı yollar kullanabilirsiniz. 
-   Pod'ları, Internet üzerindeki genel kaynaklara erişebilir.
-   Bir pod bir DNS adı ile ilişkilendirilebilir bir ortak IP adresi atayabilirsiniz.
 
## <a name="limits"></a>Sınırlar
En çok 4.000 düğümleri Kubernetes kümedeki ve 250 pod'ları Genel sınır 16.000 pod'ları her küme düğümü başına en fazla eklenti kullanırken dağıtabilirsiniz.

## <a name="constraints"></a>Kısıtlamalar
- Eklentisi ile Kubernetes kümesi dağıtırken etkin değil [Azure kapsayıcı hizmeti (AKS)](../aks/intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [ACS](../container-service/kubernetes/container-service-intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Kubernetes ile küme.
- DNS veya erişim ilkeleri de dahil olmak üzere Kubernetes ağ ilkeleri için yerel desteği yoktur.
- Eklenti pod başına ağ ilkelerini desteklemiyor.

## <a name="pricing"></a>Fiyatlandırma
CNI eklentisi kullanılarak ücretsizdir.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [Kubernetes kümesi dağıtma](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/deploy.md) içinde [kendi sanal ağ](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/features.md#using-azure-integrated-networking-cni).
