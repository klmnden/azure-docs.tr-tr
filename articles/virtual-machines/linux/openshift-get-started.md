---
title: OpenShift Azure genel bakış | Microsoft Docs
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
ms.openlocfilehash: e3ab060c1cea28f83c18dc89aeea7716ec86572a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190352"
---
# <a name="openshift-in-azure"></a>Azure'da OpenShift

OpenShift enterprise için Docker ve Kubernetes getiren açık ve Genişletilebilir bir kapsayıcı uygulama platformudur.  

OpenShift Kubernetes kapsayıcı düzenleme ve management içerir. Bunu sağlayan Geliştirici ve işlemleri merkezli araçları ekler:

- Hızlı Uygulama geliştirme.
- Kolay dağıtım ve ölçeklendirme.
- Takımlar ve uygulamalar için uzun vadeli yaşam döngüsü Bakım.

OpenShift birden çok sürümü kullanılabilir:

- OKD (eski adıyla OpenShift Origin)
- OpenShift Kapsayıcı Platformu
- OpenShift çevrimiçi
- OpenShift dedicated

Dört sürümleri kapsamında bu makalede, yalnızca iki müşterilerin Azure'da dağıtmak kullanılabilir: OpenShift Origin ve OpenShift kapsayıcı platformu.

## <a name="okd-formerly-openshift-origin"></a>OKD (eski adıyla OpenShift Origin)

OKD olduğu bir [açık kaynaklı](https://www.okd.io/) desteklenen topluluğu, OpenShift Yukarı Akış projesi. CentOS veya Red Hat Enterprise Linux (RHEL) üzerinde OKD yüklenebilir.

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Bir kapsayıcı platformudur Kurumsal kullanıma hazır [ticari sürümü](https://www.openshift.com) gelen ve Red Hat tarafından desteklenir. Bu sürümü ile müşteriler OpenShift kapsayıcı platformu için gerekli yetkilendirmeleri satın alma ve yükleme ve yönetimini tüm altyapının sorumludur.

Tüm platform müşteriler "sahip"olduğundan, bunlar kendi şirket içi veri merkezinde veya genel bulut (örneğin, Azure, AWS ve Google) yükleyebilirsiniz.

## <a name="openshift-online"></a>OpenShift çevrimiçi

Red Hat tarafından yönetilen çevrimiçi *çok kiracılı* OpenShift kapsayıcı platformu kullanan. Red Hat tüm altyapının (örneğin, VM'ler, OpenShift küme, ağ ve depolama) yönetir. 

Bu sürüm ile müşteri kapsayıcıları dağıtılır, ancak denetleyemez kapsayıcıların hangi ana bilgisayarlar üzerinde çalıştırın. Çok kiracılı çevrimiçi olduğundan, diğer müşterilerden kapsayıcıları olarak aynı sanal makine konaklarında kapsayıcıları bulunabilir. Maliyet bir kapsayıcıdır.

## <a name="openshift-dedicated"></a>OpenShift dedicated

Red Hat tarafından yönetilen adanmış *tek kiracılı* OpenShift kapsayıcı platformu kullanan. Red Hat tüm altyapının (ağ, depolama vb. VM'ler, OpenShift küme.) yönetir. Küme tek bir müşteriye özel ve genel bulut (veya Google, 2018 yılının başlarından yakında Azure ile AWS gibi) çalışır. Bir başlangıç kümesi 48,000 (Önden ödenen) yılda dört uygulama düğüm içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da OpenShift ortak önkoşulları yapılandırma](./openshift-prerequisites.md)
- [Azure'da OpenShift Origin dağıtma](./openshift-origin.md)
- [Azure'da OpenShift kapsayıcı platformu dağıtma](./openshift-container-platform.md)
- [Dağıtım sonrası görevler](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
