---
title: Azure Kubernetes küme - Operations Management izleme
description: Günlük analizi kullanarak Azure kapsayıcı hizmeti kümesinde Kubernetes izleme
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 3b014ce4c91d1dc9fae744ef4b528c98f9f787b3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="monitor-an-azure-container-service-cluster-with-log-analytics"></a>Azure kapsayıcı hizmeti kümesi günlük analizi ile izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, sahibi olduğunuzu varsayar [Azure kapsayıcı hizmeti kullanarak Kubernetes küme oluşturulan](container-service-kubernetes-walkthrough.md).

Ayrıca, sahibi olduğunuzu varsayar `az` Azure CLI ve `kubectl` araçları yüklü.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```console
$ az --version
```

Sahip değilseniz `az` yüklü, aracı yönergeler vardır [burada](https://github.com/azure/azure-cli#installation).
Alternatif olarak, kullanabileceğiniz [Azure bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview), sahip olduğu `az` Azure CLI ve `kubectl` Araçları zaten yüklenmiş.

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```console
$ kubectl version
```

Sahip değilseniz `kubectl` yüklü çalıştırabilirsiniz:
```console
$ az acs kubernetes install-cli
```

Kubectl aracınızı çalıştırabilirsiniz yüklü kubernetes anahtarları varsa test etmek için:
```console
$ kubectl get nodes
```

Yukarıdaki komut hataları çıkışı, kubernetes küme anahtarları kubectl aracına yüklemeniz gerekiyorsa. Bunu aşağıdaki komutla yapabilirsiniz:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Günlük analizi ile kapsayıcıları izleme

Günlük analizi, yönetmek ve şirket içi korumak ve altyapı bulut yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür. Kapsayıcı, tek bir konumda kapsayıcı envanter, performans ve günlükleri görüntülemenize yardımcı olan günlük analizi çözümde çözümüdür. Denetim, merkezi konumda günlükler görüntüleyerek kapsayıcıları sorun giderme ve bir ana bilgisayar üzerindeki fazladan kapsayıcı tüketen gürültülü bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözüm hakkında daha fazla bilgi için lütfen başvurmak [kapsayıcı çözüm günlük analizi](../../log-analytics/log-analytics-containers.md).

## <a name="installing-log-analytics-on-kubernetes"></a>Günlük analizi üzerinde Kubernetes yükleme

### <a name="obtain-your-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarı edinin
Günlük analizi için hizmete iletişim kurabilecek şekilde Aracısı bir çalışma alanı kimliği ve çalışma alanı anahtarı ile yapılandırılması gerekir. Çalışma alanı kimliği ve anahtarı hesabı oluşturmanız gerekir almak için <https://mms.microsoft.com>.
Lütfen bir hesap oluşturmak için aşağıdaki adımları izleyin. İşiniz bittiğinde tıklayarak kimliği ve anahtarı elde etmeniz hesabı oluşturma, **ayarları**, sonra **bağlı kaynakları**ve ardından **Linux sunucuları**, aşağıda gösterildiği gibi.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>Bir DaemonSet kullanarak günlük analizi aracı yükleme
DaemonSets Kubernetes tarafından bir kapsayıcı tek bir örneği kümedeki her ana bilgisayarda çalıştırmak için kullanılır.
Bunlar izleme aracıları çalıştırmak için mükemmel.

Burada [DaemonSet YAML dosya](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Adlı bir dosyaya Kaydet `oms-daemonset.yaml` ve yer tutucu değerlerini değiştirme `WSID` ve `KEY` çalışma alanı kimliği ve anahtarı dosyasında.

Çalışma alanı kimliği ve anahtarı DaemonSet yapılandırma ekledikten sonra kümenizdeki ile günlük analizi aracı yükleyebilirsiniz `kubectl` komut satırı aracı:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Günlük analizi aracısını Kubernetes gizli anahtarı kullanarak yükleme
Günlük analizi çalışma alanı Kimliğinizi ve anahtarınızı korumak için Kubernetes gizli DaemonSet YAML dosyasının bir parçası olarak kullanabilirsiniz.

 - Komut dosyası, gizli şablon dosyasını ve DaemonSet YAML dosyasını kopyalayın (gelen [depo](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) ve aynı dizinde olduklarından emin olun.
      - Komut dosyası - gizli gen.sh üretiliyor gizli
      - Gizli şablon - gizli template.yaml
   - DaemonSet YAML dosyası - omsagent ds secrets.yaml
 - Betiği çalıştırın. Komut dosyası günlük analizi çalışma alanı kimliği ve birincil anahtar için sorar. Ekle ve onu çalıştırabilmeniz için komut dosyasını bir gizli yaml dosyası oluşturulur.
   ```
   #> sudo bash ./secret-gen.sh
   ```

   - Gizli pod aşağıdaki çalıştırarak oluşturun: ``` kubectl create -f omsagentsecret.yaml ```

   - Denetlemek için şu komutu çalıştırın:

   ```
   root@ubuntu16-13db:~# kubectl get secrets
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

  - Arka plan programı kümesi çalıştırarak, omsagent oluşturma ``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Sonuç
İşte bu kadar! Birkaç dakika sonra günlük analizi panonuz akan verileri görüyor olmalısınız.
