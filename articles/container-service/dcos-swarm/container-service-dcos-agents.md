---
title: (KULLANIM DIŞI) Azure Container Service için DC/OS aracı havuzları
description: Genel ve özel aracı havuzları ile bir Azure Container Service DC/OS kümesi nasıl çalışır?
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 01/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 03cacda1aa405cb2d0ded579c8ddb5f6011ce3bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60478455"
---
# <a name="deprecated-dcos-agent-pools-for-azure-container-service"></a>(KULLANIM DIŞI) Azure Container Service için DC/OS aracı havuzları

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Azure Container Service'te DC/OS kümelerini iki havuz, genel bir havuz ve özel bir havuz içindeki aracı düğümlerinin içerir. Bir uygulama, kapsayıcı hizmetinizde makineler arasında erişilebilirlik etkileyen iki havuzuna dağıtılabilir. Makineleri internet'e (ortak) açık veya dahili (özel) tutulur. Bu makalede, genel ve özel havuzların yüzden kısa bir genel bakış sağlar.


* **Özel aracılar**: Özel aracı düğümleri yönlendirilemeyen bir ağ üzerinden çalıştırın. Bu ağ, yalnızca yönetim bölgesi veya bölge genel uç yönlendiricisinde aracılığıyla erişilebilir. Varsayılan olarak, DC/OS, uygulamaları özel aracı düğümlerinde başlatır. 

* **Ortak**: Genel aracı düğümleri, DC/OS uygulamaları ve Hizmetleri ortak olarak erişilebilen bir ağ üzerinden çalıştırın. 

DC/OS ağ güvenliği hakkında daha fazla bilgi için bkz: [DC/OS belgelerine](https://docs.mesosphere.com/).

## <a name="deploy-agent-pools"></a>Aracı havuzları dağıtma

Azure kapsayıcı hizmeti DC/OS aracı havuzları şu şekilde oluşturulur:

* **Özel havuzu** ne zaman belirtin aracı düğüm sayısını içerir, [DC/OS kümesi dağıtma](container-service-deployment.md). 

* **Genel havuzunu** başlangıçta önceden belirlenen sayıda aracı düğümleri içerir. DC/OS kümesi sağlandığında Bu havuz otomatik olarak eklenir.

Azure sanal makine ölçek kümeleri özel bir havuz ve genel havuzunu var. Dağıtımdan sonra bu havuzlarını yeniden boyutlandırabilirsiniz.

## <a name="use-agent-pools"></a>Aracı havuzları kullanın
Varsayılan olarak, **Marathon** yeni bir uygulamayı dağıtır *özel* aracı düğümleri. Uygulamayı açıkça dağıtmak zorunda *genel* uygulamanın oluşturulması sırasında düğümleri. Seçin **isteğe bağlı** girin ve sekme **slave_public** için **kabul edilen kaynak rolleri** değeri. Bu işlem belgelenen [burada](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ve [DC/OS](https://docs.mesosphere.com/1.7/administration/installing/oss/custom/create-public-agent/) belgeleri.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md).

* Bilgi edinmek için nasıl [güvenlik duvarını açmak](container-service-enable-public-access.md) DC/OS kapsayıcılarınızı genel erişime izin vermek için Azure tarafından sağlanan.

