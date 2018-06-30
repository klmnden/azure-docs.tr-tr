---
title: Kapsayıcı izleme çözümüne Azure günlük analizi | Microsoft Docs
description: Günlük analizi kapsayıcı izleme çözümünde Docker ve Windows görüntüleyin ve yönetin yardımcı olan tek bir konumda kapsayıcı konakları.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 584e7a211cde83d7785c7fa0962c004af2b76968
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128920"
---
# <a name="container-monitoring-solution-in-log-analytics"></a>Kapsayıcı izleme çözümüne günlük analizi

![Kapsayıcıları simgesi](./media/log-analytics-containers/containers-symbol.png)

Bu makalede ayarlamak ve kapsayıcı izleme çözümü görüntülemenize ve Docker ve Windows yönetmenize yardımcı olan günlük analizi içinde kullanmak nasıl kapsayıcı konakları tek bir konumda. Docker kendi BT altyapısı için yazılım dağıtımını otomatik hale kapsayıcıları oluşturmak için kullanılan bir yazılım sanallaştırma sistemidir.

Bir çözüm gösterilmektedir hangi kapsayıcılar olan çalışan, hangi kapsayıcı görüntü çalıştırdıkları ve kapsayıcıları çalıştırdığı. Kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgileri görüntüleyebilirsiniz. Ve görüntüleme ve Docker veya Windows ana bilgisayar uzaktan görüntüleme gerek kalmadan merkezi günlükleri arama yaparak kapsayıcıları giderebilirsiniz. Bir ana bilgisayar üzerindeki gürültülü ve alan üst limiti aşan kaynakları olabilir kapsayıcıları bulabilirsiniz. Ve merkezi CPU, bellek, depolama ve ağ kullanımı ve performans bilgilerini kapsayıcıları için görüntüleyebilirsiniz. Windows çalıştıran bilgisayarlarda, merkezileştirme ve Windows Server günlüklerinden karşılaştırmak Hyper-V ve Docker kapsayıcıları. Aşağıdaki kapsayıcı orchestrators çözümünü destekler:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift

İçinde ilgileniyorsanız, iş yüklerinin performansını izleme Kubernetes ortamlara üzerinde barındırılan AKS (Azure kapsayıcı hizmeti) dağıtılan bkz [İzleyici Azure kapsayıcı hizmeti](../monitoring/monitoring-container-health.md).  Kapsayıcı izleme çözümü, platform izlemek için destek içermez.  

Aşağıdaki diyagramda, çeşitli kapsayıcı konakları ve günlük analizi ile aracılar arasındaki ilişkileri gösterir.

![Kapsayıcıları diyagramı](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Sistem gereksinimleri ve desteklenen platformlar

Başlamadan önce önkoşulları karşılaması doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Kapsayıcı izleme çözümü desteklemek için Docker Orchestrator ve işletim sistemi platformu
Aşağıdaki tabloda, işletim sistemi desteği kapsayıcı envanter, performans ve günlükleri günlük analizi ile izleme ve Docker orchestration özetlenmektedir.   

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

- Docker 1.11 için 1.13
- Docker CE ve EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 kapsayıcı konakları olarak desteklenen Linux dağıtımları

- Ubuntu 14.04 LTS ve 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2 ve 7.3
- SLES 12
- RHEL 7.2 ve 7.3
- Red Hat OpenShift kapsayıcı Platform (OCP) 3.4 ve 3.5
- ACS Mesosphere DC/OS için 1.7.3 1.8.8
- ACS Kubernetes 1.4.5 için 1.6
    - Kubernetes olayları, Kubernetes stok ve kapsayıcı işlemleri yalnızca sürüm 1.4.1-45 ve daha sonra OMS Aracısı'nın Linux için desteklenir
- ACS Docker Swarm

### <a name="supported-windows-operating-system"></a>Desteklenen Windows işletim sistemi

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional veya Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Windows'da desteklenen docker sürümleri

- Docker 1.12 ve 1.13
- Docker 17.03.0 ve sonraki sürümler

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

1. Günlük analizi çalışma alanından kapsayıcı izlemesi çözümü eklemek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).

