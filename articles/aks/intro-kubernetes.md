---
title: Azure Kubernetes Hizmeti Tanıtımı
description: Azure’da kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için Azure Kubernetes Hizmetinin özelliklerini ve avantajlarını öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: overview
ms.date: 05/06/2019
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b14e4b452c1dcd92cc3e5447428c28f1d2c5b18e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073825"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS)

Azure Kubernetes Service (AKS), Azure'a yönetilen bir Kubernetes kümesi dağıtmayı kolaylaştırır. AKS, sorumluluğun çoğunu Azure'a devrederek Kubernetes yönetiminin karmaşıklığını ve işlemsel yükünü azaltır. Barındırılan bir Kubernetes hizmeti olarak, Azure sistem durumu izleme ve bakım gibi kritik görevleri sizin için gerçekleştirir. Kubernetes ana düğümler Azure tarafından yönetilir. Siz yalnızca aracı düğümlerini yönetir ve sürdürürsünüz. AKS, yönetilen bir Kubernetes servisi olarak ücretsizdir; ana düğümler için değil, yalnızca kümelerinizdeki aracı düğümleri için ücret ödersiniz.

Azure portalda, Azure CLI ile veya Resource Manager şablonları ve Terraform gibi şablon temelli dağıtım seçenekleri ile bir AKS kümesi oluşturabilirsiniz. Bir AKS kümesini dağıttığınızda ana düğümler ve diğer tüm Kubernetes düğümleri sizin yerinize dağıtılır ve yapılandırılır. Gelişmiş ağ özellikleri, Azure Active Directory tümleştirmesi ve izleme gibi ek özellikler de dağıtım işlemi sırasında yapılandırılabilir. Windows Server kapsayıcıları desteği Önizleme aks'deki şu anda kullanılabilir.

Kubernetes hakkında temel bilgiler hakkında daha fazla bilgi için bkz. [Kubernetes kavramlarını AKS için çekirdek][concepts-clusters-workloads].

Başlamak için, [Azure portalda][aks-portal] veya [ Azure CLI][aks-cli] ile AKS hızlı başlangıcı tamamlayın.

## <a name="access-security-and-monitoring"></a>Erişim, güvenlik ve izleme

AKS, gelişmiş güvenlik ve yönetim için, Azure Active Directory ile tümleşmenizi ve Kubernetes rol tabanlı erişim denetimlerini kullanmanızı sağlar. Ayrıca, kümelerinizin ve kaynaklarınızın sistem durumunu da izleyebilirsiniz.

### <a name="identity-and-security-management"></a>Kimlik ve güvenlik yönetimi

AKS, küme kaynaklarına erişimi sınırlandırmak içi [Kubernetes rol tabanlı erişim denetimini (RBAC)][kubernetes-rbac] desteklemektedir. RBAC, Kubernetes kaynaklarına ve ad alanlarına erişimi ve bu kaynakların izinlerini denetlemenizi sağlar. Bir AKS kümesini Azure Active Directory (AD) ile tümleştirilecek şekilde de yapılandırabilirsiniz. Azure AD tümleştirmesiyle, Kubernetes erişimi var olan kimlik ve grup üyeliklerine göre yapılandırılabilir. Var olan Azure AD kullanıcılarınıza ve gruplarınıza, AKS kaynaklarına erişim hakkı ve tümleşik oturum açma deneyimi sağlanabilir.

Kimlik hakkında daha fazla bilgi için bkz. [AKS için erişim ve kimlik seçeneklerini][concepts-identity].

AKS kümelerinizin güvenliğini sağlamak için, bkz. [Azure Active Directory’yi AKS ile Tümleştirme][aks-aad].

### <a name="integrated-logging-and-monitoring"></a>Tümleşik günlüğe kaydetme ve izleme

AKS kümenizin ve dağıtılan uygulamalarınızın nasıl çalıştığını anlamak amacıyla, kapsayıcı sağlık durumunun Azure İzleyici, düğümlerden ve denetleyicilerden bellek ve işlemci ölçümlerini toplar. Kapsayıcı günlükleri kullanıma açıktır ve ayrıca [Kubernetes ana günlüklerini de][aks-master-logs] inceleyebilirsiniz. Bu izleme verileri bir Log Analytics çalışma alanında depolanır ve Azure portalı, Azure CLI veya REST uç noktasından erişilebilir.

Daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti kapsayıcısı sistem durumunu izleme][container-health].

## <a name="clusters-and-nodes"></a>Kümeler ve düğümler

Azure sanal makinelerinde çalışan AKS düğümleri. Depoları düğümlere ve pod’lara bağlayabilir, küme bileşenlerini yükseltebilir ve GPU’lar kullanabilirsiniz. AKS karma işletim sistemleri ve Windows Server kapsayıcıları (şu anda önizlemede) desteklemek için birden çok düğüm havuzları çalışan Kubernetes kümelerini destekler. Linux düğümleri özelleştirilmiş bir Ubuntu işletim sistemi görüntüsü ve Windows Server düğümleri özelleştirilmiş bir Windows Server 2019 işletim sistemi görüntüsünü çalıştırın.

### <a name="cluster-node-and-pod-scaling"></a>Küme düğümü ve pod ölçeklendirme

Kaynaklar için talep değiştikçe, hizmetlerinizi çalıştıran küme düğümleri veya pod’lar otomatik olarak büyüyebilir veya küçülebilir. Yatay pod otomatik ölçeklendiricisini veya küme otomatik ölçeklendiricisini kullanabilirsiniz. Bu ölçeklendirme yaklaşımı AKS kümesinin taleplere otomatik olarak ayarlanmasını ve yalnızca ihtiyaç duyulan kaynakları çalıştırmasını sağlar.

Daha fazla bilgi için bkz.[Azure Kubernetes Service (AKS) kümesini ölçeklendirme][aks-scale].

### <a name="cluster-node-upgrades"></a>Küme düğümü yükseltmeleri

Azure Kubernetes Service birden fazla Kubernetes sürümü sunar. Yeni sürümler AKS'de kullanılabilir duruma geldikçe Azure portal veya Azure CLI kullanarak kümenizi yükseltebilirsiniz. Yükseltme işlemi sırasında, çalışan uygulamaların kesintiye uğramasını azaltmak için düğümler dikkatli bir şekilde kordonlanır ve boşaltılır.

Yaşam döngüsü sürümleri hakkında daha fazla bilgi edinmek için, bkz. [AKS’de desteklenen Kubernetes sürümleri][aks-supported versions]. Yükseltmeyle ilgili adımlar için bkz. [Azure Kubernetes Service (AKS) kümesini yükseltme][aks-upgrade].

### <a name="gpu-enabled-nodes"></a>GPU etkin düğümler

AKS, GPU etkin düğüm havuzlarının oluşturulmasını destekler. Azure şu an için tekli veya çoklu GPU etkin VM'ler sunmaktadır. GPU etkin VM'ler işlemci, grafik ve görselleştirme talebi yoğun olan iş yükleri için tasarlanmıştır.

Daha fazla bilgi için bkz. [AKS üzerinde GPU kullanma][aks-gpu].

### <a name="storage-volume-support"></a>Depolama birimi desteği

Uygulama iş yüklerini desteklemek isterseniz, kalıcı veriler için depolama birimleri bağlayabilirsiniz. Statik ve dinamik birimler kullanılabilir. Depolamayla kaç tane bağlı pod’un paylaşılacağına bağlı olarak, tek pod erişimi için Azure Diskleri veya birden çok eş zamanlı pod erişimi için Azure Dosyaları tarafından desteklenen depolamayı kullanabilirsiniz.

Daha fazla bilgi için [AKS uygulamalar için Depolama Seçenekleri][concepts-storage].

Kullanarak dinamik kalıcı birimleri ile çalışmaya başlama [Azure diskleri] [ azure-disk] veya [Azure dosyaları][azure-files].

## <a name="virtual-networks-and-ingress"></a>Sanal ağlar ve giriş

