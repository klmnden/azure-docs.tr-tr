---
title: "Azure genel bakış üzerinde OpenShift | Microsoft Docs"
description: "Azure genel bakış üzerinde OpenShift."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: f9641b52db91a4356f6d5789a8cd78a6bb3da02b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="openshift-overview"></a>OpenShift genel bakış

OpenShift docker ve Kubernetes kuruluşa getiren açık ve Genişletilebilir kapsayıcı uygulama platformudur.  

OpenShift Kubernetes kapsayıcı orchestration ve yönetimi için içerir. Geliştirici ve etkinleştirme işlemleri merkezli araçları ekler:

- Hızlı Uygulama geliştirme
- Kolay dağıtım ve ölçeklendirme
- Takımlar ve uygulamalar için uzun vadeli yaşam döngüsü bakım

Hangi iki Azure'da çalışması için kullanılabilir olan OpenShift birden çok teklifler vardır.

- OpenShift Origin
- OpenShift Kapsayıcı Platformu
- Çevrimiçi OpenShift
- Ayrılmış OpenShift

Kapsanan dört teklifler, iki Azure'da, kendi - dağıtmak müşterilere OpenShift kaynağı ve OpenShift kapsayıcı Platform.

## <a name="openshift-origin"></a>OpenShift Origin

[Açık kaynak](https://www.openshift.org/) desteklenen topluluk OpenShift Yukarı Akış projesi. Kaynak, CentOS veya RHEL yüklenebilir.

## <a name="openshift-container-platform"></a>OpenShift Kapsayıcı Platformu

Kurumsal hazır ([ticari teklifi](https://www.openshift.com)) Red Hat tarafından desteklenen Red Hat sürümünden. Müşteri OpenShift kapsayıcı Platform için gerekli yetkilendirmeleri satın aldıktan sonra yükleme ve tüm altyapı yönetimini sorumludur.

Tüm platform müşteri "sahibi olduğu" kullanıcılar yükleyebilir kendi şirket içi veri merkezi, genel bulut (Azure, AWS, Google, vb.), vs.

## <a name="openshift-online"></a>Çevrimiçi OpenShift

Red Hat yönetilen **çok kiracılı** OpenShift (kapsayıcı platformu kullanarak). Red Hat tüm altyapının (ağ, depolama, vb. VM'ler, OpenShift küme.) yönetir. 

Müşteri kapsayıcıları dağıtır ancak kapsayıcıların hangi ana bilgisayarda çalıştırmak denetimi yoktur. Çok kiracılı olduğuna göre kapsayıcıları aynı VM konakları diğer müşterilerden kapsayıcılar olarak birlikte bulunabilir. Maliyet bir kapsayıcıdır.

## <a name="openshift-dedicated"></a>Ayrılmış OpenShift

Red Hat yönetilen **tek Kiracı** OpenShift (kapsayıcı platformu kullanarak). Red Hat tüm altyapının (ağ, depolama, vb. VM'ler, OpenShift küme.) yönetir. Küme bir müşteriye özgü ve genel bulut (AWS, Google, Azure içinde erken 2018 gelen -) çalıştırır. Başlangıç küme $48 K/yıl (yılın tamamı için ön ödeme) dört uygulama düğüm içeriyor.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da OpenShift ortak önkoşulları yapılandırma](./openshift-prerequisites.md)
- [OpenShift kaynak dağıtma](./openshift-origin.md)
- [OpenShift kapsayıcı Platform dağıtma](./openshift-container-platform.md)
- [POST dağıtım görevleri](./openshift-post-deployment.md)
- [OpenShift dağıtım sorunlarını giderme](./openshift-troubleshooting.md)
