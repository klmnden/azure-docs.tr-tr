---
title: Azure Kubernetes Hizmeti Tanıtımı
description: Azure Kubernetes Hizmeti, Azure üzerinde kapsayıcı tabanlı uygulamaları dağıtmayı ve yönetmeyi kolaylaştırır.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: cb38285a009d8dfba175de6e3037970e6111d929
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096136"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS)

Azure Kubernetes Service (AKS), Azure'a yönetilen bir Kubernetes kümesi dağıtmayı kolaylaştırır. AKS, sorumluluğun çoğunu Azure'a devrederek Kubernetes yönetiminin karmaşıklığını ve işlemsel yükünü azaltır. Barındırılan bir Kubernetes hizmeti olarak, Azure sistem durumu izleme ve bakım gibi kritik görevleri sizin için gerçekleştirir. Ayrıca, hizmet ücretsizdir ve ana düğümler değil yalnızca kümelerinizdeki aracı düğümler için ücret ödersiniz.

Bu belge Azure Kubernetes Service (AKS) hizmetinin özelliklerine genel bakış sunmaktadır.

## <a name="flexible-deployment-options"></a>Esnek dağıtım seçenekleri

Azure Kubernetes Service portal, komut satırı ve şablon kullanan dağıtım seçenekleri (Resource Manager şablonları ve Terraform) sunar. Bir AKS kümesini dağıtırken ana ve diğer tüm Kubernetes düğümleri sizin yerinize dağıtılır ve yapılandırılır. Gelişmiş ağ özellikleri, Azure Active Directory tümleştirmesi ve izleme gibi ek özellikler de dağıtım işlemi sırasında yapılandırılabilir.

Daha fazla bilgi için bkz. [AKS portalı hızlı başlangıç][aks-portal] veya [AKS CLI hızlı başlangıç][aks-cli].

## <a name="identity-and-security-management"></a>Kimlik ve güvenlik yönetimi

AKS kümeleri [Rol Tabanlı Erişim Denetimi (RBAC)][kubernetes-rbac] desteği sunar. Bir AKS kümesi ayrıca Azure Active Directory ile tümleştirilecek şekilde de yapılandırılabilir. Bu yapılandırmada Kubernetes erişimi, Azure Active Directory kimlik ve grup üyeliklerine göre yapılandırılabilir.

Daha fazla bilgi için bkz. [Azure Active Directory ile AKS Tümleştirmesi][aks-aad].

## <a name="integrated-logging-and-monitoring"></a>Tümleşik günlüğe kaydetme ve izleme

Kapsayıcı sistem durumu kapsayıcılardan, düğümlerden ve denetleyicilerden bellek ve işlemci ölçümlerini toplayarak performans konusunda görünürlük sunar. Kapsayıcı günlükleri de toplanır. Bu veriler Log Analytics çalışma alanınızda depolanır ve Azure portal, Azure CLI veya REST uç noktasından erişilebilir.

Daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti kapsayıcısı sistem durumunu izleme][container-health].

## <a name="cluster-node-scaling"></a>Küme düğümü ölçeklendirme

Kaynak talebi arttıkça AKS kümesinin düğümleri bu talebi karşılayacak şekilde ölçeklendirilebilir. Kaynak talebi düşerse düğümler kümede ölçeklendirme yoluyla kaldırılabilir. AKS ölçeklendirme işlemleri Azure portalı veya Azure CLI kullanılarak tamamlanabilir.

Daha fazla bilgi için bkz.[Azure Kubernetes Service (AKS) kümesini ölçeklendirme][aks-scale].

## <a name="cluster-node-upgrades"></a>Küme düğümü yükseltmeleri

Azure Kubernetes Service birden fazla Kubernetes sürümü sunar. Yeni sürümler AKS'de kullanılabilir duruma geldikçe Azure portal veya Azure CLI kullanarak kümenizi yükseltebilirsiniz. Yükseltme işlemi sırasında, çalışan uygulamaların kesintiye uğramasını azaltmak için düğümler dikkatli bir şekilde kordonlanır ve boşaltılır.

Daha fazla bilgi için bkz.[Azure Kubernetes Service (AKS) kümesini yükseltme][aks-upgrade].

## <a name="http-application-routing"></a>HTTP uygulaması yönlendirme

