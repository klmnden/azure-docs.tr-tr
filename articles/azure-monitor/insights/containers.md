---
title: Azure İzleyici'de kapsayıcı izleme çözümü | Microsoft Docs
description: Azure İzleyici'de kapsayıcı izleme çözümü, Docker ve Windows görüntüleme ve yönetme yardımcı olan tek bir konumda kapsayıcı konakları.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: magoedte
ms.openlocfilehash: 5eec77084e104f7bd541405e2ef18e5a178e869c
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877797"
---
# <a name="container-monitoring-solution-in-azure-monitor"></a>Azure İzleyici'de kapsayıcı izleme çözümü

![Kapsayıcıları simgesi](./media/containers/containers-symbol.png)

Bu makalede ayarlamak ve Docker ve Windows görüntüleme ve yönetme yardımcı olan Azure İzleyicisi'nde kapsayıcı izleme çözümü kullanmak nasıl kapsayıcı konağında tek bir konumda. Docker, yazılım dağıtımı için BT altyapısını otomatikleştirmek kapsayıcılar oluşturmak için kullanılan bir yazılım sanallaştırma sistemidir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Çözüm gösterir hangi kapsayıcıları çalıştırmak, hangi kapsayıcı görüntüsü ister çalışıyor ve kapsayıcıları çalıştığı. Kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgileri görüntüleyebilirsiniz. Ayrıca, kapsayıcıları, Docker veya Windows konak uzaktan görüntüleme gerek kalmadan Merkezi günlük arama ve görüntüleme giderebilirsiniz. Bir konakta gürültülü ve alıcı aşırı kaynakları olabilir kapsayıcıları bulabilirsiniz. Ve merkezileştirilmiş CPU, bellek, depolama ve kapsayıcılar için ağ kullanım ve performans bilgilerini görüntüleyebilirsiniz. Windows çalıştıran bilgisayarlarda, merkezileştirme ve Windows Server günlüklerinden karşılaştırın Hyper-V ve Docker kapsayıcıları. Çözüm, aşağıdaki kapsayıcı düzenleyicileri destekler:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift

Dağıtılmış iş yüklerinizin performansını izleme ilgileniyorsanız Kubernetes ortamlarını barındırılan Azure Kubernetes Service (AKS), bkz: [İzleyici Azure Kubernetes hizmeti](../../azure-monitor/insights/container-insights-overview.md). Kapsayıcı izleme çözümü, platform izlemek için destek içermez.  

Aşağıdaki diyagram çeşitli kapsayıcı konağında ve Azure İzleyici ile aracıları arasındaki ilişkiler gösterilmektedir.

