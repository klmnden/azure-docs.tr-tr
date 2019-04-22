---
title: (KULLANIM DIŞI) Azure Kubernetes kümesi - Management işlemleri izleme
description: Log Analytics kullanarak Azure Container Service Kubernetes kümesini izleme
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: d7370fc14a5ede23744e04ac9d35140f2368e21f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58877406"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-log-analytics"></a>(KULLANIM DIŞI) Log Analytics ile bir Azure Container Service kümesini izleme

> [!TIP]
> Bu makale, güncelleştirilmiş sürümü kullanan Azure Kubernetes Service için bkz: [kapsayıcılar için Azure İzleyici](../../azure-monitor/insights/container-insights-overview.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu izlenecek yol, sahibi olduğunuzu varsayar [Azure Container Service kullanan bir Kubernetes kümesi oluşturuldu](container-service-kubernetes-walkthrough.md).

Ayrıca, sahibi olduğunuzu varsayar `az` Azure CLI ve `kubectl` araçlarının yüklü.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```console
$ az --version
```

Öğeniz yoksa `az` yüklü aracı yönergeler sunulmaktadır [burada](https://github.com/azure/azure-cli#installation).
Alternatif olarak, [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), sahip olduğu `az` Azure CLI ve `kubectl` Araçları zaten yüklenmiş.

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```console
$ kubectl version
```

Öğeniz yoksa `kubectl` yüklü çalıştırabilirsiniz:
```console
$ az acs kubernetes install-cli
```

Kubectl aracınızın çalıştırabileceğiniz yüklü kubernetes anahtarları varsa test etmek için:
```console
$ kubectl get nodes
```

Yukarıdaki komut hataları, kubernetes küme anahtarları kubectl aracınızın yüklemeniz gerekiyorsa. Aşağıdaki komutu kullanarak bunu yapabilirsiniz:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Log Analytics ile kapsayıcıları izleme

Log Analytics, yönetmek ve şirket içi koruma hem de bulut altyapısında yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür. Kapsayıcı çözümü, tek bir konumda kapsayıcı envanteri, performans ve günlükleri görüntülemenize yardımcı olur. Log analytics'te bir çözümdür. Denetim, kapsayıcıları merkezi konumda günlüklerini görüntüleyerek sorun giderme ve gürültülü fazla kapsayıcı bir konakta tüketen bulun.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözümü hakkında daha fazla bilgi için bkz [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md).

## <a name="installing-log-analytics-on-kubernetes"></a>Log Analytics, Kubernetes üzerinde yükleme

### <a name="obtain-your-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma
İçin Log Analytics aracısını hizmetle iletişim kurabilecek şekilde bir çalışma alanı kimliği ve bir çalışma alanı anahtarı ile yapılandırılması gerekir. Çalışma alanı kimliği ve anahtarını adresinde bir hesap oluşturmanız gereken alma <https://mms.microsoft.com>.
Lütfen bir hesap oluşturmak için adımları izleyin. Olduktan sonra hesabı oluşturmadan bitti Kimliğinizi alabilir ve anahtar tıklayarak **Log Analytics** dikey, ardından çalışma alanının adı. Ardından, altında **Gelişmiş ayarlar**, **bağlı kaynaklar**, ardından **Linux sunucuları**, aşağıda gösterildiği gibi ihtiyacınız olan bilgileri bulabilirsiniz.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>Bir DaemonSet kullanarak Log Analytics aracısını yükleme
DaemonSets tarafından Kubernetes kümesindeki her bir konakta bir kapsayıcıyı tek bir örneğini çalıştırmak için kullanılır.
Bunlar, izleme aracılarını çalıştırmak için ideal.

İşte [DaemonSet YAML dosyası](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Adlı bir dosyaya kaydedip `oms-daemonset.yaml` için yer tutucu değerlerini `WSID` ve `KEY` çalışma alanı Kimliğiniz ve anahtarınızla dosyasında.

Çalışma alanı kimliği ve anahtarı DaemonSet yapılandırmaya ekledikten sonra Log Analytics aracısını kümenizle yükleyebilirsiniz `kubectl` komut satırı aracı:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Kubernetes gizli kullanarak Log Analytics aracısını yükleme
Log Analytics çalışma alanı kimliği ve anahtarını korumak için Kubernetes gizli DaemonSet YAML dosyası bir parçası olarak kullanabilirsiniz.

- Betik, gizli şablon dosyası ve DaemonSet YAML dosyası kopyalayın (gelen [depo](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) ve aynı dizinde olduklarından emin olun.
  - Gizli dizi betiği - gizli gen.sh oluşturuluyor
  - Gizli şablon - gizli template.yaml
    - DaemonSet YAML dosyası - omsagent ds secrets.yaml
- Betiği çalıştırın. Betik Log Analytics çalışma alanı kimliği ve birincil anahtar için sorar. Ekleyin ve onu çalıştırabilmeniz için betik gizli yaml dosyası oluşturur.
  ```
  #> sudo bash ./secret-gen.sh
  ```

  - Gizli dizileri pod, aşağıdaki komutu çalıştırarak oluşturun: ```kubectl create -f omsagentsecret.yaml```

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

  - Arka plan programı kümesi çalıştırarak, omsagent oluşturma ```kubectl create -f omsagent-ds-secrets.yaml```

### <a name="conclusion"></a>Sonuç
İşte bu kadar! Birkaç dakika sonra Log Analytics panonuza akan verileri görebildiğine olmalıdır.
