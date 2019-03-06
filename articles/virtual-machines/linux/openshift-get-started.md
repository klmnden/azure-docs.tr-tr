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
ms.date: 03/01/2019
ms.author: haroldw
ms.openlocfilehash: 826377f7ae4803818fab22e333f7b9b89caf67cb
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57444897"
---
# <a name="openshift-in-azure"></a>Azure'da OpenShift

OpenShift enterprise için Docker ve Kubernetes getiren açık ve Genişletilebilir bir kapsayıcı uygulama platformudur.  

OpenShift Kubernetes kapsayıcı düzenleme ve management içerir. Bu etkinleştirme işlemleri merkezli araçlarını ve geliştirici merkezli ekler:

- Hızlı Uygulama geliştirme.
- Kolay dağıtım ve ölçeklendirme.
- Takımlar ve uygulamalar için uzun vadeli yaşam döngüsü Bakım.

OpenShift birden çok sürümü kullanılabilir:

- OpenShift Kapsayıcı Platformu
- Azure üzerinde OpenShift (tam olarak yönetilen OpenShift S1 CY2019 sonuna geçici olarak kullanıma sunulacak)
- OKD (eski adıyla OpenShift Origin)
- OpenShift dedicated
- OpenShift Online

Bu makalede ele alınan beş sürümünü yalnızca iki bugün müşterilerin Azure'da dağıtmak kullanılabilir: OpenShift kapsayıcı platformu ve OKD.

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Bir kapsayıcı platformudur Kurumsal kullanıma hazır [ticari sürümü](https://www.openshift.com) gelen ve Red Hat tarafından desteklenir. Bu sürümü ile müşteriler OpenShift kapsayıcı platformu için gerekli yetkilendirmeleri satın alma ve yükleme ve yönetimini tüm altyapının sorumludur.

Tüm platform müşteriler "sahip"olduğundan, bunlar kendi şirket içi veri merkezinde veya genel bulut (Azure gibi) yükleyebilirsiniz.

## <a name="openshift-on-azure"></a>Azure üzerinde OpenShift

Azure'da OpenShift, Azure'da çalıştırılan OpenShift tam olarak yönetilen bir tekliftir. Bu hizmet tüm dünyada yönetilen ve Microsoft ve Red Hat tarafından desteklenir. Küme, müşterinin Azure aboneliğinize dağıtırsınız. Hizmet şu anda özel Önizleme aşamasındadır ve S1 CY2019 sonuna etrafında GA sunulması planlanmaktadır. Özel önizlemeye katılmak isteyen müşteriler için lütfen doldurun [ADAYLIK formu](http://aka.ms/openshiftazureinterest).  Teklif tarife için daha yakından ettiği daha fazla bilgi sağlanacaktır

## <a name="okd-formerly-openshift-origin"></a>OKD (eski adıyla OpenShift Origin)

OKD olduğu bir [açık kaynaklı](https://www.okd.io/) desteklenen topluluğu, OpenShift Yukarı Akış projesi. CentOS veya Red Hat Enterprise Linux (RHEL) üzerinde OKD yüklenebilir.

## <a name="openshift-dedicated"></a>OpenShift dedicated

Red Hat tarafından yönetilen adanmış *tek kiracılı* OpenShift kapsayıcı platformu kullanan OpenShift. Red Hat tüm altyapının (ağ, depolama vb. VM'ler, OpenShift küme.) yönetir. Küme, tek bir müşteriye özel ve genel bulut (Azure gibi) çalışır. Dört uygulama düğüm başlangıç kümesi içerir ve tüm maliyetler, ön maliyet yıllık ve Ücretli.

## <a name="openshift-online"></a>OpenShift Online

Red Hat tarafından yönetilen çevrimiçi *çok kiracılı* OpenShift kapsayıcı platformu kullanan. Red Hat tüm altyapının (örneğin, VM'ler, OpenShift küme, ağ ve depolama) yönetir. 

Bu sürüm ile müşteri kapsayıcıları dağıtılır, ancak denetleyemez kapsayıcıların hangi ana bilgisayarlar üzerinde çalıştırın. Çok kiracılı çevrimiçi olduğundan, diğer müşterilerden kapsayıcıları olarak aynı sanal makine konaklarında kapsayıcıları bulunabilir. Maliyet bir kapsayıcıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da OpenShift ortak önkoşulları yapılandırma](./openshift-prerequisites.md)
- [Azure'da OpenShift kapsayıcı platformu dağıtma](./openshift-container-platform.md)
- [OKD azure'da dağıtma](./openshift-okd.md)
- [OpenShift Azure Stack'te dağıtma](./openshift-azure-stack.md)
- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