![Kapsayıcıları diyagramı](./media/containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Sistem gereksinimleri ve desteklenen platformlar

Başlamadan önce önkoşulları karşılaması doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Kapsayıcı izleme çözümü desteklemek için Docker Orchestrator ve işletim sistemi platformu
Aşağıdaki tabloda, Azure İzleyici ile izleme desteği kapsayıcı envanteri, performans ve Günlükler işletim sistemi ve Docker düzenleme özetlenmektedir.   

| | ACS | Linux | Windows | Kapsayıcı<br>Envanter | Görüntü<br>Envanter | Node<br>Envanter | Kapsayıcı<br>Performans | Kapsayıcı<br>Olay | Olay<br>Günlük | Kapsayıcı<br>Günlük |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Hizmet<br>Fabric | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat Aç<br>Shift | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(tek başına) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux Sunucu<br>(tek başına) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


### <a name="docker-versions-supported-on-linux"></a>Linux üzerinde desteklenen docker sürümleri

- Docker için 1.11 1.13
- Docker CE ve EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 kapsayıcı konakları olarak desteklenen Linux dağıtımları


- Ubuntu 14.04 LTS ve 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE 42.2 LEAP
- CentOS 7.2 ve 7.3
- SLES 12
- RHEL 7.2 ve 7.3
- Red Hat OpenShift kapsayıcı Platformu (OCP) 3.4 ve 3.5
- ACS Mesosphere DC/OS için 1.7.3 1.8.8
- ACS Kubernetes için 1.4.5 1.6
    - Kubernetes olayları, Kubernetes Envanter ve kapsayıcı işlemleri daha sonra Linux için Log Analytics Aracısı'nın sürümü 1.4.1-45 ile yalnızca desteklenen
- ACS Docker Swarm

[!INCLUDE [log-analytics-agent-note.md](../../../includes/log-analytics-agent-note.md)] 

### <a name="supported-windows-operating-system"></a>Desteklenen Windows işletim sistemi

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional veya Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Desteklenen Windows docker sürümleri

- Docker 1.12 ve 1.13
- Docker 17.03.0 ve üzeri

## <a name="installing-and-configuring-the-solution"></a>Çözümünü yükleme ve yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

1. Log Analytics çalışma alanınızdan kapsayıcı izleme çözümünü ekleyin [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) veya açıklanan işlemi kullanarak [çözüm galeri'sinden izleme Ekle](../../azure-monitor/insights/solutions.md).

2. Yükleyin ve Docker ile bir Log Analytics aracısını kullanın. İşletim sistemi ve Docker orchestrator bağlı olarak, aracınızı yapılandırmak için aşağıdaki yöntemleri kullanabilirsiniz.
   - Tek başına konakları için:
     - Desteklenen Linux işletim sistemlerinde yüklemek ve Docker'ı çalıştırın ve ardından yükleme ve yapılandırma [Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md).  
     - CoreOS üzerinde Linux için Log Analytics aracısını çalıştıramazsınız. Bunun yerine, Linux için Log Analytics aracısını kapsayıcı bir sürümünü çalıştırın. CoreOS dahil olmak üzere Linux kapsayıcı konağında veya Azure kamu bulutunda kapsayıcılar ile çalışıyorsanız, CoreOS dahil olmak üzere Azure kamu Linux kapsayıcı konağında gözden geçirin.
     - Windows Server 2016 ve Windows 10, Docker altyapısı ve istemci yükleme ardından bilgi toplamak ve Azure İzleyici göndermek için bir aracı bağlayın. Gözden geçirme [yüklemek ve Windows kapsayıcı konakları yapılandırma](#install-and-configure-windows-container-hosts) bir Windows ortamınız varsa.
   - Docker birden çok konak düzenleme için:
     - Bir Red Hat OpenShift ortamınız varsa, Red Hat OpenShift için bir Log Analytics aracısını Yapılandır gözden geçirin.
     - Azure Container Service kullanan bir Kubernetes kümesi varsa:
       - Gözden geçirme [Kubernetes için bir Log Analytics Linux Aracısı'nı yapılandırma](#configure-a-log-analytics-linux-agent-for-kubernetes).
       - Gözden geçirme [Kubernetes için bir Log Analytics Windows aracı yapılandırma](#configure-a-log-analytics-windows-agent-for-kubernetes).
       - Kullanım Linux Kubernetes Log Analytics aracısını dağıtmak için Helm gözden geçirin.
     - Bir Azure Container Service DC/OS kümeniz varsa, daha fazla bilgi [Azure İzleyici ile bir Azure Container Service DC/OS kümesini izleme](../../container-service/dcos-swarm/container-service-monitoring-oms.md).
     - Bir Docker Swarm modu ortamı varsa, yapılandırma Docker Swarm için bir Log Analytics aracısını edinin.
     - Bir Service Fabric kümeniz varsa, daha fazla bilgi [Azure İzleyici ile kapsayıcıları izlemek](../../service-fabric/service-fabric-diagnostics-oms-containers.md).

Gözden geçirme [Windows üzerinden Docker altyapısının](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) makale yüklemek ve Windows çalıştıran bilgisayarlarda, Docker altyapısı yapılandırma hakkında ek bilgi için.

> [!IMPORTANT]
> Docker çalıştırmalıdır **önce** yüklediğiniz [Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md) kapsayıcı konaklarınız üzerinde. Docker'ı yüklemeden önce aracıyı zaten yüklediyseniz, Linux için Log Analytics aracısını yeniden yüklemeniz gerekir. Docker hakkında daha fazla bilgi için bkz: [Docker Web sitesi](https://www.docker.com).


### <a name="install-and-configure-linux-container-hosts"></a>Yükleme ve yapılandırma Linux kapsayıcı konakları

Docker'ı yükledikten sonra aracıyı kullanmak için Docker ile yapılandırmak için kapsayıcı konağı için aşağıdaki ayarları kullanın. İlk Log Analytics çalışma alanı kimliği ve Azure portalında bulabilirsiniz anahtarı gerekir. Çalışma alanınızda tıklayın **Hızlı Başlangıç** > **bilgisayarlar** görüntülemek için **çalışma alanı kimliği** ve **birincil anahtar**.  Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

**Tüm Linux kapsayıcı konaklar için CoreOS hariç:**

- Daha fazla bilgi ve Linux için Log Analytics aracısını yüklemek adımları için bkz. [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md).

**CoreOS dahil olmak üzere tüm Linux kapsayıcı konakları için:**

İzlemek istediğiniz kapsayıcı başlatın. Değiştirin ve aşağıdaki örneği kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**CoreOS dahil olmak üzere tüm Azure kamu Linux kapsayıcı konakları için:**

İzlemek istediğiniz kapsayıcı başlatın. Değiştirin ve aşağıdaki örneği kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Geçiş için bir kapsayıcı yüklü bir Linux Aracısı'nı kullanma**

Daha önce yüklenmiş doğrudan Aracısı kullanılan ve bunun yerine bir kapsayıcıda çalışan bir aracının kullanmak istiyorsanız, önce Linux için Log Analytics aracısını kaldırmanız gerekir. Bkz: [Linux için Log Analytics aracısını kaldırma](../../azure-monitor/learn/quick-collect-linux-computer.md) başarıyla aracıyı kaldırmak nasıl yapılacağını görmek için.  

#### <a name="configure-a-log-analytics-agent-for-docker-swarm"></a>Docker Swarm için bir Log Analytics aracısını yapılandırma

Log Analytics aracısını, Docker Swarm hakkında genel bir hizmet olarak çalıştırabilirsiniz. Log Analytics Aracısı hizmeti oluşturmak için aşağıdaki bilgileri kullanın. Log Analytics çalışma alanı kimliği ve birincil anahtarı sağlamanız gerekir.

- Aşağıdaki ana düğüm üzerinde çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Docker Swarm için güvenli bir gizli dizi

Çalışma alanı kimliği ve birincil anahtar için gizli anahtar oluşturulduktan sonra Docker Swarm için gizli bilgilerinizi oluşturmak için aşağıdaki bilgileri kullanın.

1. Aşağıdaki ana düğüm üzerinde çalıştırın.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Gizli dizileri düzgün oluşturulduğunu doğrulayın.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Log Analytics aracısını kapsayıcı için gizli dizileri bağlamak için aşağıdaki komutu çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="configure-a-log-analytics-agent-for-red-hat-openshift"></a>Red Hat OpenShift için bir Log Analytics aracısını yapılandırma
Log Analytics aracısını kapsayıcı izleme verilerini toplamaya başlamak için Red Hat OpenShift için eklemenin üç yolu vardır.

* [Linux için Log Analytics aracısını yükleme](../../azure-monitor/learn/quick-collect-linux-computer.md) doğrudan her bir düğümde OpenShift  
* [Log Analytics VM uzantısını etkinleştirme](../../azure-monitor/learn/quick-collect-azurevm.md) Azure'da bulunan her OpenShift düğümde  
* Log Analytics aracısını bir OpenShift arka plan programı kümesi olarak yükleme  

Bu bölümde bir OpenShift arka plan programı kümesi olarak Log Analytics aracısını yüklemek için gerekli adımları ele.  

1. OpenShift ana düğüm için oturum açın, yaml dosyası kopyalama [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) github, ana düğümü ve Log Analytics çalışma alanı Kimliğinizi ve birincil anahtarınızı değerini değiştirin.
2. Azure İzleyici için bir proje oluşturun ve kullanıcı hesabını ayarlamak için aşağıdaki komutları çalıştırın.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Arka plan programı kümesini dağıtmak için aşağıdaki komutu çalıştırın:

    `oc create -f ocp-omsagent.yaml`

5. Bunu doğrulamak için yapılandırılır ve çalışma doğru şunu yazın:

    `oc describe daemonset omsagent`  

    ve çıktıya benzemelidir:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Log Analytics aracısını arka plan programı kümesi yaml dosyası kullanırken, Log Analytics çalışma alanı kimliği ve birincil anahtarınızı korumak için gizli dizileri kullanmak istiyorsanız, aşağıdaki adımları gerçekleştirin.

1. OpenShift ana düğüm için oturum açın, yaml dosyası kopyalama [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) ve betik oluşturma gizli [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) github'dan.  Bu betik güvenli hale getirmek Log Analytics çalışma alanı kimliği ve birincil anahtar için gizli dizi yaml dosyası oluşturacak, bilgi secrete.  
2. Azure İzleyici için bir proje oluşturun ve kullanıcı hesabını ayarlamak için aşağıdaki komutları çalıştırın. Log Analytics çalışma alanı Kimliğiniz için betik oluşturma gizli ister <WSID> ve birincil anahtar <KEY> ve isteğe bağlı olarak tamamlandıktan sonra ocp-secret.yaml dosyası oluşturur.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Gizli dizi dosyasını aşağıdaki komutu çalıştırarak dağıtın:

    `oc create -f ocp-secret.yaml`

5. Aşağıdaki komutu çalıştırarak dağıtımı doğrulama:

    `oc describe secret omsagent-secret`  

    ve çıktıya benzemelidir:  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. Log Analytics aracısını arka plan programı kümesi yaml dosyası, aşağıdaki komutu çalıştırarak dağıtın:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Aşağıdaki komutu çalıştırarak dağıtımı doğrulama:

    `oc describe ds oms`

    ve çıktıya benzemelidir:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

#### <a name="configure-a-log-analytics-linux-agent-for-kubernetes"></a>Kubernetes için bir Log Analytics Linux Aracısı'nı yapılandırma

Kubernetes için Linux için Log Analytics aracısını yüklemek çalışma alanı kimliği ve birincil anahtar için gizli dizileri yaml dosyası oluşturmak için bir betik kullanın. Konumunda [Log Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) sayfasında, olan veya olmayan gizli bilgilerinizi kullandığınız dosyalar da mevcuttur.

- Linux DaemonSet için varsayılan Log Analytics aracısını gizli bilgileri (omsagent.yaml) yok
- Linux DaemonSet yaml dosyası için Log Analytics aracısını gizli anahtarları (omsagentsecret.yaml) yaml dosyası oluşturmak için gizli dizi oluşturma betikleri ile gizli bilgilere (omsagent-ds-secrets.yaml) kullanır.

İle veya olmadan gizli dizileri omsagent DaemonSets oluşturmayı seçebilirsiniz.

**Gizli dizileri içermeyen varsayılan OMSagent DaemonSet yaml dosyası**

- İçin varsayılan Log Analytics aracısını DaemonSet yaml dosyası, değiştirin `<WSID>` ve `<KEY>` WSID ve anahtarı. Dosyayı, ana düğüme kopyalayın ve aşağıdaki komutu çalıştırın:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Gizli varsayılan OMSagent DaemonSet yaml dosyası**

1. Log Analytics'i kullanmak için aracı gizli bilgileri ile DaemonSet oluşturun gizli dizileri önce.
    1. Betik ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduklarından emin olun.
        - Gizli dizi betiği - gizli gen.sh oluşturuluyor
        - Gizli şablon - gizli template.yaml
    2. Aşağıdaki örnekte olduğu gibi betiği çalıştırın. Log Analytics çalışma alanı kimliği ve birincil anahtar için betik sorar ve bunları girdikten sonra yapmanız çalıştırabilirsiniz betik gizli yaml dosyası oluşturur.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Gizli dizileri pod, aşağıdaki komutu çalıştırarak oluşturun:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Doğrulamak için aşağıdaki komutu çalıştırın:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Çıktıya benzemelidir:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Çıktıya benzemelidir:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Arka plan programı kümesi çalıştırarak, omsagent oluşturma ```sudo kubectl create -f omsagent-ds-secrets.yaml```

2. Log Analytics aracısını DaemonSet, aşağıdaki gibi çalıştığını doğrulayın:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Kubernetes için gizli dizileri yaml dosyası için Linux için Log Analytics aracısını çalışma alanı kimliği ve birincil anahtar oluşturmak için bir betik kullanın. Aşağıdaki örnek bilgileri ile [omsagent yaml dosyası](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) gizli bilgilerinizi güvenliğini sağlamak için.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

#### <a name="configure-a-log-analytics-windows-agent-for-kubernetes"></a>Kubernetes için bir Log Analytics Windows Aracısı yapılandırın
Windows Kubernetes için Log Analytics aracısını yüklemek çalışma alanı kimliği ve birincil anahtar için gizli dizileri yaml dosyası oluşturmak için bir betik kullanın. Konumunda [Log Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes/windows) sayfasında, gizli bilgilerinizi ile kullanabileceğiniz dosyalar da mevcuttur.  Ana ve aracı düğümleri için ayrı ayrı Log Analytics aracısını yüklemeniz gerekir.  

1. Log Analytics aracısını DaemonSet kullanmak için asıl gizli bilgileri kullanarak düğümünü açın ve gizli dizileri ilk oluşturun.
    1. Betik ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduklarından emin olun.
        - Gizli dizi betiği - gizli gen.sh oluşturuluyor
        - Gizli şablon - gizli template.yaml

    2. Aşağıdaki örnekte olduğu gibi betiği çalıştırın. Log Analytics çalışma alanı kimliği ve birincil anahtar için betik sorar ve bunları girdikten sonra yapmanız çalıştırabilirsiniz betik gizli yaml dosyası oluşturur.   

        ```
        #> sudo bash ./secret-gen.sh
        ```
    3. Arka plan programı kümesi çalıştırarak, omsagent oluşturma ```kubectl create -f omsagentsecret.yaml```
    4. Denetlemek için şu komutu çalıştırın:

        ```
        root@ubuntu16-13db:~# kubectl get secrets
        ```

        Çıktıya benzemelidir:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Arka plan programı kümesi çalıştırarak, omsagent oluşturma ```kubectl create -f ws-omsagent-de-secrets.yaml```

2. Log Analytics aracısını DaemonSet, aşağıdaki gibi çalıştığını doğrulayın:

    ```
    root@ubuntu16-13db:~# kubectl get deployment omsagent
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   1         1         <none>          1h
    ```

3. Windows çalıştıran, çalışan düğümü üzerinde aracıyı yüklemek için bu bölümdeki adımları [yükleme ve Windows kapsayıcı konakları yapılandırma](#install-and-configure-windows-container-hosts).

#### <a name="use-helm-to-deploy-log-analytics-agent-on-linux-kubernetes"></a>Helm Linux Kubernetes Log Analytics aracısını dağıtmak için kullanın
Linux Kubernetes ortamınızı Log Analytics aracısını dağıtmak için Helm kullanmak için aşağıdaki adımları gerçekleştirin.

1. Arka plan programı kümesi çalıştırarak, omsagent oluşturma ```helm install --name omsagent --set omsagent.secret.wsid=<WSID>,omsagent.secret.key=<KEY> stable/msoms```
2. Sonuçlar aşağıdakine benzer görünecektir:

    ```
    NAME:   omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     3s

    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         3s
    ```
3. Çalıştırarak omsagent durumunu denetleyebilirsiniz: ```helm status "omsagent"``` ve çıktı şuna benzer olacaktır:

    ```
    keiko@k8s-master-3814F33-0:~$ helm status omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED
 
    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     17m
 
    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         17m
    ```
   Daha fazla bilgi için lütfen [kapsayıcı çözümü Helm grafiği](https://aka.ms/omscontainerhelm).

### <a name="install-and-configure-windows-container-hosts"></a>Yükleme ve Windows kapsayıcı konakları yapılandırma

Bilgileri bölümünde yüklemek ve Windows kapsayıcı konakları yapılandırmak için kullanın.

#### <a name="preparation-before-installing-windows-agents"></a>Windows aracıları yüklemeden önce hazırlama

Windows çalıştıran bilgisayarlarda aracıları yüklemeden önce Docker hizmetinin yapılandırmanız gerekir. Windows Aracısı'nı veya Azure İzleyici sanal makine uzantısı aracıları Docker Daemon programını uzaktan erişebilmesi için Docker TCP yuva kullanma ve izleme verilerini yakalama yapılandırmasını sağlar.

##### <a name="to-configure-the-docker-service"></a>Docker hizmeti yapılandırmak için  

TCP kanal ve adlandırılmış kanal Windows Server'de etkinleştirmek için aşağıdaki PowerShell komutlarını gerçekleştirin:

```
Stop-Service docker
dockerd --unregister-service
dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
Start-Service docker
```

Windows kapsayıcıları ile kullanılan Docker daemon yapılandırmasını hakkında daha fazla bilgi için bkz: [Windows üzerinden Docker altyapısının](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


#### <a name="install-windows-agents"></a>Windows aracıları yükleyin

Windows ve Hyper-V kapsayıcı izlemeyi etkinleştirmek için kapsayıcı konaklarının Windows bilgisayarlarda Microsoft Monitoring Agent (MMA) yükleyin. Şirket içi ortamınızda Windows çalıştıran bilgisayarlar için bkz: [Azure İzleyici bağlanmak Windows bilgisayarlara](../../azure-monitor/platform/agent-windows.md). Sanal makineler için Azure'da çalışan bunları Azure İzleyici için kullanılacak bağlantı [sanal makine uzantısı](../../azure-monitor/learn/quick-collect-azurevm.md).

Windows kapsayıcıları Service Fabric üzerinde çalışmasını izleyebilirsiniz. Ancak, yalnızca [Azure'da çalışan sanal makineler](../../azure-monitor/learn/quick-collect-azurevm.md) ve [şirket içi ortamınızda Windows çalıştıran bilgisayarlar](../../azure-monitor/platform/agent-windows.md) şu anda Service Fabric için desteklenir.

Kapsayıcı izleme çözümü için Windows düzgün şekilde ayarlandığını doğrulayabilirsiniz. Yönetim Paketi indirme doğru olup olmadığını denetlemek için Aranan *ContainerManagement.xxx*. Dosyaları C:\Program Files\Microsoft Monitoring Agent\Agent\Health hizmet State\Management paketleri klasöründe olmalıdır.


## <a name="solution-components"></a>Çözüm bileşenleri

Azure portalından gidin *çözüm Galerisi* ve ekleme **kapsayıcı izleme çözümü**. Windows aracıları kullanıyorsanız, bu çözümü eklediğinizde, ardından aşağıdaki Yönetim Paketi her bilgisayarda bir aracı yüklenir. Yapılandırma veya bakım için Yönetim Paketi gereklidir.

- *ContainerManagement.xxx* C:\Program Files\Microsoft Monitoring Agent\Agent\Health hizmet State\Management paketlerinin yüklü

## <a name="container-data-collection-details"></a>Kapsayıcı veri koleksiyonu ayrıntıları
Kapsayıcı izleme çözümü, kapsayıcı konağında ve kapsayıcıları etkinleştirdiğiniz aracıları kullanarak çeşitli performans ölçümleri ve günlük verilerini toplar.

Verileri üç dakikada bir şu aracı türleri tarafından toplanır.

- [Linux için log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md)
- [Windows Aracısı](../../azure-monitor/platform/agent-windows.md)
- [Log Analytics VM uzantısı](../../azure-monitor/learn/quick-collect-azurevm.md)


### <a name="container-records"></a>Kapsayıcı kayıt

Aşağıdaki tabloda, kapsayıcı izleme çözümü ve günlük araması sonuçlarında görüntülenen veri türleri tarafından toplanan kayıtları örneklerini gösterir.

| Veri türü | Günlük araması'nda veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, oluşturulma zamanı, StartedTime, FinishedTime, Analytics'teki, Containerıd, ImageID |
| Kapsayıcı görüntüsü envanterinin | `ContainerImageInventory` | TimeGenerated, bilgisayar, görüntü, ImageTag, ImageSize, VirtualSize, çalışıyor, durduruldu, durduruldu, başarısız, Analytics'teki, ImageID TotalContainer |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource LogEntry, Analytics'teki, Containerıd kapsayıcı adı |
| Kapsayıcı hizmeti günlüğü | `ContainerServiceLog`  | TimeGenerated, bilgisayar, TimeOfCommand, görüntü, komut, Analytics'teki, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory_CL`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Analytics'teki|
| Kubernetes envanteri | `KubePodInventory_CL` | TimeGenerated, bilgisayar, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, Analytics'teki |
| Kapsayıcı işlemi | `ContainerProcess_CL` | TimeGenerated, bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, Analytics'teki |
| Kubernetes olayları | `KubeEvents_CL` | TimeGenerated, bilgisayar, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, Analytics'teki, ileti |

Eklenen etiketler için *PodLabel* veri türleri: kendi özel etiketlerinizi. Tabloda belirtilen eklenmiş PodLabel etiketleri verilebilir. Bu nedenle, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` ortamınızın veri kümesinde farklı ve genel benzer `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Kapsayıcıları izleme
Azure portalında etkin çözüm sonra **kapsayıcıları** kutucuk kapsayıcı konaklarınız ve ana çalışan kapsayıcılar hakkında özet bilgileri gösterir.


![Kapsayıcıları kutucuğu](./media/containers/containers-title.png)

Çalışıyor veya durduruldu, ortam ve mi başarısız, sahip olduğunuz kaç kapsayıcının genel bir bakış kutucuğu gösterir.

### <a name="using-the-containers-dashboard"></a>Kapsayıcıları panosunu kullanma
Tıklayın **kapsayıcıları** Döşe. Buradan görünümler tarafından düzenlenen görürsünüz:

- **Kapsayıcı olayları** -kapsayıcı durumu ve başarısız olan kapsayıcılar ile bilgisayarları gösterir.
- **Kapsayıcı günlüklerini** -kapsayıcı günlük dosyalarının zaman ve bilgisayarların listesini üzerinden günlük dosyası en yüksek sayısı ile oluşturulan bir grafiği gösterir.
- **Kubernetes olayları** -Kubernetes olayları zaman ve neden pod'ların oluşturulan olaylar nedeniyle bir listesi oluşturulan bir grafiğini gösterir. *Bu veri kümesi, yalnızca Linux ortamlarda kullanılır.*
- **Kubernetes Namespace Envanter** - ad alanları ve pod'ların sayısını gösterir ve hiyerarşilerini gösterir. *Bu veri kümesi, yalnızca Linux ortamlarda kullanılır.*
- **Kapsayıcı düğümü envanteri** -kapsayıcı düğümleri/konaklarda kullanılan düzenleme türleri sayısını gösterir. Düğümleri/ana bilgisayar kapsayıcıları sayısına göre de listelenir. *Bu veri kümesi, yalnızca Linux ortamlarda kullanılır.*
- **Kapsayıcı görüntüleri envanteri** -kapsayıcı görüntüleri kullanılan toplam sayısı ve resim türleri sayısını gösterir. Görüntü sayısını da resim etiketi listelenir.
- **Kapsayıcı durumu** -çalışan kapsayıcılar olan düğümler/ana bilgisayarları kapsayıcı toplam sayısını gösterir. Bilgisayar, ana çalışan sayısı tarafından da listelenir.
- **Kapsayıcı işlemi** -kapsayıcı işlemlerin zaman içinde çalışan bir çizgi grafik gösterir. Kapsayıcılar, çalıştırarak da listelenen komutu/işlem kapsayıcıları içinde. *Bu veri kümesi, yalnızca Linux ortamlarda kullanılır.*
- **Kapsayıcı CPU performansı** -düğümleri/barındıran bilgisayar için zaman içindeki ortalama CPU kullanımını içeren bir çizgi grafik gösterir. Ayrıca listeleri düğümleri/barındıran bilgisayar, ortalama CPU kullanımı temel.
- **Kapsayıcı belleği performansı** -bellek kullanımını içeren bir çizgi grafik, zaman içinde gösterir. Ayrıca bilgisayar belleği kullanımı örneği adına göre listelenir.
- **Bilgisayar performansı** -gösteren çizgi grafikler, zamana göre CPU performansı, yüzde zaman ve megabayt boş disk alanına göre bellek kullanımı yüzdesi zaman içinde. Daha fazla ayrıntı görüntülemek için grafikteki herhangi bir satır üzerine gelerek.


Her Pano görsel bir temsilini topladığınız verileri çalıştırma bir arama alanıdır.

![Kapsayıcılar Panosu](./media/containers/containers-dash01.png)

![Kapsayıcılar Panosu](./media/containers/containers-dash02.png)

İçinde **kapsayıcı durumu** alanında, üst kısımdaki alana, aşağıda gösterildiği gibi tıklayın.

![Kapsayıcı durumu](./media/containers/containers-status.png)

Log Analytics açılır ve kapsayıcılarınızı durumuyla ilgili bilgileri görüntüler.

![Kapsayıcılar için log Analytics](./media/containers/containers-log-search.png)

Buradan, ilgilendiğiniz belirli bilgileri bulmak için değiştirmeniz arama sorgusu düzenleyebilirsiniz. Günlük sorguları hakkında daha fazla bilgi için bkz: [sorgular Azure İzleyici'de oturum](../log-query/log-query-overview.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Başarısız bir kapsayıcı bularak sorunlarını giderme

Log Analytics, bir kapsayıcı olarak işaretler **başarısız** ise sıfır olmayan çıkış kodu ile çıkıldı. Hataları ve hataları ortamda bulunan genel bakış görebilirsiniz **başarısız kapsayıcıları** alan.

### <a name="to-find-failed-containers"></a>Başarısız olan kapsayıcılar bulmak için
1. Tıklayın **kapsayıcı durumu** alan.  
   ![Kapsayıcı durumu](./media/containers/containers-status.png)
2. Log Analytics açılır ve kapsayıcılarınızı, aşağıdakine benzer durumunu görüntüler.  
   ![kapsayıcı durumu](./media/containers/containers-log-search.png)
3. Başarısız satırı'nı genişletin ve kendi ölçütleriyle sorguya eklemek için +. Ardından sorgu Summarıze satırı açıklama.
   ![başarısız olan kapsayıcılar](./media/containers/containers-state-failed-select.png)  
1. Sorguyu çalıştırmak ve sonuçları görüntüsü kimliği görüntülemek için bir satır genişletin  
   ![başarısız olan kapsayıcılar](./media/containers/containers-state-failed.png)  
1. Aşağıdaki günlük sorguyu yazın. `ContainerImageInventory | where ImageID == <ImageID>` görüntünün görüntü boyutu ve durduruldu ve başarısız görüntüleri sayısı gibi ilgili ayrıntıları görmek için.  
   ![başarısız olan kapsayıcılar](./media/containers/containers-failed04.png)

## <a name="query-logs-for-container-data"></a>Kapsayıcı verileri için sorgu günlükleri
Belirli bir hata gidermeye çalışıyorsanız, ortamınızda nerede oluştuğunu görmek için yardımcı olabilir. Aşağıdaki günlük türlerini istediğiniz bilgileri döndürmek için sorgular oluşturmanıza yardımcı olur.


- **ContainerImageInventory** – görüntüsü tarafından düzenlenen bilgileri bulmak ve görüntü kimliği veya boyutları gibi görüntü bilgileri görüntülemek için çalışırken bu türü kullanın.
- **ContainerInventory** – kapsayıcının konumunu, adlarını nelerdir ve ne hakkında bilgi almak istediğinizde, bu türü kullanın. Bunlar çalıştırdığınızdan görüntüler.
- **ContainerLog** – belirli bir hata günlüğü bilgilerini ve girişleri bulmak istediğinizde, bu türü kullanın.
- **ContainerNodeInventory_CL** burada kapsayıcı konağında bulunan konak/düğüm hakkında bilgi almak istiyorsanız, bu türü kullanın. Bu, Docker sürümü, orchestration türünü, depolama ve ağ bilgileri sağlar.
- **ContainerProcess_CL** kapsayıcı içinde çalışan işlemi hızlı bir şekilde görmek için bu türü kullanın.
- **ContainerServiceLog** – Docker Daemon programını Başlat, Durdur, silmek veya çekme komutları için denetim izi bilgilerini bulmaya çalışırken bu türü kullanın.
- **KubeEvents_CL** Kubernetes olayları görmek için bu türü kullanın.
- **KubePodInventory_CL** küme hiyerarşisi bilgileri anlamak istediğinizde bu türü kullanın.


### <a name="to-query-logs-for-container-data"></a>Kapsayıcı verileri için sorgu günlükleri
* Bildiğiniz bir görüntü son başarısız oldu ve Hata günlüklerini bulmak için seçin. Başlangıç o yansıma ile çalıştırılan bir kapsayıcı adı bularak bir **ContainerInventory** arama. Örneğin, arama `ContainerInventory | where Image == "ubuntu" and ContainerState == "Failed"`  
    ![Ubuntu kapsayıcıları arayın](./media/containers/search-ubuntu.png)

  Bu kapsayıcı ayrıntılarını görüntülemek için sonuçları herhangi bir satırın genişletin.


## <a name="example-log-queries"></a>Örnek günlük sorguları
Genellikle, bir örnek veya iki ile başlayan ve onları ortamınıza uygun değiştirme sorguları oluşturmak kullanışlıdır. Bir başlangıç noktası olarak deneme yapabileceğiniz **örnek sorgular** alan daha gelişmiş sorgular oluşturmanıza yardımcı olacak.

![Kapsayıcıları sorguları](./media/containers/containers-queries.png)


## <a name="saving-log-queries"></a>Günlük sorgularını kaydetme
Sorguları kaydetme, Azure İzleyici'de standart bir özelliktir. Bunları kaydederek bu yararlı buldunuz gerekir gelecekte kullanım için kullanışlı.

Yararlı bulabileceğiniz bir sorguyu oluşturduktan sonra Kaydet'e tıklayarak **Sık Kullanılanlar** günlük araması sayfanın üstünde. Bu işlemi daha sonra kolayca erişebilir **Panom'u** sayfası.

## <a name="next-steps"></a>Sonraki adımlar
* [Sorgu günlükleri](../log-query/log-query-overview.md) ayrıntılı kapsayıcı veri kayıtları görüntülemek için.
