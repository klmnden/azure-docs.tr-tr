---
title: Geliştirici en iyi uygulamalar - kaynak yönetimini Azure Kubernetes Hizmetleri (AKS)
description: Kaynak Yönetimi Azure Kubernetes Service (AKS) Uygulama geliştirici en iyi uygulamaları öğrenin
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: zarhoads
ms.openlocfilehash: aebade14f3a8a1095925d17325ce99b78031dc32
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757257"
---
# <a name="best-practices-for-application-developers-to-manage-resources-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) kaynaklarını yönetmek uygulama geliştiricileri için en iyi uygulamalar

Geliştirin ve Azure Kubernetes Service (AKS) uygulamaları çalıştırma gibi dikkate alınması gereken birkaç önemli alanlar vardır. Uygulama dağıtımlarını yönetme, sağladığınız Hizmetleri son kullanıcı deneyimi olumsuz yönde etkileyebilir. Geliştirme ve AKS uygulamaları çalıştırma başarılı göz önünde bulundurun yardımcı olması için bazı en iyi uygulamaları izleyebilirsiniz.

Bir uygulama Geliştirici açısından küme ve iş yüklerini çalıştırmak bu en iyi yöntemler makalesi odaklanır. Yönetim en iyi uygulamalar hakkında daha fazla bilgi için bkz: [küme yalıtımı ve kaynak yönetimi Azure Kubernetes Service (AKS) için en iyi işleci][operator-best-practices-isolation]. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Pod kaynak isteklerini ve sınırları nelerdir
> * Geliştirme ve geliştirme alanları ve Visual Studio Code ile uygulamaları dağıtma yolları
> * Nasıl kullanılacağını `kube-advisor` dağıtımları ile ilgili sorunlar olup olmadığını denetlemek için aracı

## <a name="define-pod-resource-requests-and-limits"></a>Pod kaynak isteklerini ve sınırlarını tanımlama

**En iyi uygulama kılavuzunu** - ayarlanmış istekleri ve YAML bildirimlerinizi tüm pod'ların barındırabileceğiniz pod. AKS kümesi kullanır, *kaynak kotaları*, bu değerleri tanımlamazsanız dağıtımınızı reddedilebilir.

Bir AKS kümesi içindeki işlem kaynaklarını yönetmek için bir birincil pod istekleri ve sınırları kullanmaktır. Bu istekleri ve sınırları bilmeniz ne işlem kaynakları bir pod atanmalıdır Kubernetes Zamanlayıcı olanak tanır.

* **Pod istekleri** CPU ve pod gereken bellek miktarı tanımlayın. Bu istekler pod kabul edilebilir bir performans düzeyi sağlamak için gereken bilgi işlem kaynaklarının miktarını olmalıdır.
    * Kubernetes Zamanlayıcı bir düğümde bir pod yerleştirmeye çalıştığında, pod istekleri hangi düğümün kullanılabilir yeterli kaynaklara sahip olduğunu belirlemek için kullanılır.
    * Kabul edilebilir bir performans düzeyi sürdürmek için gerekli az kaynakları tanımlayan yoksa emin olmak için bu istekleri ayarlamak için uygulamanızın performansını izleyin.
* **Pod sınırları** olan en fazla CPU ve bir pod kullanabileceği bellek miktarı. Bu sınırları bir veya iki kaçan pod çok fazla CPU ve bellek düğümünden fotoğrafını çekmenizi engellemek. Bu senaryo, düğüm üzerinde çalışan diğer pod'ların ve performansı azaltır.
    * Düğümlerinizi destekleyebileceğinden daha yüksek bir pod sınırı ayarlamanız gerekmez. Her AKS düğümü, belirli bir temel Kubernetes bileşenleri için CPU ve bellek ayırır. Uygulamanız başarıyla çalışması diğer pod'ların düğümde çok fazla kaynak tüketmeye deneyebilirsiniz.
    * Yine, farklı zamanlarda gün veya hafta sırasında uygulamanızın performansını izleyin. En üst düzey talepleri olduğunda belirlemek ve uygulamanın gereksinimlerini karşılamak için gereken kaynaklar için pod sınırları Hizala.

Pod spesifikasyonlara, bu istekleri ve sınırlarını tanımlamak için en iyi uygulamadır. Bu değerler dahil etmezseniz, hangi kaynakların gereken Kubernetes Zamanlayıcı anlamıyor. Zamanlayıcı, kabul edilebilir uygulaması performansını sağlamak için yeterli kaynağı olmayan bir düğüm üzerindeki pod zamanlayabilir. Küme Yöneticisi ayarlayabilir *kaynak kotaları* kaynak isteklerini ve limitler ayarlamanızı gerektiren bir ad alanında. Daha fazla bilgi için [AKS kaynak kotaları kümeleri][resource-quotas].

Bir CPU istek veya sınırı tanımlarken, değer CPU birimleriyle ölçülür. *1.0* CPU karşılık gelmektedir bir temel alınan sanal CPU çekirdek düğümünde için. Aynı ölçümü GPU'ları için kullanılır. Ayrıca kesirli isteği veya sınırı, genellikle millicpu içinde tanımlayabilirsiniz. Örneğin, *100 milyon* olduğu *0,1* bir temel alınan sanal CPU çekirdeği.

