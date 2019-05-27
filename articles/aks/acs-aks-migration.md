---
title: Azure kapsayıcı hizmeti (ACS) ' Azure Kubernetes Service'i (AKS) geçirme
description: Azure kapsayıcı hizmeti (ACS) ' Azure Kubernetes Service'i (AKS) geçirin.
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: dcee8da943603fb0978caf9992be76347ca197d6
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65977710"
---
# <a name="migrate-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Azure kapsayıcı hizmeti (ACS) ' Azure Kubernetes Service'i (AKS) geçirme

Bu makalede Kubernetes ile Azure Container Service (ACS) ve Azure Kubernetes Service (AKS) arasında başarılı bir geçiş planlayıp yardımcı olur. Önemli kararlar yardımcı olmak için bu kılavuz, AKS ile ACS arasındaki farkları ayrıntıları ve geçiş işlemine genel bakış sağlar.

## <a name="differences-between-acs-and-aks"></a>AKS ile ACS arasındaki farklar

ACS ve AKS geçiş etkileyen bazı önemli alanda farklılık gösterir. Tüm geçiş işleminden önce gözden geçirin ve aşağıdaki farklar ele almak planlama:

* AKS düğümlerini kullanma [yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md).
    * Yönetilmeyen diskler için AKS düğümleri eklemeden önce dönüştürülmelidir.
    * Özel `StorageClass` Azure diskleri değiştirilmesi gerekmeden için nesneleri `unmanaged` için `managed`.
    * Tüm `PersistentVolumes` kullanması gereken `kind: Managed`.
