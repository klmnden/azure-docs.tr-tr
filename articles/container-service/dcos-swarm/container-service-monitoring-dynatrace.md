---
title: (KULLANIM DIŞI) Azure DC/OS kümesi - Dynatrace ile izleme
description: Dynatrace ile bir Azure Container Service DC/OS kümesini izleme. Dynatrace OneAgent DC/OS Pano'yu kullanarak dağıtın.
services: container-service
author: MartinGoodwell
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 8f34a00d9256c288a2842e905c06d5336522eece
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119869"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>(KULLANIM DIŞI) Bir Azure Container Service DC/OS kümesi ile Dynatrace SaaS/yönetilen İzleyici

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, nasıl dağıtılacağı gösteriyoruz [Dynatrace](https://www.dynatrace.com/) OneAgent, Azure Container Service kümenizdeki tüm aracı düğümlere izlemek için. Bu yapılandırma için bir hesap ile Dynatrace SaaS/yönetilen ihtiyacınız var. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/yönetilen
Dynatrace, yüksek oranda dinamik kapsayıcı ve küme ortamları için bir bulutta yerel izleme çözümü ' dir. Bu, gerçek zamanlı kullanım verilerini kullanarak kapsayıcı dağıtımı ve bellek ayırma daha iyi iyileştirmenize imkan sağlar. Otomatik taban çizgisi oluşturma, sorunun bağıntı ve kök nedenin algılama sağlayarak uygulama ve altyapı sorunlarını otomatik olarak sunulan özelliğine sahip.

Dynatrace kullanıcı Arabirimi aşağıdaki şekilde gösterilmiştir:

![Dynatrace UI](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Önkoşullar 
[Dağıtma](container-service-deployment.md) ve [bağlanma](./../container-service-connect.md) Azure Container Service tarafından yapılandırılmış bir kümeye. [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Git [ https://www.dynatrace.com/trial/ ](https://www.dynatrace.com/trial/) Dynatrace SaaS hesabı ayarlamak için.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Dynatrace dağıtım Marathon ile yapılandırma
Bu adımlarda yapılandırma ve kümenize Marathon ile Dynatrace uygulamaları dağıtma gösterilmektedir.

1. DC/OS kullanıcı arabiriminize erişin [ http://localhost:80/ ](http://localhost:80/). DC/OS Arabiriminde bir kez gidin **Evreni** sekmesini ve ardından arama **Dynatrace**.

    ![DC/OS evreninde Dynatrace](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Yapılandırmayı tamamlamak için Dynatrace SaaS hesap veya ücretsiz bir deneme hesabı gerekir. Dynatrace panoya oturum açtığınızda, seçin **dağıtma Dynatrace**.

    ![Dynatrace PaaS tümleştirmesi kurma](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Sayfasında, seçin **PaaS tümleştirmesini ayarlama**. 

    ![Dynatrace API belirteci](./media/container-service-monitoring-dynatrace/api-token.png) 

4. DC/OS Evreni içinde Dynatrace OneAgent yapılandırma API belirtecinizi girin. 

    ![DC/OS evreninde Dynatrace OneAgent yapılandırma](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Örnekleri çalıştırmayı planladığınız düğüm sayısı için ayarlayın. Daha yüksek bir sayı çalışır, ancak DC/OS ayarı bu sayı aslında ulaşılana kadar yeni örnekleri bulmak denemeye devam edilecek. İsterseniz, ayrıca bu 1000000 gibi bir değere ayarlayabilirsiniz. Yeni bir düğüm kümeye eklendiğinde, bu durumda, Dynatrace otomatik olarak bir aracı DC/OS sürekli olarak daha fazla örnek dağıtın çalışılırken fiyatına bu yeni düğüme dağıtır.

    ![DC/OS Evreninde-örneklerinde Dynatrace yapılandırma](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Sonraki adımlar

Paketi yükledikten sonra Dynatrace panosuna geri gidin. Kümeniz kapsayıcılar için farklı ölçümlerin keşfedebilirsiniz. 