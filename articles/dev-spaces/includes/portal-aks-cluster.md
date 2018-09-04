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
ms.openlocfilehash: 4c4a5b66fe35da01a3661715e17a9fda20bc2411
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43184791"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Azure Dev Spaces için bir Kubernetes kümesi oluşturma

1. http://portal.azure.com adresinden Azure portalında oturum açın.
1. **Kaynak oluştur**’u seçin > **Kubernetes** ifadesini arayın > **Kubernetes Hizmeti** > **Oluştur** seçeneğini belirleyin.

   AKS kümesi oluşturma formunun her bir başlığının altında aşağıdaki adımları tamamlayın.

    - **PROJE AYRINTILARI**: Bir Azure aboneliği ve yeni veya mevcut bir Azure kaynak grubu seçin.
    - **KÜME AYRINTILARI**: AKS kümesi için ad, bölge (şu an için EastUS, Central US, WestEurope, WestUS2, CanadaCentral veya CanadaEast seçmeniz gerekir), sürüm ve DNS adı ön eki girin.
    - **ÖLÇEK**: AKS aracısı düğümleri için bir VM boyutu ve düğüm sayısı seçin. Azure Dev Spaces kullanmaya yeni başlıyorsanız tüm özellikleri keşfetmek için bir düğüm yeterli olacaktır. Küme dağıtıldıktan sonra da dilediğiniz zaman düğüm sayısını kolayca ayarlayabilirsiniz. AKS kümesi oluşturulduktan sonra VM boyutunu değiştiremeyeceğinizi unutmayın. Ancak ölçeklendirmeniz gerekirse AKS kümesi dağıtıldıktan sonra kolayca daha büyük VM'lere sahip yeni bir AKS kümesi oluşturabilir ve Dev Spaces özelliğini kullanarak bu büyük kümeye yeniden dağıtabilirsiniz.

   Kubernetes sürüm 1.9.6 veya üzerini seçtiğinizden emin olun.

   ![Kubernetes yapılandırma ayarları](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Tamamladığınızda **İleri: Kimlik doğrulaması**'nı seçin.

1. Rol Tabanlı Erişim Denetimi (RBAC) için dilediğiniz ayarı seçin. Azure Dev Spaces, RBAC'nin etkin veya devre dışı olduğu kümeleri destekler.

    ![RBAC ayarı](../media/common/k8s-RBAC.PNG)

1. Http Uygulama Yönlendirmesi seçeneğinin etkin olduğundan emin olun.

   ![HTTP Uygulama Yönlendirmesi'ni etkinleştirme](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!Note]
    > Mevcut bir küme üzerinde [Http Uygulama Yönlendirmesi](/azure/aks/http-application-routing)'ni etkinleştirmek için şu komutu kullanın: `az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing`

1. **Gözden geçir + oluştur**’u seçin ve sonra tamamlandığında **Oluştur**’a tıklayın.
