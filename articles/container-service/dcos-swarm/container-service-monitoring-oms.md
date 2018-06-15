---
title: İzleyici Azure DC/OS kümesi - işlem yönetimi
description: Günlük analizi ile Azure kapsayıcı hizmeti DC/OS kümesi izleyin.
services: container-service
author: keikhara
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: b326e5b686e14cefac4e6376bd3f26787ea1d10d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32164600"
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-log-analytics"></a>Azure kapsayıcı hizmeti DC/OS kümesi günlük analizi ile izleme

Günlük analizi, yönetmek ve şirket içi korumak ve altyapı bulut yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür. Kapsayıcı, tek bir konumda kapsayıcı envanter, performans ve günlükleri görüntülemenize yardımcı olan günlük analizi çözümde çözümüdür. Denetim, merkezi konumda günlükler görüntüleyerek kapsayıcıları sorun giderme ve bir ana bilgisayar üzerindeki fazladan kapsayıcı tüketen gürültülü bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözüm hakkında daha fazla bilgi için lütfen başvurmak [kapsayıcı çözüm günlük analizi](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-log-analytics-from-the-dcos-universe"></a>Günlük analizi DC/OS universe'ten ayarlama


Bu makale, bir DC/OS ayarladıktan ve basit bir web kapsayıcısı uygulamalarınızı kümede dağıtmış olan varsayar.

### <a name="pre-requisite"></a>Önkoşul
- [Microsoft Azure aboneliği](https://azure.microsoft.com/free/) -bu ücretsiz alabilirsiniz.  
- Günlük analizi çalışma alanı kurulum - Bkz aşağıda "Adım 3"
- [DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) yüklü.

1. DC/OS panosunda Universe ve 'aşağıda gösterildiği gibi OMS' araması tıklayın.

![](media/container-service-monitoring-oms/image2.png)

2. **Yükle**'ye tıklayın. Sürüm bilgileri ile yukarı pop görürsünüz ve bir **paket yükleme** veya **gelişmiş yükleme** düğmesi. Tıkladığınızda **gelişmiş yükleme**, hangi müşteri adayları, **OMS belirli yapılandırma özellikleri** sayfası.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Burada, girmeniz istenir `wsid` (günlük analizi çalışma alanı kimliği) ve `wskey` (çalışma alanı kimliği için birincil anahtar). Her ikisi de almak için `wsid` ve `wskey` hesabı oluşturmak gereken <https://mms.microsoft.com>.
Lütfen bir hesap oluşturmak için aşağıdaki adımları izleyin. İşiniz bittiğinde elde etmeniz hesabı oluşturma, `wsid` ve `wskey` tıklayarak **ayarları**, ardından **bağlı kaynakları**ve ardından **Linux sunucuları**, aşağıda gösterildiği gibi.

 ![](media/container-service-monitoring-oms/image5.png)

4. İstediğiniz ve 'Gözden geçirin ve yükleyin' düğmesini tıklatın örneklerinin sayısını seçin. Genellikle, örnekleri Aracısı kümenizdeki sahip VM'in sayısına eşit sayıda istersiniz. İzleme bilgileri ve günlük kaydı bilgileri toplamak için istediği her bir VM üzerinde tek tek kapsayıcı olarak Linux için OMS Aracısı'nı yükler.

## <a name="setting-up-a-simple-oms-dashboard"></a>Basit bir OMS Pano ayarlama

Linux VM'ler için OMS Aracısı'nı yükledikten sonra sonraki adıma OMS Pano ayarlamaktır. Bunu yapmanın iki yolu vardır: OMS portalında veya Azure portalı.

### <a name="oms-portal"></a>OMS portalı 

OMS portalı oturum açma (<https://mms.microsoft.com>) ve Git **çözüm Galerisi**.

![](media/container-service-monitoring-oms/image6.png)

İçinde olduğunda **çözüm Galerisi**seçin **kapsayıcıları**.

![](media/container-service-monitoring-oms/image7.png)

Kapsayıcı çözüm seçtikten sonra OMS Genel Bakış Panosu sayfasında kutucuk görürsünüz. Alınan kapsayıcı verileri dizini oluşturulduktan sonra çözüm görünümü döşeme bilgileriyle doldurulan kutucuk görürsünüz.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Azure portalında oturum açma <https://portal.microsoft.com/>. Git **Market**seçin **izleme + Yönetim** tıklatıp **bkz tüm**. Yazın `containers` arama. Arama sonuçlarında "kapsayıcıları" görürsünüz. Seçin **kapsayıcıları** tıklatıp **oluşturma**.

![](media/container-service-monitoring-oms/image9.png)

Tıkladığınızda **oluşturma**, onu için çalışma alanınızda sorar. Çalışma alanınızı seçin veya bir yoksa yeni bir çalışma alanı oluşturun.

![](media/container-service-monitoring-oms/image10.PNG)

Çalışma alanınızı seçtikten sonra tıklatın **oluşturma**.

![](media/container-service-monitoring-oms/image11.png)

Günlük analizi kapsayıcı çözüm hakkında daha fazla bilgi için lütfen başvurmak [kapsayıcı çözüm günlük analizi](../../log-analytics/log-analytics-containers.md).

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a>OMS Aracısı ACS DC/OS ile ölçeklendirme 

Gerçek düğüm sayısı yetersiz OMS aracısının yüklü gerekir ya da daha fazla VM ekleyerek VMSS ölçekleme durumda ölçeklendirme tarafından yapabilirsiniz `msoms` hizmet.

Marathon veya DC/OS kullanıcı Arabirimi Hizmetleri sekmesine gidin ve düğüm sayınız ölçeklendirin.

![](media/container-service-monitoring-oms/image12.PNG)

Bu OMS Aracısı henüz dağıtmadıysanız diğer düğümlere dağıtır.

## <a name="uninstall-ms-oms"></a>MS OMS kaldırma

Kaldırmak için MS OMS aşağıdaki komutu girin:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Bize bildirin!!!
Neler çalışır? Eksik nedir? Başka ne bunun sizin için kullanışlı olması gerekiyor? Konumundaki bize bildirin <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Sonraki adımlar

 Günlük analizi, kapsayıcıları izlemek için ayarladığınız göre[kapsayıcı panonuz bkz](../../log-analytics/log-analytics-containers.md).
