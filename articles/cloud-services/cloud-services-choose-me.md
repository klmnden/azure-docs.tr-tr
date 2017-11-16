---
title: "Azure işlem seçenekleri - bulut Hizmetleri | Microsoft Docs"
description: "Azure işlem barındırma seçenekleri ve nasıl çalıştıklarını hakkında bilgi edinin: App Service, Cloud Services ve sanal makineler"
services: cloud-services
documentationcenter: 
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
ms.openlocfilehash: d27a4be968dc12818f7031b59ed40fbc9f9d88d3
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Bulut Hizmetleri veya başka bir şey seçmem gerekir?
Azure Cloud Services seçimi için mi? Azure uygulamalarını çalıştırmak için farklı barındırma modelleri sağlar. Seçtiğiniz hangisinin tam olarak ne, yapmak çalıştığınız dolayısıyla her biri farklı bir hizmetler kümesini sağlar.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>Bulut hizmetleri hakkında bilgi ver
Bulut Hizmetleri örneğidir [hizmet olarak Platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Gibi [uygulama hizmeti](../app-service/app-service-web-overview.md), bu teknoloji, ölçeklenebilir, güvenilir ve çalışmaya ucuz uygulamaları desteklemek üzere tasarlanmıştır. Yalnızca bir uygulama hizmeti Vm'lerinde, bu nedenle barındırılan gibi çok bulut Hizmetleri olan ancak VM'ler daha fazla denetime sahip olursunuz. Bulut hizmeti Vm'lerinde kendi yazılım yükleyebilir ve uzak içine kullanabilirsiniz.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Daha fazla denetim ayrıca daha az kullanım kolaylığı anlamına gelir. Ek denetimi seçenekleri gerekmedikçe daha hızlı ve kolay bir web uygulaması alınacağı genellikle ve App Service'te Web uygulamalarını çalışan bulut hizmetlerine karşılaştırılır.

Bulut hizmeti rolleriyle iki tür vardır. İkisi arasındaki tek fark, rolünüze sanal makineye nasıl barındırılan ' dir.

* **Web rolü**  
Otomatik olarak dağıtır ve uygulamanızı IIS üzerinden barındırır.

* **Çalışan rolü**  
IIS kullanmaz ve uygulama bağımsız çalıştırır.

Örneğin, bir basit uygulama yalnızca bir tek bir web rolü, bir Web sitesi hizmet veren kullanabilir. Daha karmaşık bir uygulama bir web rolü kullanıcılardan gelen istekleri işleyen ve bu istekleri işlemek için bir çalışan rolü açın geçirmek için kullanabilirsiniz. (Bu iletişim kullanmayı [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) veya [Azure kuyrukları](../storage/common/storage-introduction.md).)

Yukarıdaki şekil da anlaşılacağı gibi tek bir uygulamada tüm sanal makineleri aynı bulut hizmetinde çalıştırın. Tek bir ortak IP adresi, istekte bulunan üzerinden uygulama otomatik olarak yüklenmesini kullanıcılara erişim uygulamanın VM'ler arasında dengeli. Platform [ölçeklendirir ve dağıtır](cloud-services-how-to-scale-portal.md) tek bir donanım hatası noktası önler şekilde bulut Hizmetleri uygulamasında VM'ler.

Uygulamaları sanal makinelerinde çalışan olsa bile, bulut Hizmetleri PaaS, değil Iaas sağladığını anlamak önemlidir. Hakkında düşünmek için bir yol da şudur: Iaas, gibi Azure sanal makineleri ile ilk oluşturmak ve uygulamanızın çalıştığı ortamı yapılandırın, ardından uygulamanız bu ortamına dağıtma. İşletim sistemi her VM için düzeltme eki yeni sürümlerini dağıtma gibi işlemler, bu world çoğunu yönetmekten sorumlu. PaaS içinde aksine, bu ortamın zaten gibi olur. Yapmanız gereken tek şey uygulamanızı dağıtmak. Yönetim, işletim sistemi yeni sürümlerini dağıtma dahil olmak üzere çalıştırdığı platformun sizin için işlenir.

## <a name="scaling-and-management"></a>Ölçeklendirme ve yönetim
Bulut Hizmetleri ile sanal makineleri oluşturmayın. Bunun yerine, her kaç, gibi istediğiniz Azure söyleyen bir yapılandırma dosyası girin **üç rol örnekleri web** ve **iki çalışan rolü örnekleri**, ve platform bunları sizin için oluşturur.  Hala seçtiğiniz [boyutu](cloud-services-sizes-specs.md) bu sanal makineleri yedekleme olmalı, ancak siz açıkça bunları kendiniz oluşturmayın. Uygulamanızı bir yükü işlemek gerekirse, daha fazla VM'ler için isteyebilir ve bu örnekleri Azure oluşturur. Yükü azaltır, bu örneklerde kapatın ve bunlar için ödeme durdurun.

Bulut Hizmetleri uygulaması genellikle iki adımlı bir işlem aracılığıyla kullanıcılara kullanılabilir hale getirilir. Bir geliştirici ilk [uygulamayı yükler](cloud-services-how-to-create-deploy-portal.md) platformun hazırlama alanına. Geliştirici hazır olduğunda yapma uygulama dinamik olarak değiştirmek için Azure portal kullanırlar üretime hazırlama. Bu [hazırlama ve üretim arasında geçiş yapma](cloud-services-nodejs-stage-application.md) kullanıcılarına etkilemeden yeni bir sürüme yükseltilmesi çalışan bir uygulama sağlar kapalı kalma süresi olmadan yapılabilir.

## <a name="monitoring"></a>İzleme
Bulut hizmetleri de sağlar izleme. Gibi Azure sanal makineleri, başarısız bir fiziksel sunucuda algılar ve yeni bir makine, bir sunucuda çalışan sanal makineleri yeniden başlatır. Ancak bulut Hizmetleri başarısız VM'ler ve uygulamalar, yalnızca donanım hataları algılar. Sanal makineler aksine her web ve çalışan rolü içindeki bir aracıya sahip ve bu nedenle hatalar oluştuğunda yeni VM'ler ve uygulama örnekleri başlatılamaz.

Bulut Hizmetleri PaaS yapısını çok diğer etkilere sahiptir. En önemli uygulamaları bu teknolojisini temel web veya çalışan rolü örneklerine başarısız olduğunda doğru çalışması için yazılması gereken biridir. Bunun için bir bulut Hizmetleri uygulaması kendi sanal makineleri, dosya sistemindeki durumunu korumak döndürmemelidir. Azure sanal makineler ile oluşturulan VM'ler farklı olarak, bulut Hizmetleri VM'ler için yapılan yazma kalıcı değildir; hiçbir şey bir sanal makine veri diski gibi yoktur. Bunun yerine, bir bulut Hizmetleri uygulaması açıkça tüm durumu SQL veritabanı, BLOB'lar, tablolar veya başka bir harici depolama yazmanız gerekir. Bu şekilde uygulamaları derleme bunları ölçek ve hataya, bulut Hizmetleri, her iki önemli hedeflerini daha dayanıklı kolaylaştırır.

## <a name="next-steps"></a>Sonraki adımlar
[.NET içinde bir bulut hizmeti uygulaması oluşturma](cloud-services-dotnet-get-started.md)  
[Node.js içinde bir bulut hizmeti uygulaması oluşturma](cloud-services-nodejs-develop-deploy-app.md)  
[PHP ile bir bulut hizmeti uygulaması oluşturma](../cloud-services-php-create-web-role.md)  
[Python içinde bir bulut hizmeti uygulaması oluşturma](cloud-services-python-ptvs.md)

