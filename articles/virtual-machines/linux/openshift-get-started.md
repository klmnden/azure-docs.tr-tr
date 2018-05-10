---
title: Azure genel bakış OpenShift | Microsoft Docs
description: Azure'da OpenShift genel bakış.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
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
ms.openlocfilehash: c8e740a66271c88b3abb036867d1760cc9e77607
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="openshift-in-azure"></a>Azure'da OpenShift

OpenShift Docker ve Kubernetes kuruluşa getiren açık ve Genişletilebilir kapsayıcı uygulama platformudur.  

OpenShift Kubernetes kapsayıcı orchestration ve yönetimi için içerir. Etkinleştirme Geliştirici ve işlemleri merkezli araçları ekler:

- Hızlı Uygulama geliştirme.
- Kolay dağıtım ve ölçeklendirme.
- Uzun vadeli yaşam döngüsü Bakımı ekipleri ve uygulamalar için.

Birden fazla sürümünü OpenShift vardır:

- OpenShift Origin
- OpenShift Kapsayıcı Platformu
- Çevrimiçi OpenShift
- Ayrılmış OpenShift

Dört sürümleri iki müşterilere Azure'da dağıtmak yalnızca bu makalede, kapsamdaki: OpenShift kaynağı ve OpenShift kapsayıcı Platform.

## <a name="openshift-origin"></a>OpenShift Origin

Kaynağı bir [açık kaynak](https://www.openshift.org/) desteklenen topluluk OpenShift Yukarı Akış projesi. Kaynak, CentOS veya Red Hat Enterprise Linux (RHEL) yüklenebilir.

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Kapsayıcı platformudur bir kurumsal kullanıma hazır [ticari sürümü](https://www.openshift.com) gelen ve Red Hat tarafından desteklenir. Bu sürümle müşteriler OpenShift kapsayıcı Platform için gerekli yetkilendirmeleri satın alın ve yükleme ve yönetimini tüm altyapının için sorumludur.

Tüm platform müşteriler "sahibi"için kullanıcılar kendi şirket içi veri merkezini veya genel bulut (örneğin, Azure, AWS veya Google) yükleyebilir.

## <a name="openshift-online"></a>Çevrimiçi OpenShift

Red Hat yönetimli çevrimiçidir *çok kiracılı* OpenShift kapsayıcı platformu kullanır. Red Hat tüm altyapının (örneğin, sanal makineleri, OpenShift küme, ağ ve depolama) yönetir. 

Bu sürüm ile müşterinin kapsayıcıları dağıtır ancak kapsayıcıların hangi ana bilgisayarların çalıştırmak denetimi yoktur. Çevrimiçi çok kiracılı olduğundan, diğer müşterilerden kapsayıcı olarak aynı VM konaklarda kapsayıcıları bulunabilir. Maliyet bir kapsayıcıdır.

## <a name="openshift-dedicated"></a>Ayrılmış OpenShift

Red Hat yönetilen ayrılmış *tek Kiracı* OpenShift kapsayıcı platformu kullanır. Red Hat tüm altyapının (ağ, depolama, vb. VM'ler, OpenShift küme.) yönetir. Küme bir müşteriye özgü ve genel bulut (örneğin, AWS veya içinde erken 2018 yakında Azure ile Google) çalıştırır. Başlangıç küme 48,000 (Önden Ücretli) yıl başına dört uygulama düğüm içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da OpenShift ortak önkoşulları yapılandırma](./openshift-prerequisites.md)
- [Azure'da OpenShift kaynak dağıtma](./openshift-origin.md)
- [Azure'da OpenShift kapsayıcı Platform dağıtma](./openshift-container-platform.md)
- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını gider](./openshift-troubleshooting.md)