AKS kümesi var olan bir sanal ağa dağıtılabilir. Bu yapılandırmada kümedeki her pod'a sanal ağda bir IP adresi atanır ve tümü hem kümedeki diğer pod'larla hem de sanal ağdaki diğer düğümlerle doğrudan iletişim kurabilir. Pod'lar ayrıca eş düğümlü bir sanal ağ üzerindeki diğer hizmetlere, ExpressRoute üzerinden şirket içindeki ağlara veya siteden siteye (S2S) VPN bağlantılarına da bağlanabilir.

Daha fazla bilgi için [kavramları aks'deki uygulamalar için ağ][aks-networking].

Giriş trafiği ile başlamak için, bkz. [HTTP uygulama yönlendirmesi][aks-http-routing].

### <a name="ingress-with-http-application-routing"></a>HTTP uygulama yönlendirmesiyle giriş

HTTP uygulama yönlendirmesi eklentisi, AKS kümenize dağıtılan uygulamalara daha kolay erişmenizi sağlar. HTTP uygulama yönlendirmesi çözümü etkinleştirildiğinde AKS kümenizde bir giriş denetleyicisi yapılandırır. Uygulamalar dağıtıldığında, genel olarak erişilebilir DNS adları da otomatik olarak yapılandırılır. HTTP uygulama yönlendirmesi, bir DNS bölgesi yapılandırır ve bunu AKS kümesi ile tümleştirilir. Ardından, Kubernetes giriş kaynaklarını normal olarak dağıtabilirsiniz.

Giriş trafiği ile başlamak için, bkz. [HTTP uygulama yönlendirmesi][aks-http-routing].

## <a name="development-tooling-integration"></a>Geliştirme araçlarıyla tümleştirme

Kubernetes; Helm, Draft ve Visual Studio Code için Kubernetes uzantısı gibi zengin bir geliştirme ve yönetim aracı ekosistemine sahiptir. Bu araçlar, AKS ile sorunsuz bir şekilde çalışır.

Ayrıca Azure Dev Spaces, ekiplere yönelik hızlı ve yinelemeli bir Kubernetes geliştirme deneyimi sunar. Minimum yapılandırma ile AKS içinde kapsayıcıları çalıştırabilir ve kapsayıcıların hatasını ayıklayabilirsiniz. Başlamak için, bkz. [Azure Dev Spaces][azure-dev-spaces].

Azure DevOps projesi, var olan kodunuzu ve Git deponuzu Azure’a taşımanız için kullanımı kolay bir çözüm sunar. DevOps projesi AKS gibi Azure kaynaklarını otomatik olarak oluşturur, CI için derleme işlem hattı içeren bir Azure DevOps Services yayın işlem hattı kurar, CD için yayın işlem hattı oluşturur ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Daha fazla bilgi için bkz. [Azure DevOps projesi][azure-devops].

## <a name="docker-image-support-and-private-container-registry"></a>Docker görüntü desteği ve özel kapsayıcı kayıt defteri

AKS, Docker görüntü biçimini destekler. Docker görüntülerinizin özel olarak depolanması için, AKS’yi Azure Container Registry (ACR) ile tümleştirebilirsiniz.

Özel görüntü deposu oluşturmak için, bkz. [Azure Container Registry][acr-docs].

## <a name="kubernetes-certification"></a>Kubernetes sertifikası

Azure Kubernetes Service (AKS) hizmetinin Kubernetes ile uyumlu olduğu CNCF tarafından onaylanmıştır.

## <a name="regulatory-compliance"></a>Mevzuata uyumluluk

Azure Kubernetes Service (AKS) SOC, ISO, PCI DSS ve HIPAA ile uyumludur. Daha fazla bilgi için [Microsoft Azure genel bakış Uyumluluk][compliance-doc].

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hızlı başlangıçları ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS hızlı başlangıç][aks-cli]

<!-- LINKS - external -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[compliance-doc]: https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./azure-ad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./concepts-network.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/azure/devops-project/overview
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md
[aks-master-logs]: view-master-logs.md
[aks-supported versions]: supported-kubernetes-versions.md
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[kubernetes-rbac]: concepts-identity.md#role-based-access-controls-rbac
[concepts-identity]: concepts-identity.md
[concepts-storage]: concepts-storage.md
