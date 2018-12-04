---
title: Azure Container Instances ve kapsayıcı düzenleme
description: Azure kapsayıcı örnekleri etkileşim ile kapsayıcı düzenleyicileri anlayın.
services: container-instances
author: seanmck
ms.service: container-instances
ms.topic: article
ms.date: 11/30/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 08bc344a20ade3d8bb0f7dd23a854fd03ddac006
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845816"
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

## <a name="scenarios"></a>Senaryolar

Azure Container Instances ile orchestrator tümleştirme hala yeni olsa da, birkaç farklı ortamlar çıkacaktır beklenir:

### <a name="orchestration-of-container-instances-exclusively"></a>Kapsayıcı düzenleme özel örnekler

Hızlı bir biçimde başlamasını ve ikinci fatura çünkü özel olarak Azure Container Instances temelinde bir ortamda kullanmaya başlayın ve yüksek oranda değişken iş yükleriyle başa çıkmak için en hızlı yolu sunar.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Container Instances ve sanal makineler, kapsayıcılar

Uzun süre çalışan, kararlı iş yükleri için ayrılmış sanal makine kümesi kapsayıcılarda işlemlerini genellikle aynı kapsayıcı Azure Container Instances ile çalışan daha ucuz. Ancak, kapsayıcı örnekleri, hızlı bir şekilde genişletme ve beklenmeyen veya kısa süreli ani kullanım uğraşmanız genel kapasite sözleşme için harika bir çözüm sunar.

Kümedeki sanal makinelerin sayısını ölçeklendirmeye yerine sonra bu makinelere ek kapsayıcıları dağıtma, orchestrator yalnızca ek Azure Container Instances'a kapsayıcıları zamanlayabilir ve bunlar artık olduğunuzda bunları silin gerekli.

## <a name="sample-implementation-virtual-nodes-for-azure-kubernetes-service-aks"></a>Örnek uygulama: sanal düğümü için Azure Kubernetes Service (AKS)

Uygulama iş yükleri, hızlı bir şekilde ölçeklendirmek için bir [Azure Kubernetes hizmeti](../aks/intro-kubernetes.md) kullanabileceğiniz (AKS) kümesini *sanal düğümü* Azure Container Instances'da dinamik olarak oluşturulur. Şu anda Önizleme aşamasında olan sanal düğümü ACI çalıştırma pod'ların ve AKS kümesi arasındaki ağ iletişimini etkinleştirin. 

Sanal düğümü şu anda Linux kapsayıcı örneklerini destekler. Kullanarak sanal düğümü ile çalışmaya başlama [Azure CLI](https://go.microsoft.com/fwlink/?linkid=2047538) veya [Azure portalında](https://go.microsoft.com/fwlink/?linkid=2047545).

Sanal düğümü kullanan açık kaynak [Virtual Kubelet] [ aci-connector-k8s] Kubernetes taklit edecek şekilde [kubelet] [ kubelet-doc] sınırsız sayıda düğüm olarak kaydederek Kapasite. Virtual Kubelet oluşturulmasını gönderir [pod'ların] [ pod-doc] olarak Azure Container ınstances'da kapsayıcı grupları.

Bkz: [Virtual Kubelet](https://github.com/virtual-kubelet/virtual-kubelet) sunucusuz kapsayıcı platformlarında Kubernetes API genişletme ek örnekler için proje.

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Instances kullanarak ilk kapsayıcınızı oluşturma [Hızlı Başlangıç Kılavuzu](container-instances-quickstart.md).

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/