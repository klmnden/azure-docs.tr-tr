---
title: Kavramlar - Azure Kubernetes hizmeti (AKS) güvenliği
description: Azure Kubernetes Service (Kubernetes gizli dizileri ana vm'lerde ve düğüm iletişimi ve ağ ilkeleri de dahil olmak üzere AKS), güvenlik hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: iainfou
ms.openlocfilehash: 8fd5b726c01b056d38e7e187cec8270ee4e127a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466751"
---
# <a name="security-concepts-for-applications-and-clusters-in-azure-kubernetes-service-aks"></a>Uygulama ve kümelerin Azure Kubernetes Service (AKS) için güvenlik kavramları

Azure Kubernetes Service (AKS) uygulama iş yüklerini çalıştırmanıza olarak müşteri verilerinizi korumak için kümenizin güvenliğini bir temel noktadır. Kubernetes içeren güvenlik bileşenleri gibi *ağ ilkelerini* ve *gizli dizileri*. Azure ağ güvenlik grupları gibi bileşenleri ekler ve Küme yükseltme düzenlenir. Bu güvenlik bileşenleri en son işletim sistemi güvenlik güncelleştirmelerini ve Kubernetes sürümleri çalıştıran AKS kümenizi tutmak için birleştirilir ve pod trafiği ve hassas kimlik erişimi ile güvenli hale getirme.

Bu makalede aks'deki uygulamalarınızı güvenli bir temel kavramlar tanıtılır:

- [Ana bileşenleri güvenlik](#master-security)
- [Düğüm güvenliği](#node-security)
- [Küme yükseltme](#cluster-upgrades)
- [Ağ güvenliği](#network-security)
- [Kubernetes gizli dizileri](#kubernetes-secrets)

## <a name="master-security"></a>Güvenlik Yöneticisi

AKS, Kubernetes ana bileşenleri Microsoft tarafından sağlanan bir yönetilen hizmet bir parçasıdır. API sunucusu, Zamanlayıcı vb. sağlamak için kendi tek kiracılı, adanmış Kubernetes Yöneticisi her bir AKS kümesi vardır. Bu ana yönetilir ve Microsoft tarafından korunur.

Varsayılan olarak Kubernetes API sunucusuna bir genel IP adresini kullanır ve ile tam etki alanı adı (FQDN). Kubernetes rol tabanlı erişim denetimlerine ve Azure Active Directory kullanarak API sunucusu için erişimi denetleyebilirsiniz. Daha fazla bilgi için [AKS ile Azure AD tümleştirme][aks-aad].

## <a name="node-security"></a>Düğüm güvenliği

AKS, yönetmek ve korumak Azure sanal makineleri düğümlerdir. Düğümleri Moby kapsayıcı çalışma zamanı kullanılarak en iyi duruma getirilmiş bir Ubuntu Linux dağıtımını çalıştırın. Bir AKS kümesi oluşturduğunuz ya da yukarı ölçeklendirilemez düğümlerin en son işletim sistemi güvenlik güncelleştirmelerini ve yapılandırmaları ile otomatik olarak dağıtılır.

Azure platformunun işletim sistemi güvenlik yamaları düğümlerine gecelik temelinde otomatik olarak uygular. Bir işletim sistemi güvenlik güncelleştirmesi ana bilgisayar yeniden başlatma gerektirirse, yeniden başlatma otomatik olarak gerçekleştirilmez. Düğümleri el ile yeniden başlatılabilir ya da ortak bir yaklaşım kullanmaktır [Kured][kured], Kubernetes için bir açık kaynak önyükleme arka plan programı. Kured çalışırken bir [DaemonSet] [ aks-daemonsets] ve her düğüm için bir yeniden başlatma gerekli olduğunu belirten bir dosyanın varlığını izler. Yeniden başlatma, aynı kümede yönetilir [kordon altına alma ve boşaltma işlemi](#cordon-and-drain) olarak bir küme yükseltmesi.

Düğümleri atanan genel IP adresi ile bir özel sanal ağ alt ağa dağıtılır. Yönetim ve sorun giderme amacıyla SSH, varsayılan olarak etkindir. Bu SSH erişimini, yalnızca iç IP adresi kullanılarak erişilebilir.

Depolama alanı sağlamak için düğümleri Azure yönetilen diskler kullanın. Çoğu VM düğümü boyutları için yüksek performanslı SSD'ler ile desteklenir. Premium diskler şunlardır. Yönetilen diskler üzerinde depolanan verileri otomatik olarak Azure platformu içerisindeki şifrelenir. Yedekliliği artırmak için Azure veri merkezi içinde de güvenli bir şekilde bu diskler çoğaltılır.

Kubernetes ortamlarda AKS veya başka bir yerde, şu anda tehlikeli çok kiracılı kullanım için tamamen güvenli değildir. Ek güvenlik özellikleri gibi *Pod güvenlik ilkeleri* veya daha fazla ayrıntılı rol tabanlı erişim denetimleri (RBAC) düğümleri için saldırılara daha zor hale. Ancak, tehlikeli çok kiracılı iş yüklerini çalıştırırken doğru güvenlik için bir hiper yönetici yalnızca güvenip güvenmeyeceğini güvenlik düzeyidir. Kubernetes için güvenlik etki alanı, tüm küme, tek bir düğüm olur. Bu tür tehlikeli çok kiracılı iş yükleri için fiziksel olarak izole edilmiş kümeleri kullanmanız gerekir. İş yüklerini yalıtmak için yollar hakkında daha fazla bilgi için bkz. [AKS kümesi yalıtımı için en iyi yöntemler][cluster-isolation],

## <a name="cluster-upgrades"></a>Küme yükseltme

Azure, güvenlik ve uyumluluk için veya en son özellikleri kullanmak için bir AKS kümesi ve bileşenlerini yükseltme düzenlemek için araçlar sağlar. Bu yükseltme düzenleme Kubernetes ana ve aracı bileşenlerini içerir. Görüntüleyebileceğiniz bir [kullanılabilir Kubernetes sürümlerini listesi](supported-kubernetes-versions.md) AKS kümenizin. Yükseltme işlemini başlatmak için kullanılabilir bu sürümlerinden birini belirtin. Azure ardından güvenli bir şekilde cordons her AKS düğümü boşaltır ve yükseltme gerçekleştirir.

### <a name="cordon-and-drain"></a>Cordon ve boşaltma

Yeni pod'ları üzerinde zamanlanmış değil için yükseltme işlemi sırasında AKS düğümleri ayrı ayrı kümeden kordonlanır. Düğümleri boşaltılır ve yükseltme gibi verilmiştir:

- Mevcut pod'ların düzgün bir şekilde sonlandırıldı ve Kalan düğümlerde zamanlandı.
- Düğüm yeniden, yükseltme işlemi tamamlandı ve ardından birleştirmeler AKS kümesine yedekleyin.
- Pod'ları üzerinde yeniden çalışmak üzere zamanlanır.
- Kümedeki sonraki düğüme kordonlanır ve tüm düğümleri başarıyla yükseltilene kadar aynı işlem kullanarak boşaltılır.

Daha fazla bilgi için [AKS kümesini yükseltme][aks-upgrade-cluster].

## <a name="network-security"></a>Ağ güvenliği

Bağlantı ve şirket içi ağlar ile güvenlik için mevcut Azure sanal ağ alt ağları, AKS kümesine dağıtabilirsiniz. Bu sanal ağları Azure siteden siteye VPN veya Express Route bağlantı, şirket içi ağınıza geri olabilir. Kubernetes giriş denetleyicileri, hizmetleri yalnızca bu iç ağ bağlantısı üzerinden erişilebilir olacak şekilde özel, iç IP adresi ile tanımlanabilir.

### <a name="azure-network-security-groups"></a>Azure ağ güvenlik grupları

Sanal ağlarda trafik akışını filtre uygulamak için Azure ağ güvenlik grubu kuralları kullanır. Bu kurallar, kaynak ve hedef IP aralıkları, bağlantı noktaları ve protokollere izin verilen ya da kaynaklara erişimi reddedildi tanımlayın. Varsayılan kuralları, Kubernetes API sunucusuna TLS trafiğine izin verecek şekilde oluşturulur. Yük Dengeleyiciler, bağlantı noktası eşlemelerini veya giriş yollar ile Hizmetleri oluştururken, AKS trafiği için ağ güvenlik grubu akış uygun şekilde otomatik olarak değiştirir.

## <a name="kubernetes-secrets"></a>Kubernetes gizli dizileri

Bir Kubernetes *gizli* hassas verilere erişim kimlik bilgilerine veya anahtarlara gibi pod'ların eklemesine kullanılır. Önce Kubernetes API'yi kullanarak bir gizli dizi oluşturun. Pod veya dağıtım tanımladığınızda, belirli bir gizli dizi istenebilir. Gizli dizileri için bunu gerektiren zamanlanmış bir pod düğüm yalnızca sağlanır ve gizli dizi depolanır *tmpfs*değil yazılı diske. Gizli dizi gerektiren bir düğüm üzerindeki son pod silindiğinde, gizli dizi düğümün tmpfs silindi. Gizli dizileri belirli bir ad alanı içinde depolanır ve yalnızca pod'ların aynı ad alanında tarafından erişilebilir.

Pod ya da hizmet bildirimi YAML içinde tanımlanan hassas bilgileri gizli dizileri kullanımını azaltır. Bunun yerine, YAML bildiriminizin bir parçası olarak Kubernetes API sunucusunda depolanan gizli isteyin. Bu yaklaşım, gizli yalnızca belirli pod erişim sağlar.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümelerinizi güvenliğini kullanmaya başlamak için bkz. [AKS kümesini yükseltme][aks-upgrade-cluster].

İlişkili en iyi yöntemler için bkz: [küme güvenliği ve AKS yükseltmeler için en iyi yöntemler][operator-best-practices-cluster-security].

Çekirdek Kubernetes hakkında daha fazla bilgi ve AKS kavramlar için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümesi ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS kimlik][aks-concepts-identity]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeklendirin][aks-concepts-scale]

<!-- LINKS - External -->
[kured]: https://github.com/weaveworks/kured
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/

<!-- LINKS - Internal -->
[aks-daemonsets]: concepts-clusters-workloads.md#daemonsets
[aks-upgrade-cluster]: upgrade-cluster.md
[aks-aad]: azure-ad-integration.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[cluster-isolation]: operator-best-practices-cluster-isolation.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
