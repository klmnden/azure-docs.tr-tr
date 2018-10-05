---
title: Azure Container Instances ve kapsayıcı düzenleme
description: Azure kapsayıcı örnekleri etkileşim ile kapsayıcı düzenleyicileri anlayın.
services: container-instances
author: seanmck
ms.service: container-instances
ms.topic: article
ms.date: 10/05/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c17bdb5a81640a7162ae735a4633a31cdfffbb1d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803520"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Container Instances ve kapsayıcı düzenleyicileri

Kendi küçük boyutu ve uygulama yönü nedeniyle kapsayıcıları Çevik teslim ortamları ve mikro hizmet tabanlı mimariler için uygundur. Otomatikleştirme ve çok sayıda kapsayıcılar ve nasıl etkileşim kurduklarını yönetme görevini olarak da bilinen *düzenleme*. Popüler kapsayıcı düzenleyicileri, Kubernetes, DC/OS ve Docker Swarm içerir.

Azure Container Instances, bazı düzenleme platformlarını temel zamanlama özellikleri sağlar. Ve bu platformları sağlayan bir yüksek değerli hizmetler kapsamaz olsa da Azure Container Instances için tamamlayıcı olabilir. Bu makalede, Azure Container Instances ne işler ve kapsayıcı düzenleyicileri ile tam nasıl etkileştiğinin kapsamını açıklanır.

## <a name="traditional-orchestration"></a>Geleneksel düzenleme

Standart düzenleme tanımını aşağıdaki görevleri içerir:

- **Zamanlama**: bir kapsayıcı görüntüsü ve bir kaynak isteğiyle kapsayıcı çalıştırmak için uygun bir makinede bulun.
- **Benzeşim/Anti-affinity**: kapsayıcı bir dizi diğer (performans için) yakın veya yeterince ucundaki ayırmak (kullanılabilirlik için) çalışması gerektiğini belirtmek.
- **Sistem durumu izleme**: kapsayıcı hatalarının ve otomatik olarak yeniden zamanlayabilirsiniz.
- **Yük devretme**: her bir makinede çalışan izler ve sağlam düğümleri kapsayıcılara başarısız makinelerden yeniden zamanlayın.
- **Ölçeklendirme**: ekleme veya isteğe bağlı olarak el ile veya otomatik olarak eşleşecek şekilde kapsayıcı örneklerini kaldırın.
- **Ağ**: birden çok konak makineler arasında iletişim kurmak için kapsayıcılar koordine için bir katman ağ sağlayın.
- **Hizmet bulma**: konak makineleri arasında taşımak ve IP adreslerini değiştirmek gibi bile birbirlerine otomatik olarak bulmak kapsayıcılar etkinleştirin.
- **Eşgüdümlü uygulama yükseltmeleri**: uygulama kapalı kalma süresini önlemek için kapsayıcı yükseltmelerini yönetme ve bir sorun yaşanırsa, geri alma etkinleştirin.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Azure Container Instances ile düzenleme: katmanlı bir yaklaşım

Azure Container Instances, tüm orchestrator platformlar, üzerinde çok kapsayıcılı görevleri yönetmek izin verirken tek bir kapsayıcıyı çalıştırmak için gerekli planlama ve yönetim özellikleri sağlayarak düzenlemesi için katmanlı bir yaklaşım sağlar.

Altyapının container Instances için Azure tarafından yönetildiğinden, orchestrator platformunun kendini bir uygun konak makinesi üzerinde tek bir kapsayıcıyı çalıştırmak için bulma ile ilgili gerekmez. Bunu her zaman kullanılabilir bulutun esneklik sağlar. Bunun yerine, orchestrator genişletme dahil olmak üzere, çok kapsayıcılı mimarileri ve Eşgüdümlü yükseltmeleri geliştirilmesini basitleştirin görevleri üzerinde odaklanabilirsiniz.

## <a name="potential-scenarios"></a>Olası senaryolar

Azure Container Instances ile orchestrator tümleştirme hala yeni olsa da, birkaç farklı ortamlar ortaya çıkan beklenir:

### <a name="orchestration-of-container-instances-exclusively"></a>Kapsayıcı düzenleme özel örnekler

Hızlı bir biçimde başlamasını ve ikinci fatura çünkü özel olarak Azure Container Instances temelinde bir ortamda kullanmaya başlayın ve yüksek oranda değişken iş yükleriyle başa çıkmak için en hızlı yolu sunar.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Container Instances ve sanal makineler, kapsayıcılar

Uzun süre çalışan, kararlı iş yükleri için ayrılmış sanal makine kümesi kapsayıcılarda işlemlerini genellikle aynı kapsayıcı Azure Container Instances ile çalışan daha ucuz. Ancak, kapsayıcı örnekleri, hızlı bir şekilde genişletme ve beklenmeyen veya kısa süreli ani kullanım uğraşmanız genel kapasite sözleşme için harika bir çözüm sunar.

Kümedeki sanal makinelerin sayısını ölçeklendirmeye yerine sonra bu makinelere ek kapsayıcıları dağıtma, orchestrator yalnızca ek Azure Container Instances'a kapsayıcıları zamanlayabilir ve bunlar artık olduğunuzda bunları silin gerekli.

## <a name="sample-implementation-virtual-kubelet-for-kubernetes"></a>Örnek uygulama: Kubernetes için Virtual Kubelet

[Virtual Kubelet] [ aci-connector-k8s] proje gösteren Azure Container Instances ile kapsayıcı düzenleme platformlarından nasıl tümleştirebilirsiniz.

Sanal Kubelet taklit eder, Kubernetes [kubelet] [ kubelet-doc] sınırsız kapasiteye sahip bir düğüm olarak kaydedip oluşturulmasını gönderme [pod'ların] [ pod-doc] olarak Azure Container ınstances'da kapsayıcı grupları.

Bağlayıcılar diğer düzenleyiciler için hız ve kolaylık, Azure Container ınstances'da kapsayıcı yönetme ile API orchestrator'ın gücünü birleştirerek platform temelleri ile benzer şekilde tümleştirilebilen oluşturulabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Instances kullanarak ilk kapsayıcınızı oluşturma [Hızlı Başlangıç Kılavuzu](container-instances-quickstart.md).

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/