---
title: Azure Container Registry'de Helm depolarını kullanır
description: Uygulamalarınız için grafikler depolamak için Azure Container Registry ile bir Helm deposu kullanmayı öğrenin
services: container-registry
author: iainfoulds
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: iainfou
ms.openlocfilehash: ba0e1386d67e920f1805d244f9042044bb462ec9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62109860"
---
# <a name="use-azure-container-registry-as-a-helm-repository-for-your-application-charts"></a>Azure Container Registry, uygulama grafikleri için bir Helm deposu olarak kullanma

Hızlı bir şekilde yönetmek ve Kubernetes için uygulamaları dağıtmak için kullanabileceğiniz [açık kaynaklı Helm Paket Yöneticisi][helm]. Helm ile uygulamaları olarak tanımlanan *grafikleri* bir Helm grafiği deposunda depolanır. Bu grafikler, yapılandırmaları ve bağımlılıkları tanımlayın ve uygulama yaşam döngüsü boyunca tutulan olabilir. Azure Container kayıt defteri ana bilgisayar olarak Helm grafiği depolar için kullanılabilir.

Azure Container Registry ile derleme işlem hatlarını veya diğer Azure Hizmetleri ile tümleştirebileceğiniz bir özel, güvenli Helm grafik deposuna, sahip. Azure Container Registry Helm grafik depolarında grafiklerinizi yakın dağıtımlar ve artıklığı korumak için coğrafi çoğaltma özelliklerini içerir. Yalnızca grafikleri tarafından kullanılan depolama alanı için ödeme yapar ve tüm Azure Container Registry fiyat katmanlarda kullanılabilir.

Bu makalede Azure Container Registry'de depolanan bir Helm grafiği deposu kullanma işlemini gösterir.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlamak için aşağıdaki ön koşullar karşılanmalıdır:

- **Azure Container Registry** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalında](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md).
- **Helm istemci sürümü 2.11.0 (RC bir sürüm değil) veya üzeri** - çalışma `helm version` geçerli sürümünüzü bulmak için. Ayrıca bir Kubernetes kümesi içinde başlatılan bir Helm sunucusu (Tiller) gerekir. Gerekirse, [Azure Kubernetes Service kümesi oluşturma][aks-quickstart]. Yükleme ve Helm yükseltme hakkında daha fazla bilgi için bkz. [yükleme Helm][helm-install].
- **Azure CLI Sürüm 2.0.46 veya üzeri** - çalışma `az --version` sürümü bulmak için. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="add-a-repository-to-helm-client"></a>Helm istemciye bir depo Ekle

Helm grafikleri depolayabilir bir HTTP sunucusu bir Helm depodur. Azure Container Registry, bu depolama sağlamak için Helm grafikleri ve grafik deposuna ekleyip kaldırırken dizin tanımını yönetin.

Azure Container Registry'nize bir Helm grafiği deposu olarak eklemek için Azure CLI'yı kullanın. Bu yaklaşımda, Helm istemcinizi URI ve Azure Container Registry tarafından desteklenen depo için kimlik bilgileri ile güncelleştirilir. Kimlik bilgilerini komut geçmişi, örneğin kullanıma sunulmaz şekilde bu depo bilgilerini el ile belirtmeniz gerekmez.

Gerekirse, Azure CLI için oturum açın ve yönergeleri izleyin:

```azurecli
az login
```

Azure CLI varsayılan ayarları kullanarak Azure Container Registry adı ile yapılandırma [az yapılandırma] [ az-configure] komutu. Aşağıdaki örnekte, değiştirin `<acrName>` kayıt defterinizin adıyla:

```azurecli
az configure --defaults acr=<acrName>
```

Şimdi Helm istemcinizi kullanarak Azure kapsayıcı kayıt defteri Helm grafiği deponuza ekleyin [az acr helm deposuna ekleyin] [ az-acr-helm-repo-add] komutu. Bu komut bir kimlik doğrulama belirteci Helm istemci tarafından kullanılan Azure kapsayıcı kayıt defterinizin alır. Kimlik doğrulama belirteci 1 saat boyunca geçerlidir. Benzer şekilde `docker login`, gelecekteki CLI oturumlarında Azure kapsayıcı kayıt defteri Helm grafiği deponuz ile bilgilerinizi Helm istemcinin kimliğini doğrulamak için bu komutu çalıştırabilirsiniz:

