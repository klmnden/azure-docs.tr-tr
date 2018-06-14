---
title: Azure işlem seçenekleri - Azure Cloud Services | Microsoft Docs
description: 'Azure işlem barındırma seçenekleri ve nasıl çalıştıklarını hakkında bilgi edinin: App Service, Azure bulut Hizmetleri ve sanal makineler'
services: cloud-services
documentationcenter: ''
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 2871a8c02db0ffc6d9033724e7c9f4a454afef8e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29120293"
---
# <a name="should-i-choose-azure-cloud-services-or-something-else"></a>Azure bulut Hizmetleri veya başka bir şey seçmem gerekir?
Azure Cloud Services seçimi için mi? Azure uygulamalarını çalıştırmak için farklı barındırma modelleri sağlar. Her biri farklı bir hizmetler kümesini sağlar. Seçtiğiniz hangisinin tam olarak ne, yapmak çalıştığınız bağlıdır.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-azure-cloud-services"></a>Azure Cloud Services hakkında bilgi ver
Azure bulut Hizmetleri örneğidir bir [hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Gibi [Azure App Service](../app-service/app-service-web-overview.md), bu teknoloji, ölçeklenebilir, güvenilir ve uygun maliyetli çalışması olan uygulamaları destekleyecek şekilde tasarlanmıştır. App Service sanal makineleri (VM'ler), bu nedenle barındırılıyor aynı şekilde Azure Cloud Services çok uzun. Ancak, sanal makineleri daha fazla denetime sahip olursunuz. Azure bulut hizmetlerini kullanan sanal makineler kendi yazılım yükleyebilir ve Uzaktan erişim.

![Azure bulut Hizmetleri diyagramı](./media/cloud-services-choose-me/diagram.png)

Daha fazla denetim ayrıca daha az kullanım kolaylığı anlamına gelir. Ek denetimi seçenekleri gerekmedikçe genellikle daha hızlı ve kolay bir web uygulaması alınacağı ve Web uygulamaları çalıştıran uygulama hizmeti özelliğidir Azure bulut hizmetlerine karşılaştırılır.

Azure Cloud Services rolleri iki tür vardır. İkisi arasındaki tek fark, rolünüze sanal makinelerin nasıl barındırılan şöyledir:

* **Web rolü**: otomatik olarak dağıtır ve uygulamanızı IIS üzerinden barındırır.

* **Çalışan rolü**: IIS kullanmaz ve uygulama bağımsız çalıştırır.

Örneğin, bir basit uygulama yalnızca bir tek bir web rolü, bir Web sitesi hizmet veren kullanabilir. Daha karmaşık bir uygulama kullanıcılardan gelen istekleri işlemek için bir web rolü kullanır ve bu istekleri işlemek için bir çalışan rolü açın geçirmek. (Bu iletişim kullanabilir [Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) veya [Azure kuyruk depolama](../storage/common/storage-introduction.md).)

Yukarıdaki şekil da anlaşılacağı gibi tek bir uygulamada tüm sanal makineleri aynı bulut hizmetinde çalıştırın. Tek bir ortak IP adresi, istekte bulunan üzerinden uygulama otomatik olarak yüklenmesini kullanıcılara erişim uygulamanın VM'ler arasında dengeli. Platform [ölçeklendirir ve dağıtır](cloud-services-how-to-scale-portal.md) tek bir donanım hatası noktası önler şekilde Azure Cloud Services uygulamada VM'ler.

Vm'lerde uygulamaları çalıştırmak olsa da, Azure Cloud Services PaaS, değil (Iaas) hizmet olarak altyapı sağladığını anlamak önemlidir. İşte hakkında düşünmek için bir yol. Iaas, gibi Azure sanal makineleri ile ilk oluşturun ve uygulamanızın çalıştığı ortamı yapılandırın. Bu ortam uygulamanıza dağıtırsınız. Bu world çoğunu her VM işletim sistemi düzeltme eki yeni sürümlerini dağıtma gibi şeyler yaparak yönetmekten sorumlu. PaaS içinde aksine, bu ortamın zaten gibi olur. Yapmanız gereken tek şey uygulamanızı dağıtmak. Yönetim, işletim sistemi yeni sürümlerini dağıtma dahil olmak üzere çalıştırdığı platformun sizin için işlenir.

## <a name="scaling-and-management"></a>Ölçeklendirme ve yönetim
Azure bulut Hizmetleri ile sanal makineleri oluşturmayın. Bunun yerine, her kaç, "üç web rolü örneklerini" ve "iki çalışan rolü örneklerine." gibi istediğiniz Azure söyleyen bir yapılandırma dosyası girin Platformu daha sonra bunları sizin için oluşturur. Hala seçtiğiniz [boyutu](cloud-services-sizes-specs.md) bu sanal makineleri yedekleme olmalı, ancak siz açıkça bunları kendiniz oluşturmayın. Uygulamanızı bir yükü işlemek gerekirse, daha fazla VM'ler için isteyebilir ve bu örnekleri Azure oluşturur. Yükü azaltır, bu örneklerde kapatın ve bunlar için ödeme durdurun.

Bir Azure Cloud Services uygulama genellikle iki adımlı bir işlem aracılığıyla kullanıcılara kullanılabilir hale getirilir. Bir geliştirici ilk [uygulamayı yükler](cloud-services-how-to-create-deploy-portal.md) platformun hazırlama alanına. Geliştirici hazır olduğunda yapma uygulama dinamik olarak değiştirmek için Azure portal kullanırlar üretime hazırlama. Bu [hazırlama ve üretim arasında geçiş yapma](cloud-services-how-to-manage-portal.md#swap-deployments-to-promote-a-staged-deployment-to-production) kullanıcılarına etkilemeden yeni bir sürüme yükseltilmesi çalışan bir uygulama sağlar kapalı kalma süresi olmadan yapılabilir.

## <a name="monitoring"></a>İzleme
Azure bulut hizmetleri de izleme sağlar. Sanal makineler gibi başarısız bir fiziksel sunucuda algılar ve yeni bir makine, bir sunucuda çalışan sanal makineleri yeniden başlatır. Ancak Azure Cloud Services başarısız VM'ler ve uygulamalar, yalnızca donanım hataları algılar. Sanal makineler aksine her web ve çalışan rolü içindeki bir aracıya sahip ve bu nedenle hatalar oluştuğunda yeni VM'ler ve uygulama örnekleri başlatılamaz.

Azure Cloud Services PaaS yapısını çok diğer etkilere sahiptir. En önemli uygulamaları bu teknolojisini temel web veya çalışan rolü örneklerine başarısız olduğunda doğru çalışması için yazılması gereken biridir. Bunun için Azure Cloud Services uygulamanın kendi sanal makineleri, dosya sistemindeki durumunu korumak döndürmemelidir. Sanal makineler ile oluşturulan VM'ler farklı olarak, Azure bulut Hizmetleri VM'ler için yapılan yazma kalıcı değil. Hiçbir şey bir sanal makine veri diski gibi yoktur. Bunun yerine, bir Azure Cloud Services uygulama açıkça tüm durum Azure SQL Database, BLOB'lar, tablolar veya başka bir harici depolama yazmanız gerekir. Bu şekilde uygulamaları derleme bunları ölçek kolaylaştırır ve daha başarısızlık, hangi Azure bulut Hizmetleri, her iki önemli hedeflerini dayanıklıdır.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET içinde bir bulut hizmeti uygulaması oluşturma](cloud-services-dotnet-get-started.md) 
* [Node.js içinde bir bulut hizmeti uygulaması oluşturma](cloud-services-nodejs-develop-deploy-app.md) 
* [PHP ile bir bulut hizmeti uygulaması oluşturma](../cloud-services-php-create-web-role.md) 
* [Python içinde bir bulut hizmeti uygulaması oluşturma](cloud-services-python-ptvs.md)



