---
title: Microsoft azure'da Cloud Foundry ile çalışmaya başlama | Microsoft Docs
description: Microsoft Azure üzerinde OSS veya Pivotal Cloud Foundry çalıştırın
services: virtual-machines-linux
documentationcenter: ''
author: seanmck
manager: gwallace
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
ms.openlocfilehash: e042c9cbce985882b468472425d6803862e82941
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668321"
---
# <a name="cloud-foundry-on-azure"></a>Azure’da Cloud Foundry

Cloud Foundry, çeşitli dillerde ve çerçevelerde geliştirilen 12 faktör uygulamaları oluşturmaya, dağıtmaya ve işletmeye yönelik açık kaynak bir hizmet olarak platformdur (PaaS). Bu belge, Azure ile nasıl başlangıç yapabileceğinizi üzerinde Cloud Foundry çalıştırmak için sahip olduğunuz seçenekler açıklanmaktadır.

## <a name="cloud-foundry-offerings"></a>Cloud Foundry teklifleri

Cloud Foundry, Azure üzerinde çalıştırmak kullanılabilir iki tür vardır: açık kaynaklı Cloud Foundry (OSS CF) ve Pivotal Cloud Foundry (PCF). OSS CF olduğu bir tamamen [açık kaynaklı](https://github.com/cloudfoundry) Cloud Foundry sürümünü Cloud Foundry Vakfı'tarafından yönetilir. Bir kurumsal dağıtım yazılım Inc. Pivotal Cloud Foundry, Pivotal Cloud Foundry İki teklifleri arasındaki farklar bazılarına bakacağız.

### <a name="open-source-cloud-foundry"></a>Açık kaynaklı Cloud Foundry

Azure'da Cloud Foundry OSS BOSH yönetmenin ilk dağıtma ve ardından, Cloud Foundry dağıtma dağıtabilirsiniz kullanarak [Github'da sağlanan yönergeleri](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). OSS CF kullanma hakkında daha fazla bilgi edinmek için [belgeleri](https://docs.cloudfoundry.org/) Cloud Foundry Vakfı'tarafından sağlanan.

Microsoft, aşağıdaki topluluk kanalları aracılığıyla OSS CF için mümkün olan en iyi destek sağlar:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>bosh azure CPI kanalda [Cloud Foundry Slack](https://slack.cloudfoundry.org/)
- [cf bosh posta listesi](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub sorunları için [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) ve [hizmet Aracısı](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Azure destek sözleşmenizi, Cloud Foundry, çalıştırdığınız sanal makineler gibi Azure kaynakları için destek düzeyini temel alır. En yüksek çaba topluluk desteği yalnızca, Cloud Foundry özgü bileşenler için de geçerlidir.

### <a name="pivotal-cloud-foundry"></a>Pivotal Cloud Foundry

Birtakım özel yönetim araçları ve kurumsal destek birlikte OSS dağıtım olarak aynı temel platform Pivotal Cloud Foundry içerir. PCF Azure'da çalıştırmak için Pivotal lisanstan edinmeniz gerekir. PCF teklifi Azure Marketi aracılığıyla bir 90 günlük deneme lisansı içerir.

Araçlarda [Pivotal Operations Manager](https://docs.pivotal.io/pivotalcf/customizing/), dağıtım ve Cloud Foundry foundation yönetimini basitleştiren bir web uygulaması ve [Pivotal uygulamaları Manager](https://docs.pivotal.io/pivotalcf/console/), yönetmek için bir web uygulaması Kullanıcılar ve uygulamalar.

Yukarıdaki OSS CF için listelenen destek kanallarına ek olarak, PCF lisans Pivotal desteği ile iletişime geçmenizi sağlar. Ayrıca, Microsoft ve Pivotal, Yardım için her iki taraf da başvurun ve sorunu nereden kaynaklandığını bağlı olarak uygun şekilde yönlendirilmesini sorgunuz izin destek iş akışları etkinleştirdiniz.

## <a name="azure-service-broker"></a>Azure hizmet Aracısı

Cloud Foundry teşvik eder ["on iki Faktörlü uygulama"](https://12factor.net/) durum bilgisi olmayan uygulama işlemleri ve durum bilgisi olan hizmetler yedekleme ayrılmasına yükseltir Metodoloji. [Hizmet aracıları](https://docs.cloudfoundry.org/services/api.html) sağlayın ve çok sayıda destekleyici hizmetten uygulamaları bağlamak için tutarlı bir yol sunar. [Azure hizmet Aracısı](https://github.com/Azure/meta-azure-service-broker) bazı temel Azure hizmetlerini Azure depolama ve Azure SQL dahil olmak üzere, bu kanal yoluyla sağlar.

Hizmet Aracısı Pivotal Cloud Foundry kullanıyorsanız, aynı zamanda olan [bir kutucuk olarak kullanılabilir](https://docs.pivotal.io/azure-sb/installing.html) Pivotal ağdan.

## <a name="related-resources"></a>İlgili kaynaklar

### <a name="azure-devops-services-plugin"></a>Azure DevOps Services eklentisi

Cloud Foundry sürekli tümleştirme (CI) ve sürekli teslim (CD) kullanımı dahil olmak üzere, Çevik Yazılım Geliştirme için oldukça uygundur. Azure DevOps hizmetleri yönetmek için kullanmak ve ayarlamak istediğiniz Cloud Foundry hedefleyen bir CI/CD işlem hattı, kullanabileceğiniz [Azure DevOps Hizmetleri Cloud Foundry derleme uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). Eklenti Azure'da veya başka bir çalışan olup olmadığını yapılandırın ve Cloud Foundry, dağıtımları otomatik hale getirmek kolaylaştırır ortam.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Market'ten Pivotal Cloud Foundry dağıtma](https://azure.microsoft.com/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Cloud Foundry, Azure için uygulama dağıtma](./cloudfoundry-deploy-your-first-app.md)
