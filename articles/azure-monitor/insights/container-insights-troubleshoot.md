---
title: Kapsayıcılar için Azure İzleyici gidermek için nasıl | Microsoft Docs
description: Bu makalede, sorun giderme ve kapsayıcılar için Azure İzleyici ile ilgili sorunları giderme nasıl açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: magoedte
ms.openlocfilehash: db4b468c03d93b073067083f4fae1ec86c70dde8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494686"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici sorunlarını giderme

Azure Kubernetes Service (AKS) kümenizi kapsayıcılar için Azure İzleyici ile izleme yapılandırdığınızda veri toplama işlemini engelliyor veya durum raporlama bir sorunla karşılaşabilirsiniz. Bu makalede bazı yaygın sorunlar ve sorun giderme adımları ayrıntılı olarak açıklanmaktadır.

## <a name="authorization-error-during-onboarding-or-update-operation"></a>Ekleme veya güncelleştirme işlemi sırasında Yetkilendirme hatası
Kapsayıcılar için Azure İzleyicisi'ni etkinleştirmek veya toplama ölçümleri desteklemek için küme güncelleştirme sırasında bir hata alabilirsiniz. - aşağıdaki benzeyen *< kullanıcı kimliği > istemci ' nesne kimliği '< kullanıcının objectID >' yoktur kapsamı üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi*

Ekleme veya güncelleştirme işlemi sırasında verme **izleme ölçümleri yayımcı** rol ataması, küme kaynağında denenir. Ölçümleri koleksiyonunu desteklemek, Azure İzleyici'kapsayıcıları veya güncelleştirmesi için etkinleştirme işlemini başlatan kullanıcı erişiminiz olmalıdır **Microsoft.Authorization/roleAssignments/write** AKS kümesi izni Kaynak kapsamı. Yalnızca üyelerinin **sahibi** ve **kullanıcı erişimi Yöneticisi** yerleşik roller, bu izin için erişim verilir. Ayrıntılı düzeyi izinler atanıyor, güvenlik ilkeleriniz ihtiyacınız varsa görüntülemenizi öneririz [özel roller](../../role-based-access-control/custom-roles.md) ve bu gereksinim duyan kullanıcılara atayın. 

Aşağıdaki adımları uygulayarak Azure portalından bu rolü el ile de verebilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde yazın **Kubernetes**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Azure Kubernetes**.
3. Kubernetes kümelerini listesinde listesinden seçin.
2. Sol taraftaki menüden **erişim denetimi (IAM)**.
3. Seçin **+ Ekle** rolü ataması ekleme ve seçmek için **izleme ölçümleri yayımcı** rolü altında **seçin** kutusuna **AKS** için abonelikte tanımlanan ilkeleri kümeleri sonuçlarına hizmet Filtresi. Bu kümeye özgü listeden birini seçin.
4. Seçin **Kaydet** rol atama tamamlanması. 

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Kapsayıcılar için Azure İzleyici herhangi bir bilgi bildirmeyen ancak etkin
Kapsayıcılar için Azure İzleyici başarıyla etkinleştirildikten ve yapılandırıldıktan sonra ancak durum bilgilerini görüntüleyemezsiniz veya günlük sorgudan döndürülen sonuç, aşağıdaki adımları izleyerek sorunu tanılamak: 

1. Komutunu çalıştırarak aracının durumunu denetleyin: 

    `kubectl get ds omsagent --namespace=kube-system`

    Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Aracı sürümü ile dağıtım durumunu denetlemek *06072018* veya daha sonra komutunu kullanarak:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Doğru şekilde dağıtıldığını gösterir aşağıdaki örnek çıktıya benzemelidir:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Komutunu kullanarak çalıştığını doğrulamak için pod durumunu kontrol edin: `kubectl get pods --namespace=kube-system`

    Çıkış durumu aşağıdaki örneğe benzemelidir *çalıştıran* omsagent için:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Aracı günlüklerini denetleyin. Kapsayıcı Aracısı dağıtıldığında OMI komutları çalıştırarak hızlı bir denetim çalıştırır ve sağlayıcı ve aracı sürümünü gösterir. 

5. Aracıyı eklenen başarıyla sağlandığını doğrulamak için komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

    Durum, aşağıdaki örneğe benzemelidir:

    ```
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
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="error-messages"></a>Hata iletileri

Kapsayıcılar için Azure İzleyicisi'ni kullanırken karşılaşabileceğiniz bilinen hatalar aşağıdaki tabloda özetlenmiştir.

| Hata iletileri  | Eylem |  
| ---- | --- |  
| Hata iletisi `No data for selected filters`  | Bu, yeni oluşturulan kümeleri için izleme veri akışı oluşturmak için biraz zaman alabilir. Lütfen en az veri kümeniz için görüntülenecek 10-15 dakika bekleyin. |   
| Hata iletisi `Error retrieving data` | Azure Kubenetes Service küme sistem durumu ve performans izlemesi için devam ederken, küme ve Azure Log Analytics çalışma alanı arasında bir bağlantı kurulur. Bir Log Analytics çalışma alanı, kümeniz için tüm izleme verilerini depolamak için kullanılır. Log Analytics çalışma alanınızın silindi veya kayıp sahip olduğunda bu hata oluşabilir. Gözden geçirerek, çalışma alanınızı kullanılabilir olup olmadığını denetleyin [erişimini yönetme](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json#view-workspace-details). Çalışma alanı eksikse, re-eklemek için Azure İzleyici ile kümenizi kapsayıcılar için ihtiyacınız olacak. RE-ekleme, şunları yapmanız gerekir [devre dışı](container-insights-optout.md) küme için izleme ve [etkinleştirme](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-for-a-new-cluster) yeniden kapsayıcılar için Azure İzleyici. |  
| `Error retrieving data` az aks CLI aracılığıyla kapsayıcılar için Azure İzleyici ekledikten sonra | Zaman ekleme kullanarak `az aks cli`, çok az kapsayıcılar için Azure İzleyici düzgün eklenen ayarlanmamış olabilir. Eklenen çözüm olup olmadığını denetleyin. Bunu yapmak için Log Analytics çalışma alanınıza gidin ve seçerek çözüm kullanılabilir olup olmadığını **çözümleri** sol tarafındaki bölmeden. Bu sorunu çözmek için çözüm yönergeleri izleyerek tekrar dağıtmanız gerekecektir [kapsayıcılar için Azure İzleyici dağıtma](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json) |  

Sorunun tanılanmasına yardımcı olmak için kullanılabilir bir sorun giderme betik sağladık [burada](https://github.com/Microsoft/OMS-docker/tree/ci_feature_prod/Troubleshoot#troubleshooting-script).  

## <a name="next-steps"></a>Sonraki adımlar
AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).