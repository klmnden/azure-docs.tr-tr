---
title: Azure Cloud Services nedir | Microsoft Docs
description: Azure Cloud Services nedir hakkında bilgi edinin.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: ce88dcaedf32f293fc121cda2a088388c99badee
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60337531"
---
# <a name="overview-of-azure-cloud-services"></a>Azure bulut hizmetlerine genel bakış
Azure Cloud Services, örneği bir [bir hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Gibi [Azure App Service](../app-service/overview.md), bu teknoloji, ölçeklenebilir, güvenilir ve uygun maliyetli uygulamaları desteklemek için tasarlanmıştır. App Service sanal makinelerinde (VM'ler), bu nedenle barındırıldığını aynı şekilde Azure Cloud Services çok uzun. Ancak, sanal makineleri hakkında daha fazla denetime sahip. Azure Cloud Services kullanan Vm'lerde kendi yazılım yükleyebilirsiniz ve Uzaktan erişim.

![Azure bulut Hizmetleri diyagramı](./media/cloud-services-choose-me/diagram.png)

Daha fazla denetim aynı zamanda daha az kullanım kolaylığı anlamına gelir. Ek denetimi seçenekleri gerekmedikçe, genellikle daha hızlı ve kolay bir web uygulamasını almak ve Web Apps çalıştıran özelliği App Service, Azure Cloud Services'a kıyasla.

Azure Cloud Services rolleri iki tür vardır. İkisi arasındaki tek fark, rolünüzün Vm'lerinde barındırılan nasıl şöyledir:

* **Web rolü**: Otomatik olarak dağıtır ve uygulamanızı IIS üzerinden barındırır.

* **Çalışan rolü**: IIS kullanmaz ve, uygulama başına çalıştırır.

Örneğin, basit bir uygulama yalnızca bir tek bir web rolü, hizmet veren bir Web sitesi kullanabilirsiniz. Daha karmaşık bir uygulama kullanıcılardan gelen istekleri işlemek için bir web rolü kullanın ve ardından bu istekleri işlemek için bir çalışan rolü geçirin. (Bu iletişim kullanabilir [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) veya [Azure kuyruk depolama](../storage/common/storage-introduction.md).)

Önceki şekilde anlaşılacağı gibi tek bir uygulamanın tüm sanal makineler aynı bulut hizmetinde çalıştırın. Uygulamanın sanal makinelerde kullanıcılara erişim tek bir genel IP adresi, isteklerle üzerinden uygulama otomatik olarak yük dengeli. Platform [dağıtır ve ölçekler](cloud-services-how-to-scale-portal.md) Vm'leri bir Azure Cloud Services uygulamasında bir şekilde tek bir donanım hata noktası sorununu önler.

Sanal uygulamaları çalıştırmak olsa da, Azure Cloud Services, PaaS, altyapı (ıaas) olarak değil sağladığını anlamak önemlidir. Hakkında düşünmek yöntemlerinden biri aşağıda verilmiştir. Iaas, gibi Azure sanal makineler ile ilk oluşturur ve uygulamanızın çalıştığı ortamı yapılandırın. Bu ortama uygulamanızı dağıttıktan sonra. Bu dünyanın hemen her VM'deki işletim sistemi düzeltme eki uygulama yeni sürümlerini dağıtma gibi şeyler yaparak yönetmekten sorumlu. Paas'ta aksine, bu ortam zaten var gibi olur. Yapmanız gereken tek şey uygulamanızı dağıtın. Bu yeni sürümleri işletim sistemi dağıtımı dahil olmak üzere çalışan platform Yönetimi sizin yerinize gerçekleştirilir.

## <a name="scaling-and-management"></a>Ölçeklendirme ve yönetim
Azure Cloud Services, sanal makineler oluşturmayın. Bunun yerine, her ne kadar "üç web rolü örnekleri" ve "iki çalışan rolü örnekleri." gibi istediğiniz Azure söyleyen bir yapılandırma dosyası sağlayın Platforma daha sonra bunları sizin için oluşturur. Hala seçtiğiniz [boyutu](cloud-services-sizes-specs.md) bu Vm'leri yedekleme olmalıdır, ancak açıkça bunları kendiniz oluşturursunuz yok. Uygulamanızın büyük bir yükü işlemek gerekiyorsa daha fazla sanal makine için isteyebilir ve bu örneklere Azure oluşturur. Yükünü azaltır, bu örnekleri kapatacak ve bunlar için ödeme durdurun.

Bir Azure Cloud Services uygulamasına genellikle iki adımlı bir işlem ile kullanıcılar için kullanılabilir hale getirilir. Bir geliştiricinin ilk [uygulama yüklemeleri](cloud-services-how-to-create-deploy-portal.md) platformun hazırlama alanına. Geliştirici için hazır olduğunda yapma uygulama canlı, bunlar Azure portalını kullanarak üretime hazırlama. Bu [hazırlama ve üretim arasında geçiş](cloud-services-how-to-manage-portal.md#swap-deployments-to-promote-a-staged-deployment-to-production) kullanıcılarına bozmadan yeni sürüme yükseltilmesi çalışan bir uygulama sağlar kapalı kalma süresi ile yapılabilir.

## <a name="monitoring"></a>İzleme
Azure bulut Hizmetleri, izleme de sağlar. Sanal makineler gibi başarısız bir fiziksel sunucu algılar ve bu sunucuda yeni bir makine üzerinde çalışan Vm'leri yeniden başlatır. Ancak, Azure bulut Hizmetleri başarısız Vm'lerinizi ve uygulamalarınızı, yalnızca donanım hataları algılar. Sanal makineleri farklı bir aracı her web ve çalışan rolü içinde vardır ve böylece hatalar oluştuğunda yeni VM'ler ve uygulama örnekleri başlatabilirsiniz.

Azure Cloud Services PaaS yapısını çok diğer etkileri vardır. En önemli uygulamaları bu teknolojiyi tüm web veya çalışan rolü örneği başarısız olduğunda düzgün çalışması için mesajlarının biridir. Bunu başarmak için bir Azure Cloud Services uygulamasına kendi sanal dosya sisteminde durumunu korumak olmamalıdır. Sanal makineler ile oluşturulan Vm'lerden farklı olarak, Azure bulut Hizmetleri sanal makinelerine yapılan yazma işlemleri kalıcı değildir. Sanal makine veri diski gibi hiçbir şey yoktur. Bunun yerine, bir Azure Cloud Services uygulamasına açıkça tüm durumu Azure SQL veritabanı, BLOB'lar, tablolar veya başka bir dış depolama yazmanız gerekir. Bu şekilde uygulamaları oluşturmaya bunları ölçek kolaylaştırır ve hataya daha dayanıklı, Azure bulut hizmetlerinin her iki önemli hedefleri olan.

## <a name="next-steps"></a>Sonraki adımlar
* [. NET'te bir bulut hizmeti uygulaması oluşturma](cloud-services-dotnet-get-started.md) 
* [Node.js'de bir bulut hizmeti uygulaması oluşturma](cloud-services-nodejs-develop-deploy-app.md) 
* [PHP'de bir bulut hizmeti uygulaması oluşturma](../cloud-services-php-create-web-role.md) 
* [Python'da bir bulut hizmeti uygulaması oluşturma](cloud-services-python-ptvs.md)