HTTP Uygulama Yönlendirmesi çözümü, AKS kümenize dağıtılan uygulamalara daha kolay erişmenizi sağlar. HTTP uygulama yönlendirmesi çözümü etkinleştirildiğinde AKS kümenizde bir giriş denetleyicisi yapılandırır. Uygulamalar dağıtıldığında genel olarak erişilebilir DNS adları da otomatik olarak yapılandırılır.

Daha fazla bilgi için bkz. [HTTP uygulama yönlendirmesi][aks-http-routing].

## <a name="gpu-enabled-nodes"></a>GPU etkin düğümler

AKS, GPU etkin düğüm havuzlarının oluşturulmasını destekler. Azure şu an için tekli veya çoklu GPU etkin VM'ler sunmaktadır. GPU etkin VM'ler işlemci, grafik ve görselleştirme talebi yoğun olan iş yükleri için tasarlanmıştır.

Daha fazla bilgi için bkz. [AKS üzerinde GPU kullanma][aks-gpu].

## <a name="development-tooling-integration"></a>Geliştirme araçlarıyla tümleştirme

Kubernetes; Helm, Draft ve Visual Studio Code için Kubernetes uzantısı gibi zengin bir geliştirme ve yönetim aracı ekosistemine sahiptir. Bu araçlar Azure Kubernetes Service ile birlikte sorunsuz çalışır.

Ayrıca Azure Dev Spaces, ekiplere yönelik hızlı ve yinelemeli bir Kubernetes geliştirme deneyimi sunar. Minimum yapılandırma ile Azure Kubernetes Service (AKS) içinde kapsayıcıları çalıştırabilir ve kapsayıcıların hatasını ayıklayabilirsiniz.

Daha fazla bilgi için bkz. [Azure Dev Spaces][azure-dev-spaces].

Azure DevOps projesi, var olan kodunuzu ve Git deponuzu Azure'a taşımanız için kullanımı kolay bir çözüm sunar. DevOps projesi AKS gibi Azure kaynaklarını otomatik olarak oluşturur, CI için derleme tanımı içeren bir VSTS yayın işlem hattı kurar, CD için yayın tanımı oluşturur ve ardından izleme için Azure Application Insights kaynağı oluşturur.

Daha fazla bilgi için bkz. [Azure DevOps projesi][azure-devops].

## <a name="virtual-network-integration"></a>Sanal ağ tümleştirmesi

AKS kümesi var olan bir sanal ağa dağıtılabilir. Bu yapılandırmada kümedeki her pod'a sanal ağda bir IP adresi atanır ve tümü hem kümedeki diğer pod'larla hem de sanal ağdaki diğer düğümlerle doğrudan iletişim kurabilir. Pod'lar ayrıca eş düğümlü bir sanal ağ üzerindeki diğer hizmetlere, ExpressRoute üzerinden şirket içindeki ağlara ve siteden siteye (S2S) VPN bağlantılarına da bağlanabilir.

Daha fazla bilgi için bkz. [AKS ağına genel bakış][aks-networking].

## <a name="private-container-registry"></a>Özel kapsayıcı kayıt defteri

Docker görüntülerinizin özel olarak depolanması için Azure Container Registry (ACR) ile tümleştirebilirsiniz.

Daha fazla bilgi için bkz. [Azure Container Registry (ACR)][acr-docs].

## <a name="storage-volume-support"></a>Depolama birimi desteği

Azure Kubernetes Service (AKS), kalıcı veri için depolama birimi takılmasını destekler. AKS kümeleri Azure Dosyaları ve Azure Diskleri desteğiyle oluşturulur.

Daha fazla bilgi için bkz. [Azure Dosyaları][azure-files] ve [Azure Diskleri][azure-disk].

## <a name="docker-image-support"></a>Docker görüntüsü desteği

Azure Kubernetes Service (AKS), Docker görüntüsü biçimini destekler.

## <a name="kubernetes-certification"></a>Kubernetes sertifikası

Azure Kubernetes Service (AKS) hizmetinin Kubernetes ile uyumlu olduğu CNCF tarafından onaylanmıştır.

## <a name="regulatory-compliance"></a>Mevzuata uyumluluk

Azure Kubernetes Service (AKS), SOC ve ISO/HIPAA/HITRUST ile uyumludur.

## <a name="next-steps"></a>Sonraki adımlar

AKS hızlı başlangıçları ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS hızlı başlangıç][aks-cli]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./aad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./networking-overview.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./scale-cluster.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/en-us/vsts/pipelines/actions/azure-devops-project-aks?view=vsts
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md

