---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 05736495d0d4a0c3a5072d29ad27801b6d4a7241
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967756"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Azure Dev Spaces için bir Kubernetes kümesi oluşturma

1. http://portal.azure.com adresinden Azure portalında oturum açın.
1. **Kaynak oluştur**’u seçin > **Kubernetes** ifadesini arayın > **Kubernetes Hizmeti** > **Oluştur** seçeneğini belirleyin.

   AKS kümesi oluşturma formunun her bir başlığının altında aşağıdaki adımları tamamlayın.

    - **PROJE AYRINTILARI**: Bir Azure aboneliği ve yeni veya mevcut bir Azure kaynak grubu seçin.
    - **KÜME AYRINTILARI**: AKS kümesi için ad, bölge (şu an için EastUS, Central US, WestEurope, WestUS2, CanadaCentral veya CanadaEast seçmeniz gerekir), sürüm ve DNS adı ön eki girin.
    - **ÖLÇEK**: AKS aracısı düğümleri için bir VM boyutu ve düğüm sayısı seçin. Azure Dev Spaces kullanmaya yeni başlıyorsanız tüm özellikleri keşfetmek için bir düğüm yeterli olacaktır. Küme dağıtıldıktan sonra da dilediğiniz zaman düğüm sayısını kolayca ayarlayabilirsiniz. AKS kümesi oluşturulduktan sonra VM boyutunu değiştiremeyeceğinizi unutmayın. Ancak ölçeklendirmeniz gerekirse AKS kümesi dağıtıldıktan sonra kolayca daha büyük VM'lere sahip yeni bir AKS kümesi oluşturabilir ve Dev Spaces özelliğini kullanarak bu büyük kümeye yeniden dağıtabilirsiniz.

   Kubernetes sürüm 1.10.3 veya üzerini seçtiğinizden emin olun.

   ![Kubernetes yapılandırma ayarları](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Tamamlandığında, **Sonraki: Ağ** seçeneğini belirleyin.

1. Http Uygulama Yönlendirmesi seçeneğinin etkin olduğundan emin olun.

   ![HTTP Uygulama Yönlendirmesi'ni etkinleştirme](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!IMPORTANT]
    > AKS kümenizi oluştururken Http Uygulama Yönlendirmesi'ni etkinleştirdiğinizden emin olmanız gerekir. Bu ayar daha sonra değiştirilemez.

1. Rol Tabanlı Erişim Denetimi (RBAC) için dilediğiniz ayarı seçin. Azure Dev Spaces, RBAC'nin etkin veya devre dışı olduğu kümeleri destekler.

    ![RBAC ayarı](../media/common/k8s-RBAC.PNG)

1. **Gözden geçir + oluştur**’u seçin ve sonra tamamlandığında **Oluştur**’a tıklayın.
