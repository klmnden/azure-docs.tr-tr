---
title: Azure Kubernetes Service'i (AKS) Azure kapsayıcı hizmeti (ACS) ' geçiş
description: Azure Kubernetes Service'i (AKS) Azure kapsayıcı hizmeti (ACS) ' geçiş
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: 910c96988ec0a8b8aa7b6ac8ce287c4fdc59e177
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60467570"
---
# <a name="migrating-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'i (AKS) Azure kapsayıcı hizmeti (ACS) ' geçiş

Bu belgenin amacı, Azure Kubernetes Service (AKS) ile Azure Container Service ile Kubernetes (ACS) arasındaki başarılı bir geçiş planlayıp yardımcı olmaktır. Bu kılavuz, AKS ile ACS arasındaki farkları ayrıntıları, geçiş işlemine genel bakış sağlar ve önemli kararlar yardımcı olmalıdır.

## <a name="differences-between-acs-and-aks"></a>AKS ile ACS arasındaki farklar

ACS ve AKS geçiş etkileyen bazı önemli alanda farklılık gösterir. Gözden geçirin ve herhangi bir geçiş işleminden önce aşağıdaki farklar ele almak planlama gerekir.

* AKS düğümlerini kullanma [yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md)
    * Yönetilmeyen diskler AKS düğümlerine ekli önce dönüştürülmesi gerekir
    * Özel `StorageClass` Azure diskleri gelen değiştirilmesi gerekecek nesneleri `unmanaged` için `managed`
    * Tüm `PersistentVolumes` kullanmanız gerekir `kind: Managed`
