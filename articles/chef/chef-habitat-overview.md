---
title: Uygulamanızı Azure'a dağıtmak için Habitat kullanın
description: Tutarlı bir şekilde uygulamanızı Azure sanal makineleri ve kapsayıcıları dağıtmayı öğrenin
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirme, habitat
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 73c6834eb53c0496fa673dd25ab90abc18a6a139
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267513"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Uygulamanızı Azure'a dağıtmak için Habitat kullanın
[Habitat](https://www.habitat.sh/) ilk tür, açık kaynaklı projenin, uygulama yönetimi için tamamen yeni bir yaklaşım sunmaktadır. Habitat uygulama ve onun Otomasyon dağıtım birimidir yapar. Uygulamalar bir basit "habitat içinde" sarılır, çalışma zamanı ortamı bir kapsayıcı, çıplak metal veya PaaS, olup artık odak noktasıdır ve uygulama kısıtlamaz. 

Bu makalede Habitat kullanmanın avantajları açıklanmıştır.

## <a name="support-for-the-modern-application"></a>Modern uygulama için destek
Habitat paketleri, uygulamanın kendi yaşam döngüsü boyunca çalıştırmak için gereken her şeyi içerir. Habitat'ın çekirdek bileşenleri şunlardır:
- Paketleme biçimi - Habitat paket uygulamalarda atomik, değişmez ve denetlenebilir.
- Habitat yönetici uygulama paketleri ilişkileri, yükseltme stratejisini ve güvenlik ilkelerini paketleri eş tanıma özelliğiyle birlikte çalışır. Habitat yönetici yapılandırır ve hangi ortamı varsa için uygulama yönetir.
- Habitat ekosistemi Habitat derleme planı alır, Habitat paket oluşturur ve bir depoda yayımlar bir yapı hizmeti de sağlar.

## <a name="run-any-application-anywhere"></a>Herhangi bir uygulama herhangi bir yere çalıştırın
Habitat ile uygulamaları hiçbir çalışma zamanı ortamında değiştirilmemiş çalıştırabilir. Bu her şeyi çıplak metal ve sanal makineleri için (örneğin, Docker) kapsayıcıları, küme yönetim sistemleri (örneğin, Mesosphere veya Kubernetes) ve PaaS sistemleri (örneğin, Bileşendirler bulut Foundry) içerir.

## <a name="easily-port-legacy-applications"></a>Kolay bağlantı noktası eski uygulamaları
Eski uygulamaları Habitat paketinde sarılır, uygulamalar, bunlar ilk olarak tasarlanmış ortamını bağımsızdır. Paketleri hızlı bir şekilde daha modern ortamlara Bulut veya kapsayıcıları gibi taşınabilir. Ayrıca, Habitat paketleri standart, dış kullanıma yönelik bir arabirim olduğundan, eski uygulamaları yönetmek çok daha kolay olur.

## <a name="improve-the-container-experience"></a>Kapsayıcı deneyimini geliştirmek
Habitat kapsayıcıları üretim ortamlarında yönetme karmaşıklığını azaltır. Uygulama yapılandırması bir kapsayıcıdaki otomatik hale getirerek Habitat zorluklar geliştiriciler yüz kapsayıcı tabanlı uygulamalar geliştirme ortamlarından üretime taşırken giderir.

## <a name="integrate-into-the-chef-devops-workflow"></a>Chef DevOps iş akışı ile tümleştirme
Habitat proje Chef tarafından sponsorlu. Habitat Chef'ın derin eşi görülmemiş Otomasyon özellikleri uygulamalara getirmek için altyapı Otomasyon deneyimiyle yararlanır. Chef Habitat ticari desteği sunar ve Habitat ve Chef teslim tam uygulama yayın döngüsü geliştirme dağıtımı otomatik hale getirmek için arasında sorunsuz tümleştirme emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)