```azurecli
az acr helm repo add
```

## <a name="add-a-chart-to-the-repository"></a>Depoya bir grafik ekleyin

Bu makale için var olan bir Helm grafiği genel Helm geçelim *kararlı* depo. *Kararlı* ortak uygulama grafikler içeren seçkin, genel deponun bir depodur. Paket maintainers kendi grafiklere gönderebildiği *kararlı* depoda aynı şekilde Docker Hub ortak kapsayıcı görüntüleri için genel bir kayıt defteri sağlar. Grafik genel kullanıma indirilen *kararlı* depo sonra itildi özel Azure Container Registry deponuza. Çoğu senaryoda, oluşturmak ve geliştirdiğiniz uygulamalar için kendi grafik yüklemek. Kendi Helm grafikleri oluşturma hakkında daha fazla bilgi için bkz. [Helm grafikleri geliştirme][develop-helm-charts].

İlk olarak bir dizin oluşturun *~/acr-helm*, var olan'ı indirin *stable/wordpress* grafik:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm fetch stable/wordpress
```

İndirilen grafik listeleyin ve dosya adının içerdiği Wordpress sürümünü not alın. `helm fetch stable/wordpress` Komutu, belirli bir sürüm belirtin olmadı böylece *en son* sürüme getirildi. Tüm Helm grafikleri aşağıdaki dosya adında sürüm numarası dahil [SemVer 2] [ semver2] standart. Aşağıdaki örnek çıktıda, Wordpress grafik sürümüdür *2.1.10*:

```
$ ls

wordpress-2.1.10.tgz
```

Artık grafiğin Azure CLI kullanarak Azure Container Registry Helm grafiği deponuza gönderin [az acr helm anında iletme] [ az-acr-helm-push] komutu. Gibi önceki adımda yüklenen, Helm grafiği adını *wordpress 2.1.10.tgz*:

```azurecli
az acr helm push wordpress-2.1.10.tgz
```

Birkaç dakika sonra Azure CLI'yı grafiğinizi kaydedildiğini aşağıdaki örnek çıktıda gösterildiği gibi raporları:

```
$ az acr helm push wordpress-2.1.10.tgz

{
  "saved": true
}
```

## <a name="list-charts-in-the-repository"></a>Depo listesi grafikleri

Helm istemci uzak depolar içeriği yerel önbelleğe alınmış bir kopyasını tutar. Değişiklikleri uzak depoya Helm istemcisi tarafından yerel olarak bilinen kullanılabilir grafikler listesini otomatik olarak güncelleştirmez. Depolar arasında grafikler için arama yaparken, onun yerel önbelleğe alınan dizini Helm kullanır. Önceki adımda karşıya grafik kullanmak için yerel Helm deposu dizini güncelleştirilmesi gerekir. Helm istemci depolarında yeniden dizin oluşturma veya depo dizini güncelleştirmek için Azure CLI'yı kullanın. Her zaman bir grafik deponuza ekleyin, bu adımın tamamlanması gerekir:

```azurecli
az acr helm repo add
```

Deponuzu ve güncelleştirilmiş dizini kullanılabilir yerel olarak depolanan bir grafikle aramak veya yüklemek için normal Helm istemci komutları kullanabilirsiniz. Deponuzdaki tüm grafikleri görmek için `helm search <acrName>`. Kendi Azure Container Registry adı belirtin:

```console
helm search <acrName>
```

Aşağıdaki örnek çıktıda gösterildiği gibi önceki adımda gönderilen Wordpress grafik listelenir:

```
$ helm search myacrhelm

NAME                CHART VERSION   APP VERSION DESCRIPTION
helmdocs/wordpress  2.1.10          4.9.8       Web publishing platform for building blogs and websites.
```

Ayrıca Azure CLI ile grafikleri listeleyebilirsiniz kullanarak [az acr helm list komutu][az-acr-helm-list]:

```azurecli
az acr helm list
```

## <a name="show-information-for-a-helm-chart"></a>Helm grafiği için bilgi göster

Depoda belirli bir grafik bilgilerini görüntülemek için normal Helm istemci yeniden kullanabilirsiniz. Grafiğin adlandırılmış bilgileri görmek için *wordpress*, kullanın `helm inspect`.

```console
helm inspect <acrName>/wordpress
```

Hiçbir sürüm numarası sağlandığında *son* sürümü kullanılır. Helm, aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi grafik hakkında ayrıntılı bilgi döndürür:

```
$ helm inspect myacrhelm/wordpress

