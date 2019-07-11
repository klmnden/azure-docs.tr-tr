---
title: Kubernetes şirket içi kullanma
titleSuffix: Azure Cognitive Services
description: Konuşmayı metne ve metin okuma kapsayıcı görüntülerini tanımlamak için Kubernetes ve Helm kullanarak bir Kubernetes paket oluşturacağız. Bu paket dağıtılacak için bir Kubernetes kümesi şirket içi.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/10/2019
ms.author: dapine
ms.openlocfilehash: 33d9de956a6d43145fc68f4ec46b09b8e8bf0188
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786248"
---
# <a name="use-kubernetes-on-premises"></a>Kubernetes şirket içi kullanma

Konuşmayı metne ve metin okuma kapsayıcı görüntülerini tanımlamak için Kubernetes ve Helm kullanarak bir Kubernetes paket oluşturacağız. Bu paket dağıtılacak için bir Kubernetes kümesi şirket içi. Son olarak, biz dağıtılan hizmetler ve yapılandırma seçeneklerini test etme hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Konuşma kapsayıcıları şirket içi kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
| Azure hesabı | Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][free-azure-account] oluşturun. |
| Kapsayıcı kayıt defteri erişimi | Kubernetes kümesine docker görüntüleri çekmek sırada, kapsayıcı kayıt defterine erişim gerekir. Şunları yapmanız [kapsayıcı kayıt defterine erişim isteği][speech-preview-access] ilk. |
| Kubernetes CLI | [Kubernetes CLI][kubernetes-cli] kapsayıcı kayıt defterinden Paylaşılan kimlik bilgilerini yönetmek için gereklidir. Kubernetes Kubernetes Paket Yöneticisi Helm'in önce de gereklidir. |
| Helm CLI | Bir parçası olarak [Helm CLI][helm-install] install, you'll also need to initialize Helm which will install [Tiller][tiller-install]. |
|Konuşma kaynak |Bu kapsayıcıların kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A _konuşma_ fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Her iki değeri de Azure portalında üzerinde kullanılabilir **konuşma** genel bakış ve anahtarları sayfaları ve bu, kapsayıcı başlatma için gerekli.<br><br>**{API_KEY}** : kaynak anahtarı<br><br>**{ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>Önerilen ana bilgisayar yapılandırması

Başvurmak [konuşma hizmeti kapsayıcı ana bilgisayar][speech-container-host-computer] başvuru olarak ayrıntıları. Bu *helm grafiği* otomatik olarak CPU ve bellek gereksinimlerini şirket kodunu çözer kaç (eş zamanlı istek sayısı) bağlı hesaplar, kullanıcı belirtir. Ayrıca, bu iyileştirmeler ses/metin girişi olarak yapılandırılmış olup olmadığı göre ayarlar `enabled`. Helm grafiği varsayılan olarak, iki eş zamanlı istekleri ve devre dışı bırakma iyileştirme.

| Hizmet | CPU / kapsayıcı | Bellek / kapsayıcı |
|--|--|--|
| **Konuşma metin** | bir kod çözücü 1,150 millicores en az gerektirir. Varsa `optimizedForAudioFile` etkindir, sonra da 1,950 millicores gereklidir. (varsayılan: iki kod çözücüleri) | Gerekli: 2 GB<br>Sınırlı:  4 GB |
| **Metin okuma** | bir eş zamanlı istek en az 500 millicores gerektirir. Varsa `optimizeForTurboMode` 1.000 millicores gerekli etkinse. (varsayılan: iki eş zamanlı istek) | Gerekli: 1 GB<br> Sınırlı: 2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>Kubernetes kümesine bağlanma

Ana bilgisayar, kullanılabilir bir Kubernetes kümesi olması beklenir. Bu öğreticide bakın [bir Kubernetes kümesini dağıtırken](../../aks/tutorial-kubernetes-deploy-cluster.md) bir ana bilgisayara bir Kubernetes kümesi dağıtma hakkında kavramsal anlayış için.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Docker kimlik bilgileri ile Kubernetes Küme Paylaşımı

Kubernetes kümesine izin vermek için `docker pull` gelen yapılandırılmış yansımaları `containerpreview.azurecr.io` kapsayıcı kayıt defteri, docker kimlik bilgileri kümeye aktarmak için ihtiyacınız. Yürütme [ `kubectl create` ][kubectl-create] oluşturmak için aşağıdaki komutu bir *docker-registry gizli* kapsayıcı kayıt defteri erişim önkoşul sağlanan kimlik bilgileri temel.

Komut satırı arabiriminizden seçtiğiniz, aşağıdaki komutu çalıştırın. Değiştirdiğinizden emin olun `<username>`, `<password>`, ve `<email-address>` kapsayıcı kayıt defteri kimlik bilgilerine sahip.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Zaten erişimi varsa `containerpreview.azurecr.io` kapsayıcı kayıt defteri, bunun yerine, Genel Bayrak kullanarak Kubernetes gizli oluşturabilir. Docker yapılandırma JSON karşı yürütülen aşağıdakileri göz önünde bulundurun.
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

Gizli dizi başarıyla oluşturulduğunda aşağıdaki çıktıyı konsola yazdırılır.

```console
secret "containerpreview" created
```

Gizli dizi oluşturulduğunu doğrulamak için yürütme [ `kubectl get` ][kubectl-get] ile `secrets` bayrağı.

```console
kuberctl get secrets
```

Yürütme `kubectl get secrets` yapılandırılmış tüm gizli dizilerin yazdırır.

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Dağıtım için Helm grafiği değerleri yapılandırın

Ziyaret [Microsoft Helm Hub][ms-helm-hub] Microsoft tarafından sunulan genel kullanıma açık helm grafikleri için. Microsoft Helm Hub'ından, bulabilirsiniz **Bilişsel hizmetler konuşma şirket içi grafik**. **Bilişsel hizmetler konuşma şirket içi** biz yüklersiniz grafiktir ancak biz öncelikle oluşturmanız gerekir bir `config-values.yaml` açık yapılandırmaları olan bir dosya. Microsoft depo bizim Helm örneğine ekleyerek başlayalım.

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

Ardından, bizim Helm grafiği değerleri yapılandıracağız. Aşağıdaki YAML adlı bir dosyaya kopyalayıp `config-values.yaml`. Özelleştirme hakkında daha fazla bilgi için **Bilişsel hizmetler konuşma şirket içi Helm grafiği**, bkz: [helm grafikleri](#customize-helm-charts). Değiştirin `billing` ve `apikey` değerleri kendi değerlerinizle.

```yaml
# These settings are deployment specific and users can provide customizations

# speech-to-text configurations
speechToText:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForAudioFile: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-speech-to-text
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-text-to-speech
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >
```

> [!IMPORTANT]
> Varsa `billing` ve `apikey` değerleri sağlanmadığından, hizmetleri 15 dakika sonra sona erer. Benzer şekilde, hizmetleri kullanılabilir olmayacak şekilde doğrulama başarısız olur.

### <a name="the-kubernetes-package-helm-chart"></a>Kubernetes paket (Helm grafiği)

*Helm grafiği* isteneceğini hangi docker resimleri yapılandırmasını içeren `containerpreview.azurecr.io` kapsayıcı kayıt defteri.

> A [Helm grafiği][helm-charts] ilgili Kubernetes kaynaklarının kümesini tanımlayan dosyaları koleksiyonudur. Memcached pod veya karmaşık bir şey gibi gibi bir tam web uygulaması yığınıyla HTTP sunucuları, veritabanları, önbellekler ve benzeri basit bir şey dağıtmak için tek bir grafik kullanılabilir.

Sağlanan *Helm grafikleri* konuşma hizmeti, hem okuma hem de konuşma metin hizmetlerinden docker görüntülerini çekme `containerpreview.azurecr.io` kapsayıcı kayıt defteri.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Kubernetes kümesinde Helm grafiği yükle

Yüklemek için *helm grafiği* yürütülecek gerekir [ `helm install` ][helm-install-cmd] komutuyla değiştirerek `<config-values.yaml>` uygun yolu ve dosya adı bağımsız değişkeni. `microsoft/cognitive-services-speech-onpremise` Helm grafiği aşağıdaki başvurulan edinilebilir [burada Microsoft Helm Hub][ms-helm-hub-speech-chart].

```console
helm install microsoft/cognitive-services-speech-onpremise \
    --version 0.1.0 \
    --values <config-values.yaml> \
    --name onprem-speech
```

Yükleme başarılı yürütülmesinden görmeyi beklediğiniz bir örnek çıktı aşağıdaki gibidir:

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
speech-to-text-7664f5f465-87w2d  0/1    Pending            0         0s
speech-to-text-7664f5f465-klbr8  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
speech-to-text  LoadBalancer  10.0.252.106  <pending>    80:31811/TCP  1s
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
speech-to-text-poddisruptionbudget  N/A            20%              0                    1s
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
speech-to-text  0/2    2           0          0s
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
speech-to-text-autoscaler  Deployment/speech-to-text  <unknown>/50%  2        10       0         0s
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

Kubernetes dağıtımı, tamamlanması birkaç dakikadan fazla sürebilir. Pod'ların hem Hizmetleri düzgün dağıtılmalı ve kullanılabilir olduğunu doğrulamak için aşağıdaki komutu yürütün:

```console
kubectl get all
```

Aşağıdaki çıktıya benzer bir şey görmeyi beklemelisiniz:

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/speech-to-text-7664f5f465-87w2d   1/1       Running   0          34m
pod/speech-to-text-7664f5f465-klbr8   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/speech-to-text   LoadBalancer   10.0.252.106   52.162.123.151   80:31811/TCP   34m
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/speech-to-text   2         2         2            2           34m
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/speech-to-text-7664f5f465   2         2         2         34m
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/speech-to-text-autoscaler   Deployment/speech-to-text   1%/50%    2         10        2          34m
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>Helm testleriyle Helm dağıtımı doğrulama

Yüklü Helm grafikleri tanımlamak *Helm testleri*, doğrulama için bir kolaylık olarak hizmet. Bu testler, hizmet hazırlık doğrulayın. Her ikisi de doğrulamak için **konuşma metin** ve **metin okuma** Hizmetleri, biz yürütme [Helm test][helm-test] komutu.

```console
helm test onprem-speech
```

> [!IMPORTANT]
> POD durum değilse, bu testler başarısız olur `Running` veya dağıtım altında listede yoksa `AVAILABLE` sütun. Bunu tamamlamak için üzerinde on dakika sürebilir, sabırlı olun.

Bu testler, çeşitli durum sonuçları çıkarır:

```console
RUNNING: speech-to-text-readiness-test
PASSED: speech-to-text-readiness-test
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

Yürütme alternatif olarak *helm testleri*, toplamanız mümkündü *dış IP* adresleri ve karşılık gelen bağlantı noktalarından `kubectl get all` komutu. IP ve bağlantı noktasını kullanarak bir web tarayıcısı açın ve gidin `http://<external-ip>:<port>:/swagger/index.html` API swagger sayfaları görüntülemek.

## <a name="customize-helm-charts"></a>Helm grafikleri özelleştirme

Helm grafikleri hiyerarşiktir. İçin devralma grafiği, hiyerarşik durdurulmasını sağlar, ayrıca burada devralınan kurallar daha belirli olduğundan, ayarları geçersiz kılar, ayrıntıda kavramını oluşturabilmesine olanak sağlar.

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Speech-to-Text Helm Chart Config](includes/speech-to-text-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme hakkında daha fazla ayrıntı için [sayfayı ziyaret edin][installing-helm-apps-in-aks].

> [!div class="nextstepaction"]
> [Bilişsel hizmetler, kapsayıcılar][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/helm/#helm-install
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/developing_charts
[speech-preview-access]: https://aka.ms/speechcontainerspreview
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
