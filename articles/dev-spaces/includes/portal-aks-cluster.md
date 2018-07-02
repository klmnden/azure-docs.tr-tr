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
ms.openlocfilehash: 6d2940712f8a76173de47badd45d7c7f0f0be05c
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825485"
---
## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Azure Dev Spaces için bir Kubernetes kümesi oluşturma

1. http://portal.azure.com adresinden Azure portalında oturum açın.
1. **Kaynak oluştur**’u seçin > **Kubernetes** ifadesini arayın > **Kubernetes Hizmeti** > **Oluştur** seçeneğini belirleyin.

   AKS kümesi oluşturma formunun her bir başlığının altında aşağıdaki adımları tamamlayın.

    - **PROJE AYRINTILARI**: Bir Azure aboneliği ve yeni veya mevcut bir Azure kaynak grubu seçin.
    - **KÜME AYRINTILARI**: AKS kümesi için ad, bölge (şu an için Doğu ABD, Batı Avrupa veya Kanada Doğu seçmeniz gerekir), sürüm ve DNS adı ön eki girin.
    - **ÖLÇEK**: AKS aracısı düğümleri için bir VM boyutu ve düğüm sayısı seçin. Azure Dev Spaces kullanmaya yeni başlıyorsanız tüm özellikleri keşfetmek için bir düğüm yeterli olacaktır. Küme dağıtıldıktan sonra da dilediğiniz zaman düğüm sayısını kolayca ayarlayabilirsiniz. AKS kümesi oluşturulduktan sonra VM boyutunu değiştiremeyeceğinizi unutmayın. Ancak ölçeklendirmeniz gerekirse AKS kümesi dağıtıldıktan sonra kolayca daha büyük VM'lere sahip yeni bir AKS kümesi oluşturabilir ve Dev Spaces özelliğini kullanarak bu büyük kümeye yeniden dağıtabilirsiniz.

   Kubernetes sürüm 1.9.6 veya üzerini seçtiğinizden emin olun.

   ![Kubernetes yapılandırma ayarları](../media/common/Kubernetes-Create-Cluster-2.PNG)

   Tamamlandığında, **Sonraki: Ağ** seçeneğini belirleyin.

1. Http Uygulama Yönlendirmesi seçeneğinin etkin olduğundan emin olun.

   ![HTTP Uygulama Yönlendirmesi'ni etkinleştirme](../media/common/Kubernetes-Create-Cluster-3.PNG)

    > [!IMPORTANT]
    > AKS kümenizi oluştururken Http Uygulama Yönlendirmesi'ni etkinleştirdiğinizden emin olmanız gerekir. Bu ayar daha sonra değiştirilemez.

1. **Gözden geçir + oluştur**’u seçin ve sonra tamamlandığında **Oluştur**’a tıklayın.
