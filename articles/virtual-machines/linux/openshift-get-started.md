---
title: OpenShift Azure genel bakış | Microsoft Docs
description: Azure'da OpenShift genel bakış.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: 53bed2131e81ee5ed0f46bde389262ee8349339a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006973"
---
# <a name="openshift-in-azure"></a>Azure’da OpenShift

OpenShift enterprise için Docker ve Kubernetes getiren açık ve Genişletilebilir bir kapsayıcı uygulama platformudur.  

OpenShift Kubernetes kapsayıcı düzenleme ve management içerir. Bu etkinleştirme işlemleri merkezli araçlarını ve geliştirici merkezli ekler:

- Hızlı Uygulama geliştirme.
- Kolay dağıtım ve ölçeklendirme.
- Takımlar ve uygulamalar için uzun vadeli yaşam döngüsü Bakım.

OpenShift birden çok sürümü kullanılabilir.  Bu sürümleri yalnızca iki bugün müşterilerin Azure'da dağıtmak kullanılabilir: OpenShift kapsayıcı platformu ve OKD (eski adıyla OpenShift Origin).

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Bir kapsayıcı platformudur Kurumsal kullanıma hazır [ticari sürümü](https://www.openshift.com) gelen ve Red Hat tarafından desteklenir. Bu sürümü ile müşteriler OpenShift kapsayıcı platformu için gerekli yetkilendirmeleri satın alma ve yükleme ve yönetimini tüm altyapının sorumludur.

Tüm platform müşteriler "sahip"olduğundan, bunlar kendi şirket içi veri merkezinde veya genel bulut (Azure gibi) yükleyebilirsiniz.

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Azure Red Hat OpenShift, Azure'da çalıştırılan OpenShift tam olarak yönetilen bir tekliftir. Bu hizmet tüm dünyada yönetilen ve Microsoft ve Red Hat tarafından desteklenir. Küme, müşterinin Azure aboneliğinize dağıtırsınız. Hizmet genel kullanım Mayıs 2019'geçici olarak sunulması planlanmaktadır. Hizmet genel kullanımda da geçerli olduğunda ayrı bir belge için Yönetilen hizmet kullanıma sunulacaktır

## <a name="okd"></a>OKD

OKD olduğu bir [açık kaynaklı](https://www.okd.io/) desteklenen topluluğu, OpenShift Yukarı Akış projesi. CentOS veya Red Hat Enterprise Linux (RHEL) üzerinde OKD yüklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da OpenShift ortak önkoşulları yapılandırma](./openshift-prerequisites.md)
- [Azure'da OpenShift kapsayıcı platformu dağıtma](./openshift-container-platform.md)
- [OpenShift kapsayıcı platformu kendi kendini yöneten bir Market teklifi dağıtma](./openshift-marketplace-self-managed.md)
- [OpenShift Azure Stack'te dağıtma](./openshift-azure-stack.md)
- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