appVersion: 4.9.8
description: Web publishing platform for building blogs and websites.
engine: gotpl
home: https://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: bitnami-bot
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 2.1.10
[...]
```

Azure CLI ile bir grafik bilgileri de gösterebilirsiniz [az acr helm show] [ az-acr-helm-show] komutu. Yeniden *son* grafik sürümü, varsayılan olarak döndürülür. Ekleyebilirsiniz `--version` bir grafiğin belirli bir sürümü gibi listelemek için *2.1.10*:

```azurecli
az acr helm show wordpress
```

## <a name="install-a-helm-chart-from-the-repository"></a>Depodan bir Helm grafiği yükle

Helm grafiği deponuzda depo adını belirterek yüklenir ve ardından ad grafik. Wordpress grafik yüklemek için Helm istemci kullanın:

```console
helm install <acrName>/wordpress
```

> [!TIP]
> Azure kapsayıcı kayıt defteri Helm grafiği deponuza gönderin ve daha sonra yeni bir CLI oturumunda geri, yerel Helm istemciniz güncelleştirilmiş kimlik doğrulaması belirteci gerekir. Yeni bir kimlik doğrulama belirteci almak için kullanın [az acr helm deposuna ekleyin] [ az-acr-helm-repo-add] komutu.

Yükleme işlemi sırasında aşağıdaki adımları izleyin:

- Helm istemci, yerel depo dizini arar.
- İlişkili grafik, Azure Container Registry depodan indirilir.
- Grafik, Kubernetes kümenizdeki Tiller kullanılarak dağıtılır.

Aşağıdaki sıkıştırılmış örneğe çıktı Helm grafiği dağıtılan Kubernetes kaynakları gösterir:

```
$ helm install myacrhelm/wordpress

NAME:   irreverent-jaguar
LAST DEPLOYED: Thu Sep 13 21:44:20 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                          READY  STATUS   RESTARTS  AGE
irreverent-jaguar-wordpress-7ff46d9b8c-b7v6m  0/1    Pending  0         1s
irreverent-jaguar-mariadb-0                   0/1    Pending  0         1s
[...]
```

## <a name="delete-a-helm-chart-from-the-repository"></a>Depodan bir Helm Grafiği Sil

Depodan bir grafik silmek için kullanın [az acr helm silme] [ az-acr-helm-delete] komutu. Grafik adı gibi belirtin *wordpress*ve sürümü gibi silme *2.1.10*.

```azurecli
az acr helm delete wordpress --version 2.1.10
```

Grafiğin adlandırılmış tüm sürümleri silmek isterseniz, dışlamayı `--version` parametresi.

Grafik olarak döndürülecek devam `helm search <acrName>`. Yeniden Helm istemci bir depoda kullanılabilir grafikler listesini otomatik olarak güncelleştirmez. Helm istemci depo dizini güncelleştirmek için [az acr helm deposuna ekleyin] [ az-acr-helm-repo-add] komutunu tekrar:

```azurecli
az acr helm repo add
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan ortak var olan bir Helm grafiğinden *kararlı* depo. Oluşturma ve Helm grafiklerini dağıtma hakkında daha fazla bilgi için bkz. [geliştirme Helm grafikleri][develop-helm-charts].

Helm grafikleri, kapsayıcı oluşturma işleminin bir parçası kullanılabilir. Daha fazla bilgi için [Azure kapsayıcı kayıt defteri görevleri kullanın][acr-tasks].

Azure kapsayıcı kayıt defterini yönetin ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [en iyi uygulamalar][acr-bestpractices].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[develop-helm-charts]: https://docs.helm.sh/developing_charts/
[semver2]: https://semver.org/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az-configure
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-helm-repo-add]: /cli/azure/acr/helm/repo#az-acr-helm-repo-add
[az-acr-helm-push]: /cli/azure/acr/helm#az-acr-helm-push
[az-acr-helm-list]: /cli/azure/acr/helm#az-acr-helm-list
[az-acr-helm-show]: /cli/azure/acr/helm#az-acr-helm-show
[az-acr-helm-delete]: /cli/azure/acr/helm#az-acr-helm-delete
[acr-tasks]: container-registry-tasks-overview.md