Tek bir NGINX pod için aşağıdaki temel örnek pod istekleri *100 milyon* CPU süresi ve *128 mı* bellek. Pod için kaynak sınırları kümesine *250 milyon* CPU ve *256 mı* bellek:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
```

Kaynak ölçümleri ve atamaları hakkında daha fazla bilgi için bkz: [yönetme işlem kaynaklarını kapsayıcılar için][k8s-resource-limits].

## <a name="develop-and-debug-applications-against-an-aks-cluster"></a>Geliştirme ve hata ayıklama bir AKS kümesi kullanan uygulamalar

**En iyi uygulama kılavuzunu** -geliştirme takımları dağıtma ve hata ayıklama, geliştirme alanları'nı kullanarak bir AKS kümesi karşı gerekir. Bu geliştirme modeli uygulamayı üretim ortamına dağıtılmadan önce rol tabanlı erişim denetimleri, ağ ve depolama gereksinimlerini uygulandığından emin olur.

Azure geliştirme alanları ile geliştirin, ayıklayın ve uygulamaları doğrudan bir AKS kümesine göre test edin. Geliştiriciler bir ekip içinde oluşturmak ve uygulama yaşam döngüsü boyunca sınamak için birlikte çalışır. Visual Studio veya Visual Studio Code gibi mevcut araçları kullanmaya devam edebilirsiniz. Çalıştırın ve bir AKS kümesi uygulamada hata ayıklamak için bir seçenek sağlar geliştirme alanları için bir uzantının yüklenmesi:

![Geliştirme alanları ile bir AKS kümesi uygulamalarında hata ayıklama](media/developer-best-practices-resource-management/dev-spaces-debug.png)

Bu geliştirme alanları ile tümleşik geliştirme ve test işlemi yerel test ortamları gibi azaltır [minikube][minikube]. Bunun yerine, geliştirin ve test karşı bir AKS kümesi. Bu küme güvenli ve mantıksal olarak bir küme yalıtmak için ad alanları kullanımını önceki bölümünde belirtildiği gibi yalıtılmış. Uygulamalarınızı üretime dağıtmaya hazır olduğunuzda, gerçek bir AKS kümesinde tüm geliştirme yapıldığı güvenle dağıtabilirsiniz.

## <a name="use-the-visual-studio-code-extension-for-kubernetes"></a>Kubernetes için Visual Studio Code uzantısı kullanma

**En iyi uygulama kılavuzunu** -yükleme ve kullanma YAML yazdığınızda Kubernetes için VS Code uzantısı bildirimleri. Uzantı, AKS kümesi ile sık etkileşimde bulunan uygulama sahipleri yardımcı olabilir ve tümleşik bir dağıtım çözümü için de kullanabilirsiniz.

[Kubernetes için Visual Studio Code uzantısı] [ vscode-kubernetes] geliştirin ve AKS uygulamaları dağıtmanıza yardımcı olur. Uzantı, Kubernetes kaynakları ve Helm grafikleri ve şablonlar için IntelliSense sağlar. Ayrıca gidin, dağıtma ve VS Code'un içindeki Kubernetes kaynaklardan düzenleyin. Uzantı kaynak istekleri için bir IntelliSense denetimi sağlar veya pod belirtimlerinde ayarlanan kısıtlar:

![Bellek sınırları eksik hakkında uyarı Kubernetes için VS Code uzantısı](media/developer-best-practices-resource-management/vs-code-kubernetes-extension.png)

## <a name="regularly-check-for-application-issues-with-kube-advisor"></a>Düzenli olarak kube-Danışmanı ile uygulama sorunlarını denetleyin

**En iyi uygulama kılavuzunu** -düzenli olarak en son sürümünü çalıştıran `kube-advisor` kümenizdeki sorunları algılamak için açık kaynak aracı. Mevcut bir AKS kümesinde kaynak kotaları çalıştırırsanız `kube-advisor` ilk kaynak isteklerini ve sınırları tanımlanmış olmayan pod'ların bulunamıyor.

[Kube-advisor] [ kube-advisor] , bir Kubernetes kümesi tarar ve bulduğu sorunları ilişkili AKS açık kaynaklı proje bir araçtır. Yararlı bir onay kaynak isteklerini ve sınırları limitiniz olmadığı pod'ların belirlemektir.

Birçok geliştirme ekipleri ve uygulamaları barındıran bir AKS kümesinde bu kaynağın ister ve kümesi sınırlar olmadan pod'ların izlenmesi zor olabilir. En iyi uygulama, düzenli olarak çalıştırılan `kube-advisor` AKS kümelerinizdeki.

## <a name="next-steps"></a>Sonraki adımlar

Bu en iyi yöntemler makalesi, küme ve iş yükleri bir küme işleci perspektifinin çalıştırılacağı konusunda odaklanır. Yönetim en iyi uygulamalar hakkında daha fazla bilgi için bkz: [küme yalıtımı ve kaynak yönetimi Azure Kubernetes Service (AKS) için en iyi işleci][operator-best-practices-isolation].

Bazı bu en iyi yöntemleri uygulamak için aşağıdaki makalelere bakın:

* [Geliştirme alanları ile geliştirin][dev-spaces]
* [Kube-Danışmanı ile ilgili sorunlar olup olmadığını denetleyin][aks-kubeadvisor]

<!-- EXTERNAL LINKS -->
[k8s-resource-limits]: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
[vscode-kubernetes]: https://github.com/Azure/vscode-kubernetes-tools
[kube-advisor]: https://github.com/Azure/kube-advisor
[minikube]: https://kubernetes.io/docs/setup/minikube/

<!-- INTERNAL LINKS -->
[aks-kubeadvisor]: kube-advisor-tool.md
[dev-spaces]: ../dev-spaces/get-started-netcore.md
[operator-best-practices-isolation]: operator-best-practices-cluster-isolation.md
[resource-quotas]: operator-best-practices-scheduler.md#enforce-resource-quotas