2. Yükleyin ve Docker bir OMS Aracısı ile kullanın. İşletim sistemi ve Docker orchestrator temel alınarak, aracınızı yapılandırmak için aşağıdaki yöntemleri kullanabilirsiniz.
  - Tek başına ana bilgisayarlar için:
    - Desteklenen Linux işletim sistemlerinde yüklemek ve Docker çalıştırın ve ardından yükleyin ve yapılandırın [Linux için OMS aracısının](log-analytics-agent-linux.md).  
    - CoreOS üzerinde Linux için OMS Aracısı çalıştırılamıyor. Bunun yerine, Linux için OMS aracısının kapsayıcılı bir sürümünü çalıştırın. Gözden geçirme [CoreOS dahil olmak üzere Linux kapsayıcı konakları](#for-all-linux-container-hosts-including-coreos) veya [CoreOS dahil olmak üzere Azure kamu Linux kapsayıcı konakları](#for-all-azure-government-linux-container-hosts-including-coreos) Azure Bulutu kapsayıcılarında ile çalışıyorsanız.
    - Windows Server 2016 ve Windows 10, Docker altyapısına ve istemcisi yükleme ardından bilgileri toplamak ve günlük Analizi'ne göndermek için bir aracı bağlayın. Gözden geçirme [yükleyin ve Windows kapsayıcı konakları yapılandırma](#install-and-configure-windows-container-hosts) Windows ortamınız varsa.
  - Docker çok ana bilgisayar orchestration için:
    - Red Hat OpenShift ortamınız varsa, gözden [bir OMS Aracısı Red Hat OpenShift için yapılandırma](#configure-an-oms-agent-for-red-hat-openshift).
    - Azure kapsayıcı hizmeti kullanarak Kubernetes küme varsa:
       - Gözden geçirme [Kubernetes için OMS Linux Aracısı'nı yapılandırma](#configure-an-oms-linux-agent-for-kubernetes).
       - Gözden geçirme [Kubernetes için bir OMS Windows aracı yapılandırma](#configure-an-oms-windows-agent-for-kubernetes).
       - Gözden geçirme [OMS Aracısı'nı Linux Kubernetes dağıtmak için kullanım Helm](#use-helm-to-deploy-oms-agent-on-linux-kubernetes).
    - Bir Azure kapsayıcı hizmeti DC/OS kümeniz varsa, altında daha fazla bilgi [Operations Management Suite ile Azure kapsayıcı hizmeti DC/OS kümesi izlemek](../container-service/dcos-swarm/container-service-monitoring-oms.md).
    - Bir Docker Swarm modu ortamınız varsa, altında daha fazla bilgi [Docker Swarm için bir OMS Aracısı Yapılandırma](#configure-an-oms-agent-for-docker-swarm).
    - Service Fabric kümesi varsa, altında daha fazla bilgi [izlemek OMS günlük analizi ile kapsayıcıları](../service-fabric/service-fabric-diagnostics-oms-containers.md).

Gözden geçirme [Docker altyapısına Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) yükleme ve Windows çalıştıran bilgisayarlarda, Docker altyapılarını yapılandırma hakkında ek bilgi için makalenin.

> [!IMPORTANT]
> Docker çalıştırmalıdır **önce** yüklediğiniz [Linux için OMS aracısının](log-analytics-agent-linux.md) kapsayıcı konaklarda. Docker yüklemeden önce aracıyı zaten yüklediyseniz, Linux için OMS Aracısı'nı yeniden yüklemeniz gerekir. Docker hakkında daha fazla bilgi için bkz: [Docker Web sitesi](https://www.docker.com).


### <a name="install-and-configure-linux-container-hosts"></a>Yükleme ve Linux kapsayıcı konakları yapılandırma

Docker yükledikten sonra aracı kullanmak için Docker ile yapılandırmak için kapsayıcı ana bilgisayarınız için aşağıdaki ayarları kullanın. Öncelikle günlük analizi çalışma alanı kimliği ve Azure portalında bulabilirsiniz anahtarı gerekir. Çalışma alanınızda tıklatın **Hızlı Başlangıç** > **bilgisayarlar** görüntülemek için **çalışma alanı kimliği** ve **birincil anahtar**.  Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

**Tüm Linux kapsayıcı ana bilgisayarlar için CoreOS dışında:**

- Daha fazla bilgi ve Linux için OMS Aracısı'nı yüklemek adımları için bkz: [Linux bilgisayarlarınızı günlük Analizi'ne bağlamak](log-analytics-concept-hybrid.md).

**CoreOS dahil olmak üzere tüm Linux kapsayıcı ana bilgisayarlar için:**

İzlemek istediğiniz kapsayıcı başlatın. Değiştirin ve aşağıdaki örneği kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**CoreOS dahil olmak üzere tüm Azure kamu Linux kapsayıcı ana bilgisayarlar için:**

İzlemek istediğiniz kapsayıcı başlatın. Değiştirin ve aşağıdaki örneği kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Bir kapsayıcıda yüklü bir Linux aracı kullanarak değiştirme**

Daha önce yüklenmiş doğrudan Aracısı kullanılan ve bunun yerine bir kapsayıcıda çalışan bir aracının kullanmak istiyorsanız, önce Linux için OMS Aracısı'nı kaldırmanız gerekir. Bkz: [Linux için OMS Aracısı'nı kaldırma](log-analytics-agent-linux.md) başarıyla aracıyı kaldırmak nasıl anlamak için.  

#### <a name="configure-an-oms-agent-for-docker-swarm"></a>Docker Swarm için OMS Aracısı'nı yapılandırma

Docker Swarm, OMS Aracısı genel hizmet olarak çalıştırabilirsiniz. Bir OMS Aracısı hizmeti oluşturmak için aşağıdaki bilgileri kullanın. Günlük analizi çalışma alanı kimliği ve birincil anahtarı sağlamanız gerekir.

- Aşağıdaki ana düğümde çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Docker Swarm için güvenli parolaları

Çalışma alanı kimliği ve birincil anahtar için gizli anahtar oluşturulduktan sonra Docker Swarm için gizli bilgilerinizi oluşturmak için aşağıdaki bilgileri kullanın.

1. Aşağıdaki ana düğümde çalıştırın.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Gizli düzgün bir şekilde oluşturulduğundan emin olun.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Gizli kapsayıcılı OMS Aracısı bağlamak için aşağıdaki komutu çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Red Hat OpenShift bir OMS Aracısı'nı yapılandırma
Kapsayıcı izleme verilerini toplamaya başlamak için Red Hat OpenShift için OMS aracısının eklemenin üç yolu vardır.

* [Linux için OMS Aracısı'nı yüklemek](log-analytics-agent-linux.md) doğrudan her bir düğümde OpenShift  
* [Log Analytics VM uzantısı etkinleştirmek](log-analytics-azure-vm-extension.md) Azure'da bulunan her bir OpenShift düğümüne  
* Bir OpenShift arka plan programı kümesi OMS Aracısı'nı yüklemek  

Bu bölümde bir OpenShift arka plan programı kümesi OMS Aracısı'nı yüklemek için gerekli olan adımları kapsar.  

1. OpenShift ana düğüme oturum açma ve yaml dosyasını kopyalayın [ocp omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) ana düğüme github'dan ve değerini, günlük analizi çalışma alanı Kimliğinizi ve birincil anahtarınız ile değiştirin.
2. Günlük analizi için proje oluşturma ve kullanıcı hesabı ayarlamak için aşağıdaki komutları çalıştırın.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Arka plan programı kümesi dağıtmak için şu komutu çalıştırın:

    `oc create -f ocp-omsagent.yaml`

5. Doğrulamak için yapılandırılmış ve çalışan doğru şunu yazın:

    `oc describe daemonset omsagent`  

    ve çıktı aşağıdakine benzemelidir:

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

Günlük analizi çalışma alanı kimliği ve birincil anahtar OMS Aracısı arka plan programı kümesi yaml dosyası kullanıldığında güvenli hale getirmek için gizli anahtarları kullanmak istiyorsanız, aşağıdaki adımları gerçekleştirin.

1. OpenShift ana düğüme oturum açma ve yaml dosyasını kopyalayın [ocp ds omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) ve komut dosyası oluşturuluyor gizli [ocp secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) github'dan.  Bu komut dosyası için günlük analizi çalışma alanı kimliği ve birincil anahtar güvenliğini sağlamak gizli yaml dosyası oluşturur, bilgi secrete.  
2. Günlük analizi için proje oluşturma ve kullanıcı hesabı ayarlamak için aşağıdaki komutları çalıştırın. Komut dosyası oluşturuluyor gizliliği için günlük analizi çalışma alanı kimliği ister <WSID> ve birincil anahtar <KEY> ve isteğe bağlı olarak tamamlandıktan sonra ocp secret.yaml dosyası oluşturur.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Gizli dosya, aşağıdaki komutu çalıştırarak dağıtın:

    `oc create -f ocp-secret.yaml`

5. Dağıtım, aşağıdaki komutu çalıştırarak doğrulayın:

    `oc describe secret omsagent-secret`  

    ve çıktı aşağıdakine benzemelidir:  

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

6. OMS Aracısı arka plan programı kümesi yaml dosyasını aşağıdaki çalıştırarak dağıtın:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Dağıtım, aşağıdaki komutu çalıştırarak doğrulayın:

    `oc describe ds oms`

    ve çıktı aşağıdakine benzemelidir:

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

#### <a name="configure-an-oms-linux-agent-for-kubernetes"></a>OMS Linux aracı Kubernetes için yapılandırma

Kubernetes için Linux için OMS Aracısı'nı yüklemek çalışma alanı kimliği ve birincil anahtar için gizli yaml dosyasını oluşturmak için bir komut dosyası kullanın. Konumundaki [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) sayfasında, gizli bilgileri olan veya olmayan kullanabileceğiniz dosyaları vardır.

- Linux DaemonSet için varsayılan OMS Aracısı gizli bilgileri (omsagent.yaml) yok
- Linux DaemonSet yaml dosyası için OMS aracısının gizli yaml (omsagentsecret.yaml) dosyası oluşturmak için gizli oluşturma komut dosyaları ile gizli bilgileri (omsagent-ds-secrets.yaml) kullanır.

Omsagent DaemonSets ile veya olmadan gizli oluşturmayı seçebilirsiniz.

**Varsayılan OMSagent DaemonSet yaml dosya gizlilikler olmadan**

- Varsayılan OMS Aracısı DaemonSet yaml dosyasını değiştirmeniz `<WSID>` ve `<KEY>` WSID ve anahtarı. Ana düğümünüz dosyasını kopyalayın ve şu komutu çalıştırın:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Varsayılan OMSagent DaemonSet yaml dosya gizli**

1. OMS Aracısı gizli bilgileri kullanarak DaemonSet kullanmak için parolaları oluşturun.
    1. Komut dosyası ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduklarından emin olun.
        - Komut dosyası - gizli gen.sh üretiliyor gizli
        - Gizli şablon - gizli template.yaml
    2. Aşağıdaki örnekteki gibi komut dosyasını çalıştırın. Günlük analizi çalışma alanı kimliği ve birincil anahtar için komut dosyası ister ve bunları girdikten sonra onu çalıştırabilmeniz için komut dosyası gizli yaml dosyasını oluşturur.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Gizli pod aşağıdaki çalıştırarak oluşturun:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Doğrulamak için şu komutu çalıştırın:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Çıktı aşağıdakine benzemelidir:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Çıktı aşağıdakine benzemelidir:

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

    5. Arka plan programı kümesi çalıştırarak, omsagent oluşturma ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. OMS Aracısı DaemonSet, aşağıdakine benzer çalıştığından emin olun:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Kubernetes için bir komut dosyası gizli yaml dosya çalışma alanı kimliği ve birincil anahtar için OMS aracısının Linux için oluşturmak için kullanın. Aşağıdaki örnek bilgileriyle birlikte kullandığınız [omsagent yaml dosya](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) gizli bilgilerinizin güvenliğini sağlamak için.

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

#### <a name="configure-an-oms-windows-agent-for-kubernetes"></a>OMS Windows aracı Kubernetes için yapılandırma
Windows Kubernetes için OMS Aracısı'nı yüklemek çalışma alanı kimliği ve birincil anahtar için gizli yaml dosyasını oluşturmak için bir komut dosyası kullanın. Konumundaki [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes/windows) sayfasında, gizli bilgilerinizi kullanabilirsiniz dosyası vardır.  Ana ve Aracısı düğümleri için ayrı ayrı OMS aracısı yüklemeniz gerekir.  

1. OMS Aracısı yöneticisinde gizli bilgileri kullanarak DaemonSet kullanılacak düğümünde oturum açın ve gizli anahtarları önce oluşturun.
    1. Komut dosyası ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduklarından emin olun.
        - Komut dosyası - gizli gen.sh üretiliyor gizli
        - Gizli şablon - gizli template.yaml

    2. Aşağıdaki örnekteki gibi komut dosyasını çalıştırın. OMS çalışma alanı kimliği ve birincil anahtar için komut dosyası ister ve bunları girdikten sonra onu çalıştırabilmeniz için komut dosyası gizli yaml dosyasını oluşturur.   

        ```
        #> sudo bash ./secret-gen.sh
        ```
    3. Arka plan programı kümesi çalıştırarak, omsagent oluşturma ``` kubectl create -f omsagentsecret.yaml ```
    4. Denetlemek için şu komutu çalıştırın:

        ```
        root@ubuntu16-13db:~# kubectl get secrets
        ```

        Çıktı aşağıdakine benzemelidir:

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

2. OMS Aracısı DaemonSet, aşağıdakine benzer çalıştığından emin olun:

    ```
    root@ubuntu16-13db:~# kubectl get deployment omsagent
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   1         1         <none>          1h
    ```

3. Windows çalıştıran, alt düğüm üzerinde aracıyı yüklemek için bu bölümdeki adımları [Windows kapsayıcı konakları yükleyip](#install-and-configure-windows-container-hosts).

#### <a name="use-helm-to-deploy-oms-agent-on-linux-kubernetes"></a>Helm OMS Aracısı'nı Linux Kubernetes dağıtmak için kullanın
OMS Aracısı Linux Kubernetes ortamınıza dağıtmak için Helm kullanmak için aşağıdaki adımları gerçekleştirin.

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
3. Çalıştırarak omsagent durumunu denetleyebilirsiniz: ```helm status "omsagent"``` ve çıktı aşağıdakine benzer görünecektir:

    ```
    keiko@k8s-master-3814F33-0:~$ helm status omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED
 
    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     17m
 
    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         17m
    ```
Daha fazla bilgi için lütfen ziyaret [kapsayıcı çözüm Helm grafik](https://aka.ms/omscontainerhelm).

### <a name="install-and-configure-windows-container-hosts"></a>Yükleme ve Windows kapsayıcı konakları yapılandırma

Bilgileri bölümünde yüklemek ve Windows kapsayıcı konakları yapılandırmak için kullanın.

#### <a name="preparation-before-installing-windows-agents"></a>Windows aracıları yüklemeden önce hazırlama

Windows çalıştıran bilgisayarlarda aracılar yüklemeden önce Docker hizmeti yapılandırmanız gerekir. Windows aracısı veya günlük analizi sanal makine uzantısı aracıları Docker arka plan programı uzaktan erişebilmesi için Docker TCP yuva kullanın ve izleme verilerini yakalamak için yapılandırma sağlar.

##### <a name="to-start-docker-and-verify-its-configuration"></a>Docker başlatmak ve yapılandırmasını doğrulamak için

Windows Server için adlandırılmış TCP ayarlamak için gereken adımlar şunlardır:

1. Windows PowerShell'de TCP kanal ve adlandırılmış kanal etkinleştirin.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. TCP kanalı yapılandırma dosyası ile Docker yapılandırmak ve adlandırılmış. Yapılandırma dosyası C:\ProgramData\docker\config\daemon.json bulunur.

    Daemon.json dosyasında, aşağıdakiler gerekir:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

İle Windows kapsayıcıları kullanılan Docker arka plan programı yapılandırma hakkında daha fazla bilgi için bkz: [Docker altyapısına Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


#### <a name="install-windows-agents"></a>Windows aracılarını yükleme

Windows ve Hyper-V kapsayıcı izlemeyi etkinleştirmek için kapsayıcı konaklar Windows bilgisayarlarda Microsoft İzleme Aracısı'nı (MMA) yükleyin. Şirket içi ortamınızda Windows çalıştıran bilgisayarlar için bkz: [günlük analizi bağlanmak Windows bilgisayarlara](log-analytics-windows-agent.md). Sanal makineler için Azure'da çalışan bunları günlük analizi için kullanılacak bağlantı [sanal makine uzantısı](log-analytics-azure-vm-extension.md).

Service Fabric üzerinde çalışan Windows kapsayıcıları izleyebilirsiniz. Ancak, yalnızca [Azure'da çalışan sanal makineler](log-analytics-azure-vm-extension.md) ve [şirket içi ortamınızda Windows çalıştıran bilgisayarlar](log-analytics-windows-agent.md) için Service Fabric şu anda desteklenmiyor.

Kapsayıcı izleme çözümü Windows için doğru ayarlandığını doğrulayabilirsiniz. Yönetim Paketi indirme doğru olduğunu denetlemek için Ara *ContainerManagement.xxx*. Dosyaları C:\Program Files\Microsoft Monitoring Agent\Agent\Health hizmet State\Management paketleri klasörde olmalıdır.


## <a name="solution-components"></a>Çözüm bileşenleri

OMS Portalı'ndan gidin *Çözümleri Galerisi* ve ekleme **kapsayıcı izlemesi çözümü**. Windows aracılarının kullanıyorsanız, bu çözüm eklediğinizde, ardından aşağıdaki Yönetim Paketi her bilgisayara bir aracı ile yüklenir. Hiçbir yapılandırma ve bakım için Yönetim Paketi gereklidir.

- *ContainerManagement.xxx* C:\Program Files\Microsoft Monitoring Agent\Agent\Health hizmet State\Management paketlerinde yüklü

## <a name="container-data-collection-details"></a>Kapsayıcı veri toplama ayrıntıları
Kapsayıcı izleme çözümü, kapsayıcı konakları ve etkinleştirmeniz aracıları kullanarak kapsayıcıları çeşitli performans ölçümleri ve günlük verilerini toplar.

Verileri üç dakikada aşağıdaki Aracısı türleri tarafından toplanır.

- [Linux için OMS Aracısı](log-analytics-linux-agents.md)
- [Windows Aracısı](log-analytics-windows-agent.md)
- [Log Analytics VM uzantısı](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Kapsayıcı kayıtlarını

Aşağıdaki tablo kapsayıcı izleme çözümü ve günlük arama sonuçlarında görünecek veri türleri tarafından toplanan kayıtları örnekleri gösterir.

| Veri türü | Günlük arama veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcıları için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, CounterValue, TimeGenerated, sayaç yolu, SourceSystem |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komutu, CreatedTime, StartedTime, FinishedTime, SourceSystem, Containerıd, ImageID kapsayıcı adı |
| Kapsayıcı görüntü envanteri | `ContainerImageInventory` | TimeGenerated, bilgisayar, görüntü, ImageTag, ImageSize, VirtualSize, duraklatıldı, çalışıyor, durduruldu, başarısız, SourceSystem, ImageID, TotalContainer |
| Kapsayıcı günlük | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource, LogEntry, SourceSystem, Containerıd kapsayıcı adı |
| Kapsayıcı hizmeti oturum açma | `ContainerServiceLog`  | TimeGenerated, bilgisayar, TimeOfCommand, görüntü, komutu, SourceSystem, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory_CL`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes envanteri | `KubePodInventory_CL` | TimeGenerated, bilgisayar, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Kapsayıcı işlemi | `ContainerProcess_CL` | TimeGenerated, bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes olayları | `KubeEvents_CL` | TimeGenerated, bilgisayar, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, ileti |

Etiketleri eklenmiş için *PodLabel* veri türleridir kendi özel etiketler. Tabloda gösterilen eklenmiş PodLabel etiketleri verilebilir. Bu nedenle, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` ortamınızı ait veri kümesinde farklı ve genel benzer `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Kapsayıcıları izleme
Günlük analizi portalında etkin çözüm sonra **kapsayıcıları** döşeme kapsayıcı konaklarınızın ve ana bilgisayarlarda çalışan kapsayıcıları hakkında özet bilgileri gösterir.


![Kapsayıcıları döşeme](./media/log-analytics-containers/containers-title.png)

Çalışıyor veya durdurulmuştur, ortam ve olup bunların başarısız, sahip olduğunuz kaç kapsayıcıları genel bir bakış kutucuğu gösterir.

### <a name="using-the-containers-dashboard"></a>Kapsayıcıları panosunu kullanma
Tıklatın **kapsayıcıları** döşeme. Buradan düzenlenmiş görünümler görürsünüz:

- **Kapsayıcı olayları** -kapsayıcı durumu ve başarısız kapsayıcıları sahip bilgisayarları gösterir.
- **Kapsayıcı günlükleri** -kapsayıcı günlük dosyalarının zaman ve bilgisayarların bir listesini günlük dosyalarının en yüksek sayısı ile oluşturulan bir grafik gösterir.
- **Kubernetes olayları** -zaman ve neden pod'ları oluşturulan olaylar nedeniyle listesini oluşturulan Kubernetes olayları bir grafiğini gösterir. *Bu veri kümesi yalnızca Linux ortamlarda kullanılır.*
- **Kubernetes Namespace stok** - ad alanları ve pod'ları sayısını gösterir ve bunların hiyerarşi gösterir. *Bu veri kümesi yalnızca Linux ortamlarda kullanılır.*
- **Kapsayıcı düğümü stok** -kapsayıcı düğümlerini/ana bilgisayarda kullanılan orchestration türleri sayısını gösterir. Düğümleri/barındıran bilgisayar da kapsayıcıları sayısına göre listelenir. *Bu veri kümesi yalnızca Linux ortamlarda kullanılır.*
- **Kapsayıcı görüntüleri stok** -resim türleri sayısını ve kullanılan kapsayıcı görüntü toplam sayısını gösterir. Görüntü sayısını da resim etiketi listelenir.
- **Kapsayıcıları durum** -çalışan kapsayıcılar olan düğümler/ana bilgisayarlar kapsayıcısı toplam sayısını gösterir. Bilgisayarları da ana çalışan sayısı tarafından listelenir.
- **Kapsayıcı işlem** -kapsayıcı işlemlerin zaman içinde çalışan bir çizgi grafiği gösterir. Kapsayıcılar, çalıştırarak da listelenen komutu/işlemi kapsayıcıları içinde. *Bu veri kümesi yalnızca Linux ortamlarda kullanılır.*
- **Kapsayıcı CPU performans** -ortalama CPU kullanımı düğümleri/barındıran bilgisayar için zaman içinde bir çizgi grafiği gösterir. Ayrıca listeleri düğümleri/barındıran bilgisayar ortalama CPU kullanımı temel.
- **Kapsayıcı bellek performansı** -zaman içinde bir çizgi grafiği bellek kullanımını gösterir. Ayrıca bilgisayar bellek kullanımı örneği adına göre listeler.
- **Bilgisayar performansı** -gösterir çizgi grafiklerde Yüzde CPU performansı, zamana kadar zaman ve megabayt boş disk alanı üzerinden bellek kullanımı yüzdesi Zamanla. Daha fazla ayrıntı görüntülemek için bir grafik herhangi bir satırda üzerine getirin.


Her Pano görsel bir toplanan veriler üzerinde çalıştırılan bir arama alanıdır.

![Kapsayıcıları Panosu](./media/log-analytics-containers/containers-dash01.png)

![Kapsayıcıları Panosu](./media/log-analytics-containers/containers-dash02.png)

İçinde **kapsayıcı durumu** alanında, aşağıda gösterildiği gibi üst alanı tıklatın.

![Kapsayıcı durumu](./media/log-analytics-containers/containers-status.png)

Kapsayıcılarınızı durumu hakkındaki bilgileri görüntüleyen günlük arama açılır.

![Günlük arama kapsayıcıları için](./media/log-analytics-containers/containers-log-search.png)

Buradan, ilgilendiğiniz belirli bilgileri bulmak için değiştirmek için arama sorgusu düzenleyebilirsiniz. Günlük aramaları hakkında daha fazla bilgi için bkz: [günlük analizi aramaları oturum](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Başarısız bir kapsayıcı bulma ile ilgili sorunları giderme

Günlük analizi kapsayıcı olarak işaretler **başarısız** sıfır olmayan çıkış kodu ile çıkıldı durumunda. Hatalar ve hataları ortamında özetini görebilirsiniz **başarısız kapsayıcıları** alanı.

### <a name="to-find-failed-containers"></a>Bulunacak kapsayıcıları başarısız oldu
1. Tıklatın **kapsayıcı durumu** alanı.  
   ![kapsayıcı durumu](./media/log-analytics-containers/containers-status.png)
2. Günlük arama açar ve kapsayıcılarınızı, aşağıdakine benzer durumunu görüntüler.  
   ![kapsayıcı durumu](./media/log-analytics-containers/containers-log-search.png)
3. Ardından, ek bilgileri görüntülemek için başarısız kapsayıcıları toplanmış değeri'ı tıklatın. Genişletme **daha fazla Göster** resim kimliğini görüntülemek için  
   ![başarısız kapsayıcıları](./media/log-analytics-containers/containers-state-failed.png)  
4. Ardından, aşağıdaki arama sorgusu yazın. `ContainerInventory <ImageID>` görüntünün görüntü boyutu ve durduruldu ve başarısız resimlerinin sayısı gibi ayrıntılarını görmek için.  
   ![başarısız kapsayıcıları](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Kapsayıcı verileri için arama günlüklerini
Belirli bir hata gidermeye çalışıyorsanız, ortamınızda nerede oluştuğunu görmek için yardımcı olabilir. Aşağıdaki Günlük türleri istediğiniz bilgileri döndürmek için sorgular oluşturmanıza yardımcı olur.


- **ContainerImageInventory** – görüntüsü tarafından düzenlenmiş bilgileri bulmak ve resim kimlikleri ve boyutları gibi görüntü bilgileri görüntülemek için çalışırken bu türü kullanın.
- **ContainerInventory** – kapsayıcı konumunu, adlarını nedir ve ne hakkında bilgi almak istediğinizde bu türü kullanın çalıştırma kaynağınız görüntüler.
- **ContainerLog** – belirli hata günlüğü bilgilerini ve girişleri bulmak istediğiniz durumlarda bu türü kullanın.
- **ContainerNodeInventory_CL** kapsayıcıları burada bulunan konak/düğüm hakkında bilgi almak istediğinizde bu türü kullanın. Bu, Docker sürüm, orchestration türünü, depolama ve ağ bilgilerini sağlar.
- **ContainerProcess_CL** kapsayıcı içinde çalışan işlemi hızlıca görmek için bu türü kullanın.
- **ContainerServiceLog** – Docker arka plan programı, Başlat, Durdur, silmek veya çekme komutları için denetim izi bilgilerini bulmaya çalışırken bu türü kullanın.
- **KubeEvents_CL** Kubernetes olayları görmek için bu türü kullanın.
- **KubePodInventory_CL** küme hiyerarşisi bilgileri anlamak istediğinizde bu türü kullanın.


### <a name="to-search-logs-for-container-data"></a>Kapsayıcı veri günlüklerini aramak için
* Bildiğiniz bir görüntüyü yakın zamanda başarısız oldu ve Hata günlüklerini bulabilmelerini seçin. Başlat, görüntü ile çalışan bir kapsayıcı adı bularak bir **ContainerInventory** arama. Örneğin, arama `ContainerInventory | where Image == "ubuntu" and ContainerState == "Failed"`  
    ![Ubuntu kapsayıcıları için arama](./media/log-analytics-containers/search-ubuntu.png)

  İleri'kapsayıcı adı **adı**ve bu günlükleri arayın. Bu örnekte bu değer `ContainerLog | where Name == "cranky_stonebreaker"`’dur.

**Performans bilgilerini görüntüleme**

Sorgular oluşturmak başlangıç olduğunda ilk olası yenilikleri görmek için yardımcı olabilir. Örneğin, tüm performans verilerini görmek için geniş bir sorgu aşağıdaki arama sorgusu yazarak deneyin.

```
Perf
```

![kapsayıcıları performansı](./media/log-analytics-containers/containers-perf01.png)

Bunu sorgunuzu sağındaki adını yazarak belirli bir kapsayıcıya görüyorsunuz performans verilerini kapsamını belirleyebilirsiniz.

```
Perf <containerName>
```

Tek bir kapsayıcı için toplanan performans ölçümleri listesini gösterir.

![kapsayıcıları performansı](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayan ve bunları ortamınıza uygun değiştirme sorgular oluşturmak kullanışlıdır. Bir başlangıç noktası olarak deneme yapabileceğiniz **örnek sorgular** daha gelişmiş sorgular oluşturmanıza yardımcı olmak için alanı.

![Kapsayıcıları sorguları](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Arama sorguları günlük kaydetme
Sorguları kaydetme günlük analizi standart bir özelliktir. Bunları kaydederek yararlı buldunuz o gerekir gelecekte kullanım için kullanışlı.

Size kullanışlı bir sorgu oluşturduktan sonra tıklayarak Kaydet **Sık Kullanılanlar** sayfanın üst kısmındaki günlük arama. Bundan daha sonra kolayca erişebilirsiniz sonra **My Pano** sayfası.

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı kapsayıcı veri kayıtları görüntülemek için.
