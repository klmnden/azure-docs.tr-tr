---
title: Habitat uygulamanızı azure'a dağıtmak için kullanın
description: Uygulamanızın Azure sanal makineler ve kapsayıcılar için sürekli olarak dağıtma hakkında bilgi edinin
keywords: Azure, chef, devops, sanal makineler, genel bakış, otomatikleştirin, habitat'ın
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 4847d9ce551c9acf1e4fb6325c770187b2cfd89f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052300"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Habitat uygulamanızı azure'a dağıtmak için kullanın
[Habitat](https://www.habitat.sh/) bir ilk tür, açık kaynak projenin, uygulama yönetimi için tamamen yeni bir yaklaşım sunar. Habitat'ın dağıtım birimi uygulama ve onun Otomasyon sağlar. Uygulamaları bir basit "habitat içinde" sarıldığında çalışma zamanı ortamı bir kapsayıcı, çıplak veya PaaS, olup artık odak noktası ve uygulama kısıtlamaz. 

Bu makalede, Habitat kullanmanın avantajları anlatılmaktadır.

## <a name="support-for-the-modern-application"></a>Modern uygulama için destek
Habitat paketlerinin uygulama yaşam döngüsü boyunca çalıştırmak için gereken her şeyi içerir. Habitat'ın temel bileşenleri şunlardır:
- Paketleme biçimini - Habitat paket uygulamalarda atomik, sabit ve denetlenebilir.
- Habitat gözetmen ilişkileri, yükseltme stratejisini ve güvenlik ilkeleri uygulama paketleri paketleri eş tanıma ile çalışır. Habitat gözetmen yapılandırır ve uygulama için hangi ortamı varsa yönetir.
- Habitat ekosistemi, ayrıca, Habitat derleme planı alır, Habitat paketi oluşturur ve bir deposu için yayımlayan bir derleme hizmeti sağlar.

## <a name="run-any-application-anywhere"></a>Herhangi bir uygulama her yerde çalıştırın
Habitat ile uygulamaları herhangi bir çalışma zamanı ortamında değiştirilmemiş olarak çalıştırabilir. Uygulama kapsayıcıları (örneğin, Docker), küme yönetim sistemleri (örneğin, Mesosphere veya Kubernetes) ve PaaS sistemleri (örneğin, Pivotal Cloud Foundry) herhangi bir şey çıplak ve sanal makineler olabilir.

## <a name="easily-port-legacy-applications"></a>Kolayca bağlantı noktası eski uygulamaları
Eski uygulamaları Habitat paketinde sarıldığında uygulamaları, bunlar ilk olarak tasarlanmış olan ortamı bağımsızdır. Paketler, kapsayıcılar ve bulut gibi daha modern ortamları için hızlı bir şekilde taşınabilir. Ayrıca, Habitat paketlerinin standart ve dışa doğru kullanıma yönelik bir arabirim olduğundan, eski uygulamaları yönetmek çok daha kolay hale gelir.

## <a name="improve-the-container-experience"></a>Kapsayıcı deneyimini geliştirin
Habitat'ın üretim ortamlarında kapsayıcıları yönetmenin karmaşıklığını azaltır. Bir kapsayıcıdaki uygulama yapılandırmasını otomatik hale getirerek, Habitat zorlukları geliştiriciler yüz kapsayıcı tabanlı uygulamaları üretim ortamına geliştirme ortamlarından taşırken ele alır.

## <a name="integrate-into-the-chef-devops-workflow"></a>Chef DevOps iş akışına tümleştirin
Habitat proje tarafından Chef sponsorlu. Habitat ile uygulamaları için eşi görülmemiş bir Otomasyon özellikler getirmek üzere, altyapı otomasyonunu Chef'in deneyime yararlanır. Chef, habitat'ın ticari desteği sunan ve Habitat ile Chef teslim uygulama sürüm döngüsü geliştirme dağıtımı otomatik hale getirmek için arasında sorunsuz tümleştirme emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)