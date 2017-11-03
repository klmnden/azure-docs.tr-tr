---
title: "İzleyici Azure DC/OS kümesi - Operations Management | Microsoft Docs"
description: "Microsoft Operations Management Suite ile Azure kapsayıcı hizmeti DC/OS kümesi izleyin."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 9b8f96b34b53982c469273a3df9751ceb7930d60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Operations Management Suite ile Azure kapsayıcı hizmeti DC/OS kümesi izleme

Microsoft Operations Management Suite (OMS), şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft'un bulut tabanlı BT yönetim çözümüdür. Kapsayıcı, tek bir konumda kapsayıcı envanter, performans ve günlükleri görüntülemenize yardımcı OMS günlük analizi çözümde çözümüdür. Denetim, merkezi konumda günlükler görüntüleyerek kapsayıcıları sorun giderme ve bir ana bilgisayar üzerindeki fazladan kapsayıcı tüketen gürültülü bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözüm hakkında daha fazla bilgi için lütfen başvurmak [kapsayıcı çözüm günlük analizi](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-the-dcos-universe"></a>DC/OS universe'ten OMS ayarlama


Bu makale, bir DC/OS ayarladıktan ve basit bir web kapsayıcısı uygulamalarınızı kümede dağıtmış olan varsayar.

### <a name="pre-requisite"></a>Önkoşul
- [Microsoft Azure aboneliği](https://azure.microsoft.com/free/) -bu ücretsiz alabilirsiniz.  
- Microsoft OMS çalışma Kurulumu - bkz aşağıda "Adım 3"
- [DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) yüklü.

1. DC/OS panosunda Universe ve 'aşağıda gösterildiği gibi OMS' araması tıklayın.

![](media/container-service-monitoring-oms/image2.png)

2. **Yükle**'ye tıklayın. OMS sürüm bilgilerle yukarı pop görürsünüz ve bir **paket yükleme** veya **gelişmiş yükleme** düğmesi. Tıkladığınızda **gelişmiş yükleme**, hangi müşteri adayları, **OMS belirli yapılandırma özellikleri** sayfası.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Burada, girmeniz istenir `wsid` (OMS çalışma alanı kimliği) ve `wskey` (OMS birincil anahtar çalışma alanı kimliği için). Her ikisi de almak için `wsid` ve `wskey` OMS hesabı oluşturmak gereken <https://mms.microsoft.com>. Lütfen bir hesap oluşturmak için aşağıdaki adımları izleyin. İşiniz bittiğinde elde etmeniz hesabı oluşturma, `wsid` ve `wskey` tıklayarak **ayarları**, ardından **bağlı kaynakları**ve ardından **Linux sunucuları**, aşağıda gösterildiği gibi.

 ![](media/container-service-monitoring-oms/image5.png)

4. İstediğiniz ve 'Gözden geçirin ve yükleyin' düğmesini tıklatın sayı, OMS örnekleri seçin. Genellikle, OMS örnekleri Aracısı kümenizdeki sahip VM'in sayısına eşit sayıda istersiniz. Linux için OMS aracısının izleme bilgileri ve günlük kaydı bilgileri toplamak için istediği her bir VM üzerinde tek tek kapsayıcıları yükler gibidir.

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

OMS kapsayıcı çözüm hakkında daha fazla bilgi için lütfen başvurmak [kapsayıcı çözüm günlük analizi](../../log-analytics/log-analytics-containers.md).

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

 OMS, kapsayıcıları izlemek için ayarladığınız göre[kapsayıcı panonuz bkz](../../log-analytics/log-analytics-containers.md).
