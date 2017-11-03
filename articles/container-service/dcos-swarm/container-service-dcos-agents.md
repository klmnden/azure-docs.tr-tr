---
title: "Azure kapsayıcı hizmeti DC/OS Aracısı havuzlarında | Microsoft Docs"
description: "Ortak ve özel aracı havuzları, Azure kapsayıcı hizmeti DC/OS kümesi ile nasıl çalışır?"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kapsayıcılar, Mikro hizmetler, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: da4a196b1a73c78dfff7d8310edcc349b8d10665
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>Azure kapsayıcı hizmeti DC/OS aracısı havuzları
Azure kapsayıcı hizmeti DC/OS kümelerde iki havuzları, ortak bir havuzu ve özel bir havuzu Aracısı düğümleri içerir. Bir uygulama ya da havuzuna kapsayıcı hizmetiniz makinelerinizde arasında erişilebilirlik etkileyen dağıtılabilir. Makineler (Genel) Internet'e açık veya dahili (özel) tutulur. Bu makalede vardır neden ortak ve özel havuzları kısa bir genel bakış sağlar.


* **Özel aracılar**: özel aracı düğümleri yönlendirilemeyen bir ağ üzerinden çalıştırın. Bu ağ, yalnızca yönetici bölgeden veya genel bölge sınır yönlendiricisi aracılığıyla erişilebilir. Varsayılan olarak, DC/OS özel aracı düğümlerde uygulamaları başlatır. 

* **Ortak aracıları**: ortak aracı düğümleri, genel olarak erişilebilir bir ağ üzerinden DC/OS uygulamaları ve Hizmetleri çalıştırın. 

DC/OS ağ güvenliği hakkında daha fazla bilgi için bkz: [DC/OS belgelerine](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Aracı havuzu dağıtma

Azure kapsayıcı hizmeti DC/OS aracısı havuzları şu şekilde oluşturulur:

* **Özel havuzu** ne zaman belirtin Aracısı düğüm sayısını içerir, [DC/OS kümesi dağıtma](container-service-deployment.md). 

* **Ortak havuzu** başlangıçta Aracısı düğümleri sayısı önceden belirlenmiştir içerir. DC/OS kümesi sağlandığında bu havuzu otomatik olarak eklenir.

Özel havuzu ve ortak havuzu Azure sanal makine ölçek kümeleridir. Dağıtımdan sonra bu havuzlarını yeniden boyutlandırabilirsiniz.

## <a name="use-agent-pools"></a>Aracı havuzlarını kullanın
Varsayılan olarak, **Marathon** herhangi yeni bir uygulama dağıtır *özel* Aracısı düğümleri. Açıkça uygulamayı dağıtmak zorunda *ortak* uygulama oluşturulması sırasında düğüm. Seçin **isteğe bağlı** sekmesinde ve girin **slave_public** için **kabul edilen kaynak rolleri** değeri. Bu işlem belgelenen [burada](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ve [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) belgeleri.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md).

* Bilgi nasıl [Güvenlik Duvarı](container-service-enable-public-access.md) DC/OS kapsayıcılarınızı ortak erişmesine izin vermek için Azure tarafından sağlanan.

