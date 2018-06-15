---
title: İzleyici Azure DC/OS kümesi - Dynatrace
description: Azure kapsayıcı hizmeti DC/OS kümesi Dynatrace ile izleyin. Dynatrace OneAgent DC/OS Panoyu kullanarak dağıtın.
services: container-service
author: MartinGoodwell
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 3d1bfc3bb61781d487c40831edd5da6fcb5a7df9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32162050"
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>Azure kapsayıcı hizmeti DC/OS kümesi Dynatrace SaaS/yönetilen'ile izleme

Bu makalede, nasıl dağıtılacağı gösteriyoruz [Dynatrace](https://www.dynatrace.com/) Azure kapsayıcı hizmeti kümenizdeki tüm Aracısı düğümleri izlemek için OneAgent. Bu yapılandırma için bir hesap ile Dynatrace SaaS/yönetilen gerekir. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/yönetilen
Dynatrace bir bulut yerel izleme için yüksek oranda dinamik kapsayıcı ve küme ortamları çözümüdür. Gerçek zamanlı kullanım verilerini kullanarak bellek ayırmaları ve kapsayıcı dağıtımlarını daha iyi iyileştirmek sağlar. Otomatik olarak uygulama ve altyapı sorunları otomatik taban çizgisi oluşturma, sorunu bağıntı ve temel nedenin algılama sağlayarak yerini tam olarak belirtmekte yeteneğine sahiptir.

Aşağıdaki şekil Dynatrace UI gösterir:

![Dynatrace kullanıcı Arabirimi](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Önkoşullar 
[Dağıtma](container-service-deployment.md) ve [bağlanmak](./../container-service-connect.md) Azure kapsayıcı hizmeti tarafından yapılandırılmış bir kümeye. [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Git [ https://www.dynatrace.com/trial/ ](https://www.dynatrace.com/trial/) bir Dynatrace SaaS hesabı ayarlamak için.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Marathon ile Dynatrace dağıtımını yapılandırma
Bu adımları yapılandırmak ve Marathon kümenizle Dynatrace uygulamaları dağıtmak nasıl gösterir.

1. DC/OS kullanıcı Arabirimi aracılığıyla erişim [ http://localhost:80/ ](http://localhost:80/). DC/OS kullanıcı Arabiriminde bir kez gidin **Universe** sekmesinde arayın ve sonra **Dynatrace**.

    ![DC/OS Universe Dynatrace](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Yapılandırmayı tamamlamak için bir Dynatrace SaaS hesap veya ücretsiz bir deneme hesabı gerekir. Dynatrace panoya oturum sonra seçeneğini **dağıtmak Dynatrace**.

    ![PaaS tümleştirme Dynatrace ayarlama](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Sayfasında, seçin **PaaS tümleştirmesini ayarlama**. 

    ![Dynatrace API belirteci](./media/container-service-monitoring-dynatrace/api-token.png) 

4. DC/OS Universe içinde Dynatrace OneAgent yapılandırma içine API belirtecinizi girin. 

    ![DC/OS Universe Dynatrace OneAgent yapılandırma](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Örnekleri çalıştırmayı amaçladığınız düğüm sayısını ayarlayın. Daha yüksek bir sayı çalışır, ancak DC/OS ayarı bu sayıyı gerçekte ulaşılana kadar yeni örneklerini bulmak denemeye devam edilecek. İsterseniz, ayrıca bu 1000000 gibi bir değere ayarlayabilirsiniz. Bu durumda, yeni bir düğüm kümeye eklendiğinde Dynatrace bir aracıyı otomatik olarak yeni o düğümde, DC/OS sürekli başka örnekleri dağıtılmaya çalışılırken fiyat dağıtır.

    ![DC/OS Universe-örneklerde Dynatrace yapılandırma](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Sonraki adımlar

Paketi yükledikten sonra Dynatrace panosuna geri gidin. Küme içinde kapsayıcıları farklı kullanım ölçümleri gözden geçirebilirsiniz. 