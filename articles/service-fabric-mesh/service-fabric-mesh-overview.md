---
title: Azure Service Fabric Mesh'e genel bakış | Microsoft Docs
description: Azure Service Fabric Mesh hakkında bilgi edinin. Service Fabric Mesh ile uygulamanızın altyapı gereksinimleri konusunda endişelenmeden uygulamanızı dağıtabilir ve ölçeklendirebilirsiniz.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 10/1/2018
ms.topic: overview
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 44c58e09c478c9dba9143b10d150babc553f4695
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810893"
---
# <a name="what-is-service-fabric-mesh"></a>Service Fabric Mesh nedir?

Bu video Service Fabric Mesh için bir genel bakış sağlamaktadır.
> [!VIDEO https://www.youtube.com/embed/7qWeVGzAid0]

Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Service Fabric Mesh üzerinde barındırılan uygulamalar, altyapı konusunda endişelenmenize gerek kalmadan çalışır ve ölçeklendirilir.  Service Fabric Mesh, binlerce makineden oluşan kümelere sahiptir.  Tüm küme işlemleri geliştiriciden gizli olarak gerçekleştirilir. Tek yapmanız gereken kodunuzu yükleyip ihtiyacınız olan kaynakları, kullanılabilirlik gereksinimlerini ve kaynak sınırlarını belirtmektir.  Service Fabric Mesh, altyapıyı otomatik olarak dağıtır ve altyapı hatalarını da yöneterek uygulamalarınızın yüksek oranda kullanılabilir durumda olmasını sağlar. Altyapı konusunda değil yalnızca uygulamanızın durumuna ve yanıt süresine dikkat etmeniz gerekir.  

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Bu makale, Service Fabric Mesh'in önemli avantajlarına genel bakış niteliğindedir.

## <a name="great-developer-experience"></a>Üstün geliştirici deneyimi

Service Fabric Mesh, kapsayıcı içinde çalıştırılabilen tüm programlama dillerini ve çerçeveleri destekler. Visual Studio 2017 ve Visual Studio Code araçları için sunulan destek, .NET ve .NET Core uygulamaları için güçlü bir düzenleme ve hata ayıklama deneyimi sağlar. 

Service Fabric Mesh ile şunları yapabilirsiniz:

- Var olan uygulamaları kapsayıcılara taşıyarak uygun ölçekte modernleştirebilir ve çalıştırabilirsiniz.
- Azure'da yeni mikro hizmet uygulamalarını uygun ölçekte derleyebilir ve dağıtabilirsiniz.  Diğer Azure hizmetleri veya kapsayıcılarda çalışan mevcut uygulamalarla tümleştirebilirsiniz. Her mikro hizmet CPU çekirdeği, bellek, disk alanı ve daha fazlası için kaynak yönetim ilkelerinin tanımlanmış olduğu güvenli ve ağdan ayrılmış bir uygulamanın parçasıdır.
- Var olan uygulamalarda değişiklik yapmadan tümleştirme ve uzatma işlemleri gerçekleştirebilirsiniz. Var olan uygulamayı yeni uygulamaya bağlamak için kendi sanal ağınızı kullanabilirsiniz.  
- Service Fabric Mesh'e geçiş yaparak var olan Cloud Services uygulamalarınızı modernleştirebilirsiniz.  

## <a name="simple-operational-lifecycle"></a>Basit işlemsel yaşam döngüsü

Uygulama yükseltmesi ve sürüm oluşturma, uygulamaları izleme ve üretim ortamlarında hata ayıklama işlemleri dahil olmak üzere çalışan uygulamaları kolayca yönetebilirsiniz. Bu uygulamalar tek bir mikro hizmetten veya kendi ağında bulunan birden fazla mikro hizmetten oluşabilir. Uygulamalar hızlı dağıtım, yerleştirme ve yük devretme süreleri sayesinde verimli bir şekilde çalışır.

Service Fabric Mesh ile şunları yapabilirsiniz:

- Altyapı sağlamak veya yönetmek zorunda kalmadan uygulamaları dağıtma ve yönetme.  Service Fabric Mesh altyapıyı sizin için sağlar, yükseltir, düzeltme eki uygular ve bakımını yapar.
- Uygulamaları kolayca paketlemek ve dağıtmak için tümleşik araçları kullanarak sürekli tümleştirmeye geçiş yapabilirsiniz.
- Azure'da Service Fabric Mesh hizmetine dağıttığını tüm kaynaklar (uygulamalar, hizmetler, gizli diziler gibi) Azure Resource Manager kaynağı olduğundan Azure Resource Manager kaynaklarının (denetim kaydı ve [rol tabanlı erişim denetimi](/azure/role-based-access-control/overview) gibi) tüm özelliklerinden faydalanabilirsiniz.
- Kaynakları [Azure portal](https://portal.azure.com), Resource Manager şablonları veya Azure CLI/PowerShell kitaplıklarını kullanarak dağıtabilir ve yönetebilirsiniz.
- [Application Insights](/azure/application-insights/)'ı (veya istediğiniz bir aracı) kullanarak işlem izleme ve uyarı ayarlarını yapabilir, platformdan işlem ve tanılama izlemelerini alabilirsiniz.
- [Application Insights](/azure/application-insights/)'ı veya istediğiniz bir aracı kullanarak uygulama modelinden gelen uygulama tanılama bilgilerine erişebilirsiniz.
- Hizmetler için otomatik ölçeklendirme kurallarını uygulama tanımında belirterek kaynak kullanımı iyileştirin.

## <a name="mission-critical-platform-capabilities"></a>Görev açısından kritik platform özellikleri

Service Fabric Mesh, [Azure Kullanılabilirlik Alanları](/azure/availability-zones/az-overview)'nı ve/veya coğrafi bölge sınırlarını kullanan bir küme koleksiyonu oluşturur. Uygulamalar ölçeklendirme, donanım gereksinimleri, dayanıklılık gereksinimleri ve güvenlik ilkeleri gibi bir dizi amaçla tarif edilir.  Uygulama dağıtıldığında Service Fabric Mesh çalıştırmak için en uygun yeri belirler.

Service Fabric Mesh ile şunları yapabilirsiniz:

- Yüksek kullanılabilirlik, ölçek büyütme/küçültme, bulunabilirlik, yönetim, mesaj yönlendirme, güvenilir mesajlaşma, kesintisiz yükseltme, güvenlik/gizli dizi yönetimi, olağanüstü durum kurtarma, durum yönetimi, yapılandırma yönetimi ve dağıtılmış işlem özelliklerinden faydalanabilirsiniz.
- Uygulama oluştururken çoklu uygulama modelleri arasından seçim yapabilirsiniz.
- Swagger tarafından oluşturulan dile özgü bağlamaları kullanarak REST uç noktaları ile sunulan platform özelliklerini kullanabilirsiniz.
- Coğrafi güvenilirlik için uygulamaları birden fazla [Kullanılabilirlik Alanında](/azure/availability-zones/az-overview) ve bölgede dağıtabilirsiniz.
- Azure tarafından sağlanan tüm güvenlik ve uyumluluk özelliklerinden faydalanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yalnızca birkaç adımda Visual Studio ile örnek bir proje dağıtabilirsiniz. Daha fazla bilgi için bkz. [ASP.NET Core web sitesi oluşturma](service-fabric-mesh-quickstart-dotnet-core.md). 

[Sık sorulan sorulara](service-fabric-mesh-faq.md) yanıtlar bulun.


<!-- Links -->

[service-fabric-overview]: ../service-fabric/service-fabric-overview.md
