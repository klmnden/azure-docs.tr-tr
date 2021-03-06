---
title: Azure Red Hat OpenShift giriş | Microsoft Docs
description: Kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için Microsoft Azure Red Hat OpenShift avantajları ve özellikleri öğrenin.
services: container-service
author: jimzim
ms.author: jzim
ms.service: container-service
manager: jeconnoc
ms.topic: overview
ms.date: 05/08/2019
ms.custom: mvc
ms.openlocfilehash: 7cabedaec1190437aa9f225397afa8871cb06e88
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306368"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft *Azure Red Hat OpenShift* hizmeti sayesinde dağıtmak tam olarak yönetilen [OpenShift](https://www.openshift.com/) kümeleri.

Azure Red Hat OpenShift genişletir [Kubernetes](https://kubernetes.io/). Kubernetes ile üretimde çalışan kapsayıcılar ek araçlar ve görüntü kayıt, Depolama Yönetimi, ağ çözümleri ve günlüğe kaydetme gibi kaynaklar gerektirir ve izleme araçları, hepsi birlikte tutulan ve test edilmiş olması gerekir. Kapsayıcı tabanlı uygulamaları oluşturmak, ara yazılım, çerçeveler, veritabanları ve CI/CD araçlarıyla daha da fazla tümleştirme iş gerektirir. Azure Red Hat OpenShift tüm bu uygulama sağlarken BT ekipleri için işlemlerinin bir kolayca getiren tek bir platform yürütmek ihtiyaç duydukları takımlar birleştirir.

Azure Red Hat OpenShift ortaklaşa mühendislik işletilen ve tümleşik destek deneyimi sağlamak için Red Hat ve Microsoft tarafından desteklenir. Çalışması için hiçbir sanal makine vardır ve hiçbir düzeltme eki uygulama gereklidir. Ana, altyapı ve uygulama düğümleri güncelleştirilmiş, Yama ve Red Hat ve Microsoft tarafından sizin adınıza izlenen. Azure Red Hat OpenShift kümeleriniz, Azure aboneliğinize dağıtılır ve Azure faturanızda yer alır.

Kendi kayıt defteri, ağ, depolama ve CI/CD çözümleri veya kullanmak için otomatik kaynak kodu yönetimi, kapsayıcı ve uygulama yerleşik çözüm oluşturur, dağıtımlar, ölçeklendirme, sistem durumu yönetimi ve daha fazla seçebilirsiniz. Azure Red Hat OpenShift bir tümleşik oturum açma deneyimi Azure Active Directory aracılığıyla sağlar.

Başlamak için tamamlamak [Azure Red Hat OpenShift küme oluşturma](tutorial-create-cluster.md) öğretici.

## <a name="access-security-and-monitoring"></a>Erişim, güvenlik ve izleme

Geliştirilmiş güvenlik ve Yönetim için Azure Red Hat OpenShift Azure Active Directory (Azure AD) ile tümleştirme ve Kubernetes rol tabanlı erişim denetimi (RBAC) kullanmanıza olanak tanır. Ayrıca, kümelerinizin ve kaynaklarınızın sistem durumunu da izleyebilirsiniz.

## <a name="cluster-and-node"></a>Küme ve düğüm

Azure Red Hat OpenShift düğümleri Azure sanal makineler üzerinde çalıştırın. Depoları düğümlere ve pod’lara bağlayabilir, küme bileşenlerini yükseltebilir ve GPU’lar kullanabilirsiniz.

## <a name="virtual-networks-and-ingress"></a>Sanal ağlar ve giriş

Bir Azure Red Hat OpenShift kümesi mevcut bir sanal ağ eşlemesi aracılığıyla bağlanabilirsiniz. Bu yapılandırmada, pod'ların eşlenen sanal ağdaki diğer hizmetlere ve şirket içi ağlara üzerinden bağlanabilirsiniz [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) veya siteden siteye (S2S) VPN bağlantıları.

Bkz: [kümenin sanal ağ mevcut bir sanal ağa bağlama](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) Ayrıntılar için.

## <a name="kubernetes-certification"></a>Kubernetes sertifikası

Azure Red Hat OpenShift hizmeti, Kubernetes uyumlu sertifikalı CNCF olmuştur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Red Hat OpenShift önkoşulları öğrenin:

> [!div class="nextstepaction"]
> [Geliştirme ortamınızı kurma](howto-setup-environment.md)