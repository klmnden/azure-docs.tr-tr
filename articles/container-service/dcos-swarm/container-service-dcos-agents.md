---
title: Azure Container Service için DC/OS aracı havuzları
description: Genel ve özel aracı havuzları ile bir Azure Container Service DC/OS kümesi nasıl çalışır?
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 01/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 17029f51be9fed8fc36c5f919ece84acbf0461d9
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "42055384"
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>Azure Container Service için DC/OS aracı havuzları
Azure Container Service'te DC/OS kümelerini iki havuz, genel bir havuz ve özel bir havuz içindeki aracı düğümlerinin içerir. Bir uygulama, kapsayıcı hizmetinizde makineler arasında erişilebilirlik etkileyen iki havuzuna dağıtılabilir. Makineleri internet'e (ortak) açık veya dahili (özel) tutulur. Bu makalede, genel ve özel havuzların yüzden kısa bir genel bakış sağlar.


* **Özel aracılar**: özel aracı düğümleri yönlendirilemeyen bir ağ üzerinden çalıştırın. Bu ağ, yalnızca yönetim bölgesi veya bölge genel uç yönlendiricisinde aracılığıyla erişilebilir. Varsayılan olarak, DC/OS, uygulamaları özel aracı düğümlerinde başlatır. 

* **Ortak**: Genel aracı düğümleri DC/OS uygulamaları ve Hizmetleri ortak olarak erişilebilen bir ağ üzerinden çalıştırın. 

DC/OS ağ güvenliği hakkında daha fazla bilgi için bkz: [DC/OS belgelerine](https://dcos.io/docs/1.8/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Aracı havuzları dağıtma

Azure kapsayıcı hizmeti DC/OS aracı havuzları şu şekilde oluşturulur:

* **Özel havuzu** ne zaman belirtin aracı düğüm sayısını içerir, [DC/OS kümesi dağıtma](container-service-deployment.md). 

* **Genel havuzunu** başlangıçta önceden belirlenen sayıda aracı düğümleri içerir. DC/OS kümesi sağlandığında Bu havuz otomatik olarak eklenir.

Azure sanal makine ölçek kümeleri özel bir havuz ve genel havuzunu var. Dağıtımdan sonra bu havuzlarını yeniden boyutlandırabilirsiniz.

## <a name="use-agent-pools"></a>Aracı havuzları kullanın
Varsayılan olarak, **Marathon** yeni bir uygulamayı dağıtır *özel* aracı düğümleri. Uygulamayı açıkça dağıtmak zorunda *genel* uygulamanın oluşturulması sırasında düğümleri. Seçin **isteğe bağlı** girin ve sekme **slave_public** için **kabul edilen kaynak rolleri** değeri. Bu işlem belgelenen [burada](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ve [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) belgeleri.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md).

* Bilgi edinmek için nasıl [güvenlik duvarını açmak](container-service-enable-public-access.md) DC/OS kapsayıcılarınızı genel erişime izin vermek için Azure tarafından sağlanan.