* AKS destekler [birden çok düğüm havuzları](https://docs.microsoft.com/azure/aks/use-multiple-node-pools) (şu anda önizlemede).
* Windows Server tabanlı düğümler şu anda [AKS önizlemede](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* AKS, sınırlı bir kümesini destekler [bölgeleri](https://docs.microsoft.com/azure/aks/quotas-skus-regions).
* AKS, barındırılan bir Kubernetes denetim düzlemi ile yönetilen bir hizmettir. ACS şablonunuz yapılandırmasını daha önce değiştirdiyseniz, uygulamalarınızı değiştirmeniz gerekebilir.

## <a name="differences-between-kubernetes-versions"></a>Kubernetes sürümleri arasındaki farklar

(Örneğin, gelen 1.7.x 1.9.x için) daha yeni bir Kubernetes sürümüne geçiş yapıyorsanız Kubernetes API'sine yapılan birkaç değişiklik anlamak için aşağıdaki kaynakları gözden geçirin:

* [Bir ThirdPartyResource CustomResourceDefinition için geçirme](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [İş yükleri API değişiklikleri sürüm 1.8 ve 1.9](https://kubernetes.io/docs/reference/workloads-18-19/)

## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

### <a name="agent-pools"></a>Aracı havuzları

Kubernetes denetim düzlemi AKS yönetir olsa da, yeni kümeye dahil etmek düğüm sayısı ve boyutu hala tanımlayın. AKS için ACS 1:1 eşleme istediğiniz varsayılarak, var olan ACS düğüm bilgilerinizi yakalama isteyebilirsiniz. Yeni AKS kümenizi oluşturmak için bu verileri kullanabilirsiniz.

Örnek:

| Ad | Count | VM boyutu | İşletim sistemi |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Ek sanal makineler, geçiş sırasında aboneliğinizi içine dağıtılacak olduğundan, kotalar ve sınırlar bu kaynaklar için yeterli olduğunu doğrulamanız gerekir. 

Daha fazla bilgi için [Azure aboneliği ve hizmet sınırlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits). Azure portalında, geçerli kotanızı denetlemek için Git [abonelikler dikey penceresinden](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)aboneliğinizi seçin ve ardından **kullanım ve kotalar**.

### <a name="networking"></a>Ağ

Karmaşık uygulamalar için genellikle zaman içinde yerine tek seferde geçirmek. Ağ üzerinden iletişim kurmak eski ve yeni ortamlar gerekebilir anlamına gelir. Daha önce kullanılan uygulamalarını `ClusterIP` Hizmetleri iletişim kurmak için tür olarak açığa gerekebilir `LoadBalancer` ve uygun şekilde korunması.

Geçişi tamamlamak için istemcileri AKS'de çalışan yeni hizmetleri işaret edecek şekilde isteyeceksiniz. AKS kümenizi önünde yer alan yük dengeleyiciye işaret edecek şekilde DNS güncelleştirerek trafiği yeniden yönlendirme öneririz.

### <a name="stateless-applications"></a>Durum bilgisiz uygulamalar

Durum bilgisi olmayan uygulama geçiş en basit bir durumdur. YAML tanımlarınızı yeni kümeye uygulama, her şeyin beklendiği gibi çalıştığından emin olun ve yeni kümenize etkinleştirmek için trafiği yeniden yönlendirmeniz.

### <a name="stateful-applications"></a>Durum bilgisi olan uygulamalar

Durum bilgisi olan uygulamalar, veri kaybı veya beklenmeyen kapalı kalma süresini önlemek için geçişiniz dikkatle planlayın.

#### <a name="highly-available-applications"></a>Yüksek kullanılabilirliğe sahip uygulamalar

Yüksek kullanılabilirlik yapılandırmasında durum bilgisi olan bazı uygulamalar dağıtabilirsiniz. Bu uygulamalar genelinde çoğaltma kopyalayabilir. Şu anda bu tür bir dağıtım kullanıyorsanız, yeni AKS kümesinde yeni bir üye oluşturun ve aşağı akış çağıranlar üzerinde çok az etkisi olan geçirmenize mümkün olabilir. Genellikle, bu senaryo için geçiş adımları şunlardır:

1. Yeni bir ikincil çoğaltmaya AKS'de oluşturun.
2. Verileri çoğaltmak bekleyin.
3. Üzerinde bir ikincil çoğaltma yeni birincil hale getirmek başarısız.
4. Trafik, AKS kümeye işaret edin.

#### <a name="migrating-persistent-volumes"></a>Kalıcı birimleri geçirme

AKS için var olan kalıcı birimler geçiriyorsanız, genellikle aşağıdaki adımları izleyin:

1. Sessiz mod uygulamaya yazar. (Bu adım isteğe bağlıdır ve kapalı kalma süresi gerektirir.)
2. Disklerin anlık görüntüleri alın.
3. Yeni yönetilen diskler, anlık görüntüler oluşturun.
4. AKS kalıcı birimler oluşturun.
5. Pod belirtimlerine güncelleştirme [var olan birimler kullanmak](https://docs.microsoft.com/azure/aks/azure-disk-volume) yerine PersistentVolumeClaims (statik sağlama).
6. AKS için uygulamayı dağıtın.
7. Doğrulayın.
8. Trafik, AKS kümeye işaret edin.

> [!IMPORTANT]
> Sessiz mod Yazar değil seçerseniz, yeni dağıtımda veri çoğaltmak gerekir. Aksi takdirde disk anlık görüntülerini gerçekleştirdiğiniz sonra yazılan veri kaçırmayın.

Bazı açık kaynak araçları, yönetilen diskler oluşturma ve birimler Kubernetes kümeleri arasında geçiş yardımcı olabilir:

* [Azure CLI Disk kopyası uzantısı](https://github.com/noelbundick/azure-cli-disk-copy-extension) kopyalar ve kaynak grupları ve Azure bölgeleri arasında disklere dönüştürür.
* [Azure CLI Kube uzantısı](https://github.com/yaron2/azure-kube-cli) ACS Kubernetes birimleri numaralandırır ve bunları bir AKS kümesine geçirir.

#### <a name="azure-files"></a>Azure Dosyaları

Disklerden farklı olarak, birden çok konak için eşzamanlı olarak Azure dosyaları bağlanabilir. AKS kümenizi Azure ve Kubernetes ACS kümenize yine de kullanan bir pod oluşturmaktan önlemez. Veri kaybı ve beklenmeyen davranışı önlemek için kümeler için de indirdiğiniz dosyaları aynı anda yazmayın emin olun.

Uygulamanız için aynı dosya paylaşımını işaret eden birden çok çoğaltma barındırabilir, durum bilgisi olmayan geçiş adımlarını izleyin ve yeni kümenize YAML tanımlarınızı dağıtın. Aksi durumda, bir olası geçiş yaklaşımı aşağıdaki adımları içerir:

1. Bir yineleme sayısı 0 ile AKS uygulamanıza dağıtın.
2. Uygulama üzerinde ACS 0 olarak ölçeklendirin. (Bu adım, kapalı kalma süresi gerekir.)
3. Uygulamayı AKS 1 kadar ölçeklendirin.
4. Doğrulayın.
5. Trafik, AKS kümeye işaret edin.

Bir boş paylaşımı ve kaynak verilerin bir kopyasını oluşturma ile başlamak isterseniz kullanabileceğiniz [ `az storage file copy` ](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) komutlarını verilerinizi geçirin.

### <a name="deployment-strategy"></a>Dağıtım stratejisi

AKS için bir bilinen iyi yapılandırmasını dağıtmak için mevcut bir CI/CD ardışık düzeninizi kullanmanızı öneririz. Var olan dağıtım görevlerinizi kopyalamak ve emin `kubeconfig` yeni AKS kümeye işaret eder.

Bu mümkün değilse, kaynak tanımları ACS'den dışarı aktarın ve ardından bunları AKS için geçerlidir. Kullanabileceğiniz `kubectl` nesneleri dışarı aktarmak için.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Çeşitli açık kaynak Araçlar, dağıtım ihtiyaçlarınıza bağlı olarak yardımcı olabilir:

* [Velero](https://github.com/heptio/ark) (Bu araç, Kubernetes 1.7 gerektirir.)
* [Azure Kube CLI uzantısı](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Geçiş adımları

1. [AKS kümesi oluşturma](https://docs.microsoft.com/azure/aks/create-cluster) Azure portalı, Azure CLI veya Azure Resource Manager şablonu aracılığıyla.

   > [!NOTE]
   > Azure Resource Manager şablonları AKS için bulma [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) github deposu.

2. YAML tanımlarınızı gerekli değişiklikleri yapın. Örneğin, `apps/v1beta1` ile `apps/v1` için `Deployments`.

3. [Birimlerini geçirmek](#migrating-persistent-volumes) (isteğe bağlı) ACS kümenizden AKS kümenizi.

4. CI/CD sisteminize AKS uygulama dağıtmak için kullanın. Veya YAML tanımları uygulamak için kubectl kullanma.

5. Doğrulayın. Uygulamalarınızı beklendiği gibi çalıştığını ve geçirilen tüm veriler üzerinde kopyalandığından emin olun.

6. Trafiği yönlendirin. DNS istemcileri, AKS dağıtımına işaret edecek şekilde güncelleştirin.

7. Geçiş sonrası görevleri tamamlayın. Birimleri geçişi ve seçtiğiniz değil sessiz moda alın yazma işlemleri, verileri yeni kümeye kopyalayın.