* AKS, şu anda yalnızca bir aracı havuzu destekler
* Windows Server tabanlı düğümler şu anda [özel Önizleme](https://azure.microsoft.com/blog/kubernetes-on-azure/)
* AKS listesini kontrol edin [desteklenen bölgeler](https://docs.microsoft.com/azure/aks/container-service-quotas)
* AKS, barındırılan bir Kubernetes denetim düzlemi ile yönetilen bir hizmettir. ACS şablonunuz yapılandırmasını daha önce değiştirdiyseniz, uygulamalarınızı değiştirmeniz gerekebilir

### <a name="differences-between-kubernetes-versions"></a>Kubernetes sürümleri arasındaki farklar

Daha yeni bir Kubernetes sürümüne geçiş yapıyorsanız, (örn: birkaç 1.7.x 1.9.x için), ilgilenmenizi gerektiren k8s API'sine değiştirir.

* [Bir ThirdPartyResource CustomResourceDefinition için geçirme](https://kubernetes.io/docs/tasks/access-kubernetes-api/migrate-third-party-resource/)
* [İş yükleri API değişiklikleri sürüm 1.8 ve 1.9](https://kubernetes.io/docs/reference/workloads-18-19/).

## <a name="migration-considerations"></a>Geçiş konuları

### <a name="agent-pools"></a>Aracı havuzları

Kubernetes denetim düzlemi AKS yönetir olsa da, yeni kümeye dahil etmek istediğiniz düğüm sayısı ve boyutu hala tanımlayın. AKS için ACS 1:1 eşleme istediğiniz varsayılarak, var olan ACS düğüm bilgilerinizi yakalama isteyebilirsiniz. Bu veriler, yeni bir AKS kümesi oluşturulurken kullanacaksınız.

Örnek:

| Ad | Sayı | VM Boyutu | İşletim Sistemi |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Ek sanal makineler, geçiş sırasında aboneliğinizi içine dağıtılacak olduğundan, kotalar ve sınırlar bu kaynaklar için yeterli olduğunu doğrulamanız gerekir. İnceleyerek daha fazla bilgi [Azure aboneliği ve hizmet sınırlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits). Geçerli kotanızı denetlemek için Git [abonelikler dikey penceresinden](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure portalında aboneliğinizi seçin ve ardından `Usage + quotas`.

### <a name="networking"></a>Ağ

Karmaşık uygulamalar için genellikle zaman içinde yerine tek seferde geçirmek. Ağ üzerinden iletişim kurmak eski ve yeni ortamlar gerektiği anlamına gelir. Daha önce kullanmanız mümkün uygulamaları `ClusterIP` Hizmetleri iletişim kurmak için tür olarak açığa gerekebilir `LoadBalancer` ve güvenliği uygun şekilde.

Geçişi tamamlamak için istemcileri AKS'de çalışan yeni hizmetler işaret edecek şekilde isteyeceksiniz. Trafiği yönlendirmek için önerilen yöntem, DNS, AKS kümenizin önünde yer alan yük dengeleyiciye işaret edecek şekilde güncelleştirerek ' dir.

### <a name="stateless-applications"></a>Durum bilgisiz uygulamalar

Durum bilgisi olmayan uygulama geçiş en basit bir durumdur. YAML tanımlarınızı yeni kümeye uygulama, her şeyin beklendiği gibi çalıştığını doğrulamak ve yeni kümenize etkin hale getirmek için trafiği yeniden yönlendirmeniz.

### <a name="stateful-applications"></a>Durum bilgisi olan uygulamalar

Geçirme durum bilgisi olan uygulamalar, veri kaybı veya beklenmeyen kapalı kalma süresi kaçınmak dikkatli planlama gerekir.

#### <a name="highly-available-applications"></a>Yüksek kullanılabilirliğe sahip uygulamalar

Durum bilgisi olan bazı uygulamalar, bir yüksek kullanılabilirlik yapılandırmasında dağıtılabilir ve yinelemeleri arasında verileri kopyalayabilirsiniz. Bu geçerli dağıtımınız tanımlıyorsa, yeni AKS kümesinde yeni bir üye oluşturun ve aşağı akış arayanlara etkiyle geçirme mümkün olabilir. Genellikle, bu senaryo için geçiş adımları şunlardır:

1. AKS üzerinde yeni bir ikincil çoğaltma oluşturma
2. Verileri çoğaltmak bekle
3. Üzerinde ikincil çoğaltma yeni birincil hale getirmek başarısız
4. AKS kümesi noktası trafiği

#### <a name="migrating-persistent-volumes"></a>Kalıcı birimleri geçirme

AKS için var olan kalıcı birimler geçiş yapıyorsanız, dikkate alınması gereken birkaç faktör vardır. Genellikle, adımlar şunlardır:

1. (İsteğe bağlı) Sessiz mod (kapalı kalma süresi gerektirir) uygulamaya yazar.
2. Anlık görüntü diskleri
3. Yeni yönetilen diskler, anlık görüntüler oluşturun
4. AKS kalıcı birimler oluşturun
5. Pod belirtimlerine güncelleştirme [var olan birimler kullanmak](https://docs.microsoft.com/azure/aks/azure-disk-volume) PersistentVolumeClaims (statik sağlama) yerine
6. AKS için uygulama dağıtma
7. Doğrulama
8. AKS kümesi noktası trafiği

> **Önemli**: Değil sessiz moda alın yazma işlemlerini seçerseniz, bu yana disk anlık görüntü yazılan veri eksik gibi yeni dağıtım, verileri çoğaltmak gerekir

Açık kaynak araçları mevcut yönetilen diskler oluşturma ve birimler Kubernetes kümeleri arasında geçiş yardımcı olabilir.

* [noelbundick/azure-CLI-disk-extension](https://github.com/noelbundick/azure-cli-disk-copy-extension) - kopyalayın ve kaynak grupları ve Azure bölgeleri arasında disk dönüştürme
* [yaron2/azure-kube-cli](https://github.com/yaron2/azure-kube-cli) - ACS Kubernetes birimleri listeleme ve bunları bir AKS kümeye geçirme

#### <a name="azure-files"></a>Azure Dosyaları

Disklerden farklı olarak, birden çok konak için eşzamanlı olarak Azure dosyaları bağlanabilir. Azure ne Kubernetes AKS kümenizde ACS kümeniz tarafından hala kullanılıyor bir Pod oluşturmanızı engeller. Veri kaybı ve beklenmeyen davranışı önlemek için her iki kümeye de aynı anda aynı dosyalara yazma olmayan emin olmalısınız.

Uygulama aynı dosya paylaşımına işaret eden birden çok çoğaltma barındırabilir durum bilgisi olmayan geçiş adımlarını izleyin ve yeni kümenize YAML tanımlarınızı dağıtın.

Aksi durumda, bir olası geçiş yaklaşımı aşağıdaki adımları içerir:

1. Bir yineleme sayısı 0 ile AKS uygulamanızı dağıtma
2. Ölçek ACS uygulamayı 0 (kapalı kalma süresi gerektirir)
3. Uygulama AKS'de 1 kadar ölçeklendirin.
4. Doğrulama
5. AKS kümesi noktası trafiği

Burada boş bir paylaşımı ile başlayın, ardından kaynak verilerin bir kopyasını aktarmak istediğiniz durumlarda kullanabilirsiniz [ `az storage file copy` ](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) komutlarını verilerinizi geçirin.

### <a name="deployment-strategy"></a>Dağıtım stratejisi

Önerilen yöntem, AKS için bir bilinen iyi yapılandırmasını dağıtmak için mevcut bir CI/CD ardışık düzeninizi kullanmaktır. Var olan dağıtım görevlerinizi kopyalamak ve emin olmanız, `kubeconfig` yeni AKS kümeye işaret eder.

Burada, mümkün olmayan durumlarda ACS'den kaynak tanımını dışarı aktarın ve ardından bunları AKS için uygulama gerekir. Kullanabileceğiniz `kubectl` nesneleri dışarı aktarmak için.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Ayrıca, gereksinimlerinize bağlı olarak yardımcı olabilecek birçok açık kaynak araçları vardır:

* [heptio/ark](https://github.com/heptio/ark) -k8s gerektirir 1.7
* [yaron2/azure-kube-CLI](https://github.com/yaron2/azure-kube-cli)
* [mhausenblas/reshifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Geçiş adımları

### <a name="1-create-an-aks-cluster"></a>1. AKS kümesi oluşturma

Docs için izleyebileceğiniz [AKS kümesi oluşturma](https://docs.microsoft.com/azure/aks/create-cluster) Azure portalı, Azure CLI veya Resource Manager şablonu aracılığıyla.

> AKS Azure Resource Manager şablonları kullanarak bulabilirsiniz [Azure/AKS](https://github.com/Azure/AKS/tree/master/examples/vnet) GitHub deposunu

### <a name="2-modify-applications"></a>2. Uygulamaları değiştirme

YAML tanımlarınıza gereken değişiklikleri yapın. Örn: değiştirerek `apps/v1beta1` ile `apps/v1` için `Deployments`

### <a name="3-optional-migrate-volumes"></a>3. (İsteğe bağlı) Birimlerini geçirmek

Birimleri, ACS kümenizden, AKS kümeye geçirme. Daha fazla ayrıntı bulunabilir [geçirme kalıcı birimler](#migrating-persistent-volumes) bölümü.

### <a name="4-deploy-applications"></a>4. Uygulamaları dağıtma

AKS uygulamaları dağıtmak veya YAML tanımları uygulamak için kubectl kullanmayı CI/CD sisteminize kullanın.

### <a name="5-validate"></a>5. Doğrulama

Uygulamalarınızı beklendiği gibi çalıştığını ve geçirilen tüm veriler üzerinde kopyalandığını doğrulayın.

### <a name="6-redirect-traffic"></a>6. Trafiği yeniden yönlendirme

DNS istemcileri, AKS dağıtımına işaret edecek şekilde güncelleştirin.

### <a name="7-post-migration-tasks"></a>7. Geçiş sonrası görevler

Birimleri geçişi ve seçtiğiniz değil sessiz moda alın yazma işlemleri, verileri yeni kümeye kopyalamak gerekir.
