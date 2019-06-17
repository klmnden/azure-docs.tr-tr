---
title: (KULLANIM DIŞI) Azure DC/OS kümesi - Management işlemleri izleme
description: Log Analytics ile bir Azure Container Service DC/OS kümesini izleme.
services: container-service
author: keikhara
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 290141136672729060f5156d645c47ac303fa0c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60810095"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-log-analytics"></a>(KULLANIM DIŞI) Log Analytics ile bir Azure Container Service DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Log Analytics, yönetmek ve şirket içi koruma hem de bulut altyapısında yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür. Kapsayıcı çözümü, tek bir konumda kapsayıcı envanteri, performans ve günlükleri görüntülemenize yardımcı olur. Log analytics'te bir çözümdür. Denetim, kapsayıcıları merkezi konumda günlüklerini görüntüleyerek sorun giderme ve gürültülü fazla kapsayıcı bir konakta tüketen bulun.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözümü hakkında daha fazla bilgi için bkz: [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md).

## <a name="setting-up-log-analytics-from-the-dcos-universe"></a>DC/OS evreni Log Analytics'ten ayarlama


Bu makalede, bir DC/OS kümesi ve kümede basit web kapsayıcı uygulamaları dağıtmış olduğunuz varsayılır.

### <a name="pre-requisite"></a>Önkoşul
- [Microsoft Azure aboneliği](https://azure.microsoft.com/free/) -ücretsiz bir abonelik edinebilirsiniz.  
- Günlük analizi çalışma alanı kurulum - bkz. "3. adım" altında
- [DC/OS CLI](https://docs.mesosphere.com/1.12/cli) yüklü.

1. DC/OS Panoda Evreni ve arama için 'OMS' aşağıda gösterildiği gibi tıklayın.

   >[!NOTE]
   >OMS, artık Log Analytics da adlandırılır.

   ![](media/container-service-monitoring-oms/image2.png)

2. **Yükle**'ye tıklatın. Sürüm bilgileri içeren bir açılır pencere görürsünüz ve **yükleme paketi** veya **gelişmiş yükleme** düğmesi. Tıkladığınızda **gelişmiş yükleme**, hangi müşteri adayları, **OMS belirli yapılandırma özellikleri** sayfası.

   ![](media/container-service-monitoring-oms/image3.png)

   ![](media/container-service-monitoring-oms/image4.png)

3. Burada, girmeniz istenir `wsid` (Log Analytics çalışma alanı kimliği) ve `wskey` (çalışma alanı kimliği için birincil anahtar). Her ikisi de almak için `wsid` ve `wskey` adresinde bir hesap oluşturmak için ihtiyacınız <https://mms.microsoft.com>.
   Hesap oluşturmak için adımları izleyin. İşiniz bittiğinde elde etmeniz hesabı oluşturma, `wsid` ve `wskey` tıklayarak **ayarları**, ardından **bağlı kaynaklar**, ardından **Linuxsunucuları**, aşağıda gösterildiği gibi.

   ![](media/container-service-monitoring-oms/image5.png)

4. İstediğiniz ve 'Gözden geçirin ve Yükle' düğmesine tıklayın. örnek sayısını seçin. Genellikle, aracı kümenizde sahip VM'nin sayısına eşit örneklerinin isteyeceksiniz. İzleme bilgileri ve günlük bilgilerini toplamak isteyen her VM'de ayrı ayrı kapsayıcıları olarak Linux için log Analytics aracısını yükler.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

## <a name="setting-up-a-simple-log-analytics-dashboard"></a>Basit bir Log Analytics Pano ayarlama

Vm'lerinde Linux için Log Analytics aracısını yükledikten sonra sonraki adım Log Analytics Pano ayarlamaktır. Azure Portalı aracılığıyla pano ayarlayabilirsiniz.

### <a name="azure-portal"></a>Azure portal 

Adresinden Azure portalında oturum açın <https://portal.microsoft.com/>. Git **Market**seçin **izleme + Yönetim** tıklatıp **bkz tüm**. Yazarak `containers` Ara. Arama sonuçlarında "kapsayıcı" görürsünüz. Seçin **kapsayıcıları** tıklatıp **Oluştur**.

![](media/container-service-monitoring-oms/image9.png)

' A tıkladığınızda **Oluştur**, bunu çalışma alanınız için sorar. Çalışma alanınızı seçin veya bir yoksa yeni bir çalışma alanı oluşturun.

![](media/container-service-monitoring-oms/image10.PNG)

Çalışma alanınızı seçtikten sonra tıklayın **Oluştur**.

![](media/container-service-monitoring-oms/image11.png)

Log Analytics kapsayıcı çözümü hakkında daha fazla bilgi için bkz [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md).

### <a name="how-to-scale-log-analytics-agent-with-acs-dcos"></a>Log Analytics aracısını ACS DC/OS ile ölçeklendirme 

Log Analytics aracısını gerçek düğüm sayısı eksikliği yüklü gerekir veya sanal makine ölçek kümesi VM daha fazla ekleyerek'kurmak ölçeklendirme durumunda, ölçeklendirerek yapabilirsiniz `msoms` hizmeti.

Marathon veya DC/OS UI Hizmetleri sekmesine gidin ve, düğüm sayısının ölçeğini artırın.

![](media/container-service-monitoring-oms/image12.PNG)

Bu, Log Analytics aracısını henüz dağıtmadınız diğer düğümlere dağıtır.

## <a name="uninstall-ms-oms"></a>MS OMS kaldırma

Kaldırmak için MS OMS aşağıdaki komutu girin:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Bunu bize bildirin!!!
Neyin işe yaradığını? Eksik olan nedir? Bu sizin için yararlı olacak başka ne gerekiyor? Bize bildirin <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Sonraki adımlar

 Log Analytics'i ayarlama, kapsayıcılarınızı izleyecek şekilde ayarladığınız göre[kapsayıcı panonuzu görmek](../../azure-monitor/insights/containers.md).
