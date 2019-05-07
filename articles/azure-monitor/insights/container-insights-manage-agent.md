---
title: Azure İzleyici kapsayıcı Aracısı'nı yönetme | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure İzleyici tarafından kullanılan kapsayıcılı Log Analytics aracısını ile en sık kullanılan bakım görevlerini yönetmek açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2018
ms.author: magoedte
ms.openlocfilehash: e1d47be159d4721aed4b055a51acf675688b855e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65071793"
---
# <a name="how-to-manage-the-azure-monitor-for-containers-agent"></a>Azure İzleyici kapsayıcı Aracısı'nı yönetme
Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. İlk dağıtımdan sonra yordamı veya yaşam döngüsü sırasında gerçekleştirmeniz gerekebilir isteğe bağlı görevleri vardır. Bu makale hakkında ayrıntılı bilgi el ile aracı yükseltme ve belirli bir kapsayıcıda ortam değişkenlerinin koleksiyonunu devre dışı bırakın. 

## <a name="how-to-upgrade-the-azure-monitor-for-containers-agent"></a>Azure İzleyici kapsayıcı Aracısı yükseltme
Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. Aracıyı yeni bir sürümü yayımlandığında, aracıyı Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilir.  

Bu makalede, aracı yükseltme başarısız olursa, el ile aracı yükseltme işlemi açıklanmaktadır. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

### <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>İzlenen bir Kubernetes kümesinde aracı yükseltme
Aracı yükseltme işlemi, iki anlaşılır adımdan oluşur. İlk adım, Azure İzleyici ile Azure CLI kullanarak kapsayıcıları için izleme devre dışı bırakmaktır.  İçinde açıklanan adımları izleyin [izlemeyi devre dışı](container-insights-optout.md?#azure-cli) makalesi. Azure CLI kullanarak çözüm ve çalışma alanında depolanan karşılık gelen verileri etkilemeden Kümedeki düğümlerden aracıyı kaldırmak sağlıyor. 

>[!NOTE]
>Bu bakım etkinliği gerçekleştiriyorsanız, kümedeki düğümler, toplanan verileri ilettiğiniz değil ve performans görünümlerine veri gösterilmez arasındaki zaman aracıyı kaldırın ve ardından yeni sürümü yükleyin. 
>

Aracısı'nın yeni sürümünü yüklemek için açıklanan adımları izleyin. [Azure CLI kullanarak izlemeyi etkinleştirin](container-insights-enable-new-cluster.md#enable-using-azure-cli), bu işlemi tamamlamak için.  

İzleme yeniden etkinleştirdikten sonra bu küme için güncelleştirilmiş sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. Aracı başarıyla yükseltildi doğrulamak için komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

Durum, aşağıdaki örneğe benzemelidir burada değeri *OMI* ve *omsagent* belirtilen en son sürümü ile eşleşmelidir [Aracı sürüm geçmişi](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="how-to-disable-environment-variable-collection-on-a-container"></a>Bir kapsayıcı ortam değişkeni koleksiyonunu devre dışı bırakma
Kapsayıcılar için Azure İzleyici, bir pod içinde çalışan kapsayıcılar ortam değişkenlerini toplar ve bunları seçilen kapsayıcı içindeki özellik bölmesinde sunar **kapsayıcıları** görünümü. AKS kümesi ya da sonra dağıtım sırasında belirli bir kapsayıcı için koleksiyonu devre dışı bırakarak bu davranışı denetleyebilirsiniz ortam değişkenini ayarlayarak *AZMON_COLLECT_ENV*. Bu özellik, aracı sürümü – ciprod11292018 kullanılabilir ve daha yüksek.  

Yeni veya mevcut bir kapsayıcısındaki ortam değişkenlerinin koleksiyonunu devre dışı bırakmak için değişkeni ayarlayın **AZMON_COLLECT_ENV** değeriyle **False** Kubernetes dağıtımı yaml yapılandırma dosyanızdaki.   

```  
- name: AZMON_COLLECT_ENV  
  value: "False"  
```  

AKS kapsayıcı değişikliği uygulamak için aşağıdaki komutu çalıştırın: `kubectl apply -f  <path to yaml file>`.

Yapılandırma değişikliğinin etkili sürdü doğrulamak için bir kapsayıcıda seçin **kapsayıcıları** kapsayıcılar için Azure İzleyici ve özellik paneli görüntülemek için genişletme **ortam değişkenlerini**.  Bölümün yalnızca daha önce - oluşturulan değişken göstermelidir **AZMON_COLLECT_ENV = FALSE**. Diğer tüm kapsayıcılar için ortam değişkenlerini bölümünde bulunan tüm ortam değişkenlerini listelemelisiniz.   

Ortam değişkenlerini bulunmasını yeniden etkinleştirmek için daha önce aynı işlem geçerlidir ve değerini **False** için **True**ve yeniden `kubectl` kapsayıcıyı güncelleştirmeye yönelik komutu.  

```  
- name: AZMON_COLLECT_ENV  
  value: "True"  
```  

## <a name="next-steps"></a>Sonraki adımlar
Aracı yükseltme sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](container-insights-troubleshoot.md) desteği.