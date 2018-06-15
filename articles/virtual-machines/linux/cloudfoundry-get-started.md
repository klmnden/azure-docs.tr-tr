---
title: Microsoft azure'da bulut Foundry ile çalışmaya başlama | Microsoft Docs
description: Microsoft Azure üzerinde OSS veya Bileşendirler bulut Foundry çalıştırın
services: virtual-machines-linux
documentationcenter: ''
author: seanmck
manager: jeconnoc
editor: ''
tags: ''
keywords: ''
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 42910675bcf512a3d6c76369adc9f41215420a78
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187768"
---
# <a name="cloud-foundry-on-azure"></a>Azure’da Cloud Foundry

Bulut Foundry bir açık kaynak platform olarak-hizmet (PaaS) oluşturmak, dağıtmak ve çeşitli dilleri ve çerçeveleri geliştirilen uygulamaları 12 faktör işletim ' dir. Bu belgede Azure ve size nasıl başlayabiliriz bulut Foundry çalıştırmak için sahip olduğunuz seçenekler açıklanmaktadır.

## <a name="cloud-foundry-offerings"></a>Bulut Foundry teklifleri

Azure üzerinde çalıştırmak için kullanılabilir bulut Foundry iki tür vardır: açık kaynak bulut Foundry (OSS CF) ve Bileşendirler bulut Foundry (PCF). OSS CF olan bir tamamen [açık kaynak](https://github.com/cloudfoundry) bulut Foundry sürümü bulut Foundry Foundation tarafından yönetilir. Bir kurumsal dağıtım bulut Foundry Bileşendirler yazılım Inc., bileşendirler bulut Foundry Bazı iki teklifleri arasındaki farklar arayın.

### <a name="open-source-cloud-foundry"></a>Açık kaynak bulut Foundry

Azure üzerinde OSS bulut Foundry BOSH yönetmenin ilk dağıtma ve bulut Foundry dağıtma dağıtabilirsiniz kullanarak [Github'da sağlanan yönergeleri](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). OSS CF kullanma hakkında daha fazla bilgi edinmek için [belgelerine](https://docs.cloudfoundry.org/) bulut Foundry Foundation tarafından sağlanan.

Microsoft aşağıdaki topluluk kanallar aracılığıyla OSS CF için en yüksek çaba destek sağlar:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>bosh azure CPI kanalda [bulut Foundry kayma](https://slack.cloudfoundry.org/)
- [cf bosh posta listesi](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub sorunlar için [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) ve [hizmet Aracısı](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Bulut Foundry çalıştırdığı sanal makineler gibi Azure kaynaklarınızı desteği düzeyini Azure destek sözleşmenizi temel alır. En yüksek çaba topluluk desteği yalnızca bulut Foundry özgü bileşenleri için geçerlidir.

### <a name="pivotal-cloud-foundry"></a>Bileşendirler bulut Foundry

Bileşendirler bulut Foundry OSS dağıtımı, bir dizi özel yönetim araçları ve kurumsal destek birlikte aynı çekirdek platformu içerir. Azure üzerinde PCF çalıştırmak için Pivotal lisanstan edinmeniz gerekir. Azure Marketi'nden PCF teklif 90 günlük deneme lisansı içerir.

Araçlarda [Bileşendirler Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), dağıtım ve bir bulut Foundry temel yönetimini basitleştiren bir web uygulaması ve [Bileşendirler uygulamaları Yöneticisi](https://docs.pivotal.io/pivotalcf/console/), kullanıcılar ve uygulamalar yönetmek için bir web uygulaması.

Yukarıdaki OSS CF için listelenen destek kanallarını yanı sıra PCF lisans, Pivotal desteğine başvurun hakkı verir. Ayrıca, Yardım için her iki taraf başvurun ve sorunu burada arasındadır bağlı olarak uygun şekilde yönlendirilmesini, soru izin destek iş akışları Microsoft ve Pivotal etkinleştirdiniz.

## <a name="azure-service-broker"></a>Azure hizmet Aracısı

Bulut Foundry teşvik eden ["on iki öğeli uygulama"](https://12factor.net/) temiz ayrılması durum bilgisiz uygulama işlemleri ve durum bilgisi olan hizmetler yedekleme yükseltir Metodoloji. [Hizmet aracıların](https://docs.cloudfoundry.org/services/api.html) sağlamak ve uygulamaları destekleyen hizmetlere bağlanmak için tutarlı bir yol sunar. [Azure hizmet Aracısı](https://github.com/Azure/meta-azure-service-broker) bazı Azure depolama ve Azure SQL dahil olmak üzere bu kanal üzerinden anahtar Azure hizmetleri sağlar.

Hizmet Aracısı Bileşendirler bulut Foundry kullanıyorsanız, aynı zamanda olan [kullanılabilir bir kutucuk olarak](https://docs.pivotal.io/azure-sb/installing.html) Bileşendirler ağdan.

## <a name="related-resources"></a>İlgili kaynaklar

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services eklentisi

Bulut Foundry sürekli tümleştirme (CI) ve kesintisiz teslim (CD) kullanımını da dahil olmak üzere Çevik Yazılım Geliştirme için uygundur. Projelerinizi yönetmek için Visual Studio Team Services (VSTS) kullanın ve ayarlamak istiyorsanız bulut Foundry hedefleme CI/CD kanal, kullanabileceğiniz [VSTS bulut Foundry yapı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). Eklenti Azure veya başka bir çalışan olup olmadığını yapılandırın ve bulut Foundry dağıtımları otomatikleştirmek basit kolaylaştırır ortamı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Marketi'nden Bileşendirler bulut Foundry dağıtma](https://azure.microsoft.com/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Foundry Azure içinde bulut için bir uygulama dağıtma](./cloudfoundry-deploy-your-first-app.md)
