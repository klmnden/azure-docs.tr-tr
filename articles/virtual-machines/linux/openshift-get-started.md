---
title: OpenShift Azure genel bakış | Microsoft Docs
description: Azure'da OpenShift genel bakış.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: d68215359d50ac153d6df3bbcce5a9b6171698bb
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085448"
---
# <a name="openshift-in-azure"></a>Azure'da OpenShift

OpenShift enterprise için Docker ve Kubernetes getiren açık ve Genişletilebilir bir kapsayıcı uygulama platformudur.  

OpenShift Kubernetes kapsayıcı düzenleme ve management içerir. Bu etkinleştirme işlemleri merkezli araçlarını ve geliştirici merkezli ekler:

- Hızlı Uygulama geliştirme.
- Kolay dağıtım ve ölçeklendirme.
- Takımlar ve uygulamalar için uzun vadeli yaşam döngüsü Bakım.

OpenShift birden çok sürümü kullanılabilir:

- OpenShift Kapsayıcı Platformu
- Azure üzerinde OpenShift (tam olarak yönetilen OpenShift erken CY2019 içinde kullanıma sunulacak)
- OKD (eski adıyla OpenShift Origin)
- OpenShift dedicated
- OpenShift çevrimiçi

Beş sürümünü iki müşterilerin Azure'da dağıtmak hemen kullanılabilir yalnızca bu makalede ele: OpenShift kapsayıcı platformu ve OKD.

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Bir kapsayıcı platformudur Kurumsal kullanıma hazır [ticari sürümü](https://www.openshift.com) gelen ve Red Hat tarafından desteklenir. Bu sürümü ile müşteriler OpenShift kapsayıcı platformu için gerekli yetkilendirmeleri satın alma ve yükleme ve yönetimini tüm altyapının sorumludur.

Tüm platform müşteriler "sahip"olduğundan, bunlar kendi şirket içi veri merkezinde veya genel bulut (örneğin, Azure, AWS ve Google) yükleyebilirsiniz.

## <a name="openshift-on-azure"></a>Azure üzerinde OpenShift

Azure'da OpenShift, Azure'da çalıştırılan OpenShift tam olarak yönetilen bir tekliftir. Bu hizmet tüm dünyada yönetilen ve Microsoft ve Red Hat tarafından desteklenir. Küme, müşterinin Azure aboneliğinize dağıtırsınız. Hizmet şu anda özel Önizleme aşamasındadır ve GA erken CY 2019'de sunulması planlanmaktadır. Teklif tarife için daha yakından ettiği daha fazla bilgi sağlanacaktır

## <a name="okd-formerly-openshift-origin"></a>OKD (eski adıyla OpenShift Origin)

OKD olduğu bir [açık kaynaklı](https://www.okd.io/) desteklenen topluluğu, OpenShift Yukarı Akış projesi. CentOS veya Red Hat Enterprise Linux (RHEL) üzerinde OKD yüklenebilir.

## <a name="openshift-dedicated"></a>OpenShift dedicated

Red Hat tarafından yönetilen adanmış *tek kiracılı* OpenShift kapsayıcı platformu kullanan OpenShift. Red Hat tüm altyapının (ağ, depolama vb. VM'ler, OpenShift küme.) yönetir. Küme, tek bir müşteriye özel ve genel bulut (örneğin, AWS veya Google) çalıştırır. Dört uygulama düğüm başlangıç kümesi içerir ve tüm maliyetler, ön maliyet yıllık ve Ücretli.

## <a name="openshift-online"></a>OpenShift çevrimiçi

Red Hat tarafından yönetilen çevrimiçi *çok kiracılı* OpenShift kapsayıcı platformu kullanan. Red Hat tüm altyapının (örneğin, VM'ler, OpenShift küme, ağ ve depolama) yönetir. 

Bu sürüm ile müşteri kapsayıcıları dağıtılır, ancak denetleyemez kapsayıcıların hangi ana bilgisayarlar üzerinde çalıştırın. Çok kiracılı çevrimiçi olduğundan, diğer müşterilerden kapsayıcıları olarak aynı sanal makine konaklarında kapsayıcıları bulunabilir. Maliyet bir kapsayıcıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da OpenShift ortak önkoşulları yapılandırma](./openshift-prerequisites.md)
- [Azure'da OpenShift kapsayıcı platformu dağıtma](./openshift-container-platform.md)
- [OKD azure'da dağıtma](./openshift-okd.md)
- [OpenShift Azure Stack'te dağıtma](./openshift-azure-stack.md)
- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
